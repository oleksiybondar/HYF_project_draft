# Frontend Technical Task

## Goal
Implement a frontend application that supports the project’s core user flows and integrates correctly with the **provided API contract**.

- The API contract (endpoints, required fields, pagination rules, auth rules, and error format) is defined in `05-shared-api-contract.md`.
- This document describes the frontend work in **human, user-flow-oriented terms** and clarifies what is **strict** vs where there is **freedom**.

---

## Technology Stack
The target technology stack (framework, language, tooling) is defined by the course and mentors.

Trainees must use the prescribed stack for their cohort. Choosing an alternative stack is out of scope for this task.

---

## Strict vs Flexible

### Strict (must follow the contract)
- All API requests and responses must conform to `05-shared-api-contract.md`
- Authentication rules (public vs protected actions)
- Pagination behavior and query parameters
- Correct handling of error responses
- Core flow semantics: catalog → cart → checkout → orders

### Flexible (you choose)
- Component structure and internal state organization
- UI layout and visual design
- State management approach (local state, context, etc.)
- Interaction patterns (pagination UI, infinite scroll, etc.)
- Styling and presentation

---

## Required Features

### 1) Authentication Flow
The frontend must support a minimal authentication flow:
- user sign up
- user login
- authenticated user state (“logged in / logged out”)

Rules:
- Authenticated state must be derived from the access token
- Protected actions must not be accessible to unauthenticated users
- When a protected action is attempted while unauthenticated, the user must be redirected to login or shown an appropriate message

---

### 2) Catalog (Chosen Subdomain)
The frontend must support browsing a catalog for the chosen subdomain (e.g. items/products, tickets, tables, events).

Users must be able to:
- browse a **paginated** list of entities
- search entities using a query string (**paginated**)
- view entity details

**Creativity space:** you may design the UI and layout freely and display any additional subdomain-specific fields returned by the API.

---

### 3) Cart Interaction
Authenticated users must be able to:
- view the current cart
- add an entity to the cart
- update item quantity
- remove items from the cart

Rules:
- Cart UI must always reflect backend state
- Errors during cart operations must be handled gracefully
- UI must not assume cart state without confirmation from the API

---

### 4) Checkout Flow
Users must be able to:
- complete checkout
- optionally enter a promo code (if supported by the backend)
- receive confirmation of successful order creation

After checkout:
- the cart UI must be reset or cleared
- the user must not be able to re-checkout the same cart
- the UI must transition to an appropriate confirmation or order view

---

### 5) Order History
Authenticated users must be able to:
- view a **paginated** list of past orders
- view order details

---

### 6) Global State Management
The frontend must manage cross-application state in a clear and predictable way.

At minimum:
- authentication state must be accessible across the application
- cart state must be accessible across the application

How this is implemented (context, global store, etc.) is up to the trainee, as long as behavior is correct and explainable.

---

### 7) Loading, Error, and Empty States
For all major user flows (catalog, cart, checkout, orders), the UI must explicitly handle:
- loading states
- error states
- empty states

The user should never be left without feedback during asynchronous operations.

---

## Optional Advanced Features
Optional features may be implemented **only after** all required features are complete and stable.

Examples:
- improved UX for promo code validation
- enhanced filtering or sorting UI
- optimistic updates (with proper fallback handling)

---

## Deliverables
A complete frontend submission includes:
- A running frontend application integrating with the shared API
- Clear instructions to run the application locally
- Basic verification that all required flows work end-to-end

---

## Out of Scope
- Visual perfection or branding quality
- Advanced animations or micro-interactions
- Offline support
- Performance optimization beyond basic correctness