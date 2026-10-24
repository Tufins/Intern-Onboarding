# Day 2 — BookSwap API Design

## Overview
This folder contains the Day-2 BookSwap assignment deliverables, covering API design, storage decisions, system architecture, and mock API testing.

## Deliverables
- `openapi/Inshiraff-day2-bookswap-openapi.yaml` — OpenAPI specification for the BookSwap API.
- `decisions/Inshiraff-day2-storage-decisions.md` — Storage technology decisions and rationale.
- `diagrams/Inshiraff-day2-container-diagram.drawio` — Editable container architecture diagram.
- `diagrams/Inshiraff-day2-container-diagram.png` — PNG version of the architecture diagram.
- `Inshiraff-day2-mock-report.md` — Mock API smoke-test results and observations.

## Validate the OpenAPI Specification
Run from the `Intern-Onboarding` project root:

```powershell
npx.cmd @apidevtools/swagger-cli validate ".\Day-2\openapi\Inshiraff-day2-bookswap-openapi.yaml"
```

## Run the Mock API
Start the Prism mock server:

```powershell
npx.cmd @stoplight/prism-cli mock ".\Day-2\openapi\Inshiraff-day2-bookswap-openapi.yaml"
```

The mock server should be available at `http://127.0.0.1:4010`.

## Smoke Tests
The report covers:
1. Listing books with authentication.
2. Creating a book with a valid payload.
3. Rejecting a book with a missing title.
4. Creating a borrow request using a UUID-formatted book ID.
5. Rejecting a book-list request without an Authorization header.

## Testing Limitations
These tests use a Prism mock server, not a deployed backend. The placeholder `test-token` is not a verified JWT, and generated mock responses do not prove that database persistence, authentication, or email delivery works in a real system.
