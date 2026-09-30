# Jenkins CI/CD Configuration for TestRail Integration

## Overview

This document describes how the Jenkins freestyle job `saucedemo_run` is configured to run Cucumber/Playwright tests and automatically post results to TestRail.

## Architecture

```
Jenkins Job (saucedemo_run)
  ├── Git: pulls saucedemo-qa-portfolio repo
  ├── Maven: runs Cucumber tests with -D flags
  ├── TestRailReportingHook.java: @After hook reads @Cxx tags
  └── TestRail API: POST /api/v2/add_result_for_case/{run_id}/{case_id}
```

## Jenkins Job Configuration

### Source Code Management
- **Git Repository**: `https://github.com/jgupta-git/saucedemo-qa-portfolio.git`
- **Branch**: `*/main`

### Build Parameters (String Parameters)

| Parameter | Default Value | Description |
|-----------|--------------|-------------|
| `TESTRAIL_URL` | `https://testingdemoforinterview.testrail.io` | TestRail instance URL (no trailing slash) |
| `TESTRAIL_USER` | `jigyasagupta2@gmail.com` | TestRail login email |
| `TESTRAIL_RUN_ID` | `3` | Active test run ID in TestRail |
| `TESTRAIL_ENABLED` | `true` | Set to `false` to skip posting results |

### Build Environment — Credentials Binding

| Variable | Credential ID | Type |
|----------|--------------|------|
| `TESTRAIL_API_KEY` | `TESTRAIL_API_KEY` | Secret text |

> **Security**: The API key is bound as an environment variable through Jenkins Credentials, never passed as a Maven `-D` flag. This prevents it from appearing in build logs.

### Maven Goals

```
clean verify -P saucedemo \
  -DTESTRAIL_URL="${TESTRAIL_URL}" \
  -DTESTRAIL_USER="${TESTRAIL_USER}" \
  -DTESTRAIL_RUN_ID="${TESTRAIL_RUN_ID}" \
  -DTESTRAIL_ENABLED="${TESTRAIL_ENABLED}"
```

### Post-Build Actions
- **Publish HTML reports**: `target/cucumber-html-reports`
- **Archive artifacts**: `target/cucumber-reports/*.json`

## How It Works

1. Jenkins pulls the latest code from GitHub
2. Maven runs the Cucumber test suite with the `saucedemo` profile
3. After each scenario, `TestRailReportingHook.java` fires as a Cucumber `@After` hook
4. The hook extracts the `@Cxx` tag (e.g., `@C2`) from the scenario to get the TestRail case ID
5. It reads configuration from Maven system properties (`-D` flags) and environment variables
6. It POSTs the result (status 1=passed, 5=failed) to:
   ```
   POST {TESTRAIL_URL}/index.php?/api/v2/add_result_for_case/{run_id}/{case_id}
   ```
7. TestRail updates the test run with PASSED/FAILED status for each case

## Configuration Priority

The hook reads configuration using a dual-source approach:

```
System.getProperty(key)  →  Maven -D flags (highest priority)
System.getenv(key)       →  OS environment variables (fallback)
```

This allows Jenkins to pass most values as Maven parameters while keeping the API key secure through credential binding as an environment variable.

## Disabling TestRail Reporting

Set `TESTRAIL_ENABLED=false` in the Jenkins build parameters. This is useful when:
- The TestRail trial has expired
- Running tests locally without TestRail access
- Debugging test failures without posting results
