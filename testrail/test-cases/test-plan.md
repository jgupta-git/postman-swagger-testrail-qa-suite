# TestRail Test Plan — SauceDemo QA Portfolio

## Milestone

| Field | Value |
|-------|-------|
| **Name** | SauceDemo v1.0 — UAT Ready |
| **Description** | End-to-end validation of SauceDemo e-commerce site covering login, checkout, product integrity, sorting, session management, and visual regression |
| **Status** | Completed |

## Test Plan

| Field | Value |
|-------|-------|
| **Name** | Regression & Smoke Test Suite |
| **Milestone** | SauceDemo v1.0 — UAT Ready |
| **Description** | Full regression suite executed via Cucumber/Playwright automation with results posted to TestRail through Jenkins CI/CD |

## Test Run

| Field | Value |
|-------|-------|
| **Run ID** | 3 |
| **Name** | Automated Regression — Full Suite |
| **Assignee** | Jigyasa Gupta |
| **Include Cases** | All 13 cases (C2–C14) |
| **Result** | 13/13 PASSED (100%) |

## Test Case Coverage by Section

| Section | Cases | IDs | Priority |
|---------|-------|-----|----------|
| Login | 4 | C2–C5 | High / Medium |
| PDF Receipt | 1 | C6 | High |
| Product Image Integrity | 2 | C7–C8 | Medium |
| Product Sort | 2 | C9–C10 | Medium |
| Session Management | 2 | C11–C12 | Medium / High |
| Visual Regression | 2 | C13–C14 | Medium |

## Automation Details

| Component | Technology |
|-----------|-----------|
| Test framework | Cucumber 7.x + Playwright (Java) |
| Runner | Maven (`saucedemo` profile) |
| CI/CD | Jenkins freestyle job (`saucedemo_run`) |
| Reporting hook | `TestRailReportingHook.java` — Cucumber `@After` hook |
| Result posting | TestRail API v2 (`add_result_for_case`) |
| Tag convention | `@Cxx` on each Cucumber scenario → maps to TestRail case ID |

## How Results Flow

```
Cucumber Scenario (@C2 tag)
  → Playwright runs the browser test
  → @After hook fires (TestRailReportingHook)
  → Extracts case ID from @Cxx tag
  → POSTs PASSED (1) or FAILED (5) to TestRail API
  → TestRail Run #3 updates automatically
```

## Configuration

Results posting is controlled by Jenkins build parameters:

- `TESTRAIL_URL` — TestRail instance (no trailing slash)
- `TESTRAIL_USER` — login email
- `TESTRAIL_API_KEY` — bound via Jenkins Credentials (never in build logs)
- `TESTRAIL_RUN_ID` — active run ID
- `TESTRAIL_ENABLED` — set `false` to skip posting

See [`jenkins-config.md`](../automation-integration/jenkins-config.md) for full Jenkins job setup.

## Notes

- TestRail instance: `testingdemoforinterview.testrail.io` (30-day trial)
- The `add_result_for_case` endpoint takes **two** path parameters: `{run_id}/{case_id}` — no project ID
- Case IDs are numeric (extracted from `@C2` → `2`)
- Status codes: `1` = Passed, `5` = Failed
