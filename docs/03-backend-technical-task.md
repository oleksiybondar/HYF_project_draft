# Backend Technical Task

## Goal
Implement a backend service that supports the project’s core flows and integrates with the **provided API contract**.

- The contract (endpoints, required fields, request/response schemas, pagination rules, auth rules, and error format) is defined in `05-shared-api-contract.md`.
- This document describes the backend work in **human, feature-oriented terms** and clarifies what is **strict** vs where there is **freedom**.

---

## Technology Stack
The target technology stack (language, framework, database, tooling) is defined by the course and mentors.

Trainees must use the prescribed stack for their cohort. Choosing an alternative stack is out of scope for this task.

---

## Strict vs Flexible

### Strict (must follow the contract)
- All required endpoints, request/response shapes, and behaviors defined in `05-shared-api-contract.md`
- Authentication boundary rules (public vs protected routes)
- Pagination behavior and response shape for collection endpoints
- Standard error response format
- Core flow semantics: catalog → cart → checkout → orders

### Flexible (you choose)
- Internal structure (layers, folders, design patterns)
- Database schema design (as long as behavior matches the contract)
- Validation strategy (where and how you validate inputs)
- Testing approach and tooling
- Additional subdomain-specific fields (within the chosen subdomain)

---

## Required Features

### 1) Authentication
Provide a minimal username/password authentication flow:
- Users can sign up
- Users can log in and receive an access token
- The backend can return the current authenticated user (“who am I”)

All technical details (JWT usage, headers, endpoint names, and responses) must match `05-shared-api-contract.md`.

---

### 2) Catalog (Chosen Subdomain)
Choose one subdomain (skin) for the catalog, for example:
- products/items
- tickets
- tables
- events

Users must be able to:
- browse a **paginated** list of entities
- search entities using a query string (**paginated**)
- view an entity detail

**Creativity space:** you may add any reasonable number of additional fields that fit your chosen subdomain (e.g., category, date, location, capacity).  
**Constraint:** the required base fields and contract behavior must remain intact.

---

### 3) Cart Management
Authenticated users must have a **single active cart**.

Users must be able to:
- view the active cart
- add an entity to the cart
- update quantity
- remove items

The backend must keep totals consistent and return a predictable cart representation as defined in the contract.

---

### 4) Checkout → Order
Users must be able to complete checkout:
- checkout finalizes the active cart into an order
- an order is immutable after creation (from the API perspective)
- the order represents a snapshot of the cart at the time of checkout

After successful checkout, the active cart is cleared or replaced, as long as the externally visible behavior matches the contract.

---

### 5) Order History
Authenticated users must be able to:
- list past orders (**paginated**)
- view order details

---

### 6) Pagination and Search Consistency
Collection endpoints must:
- require `page`
- accept `limit` (optional)
- return `page`, `limit`, `total`, `items`

Search must:
- require `q`
- be paginated
- behave consistently (no “sometimes paginated, sometimes not”)

---

### 7) Error Handling
All error responses must follow the standard error shape in `05-shared-api-contract.md`.

At minimum, handle:
- invalid input (missing required fields, invalid values)
- not found resources
- unauthorized access (missing/invalid token)
- conflict cases where applicable (e.g., invalid/used promo code if that feature is implemented)

---

## Optional Advanced Features
Optional features may be implemented **only after** all required features are complete and stable.

### Promo Codes (Optional)
If implemented, support applying promo codes during checkout and (optionally) provide promo-code management endpoints as described in `05-shared-api-contract.md`.

---

## Deliverables
A complete backend submission includes:
- A running API implementing the contract-defined flows
- Persistent storage (database) appropriate to the prescribed cohort stack
- A reproducible setup (clear run instructions)
- Basic API verification (e.g., a minimal test suite or a clear manual test script)

---

## Out of Scope
- Production-grade security hardening
- Performance tuning beyond basic correctness
- Advanced authentication flows (SSO/OIDC, refresh token rotation) unless explicitly assigned as optional work