# Shared API Contract

> This document defines the **shared API contract** used for integration between frontend and backend implementations. It is intentionally **minimal** and focused on interoperability, predictable behavior, and a stable mental model.
>
> - Base path is fixed: `/api`
> - Authentication uses **JWT access tokens** (refresh is optional/advanced).
> - The contract defines a **fixed set of core flows** (catalog → cart → checkout → orders).
> - Each trainee implements **one chosen subdomain** (e.g. products, tickets, tables). Subdomains share the same route set and semantics.

---

## Global Conventions

### Base URL

All endpoints are prefixed with:

- `/api`

### Authentication

Protected endpoints require the header:

- `Authorization: Bearer <accessToken>`

If the token is missing or invalid:

- respond with `401 Unauthorized` using the error format defined below.

### Common Data Types

- `id`: string (implementation may use UUID, integer, etc., but must be stable)
- `price`: number
- `timestamp`: ISO 8601 string

### Standard Error Format

All error responses must follow this shape:

```json
{
  "error": {
    "code": "STRING_CODE",
    "message": "Human readable message",
    "details": {
      "field": "optional",
      "reason": "optional"
    }
  }
}
```

Notes:

- `details` is optional.
- Exact `code` values are not fully standardized here; the key requirement is **consistency**.

---

## Pagination

Endpoints returning collections must support pagination.

### Query Parameters

- `page` (required): integer, 1-based
- `limit` (optional): integer (a default may be used; a maximum limit may be enforced)

### Response Shape

Paginated responses must follow:

```json
{
  "page": 1,
  "limit": 20,
  "total": 123,
  "items": []
}
```

---

## Subdomains (Skins)

A **subdomain** is the chosen resource vocabulary representing the theme.

Examples:

- `/api/items` (products)
- `/api/tickets`
- `/api/tables`
- `/api/events`

All subdomains expose the **same route set** and **same semantics**.

### Required Entity Fields (Base)

All entities returned from a subdomain endpoint must include the following required base fields:

```json
{
  "id": "string",
  "name": "string",
  "price": 0
}
```

Trainees may add any reasonable number of additional, subdomain-specific fields (e.g. `category`, `date`, `capacity`, `location`) as long as:

- the required base fields remain present and correct
- core flow semantics remain unchanged
- additional fields are documented for the chosen subdomain

---

## Authentication & User Endpoints

### Sign Up

`POST /api/auth/signup`

**Request**

```json
{
  "username": "string",
  "password": "string"
}
```

**Response (201)**

```json
{
  "user": {
    "id": "string",
    "username": "string"
  }
}
```

---

### Login

`POST /api/auth/login`

**Request**

```json
{
  "username": "string",
  "password": "string",
  "displayName": "string",
  "email": "string"
}
```

**Response (200)**

```json
{
  "accessToken": "string",
  "user": {
    "id": "string",
    "username": "string"
  }
}
```

Notes:

- Access token lifetime is intentionally long for training (e.g., up to 1 day).

---

### Logout (Optional)

`POST /api/auth/logout`

**Behavior**

- In access-token-only mode, logout is primarily client-side (discard token).
- Endpoint may exist as a no-op for consistency.

**Response (200)**

```json
{ "ok": true }
```

---

### Current User

`GET /api/auth/me`

**Response (200)**

```json
{
  "id": "string",
  "username": "string",
  "profile": {
    "displayName": "string",
    "email": "string"
  }
}
```

---

### Update Profile (Optional)

`PUT /api/users/me`

**Request**

```json
{
  "profile": {
    "displayName": "string",
    "email": "string"
  }
}
```

**Response (200)**

```json
{
  "id": "string",
  "username": "string",
  "profile": {
    "displayName": "string",
    "email": "string"
  }
}
```

---

### Change Password (Optional)

`POST /api/users/me/change-password`

**Request**

```json
{
  "currentPassword": "string",
  "newPassword": "string"
}
```

**Response (200)**

```json
{ "ok": true }
```

---

## Catalog (Subdomain Entities)

The following endpoints exist for each chosen subdomain.

### List

`GET /api/{domain}`

**Query params**

- `page` (required)
- `limit` (optional)

**Response (200)**

```json
{
  "page": 1,
  "limit": 20,
  "total": 123,
  "items": [
    { "id": "1", "name": "Sample", "price": 10 }
  ]
}
```

---

### Search

`GET /api/{domain}/search`

**Query params**

- `q` (required)
- `page` (required)
- `limit` (optional)

Optional query params (if implemented):

- `sort` (e.g. `price`, `name`, `createdAt`)
- `order` (`asc` or `desc`)

**Response** Same shape as List.

---

### Detail

`GET /api/{domain}/{id}`

**Response (200)**

```json
{ "id": "1", "name": "Sample", "price": 10 }
```

---

### Create (Optional)

`POST /api/{domain}`

**Request**

```json
{ "name": "string", "price": 0 }
```

**Response (201)**

```json
{ "id": "string", "name": "string", "price": 0 }
```

---

### Update (Optional)

`PUT /api/{domain}/{id}`

**Request**

```json
{ "name": "string", "price": 0 }
```

**Response (200)**

```json
{ "id": "string", "name": "string", "price": 0 }
```

---

### Delete (Optional)

`DELETE /api/{domain}/{id}`

**Response (200)**

```json
{ "ok": true }
```

---

## Cart

Cart endpoints require authentication.

### Read Active Cart

`GET /api/cart`

**Response (200)**

```json
{
  "id": "string",
  "status": "active",
  "items": [
    {
      "itemId": "string",
      "name": "string",
      "unitPrice": 0,
      "quantity": 1,
      "lineTotal": 0
    }
  ],
  "subtotal": 0,
  "discount": 0,
  "total": 0
}
```

Notes:

- A user has at most one active cart.

---

### Add Item to Cart

`POST /api/cart/items`

**Request**

```json
{ "itemId": "string", "quantity": 1 }
```

**Response (200)** Returns updated cart (same shape as `GET /api/cart`).

---

### Update Cart Item Quantity

`PATCH /api/cart/items/{itemId}`

**Request**

```json
{ "quantity": 2 }
```

**Response (200)** Returns updated cart.

---

### Remove Item from Cart

`DELETE /api/cart/items/{itemId}`

**Response (200)** Returns updated cart.

---

### Checkout (Finalize Cart)

`POST /api/cart/checkout`

**Request (optional promo code)**

```json
{ "promoCode": "WELCOME10" }
```

**Response (201)**

```json
{
  "order": {
    "id": "string",
    "createdAt": "timestamp",
    "items": [
      {
        "itemId": "string",
        "name": "string",
        "unitPrice": 0,
        "quantity": 1,
        "lineTotal": 0
      }
    ],
    "subtotal": 0,
    "discount": 0,
    "total": 0
  }
}
```

Notes:

- Checkout transitions the active cart into an immutable order.
- After successful checkout, the active cart is cleared or replaced.

---

## Orders

Order endpoints require authentication.

### List Orders (History)

`GET /api/orders`

**Query params**

- `page` (required)
- `limit` (optional)

**Response (200)**

```json
{
  "page": 1,
  "limit": 20,
  "total": 12,
  "items": [
    {
      "id": "string",
      "createdAt": "timestamp",
      "subtotal": 0,
      "discount": 0,
      "total": 0
    }
  ]
}
```

---

### Order Detail

`GET /api/orders/{orderId}`

**Response (200)**

```json
{
  "id": "string",
  "createdAt": "timestamp",
  "items": [
    {
      "itemId": "string",
      "name": "string",
      "unitPrice": 0,
      "quantity": 1,
      "lineTotal": 0
    }
  ],
  "subtotal": 0,
  "discount": 0,
  "total": 0
}
```

---

## Promo Codes (Optional Feature)

Promo code endpoints are optional. When implemented, promo codes are applied at checkout.

### Create Promo Code (Optional)

`POST /api/promo-codes`

**Request**

```json
{
  "code": "WELCOME10",
  "type": "percent",
  "value": 10
}
```

**Response (201)**

```json
{
  "code": "WELCOME10",
  "type": "percent",
  "value": 10,
  "used": false
}
```

---

### Validate Promo Code (Optional)

`GET /api/promo-codes/{code}`

**Response (200)**

```json
{
  "code": "WELCOME10",
  "type": "percent",
  "value": 10,
  "used": false
}
```

If code is invalid or already used:

- respond with `404` or `409` using the standard error format.

---

### Delete Promo Code (Optional)

`DELETE /api/promo-codes/{code}`

**Response (200)**

```json
{ "ok": true }
```

---

## Notes on Flexibility

- Subdomain naming (e.g., `items` vs `tickets`) is flexible, but route semantics are fixed.
- Additional subdomain-specific fields are allowed, but must not break the required base fields or core flows.
- Implementation choices (database, framework, internal structure) are out of scope of this contract.

