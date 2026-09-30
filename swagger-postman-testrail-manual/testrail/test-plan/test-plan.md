# TestRail Test Plan — Petstore API Manual Testing

## Milestone

| Field | Value |
|-------|-------|
| **Name** | Petstore API v1.0 — Manual QA |
| **Description** | Manual API testing of the Swagger Petstore covering all 20 endpoints with positive, negative, and boundary tests |
| **Status** | Completed |

## Test Plan

| Field | Value |
|-------|-------|
| **Name** | Petstore API — Full Manual Regression |
| **Milestone** | Petstore API v1.0 — Manual QA |
| **Description** | Manual regression suite executed via Postman with test cases tracked in TestRail |

## Test Run

| Field | Value |
|-------|-------|
| **Run ID** | 1 |
| **Name** | Manual Regression — Petstore API |
| **Assignee** | Jigyasa Gupta |
| **Include Cases** | All 25 cases (TC-01 – TC-25) |
| **Result** | 22/25 PASSED, 3 FAILED (bugs filed) |

## Test Case Coverage by Section

| Section | Cases | IDs | Type | Priority |
|---------|-------|-----|------|----------|
| Pet (positive) | 8 | TC-01 – TC-08 | Functional | High / Medium |
| Store (positive) | 4 | TC-09 – TC-12 | Functional | High / Medium |
| User (positive) | 8 | TC-13 – TC-20 | Functional | High / Medium |
| Negative tests | 5 | TC-21 – TC-25 | Negative | High / Medium |

## Tools

| Component | Technology |
|-----------|-----------|
| API spec | Swagger / OpenAPI 2.0 |
| API client | Postman (manual execution + test scripts) |
| CLI runner | Newman (Postman CLI) |
| Test management | TestRail |
| Target API | [Swagger Petstore](https://petstore.swagger.io/) |

## How Results Flow

```
Postman Collection (23 requests with test scripts)
  → Execute manually or via Newman CLI
  → Record PASS/FAIL per test case
  → Update TestRail test run with results
  → File bug reports for failures
```

## Configuration

| Parameter | Value |
|-----------|-------|
| Base URL | `https://petstore.swagger.io/v2` |
| Auth | API Key (`special-key` in header for delete operations) |
| Environment | Petstore — Dev |

## Notes

- The Petstore API is a public demo — data resets periodically and is shared across all users
- 3 bugs were found and documented in the [`bug-reports/`](../bug-reports/) directory
- Test cases are written in TestRail CSV import format for easy upload
- Newman reports can be generated with: `newman run collections/Petstore-API-Tests.postman_collection.json -e environments/Petstore-Dev.postman_environment.json --reporters cli,html`
