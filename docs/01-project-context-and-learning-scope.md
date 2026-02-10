# Project Context & Learning Scope

## Purpose of the Project
This project defines a shared, realistic training exercise intended to prepare trainees for junior frontend and backend roles. The primary goal is to develop **problem-solving and planning skills**: understanding a problem space, modeling data and state, and implementing coherent flows against a given contract.

Writing code is treated as an outcome of correct reasoning, not the objective by itself. Code that works but is not understood is considered insufficient for this exercise.

The project is designed to simulate real-world collaboration, where one side of the system is given and trainees must integrate with it correctly.

---

## Learning Scope
The learning scope focuses on foundational capabilities required for junior engineers:

- Translating a problem description into data models, states, and flows
- Working within explicit constraints and assumptions
- Integrating with a predefined API contract
- Handling asynchronous behavior, errors, and partial states
- Making deliberate trade-offs rather than implementing ad-hoc solutions

This project is **not** intended to produce production-grade software. Maintainability, scalability, and advanced optimization are out of scope at this stage, but **clear understanding of behavior and intent is mandatory**.

---

## Type of System Simulated
The exercise simulates a user-facing, transactional application with the following characteristics:

- A catalog or list of items that users can browse and search
- Authenticated users can create and manage a temporary collection (e.g. cart, list)
- A controlled transition from a temporary state to a finalized state (e.g. order, booking)
- Users can view historical results of completed actions

While the concrete theme may vary (e-commerce, ticketing, reservations, events), the **core flows and responsibilities remain the same**.

### Subdomains (Skins)
The same project structure can represent multiple real-world subdomains (e.g. e-commerce products, cinema tickets, event registrations, table reservations, etc.). These subdomains are treated as **skins**: they reuse the **same core flows and responsibilities**, while changing naming, presentation, and optional extra fields.

HYF provides an etalon backend + frontend that can support multiple subdomains as an integration target. The internal implementation of this flexibility is intentionally not described here.

---

## Roles and Responsibilities

### Frontend Trainee
The frontend trainee is responsible for implementing user flows and UI state management while integrating with the provided backend contract. This includes:

- Correct handling of authentication state and access restrictions
- Mapping UI interactions to API queries (search, pagination, mutations)
- Managing global and local state transitions across the application
- Handling loading, error, and empty states consistently

### Backend Trainee
The backend trainee is responsible for modeling data and enforcing correctness through explicit logic or constraints. This includes:

- Designing coherent data models and relationships
- Enforcing entity lifecycles and ownership rules
- Providing predictable, bounded query behavior (e.g. pagination, filtering)
- Returning consistent and meaningful error responses

Both roles are evaluated on correctness, clarity of reasoning, and adherence to the shared contract.

---

## Shared Contract and Integration
A core premise of the exercise is that frontend and backend work **against a shared contract**.

- Trainees must assume that the other side of the system is implemented independently
- API shape, error formats, and pagination behavior are part of the contract
- Mocking arbitrary or invented data structures is discouraged

A reference (etalon) backend and frontend are provided by HYF to serve as a stable integration target and feasibility proof. Their internal implementation details are irrelevant at this stage.

---

## Intentional Simplifications and Assumptions
To keep the scope achievable and focused on learning goals, the following simplifications apply:

- Authentication is intentionally simple
- Inventory is assumed to be unlimited
- No accounting, shipping, taxation, or refunds are modeled
- No advanced security or concurrency guarantees are required
- The system is single-tenant and user-centric

These assumptions are part of the problem definition and should not be expanded unless explicitly stated in later documents.

---

## Scope Boundaries

### In Scope
- Clear definition of entities and their relationships
- Explicit user flows and state transitions
- Integration with a predefined API contract
- Correct handling of errors and asynchronous states

### Out of Scope
- Production-grade security and performance
- Advanced domain features beyond the defined core flows
- Architectural optimization or framework comparison
- Mentor evaluation or defense procedures

---

## Outcome of This Document
This document establishes a **shared mental model** for the project. It intentionally avoids technical implementation details and serves as the foundation for subsequent documents that define themes, tasks, and technical requirements.

All further specifications build on the context and boundaries defined here.

