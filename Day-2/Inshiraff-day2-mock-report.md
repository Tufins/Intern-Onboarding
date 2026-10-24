# BookSwap — Mock Smoke Test Report

**Project:** BookSwap community book-sharing API  
**Specification:** `Day-2/openapi/Inshiraff-day2-bookswap-openapi.yaml`  
**Mock server:** Stoplight Prism  
**Test date:** 24 October 2026  
**Environment:** Local Prism mock at `http://127.0.0.1:4010`

## 1. Setup

Started the mock server from the project root using:

```powershell
npx.cmd @stoplight/prism-cli mock ".\Day-2\openapi\Inshiraff-day2-bookswap-openapi.yaml"
```

Prism started at `http://127.0.0.1:4010`.

Requests were sent from a second PowerShell terminal using `curl.exe` and `Invoke-WebRequest -UseBasicParsing`. No Postman/Bruno collection was used.

**Important test limitation:** These tests exercise Prism's generated mock responses, not a deployed BookSwap API. The placeholder `Bearer test-token` is not a real JWT. These results do not prove real token signature validation, database persistence, or production business logic.

## 2. Results summary

| # | Endpoint | Method | Request | Expected status | Actual status | Result |
|---|---|---|---|---:|---:|---|
| 1 | `/books?page=1&pageSize=20` | GET | Bearer token supplied | 200 | 200 | Pass |
| 2 | `/books` | POST | Valid-looking book payload (`title`, `author`, `isbn`, `condition`) | 201 | 201 | Pass |
| 3 | `/books` | POST | Book payload with `title` omitted | 400 or 422 | 422 | Pass |
| 4 | `/books/{bookId}/borrow-requests` | POST | Valid UUID in path; Bearer token supplied; no body | 201 | 201 | Pass with valid UUID |
| 5 | `/books?page=1&pageSize=20` | GET | No `Authorization` header | 401 | 401 | Pass |

**Summary:** 5 tests were run in the retest, and all 5 returned the expected status codes for the requests shown above.

### Additional observation for Test 4

The assignment's sample request uses `/books/999/borrow-requests`, but the OpenAPI path parameter defines `bookId` as a string with `format: uuid` (around lines 98–104). Sending `999` returned `400`. Retesting with a UUID-formatted ID returned `201`.

The table records the successful UUID-formatted request. The literal `999` attempt should not be described as a pass; it revealed a mismatch between the sample test input and the API contract.

## 3. Findings

### Finding 1 — The mock returns generated data, not necessarily the submitted values

The valid book creation request returned `201 Created`, but Prism's response contained generated sample values such as `"title":"string"` and `"condition":"new"` rather than the values submitted (`Atomic Habits` and `good`).

**Meaning:** This is a mock response, so the status code shows that the request matched the mock contract. It does not prove that a real API stores or returns the submitted values.

### Finding 2 — `bookId` must be UUID-formatted

The borrow-request endpoint is defined at `/books/{bookId}/borrow-requests`. Its `bookId` path parameter is required and uses `format: uuid` (around lines 98–104).

- Using `999` returned `400`.
- Using a UUID-formatted value returned `201`.

**Meaning:** Test data should follow the OpenAPI contract. The assignment's sample `/books/999/borrow-requests` conflicts with the UUID requirement and should be updated to use a UUID.

### Finding 3 — Pagination response values need closer review

For the authenticated `GET /books?page=1&pageSize=20` request, Prism returned `200 OK`, but its generated body included `"page":0` and `"pageSize":0`.

**Meaning:** The mock's sample response does not reflect the requested pagination values. This may be a generated example/default issue rather than a real API defect, but the response schema and examples should be reviewed so the expected page numbering is clear.

## 4. Specification changes recommended

1. **Align the smoke-test example with the UUID contract.**  
   **File:** `Day-2/openapi/Inshiraff-day2-bookswap-openapi.yaml`  
   **Location:** `POST /books/{bookId}/borrow-requests`, approximately lines 92–104.  
   Keep the `uuid` format if UUIDs are the intended book IDs, and update the assignment/test command to use a UUID instead of `999`. If numeric IDs are intended instead, change the schema and examples consistently.

2. **Clarify pagination constraints and examples.**  
   **File:** `Day-2/openapi/Inshiraff-day2-bookswap-openapi.yaml`  
   **Location:** `GET /books` query parameters and the page-response schema.  
   Define `page` as an integer with `minimum: 1` and `default: 1`; define `pageSize` as an integer with `minimum: 1`, a sensible maximum (for example, 100), and `default: 20`. Ensure the `200` response example uses `page: 1` and `pageSize: 20` for a request with those parameters. Confirm exact line numbers in the local file before submitting.

3. **Make the mock-auth limitation explicit in test documentation.**  
   **File:** `Day-2/Inshiraff-day2-mock-report.md`  
   Document that `Bearer test-token` is a placeholder accepted by the mock and is not evidence of real JWT verification. A separate integration test against the actual API/authentication configuration is required to verify JWT validation.

## 5. Re-running the tests

1. Open PowerShell at the project root.
2. Start Prism with the command in the Setup section and leave that terminal open.
3. Open a second PowerShell terminal.
4. Run the five requests and compare the returned status codes with the table above.
5. For the authenticated GET test, use an `Authorization: Bearer test-token` header. For the unauthenticated test, omit the header.
6. For the borrow-request test, use a UUID-formatted `bookId`; the endpoint does not define a request body.
7. Record any changed responses rather than assuming the mock always returns the same generated sample values.

## 6. Validation note

The OpenAPI file was previously validated with:

```powershell
npx.cmd @apidevtools/swagger-cli validate ".\Day-2\openapi\Inshiraff-day2-bookswap-openapi.yaml"
```

The reported validation result was successful. Re-run validation after making any specification changes.
