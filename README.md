# Postman + Swagger + TestRail — QA Portfolio

A complete QA portfolio showcasing both **manual** and **automated** API/UI testing workflows, with test management in TestRail. Two self-contained projects demonstrate the full testing lifecycle — from spec analysis and test case design through execution, result reporting, and defect tracking.

---

## Projects

### 1. [Swagger + Postman + TestRail — Manual API Testing](swagger-postman-testrail-manual/)

Manual API testing of the [Swagger Petstore](https://petstore.swagger.io/) using Postman, with test cases managed in TestRail.

- **API Under Test**: Swagger Petstore (OpenAPI 2.0) — 20 endpoints across 3 tags
- **Tools**: Postman (23 requests with chained variables and assertions), Newman CLI, TestRail
- **Test Cases**: 25 manual test cases (TC-01 – TC-25) covering positive flows, negative tests, and boundary conditions
- **Results**: 22/25 PASSED — 3 failures traced to API bugs
- **Bugs Filed**: 3 defects (credentials in query string, missing deprecation headers, no server-side validation)

> **Milestone Report**: [Petstore API v1.0 — Manual QA](swagger-postman-testrail-manual/artifacts/testrail-milestone-report.pdf) — 84% pass rate

---

### 2. [TestRail Automation Integration — CI/CD Reporting](testrail-automation-integration/)

Automated test result reporting from a Cucumber/Playwright test suite to TestRail, triggered by Jenkins CI/CD. Every Cucumber scenario posts its PASS/FAIL status to TestRail in real time via a custom Java `@After` hook.

- **Application Under Test**: [SauceDemo](https://www.saucedemo.com/) (e-commerce demo site)
- **Tools**: Cucumber 7.x (BDD), Playwright (Java), Maven, Jenkins, TestRail API v2
- **Test Cases**: 13 automated test cases (C2–C14) across 6 sections — login, PDF receipt, product images, sort, session management, visual regression
- **Results**: 13/13 PASSED — 100% automated, zero manual entry
- **Integration**: Custom `@After` hook reads `@Cxx` tags and POSTs results to TestRail automatically

> **Milestone Report**: [SauceDemo v1.0 — UAT Ready](testrail-automation-integration/reports/testrail-milestone-report.pdf) — 100% pass rate

---

## How They Complement Each Other

| Dimension | Manual (Petstore) | Automated (SauceDemo) |
|-----------|-------------------|----------------------|
| **Testing Type** | Manual API testing via Postman | Automated UI/E2E testing via Playwright |
| **Spec Source** | OpenAPI/Swagger spec analysis | BDD feature files (Gherkin) |
| **Execution** | Postman Collection Runner / Newman | Jenkins CI/CD pipeline |
| **Result Reporting** | Manual entry in TestRail | Automated via TestRail API hook |
| **Defect Tracking** | Markdown bug reports (Jira-style) | Automated pass/fail — no defects found |

---

## Author

**Jigyasa Gupta** — QA Engineer
[GitHub](https://github.com/jgupta-git)
