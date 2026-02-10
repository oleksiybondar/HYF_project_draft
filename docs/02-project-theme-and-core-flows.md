# Project Theme & Core Flows

## Theme Overview
The project is centered around a **simple transactional domain** commonly found in many real-world systems, such as e‑commerce, ticket sales, reservations, event registrations, or similar use cases.

Despite differences in naming or presentation, these domains share the same structural characteristics:
- a list of available items that users can browse and search
- authenticated users can select items into a temporary collection
- the temporary collection can be finalized into a completed action
- users can later review their completed actions

This project intentionally focuses on these shared characteristics rather than on domain-specific details.

---

## Core Conceptual Entities
The following entities are defined at a conceptual level. Concrete schemas and representations are defined in later documents.

- **User** – an authenticated identity performing actions in the system
- **Item** – a purchasable or reservable unit presented to users
- **Cart** – a temporary, mutable collection of selected items belonging to a user
- **Order** – a finalized, immutable result of completing a cart

These entities exist in all supported themes, even if they are named differently (e.g. product, ticket, seat, session).

---

## Mandatory Core Flows
The system must support the following flows end‑to‑end:

1. **Browsing and Searching**
   - Users can view a list of items
   - Users can search and paginate through available items

2. **Authentication Gate**
   - Unauthenticated users can browse items
   - Mutating actions require authentication
   - Unauthorized actions result in a controlled redirect or error

3. **Cart Lifecycle**
   - Authenticated users can create or access a single active cart
   - Items can be added, updated, or removed
   - Cart state remains consistent across interactions

4. **Checkout / Finalization**
   - A cart can be finalized into an order
   - The cart transitions from a mutable to an immutable state
   - The finalized order represents a snapshot of the cart at completion time

5. **History and Review**
   - Users can view a list of completed orders
   - Order details are accessible after completion

These flows define the minimum functional scope of the project.

---

## Frontend Achievement Definition
A frontend implementation is considered complete when it demonstrates:

- Correct implementation of all mandatory user flows
- Explicit handling of authentication state and access restrictions
- Deterministic mapping between UI state and API queries (search, pagination, mutations)
- Reliable synchronization of cart state with the backend
- Correct UI cleanup and state reset after order completion
- Clear handling of loading, error, and empty states

Frontend success is measured by **flow correctness and state management**, not visual polish.

---

## Backend Achievement Definition
A backend implementation is considered complete when it demonstrates:

- Explicit modeling of core entities and their relationships
- Enforcement of entity lifecycles and ownership rules
- Predictable and bounded query behavior (pagination and filtering)
- Correct handling of authentication and authorization boundaries
- Consistent and meaningful error responses

Backend success is measured by **correctness, clarity of modeling, and reasoning**, not by technical complexity or optimization.

---

---

## Subdomains, Shared API Paths, and Extensible Fields
Trainees may choose a **subdomain (skin)** such as products, tickets, tables, events, or similar. A subdomain changes vocabulary and presentation but **does not change the core flows** defined in this document.

### Shared route set per subdomain (conceptual)
Each subdomain exposes the same route set under its base path. For example, using `items` as one subdomain:

- `GET /items?page=...`
- `GET /items/search?...&page=...`
- `GET /items/{id}`
- `POST /items`
- `PUT /items/{id}`
- `DELETE /items/{id}`

A different subdomain (e.g. `tickets`, `tables`, `movies`) uses the same route set under its own base path (e.g. `/tickets/...`, `/tables/...`) while preserving identical behavior and flow semantics.

### Mandatory fields and subdomain-specific fields
To keep the contract stable across subdomains, each entity includes a small set of **mandatory base fields** (for example: `name`, `price`). Subdomains may define additional fields that are specific to their skin. These additional fields are treated as **extensible fields** and are represented consistently in the shared API contract (defined later in `05-shared-api-contract.md`).

### Etalon frontend modes
The HYF-provided etalon frontend supports:
- a **themed mode** for a canonical subdomain with “nice” UI components, and
- a **generic mode** that can render and submit subdomain-specific fields based on configuration (less visually polished, but integratable).

---

## Optional and Advanced Scope
Optional features may be implemented **only after** all mandatory flows are complete and stable. Examples include:

- discounts or promotional rules
- item variants or categories
- simple stock limits
- sandboxed payment simulation

Optional scope exists to allow deeper exploration but is not required for completion of the project.

---

## Rationale for Theme and Scope
This theme was selected because it:

- forces explicit data modeling and state transitions
- is understandable without domain expertise
- naturally exercises frontend and backend responsibilities equally
- allows multiple valid implementation approaches
- scales across different visual or narrative themes without changing core logic

The defined scope represents a balanced and realistic baseline for junior frontend and backend readiness.

