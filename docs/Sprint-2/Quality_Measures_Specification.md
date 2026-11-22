# Quality Measures Specification and Verification Plan

**Project:** Education Management System (SENG 321, Team 23)
**Sprint:** Sprint 2
**Work item:** S2-06 (GitHub #16, Jira SCRUM-17)
**Owner:** Serhat Eroğlu (QA & Test Automation)

## 1. Purpose and Scope

This document defines how the quality of the Education Management System is measured, what counts as acceptable, and how each measure is verified. It applies to all code, API contract changes and pull requests from Sprint 2 onwards.

It covers four areas:

1. Unit test coverage (Jest/Vitest)
2. Static code analysis (SonarQube)
3. API contract validation (OpenAPI 3.1)
4. Pull request peer review

Non-functional requirements such as performance and availability are specified separately in S2-05.

## 2. Quality Measures

| ID | Area | Measure | Acceptance Threshold | Verification Tool | When Verified |
|---|---|---|---|---|---|
| QM-01 | Unit tests | Overall line coverage | ≥ 50% | Jest/Vitest with coverage report | Every PR (CI), sprint end |
| QM-02 | Unit tests | Line and branch coverage of grade entry and report card calculation modules | ≥ 80% | Jest/Vitest with coverage report | Every PR that touches these modules |
| QM-03 | Unit tests | Passing unit tests | 100% pass, no skipped tests on `main` | Jest/Vitest | Every PR (CI) |
| QM-04 | Static analysis | SonarQube Quality Gate | Passed | SonarQube | Every PR (CI) |
| QM-05 | Static analysis | New blocker and critical issues (bugs, vulnerabilities) | 0 | SonarQube | Every PR (CI) |
| QM-06 | Static analysis | Reliability, Security and Maintainability ratings | A | SonarQube | Sprint end |
| QM-07 | Static analysis | Technical debt ratio | ≤ 5% | SonarQube | Sprint end |
| QM-08 | Static analysis | Duplicated lines on new code | ≤ 3% | SonarQube | Every PR (CI) |
| QM-09 | Static analysis | Security hotspots reviewed | 100% | SonarQube | Sprint end |
| QM-10 | API contract | `api/seng321-project.yaml` validates against OpenAPI 3.1 | 0 errors | openapi-spec-validator, Redocly CLI lint | Every PR that changes the contract |
| QM-11 | API contract | Every operation has an `operationId`, a success response and documented error responses (`application/problem+json`) | 100% of operations | Redocly CLI lint, manual review | Every PR that changes the contract |
| QM-12 | API contract | Protected endpoints declare `bearerAuth` and a `401` response | 100% of protected operations | Manual review | Every PR that changes the contract |
| QM-13 | API contract | Implemented responses match the contract | 0 schema mismatches | Integration tests validating responses against the OpenAPI schema | From Sprint 3, every PR (CI) |
| QM-14 | Peer review | Approvals before merge into `develop` or `main` | ≥ 1 approval from another team member | GitHub branch protection | Every PR |
| QM-15 | Peer review | Approval refers to the latest commit | Re-review after new commits or force pushes | GitHub ("dismiss stale approvals"), reviewer check | Every PR |
| QM-16 | Peer review | PR linked to a GitHub issue / Jira item and all checks passing before merge | 100% of merged PRs | GitHub PR page, CI checks | Every PR |

**Note on SonarQube coverage:** the default "Sonar way" Quality Gate requires 80% coverage on new code. The team's Quality Gate will be configured to use the 50% project threshold (QM-01), while QM-02 keeps a stricter target for the business-critical calculation logic.

## 3. Priority Test Targets

Unit and integration tests should cover the acceptance criteria of the product backlog first:

| User Story | What to Test | Example Cases |
|---|---|---|
| US-01 Login | Valid and invalid credentials | Valid user returns `200` with a role-based token; wrong password or unknown user returns `401`; malformed body returns `400` |
| US-02 Class creation | Capacity validation and role check | Capacity `1` and `20` accepted (`201`); `0` and `-5` rejected (`400`); non-manager returns `403` |
| US-03 Grade entry | Score range and teacher authorization | Scores `0` and `100` accepted; `-1` and `101` rejected (`400`); teacher not assigned to the class returns `403` |
| US-04 Report card | Weighted average and pass/fail threshold | 40% midterm + 60% final computed correctly; average just below the threshold gives `isPassed: false`, at the threshold gives `isPassed: true`; weights not summing to 100 are rejected |

Boundary values (`0`, `100`, the pass threshold and the values just around it) must be tested explicitly.

## 4. Verification Plan

| Stage | Checks | Responsible | Evidence |
|---|---|---|---|
| Every pull request | QM-03, QM-04, QM-05, QM-08, QM-14 to QM-16; QM-01 and QM-02 once tests exist; QM-10 to QM-12 when the API contract changes | PR author, reviewer, QA | CI check results and review approval on the PR page |
| Sprint end | Full coverage report, SonarQube dashboard (QM-06, QM-07, QM-09), API contract lint | QA | Coverage and SonarQube report links in the sprint submission |
| Final submission | All measures, including QM-13 integration tests | QA, DevOps | Integration and end-to-end test report, SonarQube technical debt report |

The CI pipeline (GitHub Actions) is set up by DevOps in Sprint 3. Until then, QM-10 to QM-12 and QM-14 to QM-16 are verified manually during PR review.

## 5. Rollout by Sprint

| Sprint | Measures in effect |
|---|---|
| Sprint 2 | QM-10 to QM-12 (API contract), QM-14 to QM-16 (peer review) |
| Sprint 3 (MVP) | Adds QM-01 to QM-05 and QM-08 in CI; at least 50% unit test coverage required |
| Final | All measures, including QM-06, QM-07, QM-09 and QM-13 |

## 6. Defect Tracking

Defects are reported as GitHub issues with the `bug` label and linked to the related user story. Each defect gets a severity:

| Severity | Definition | Expected Handling |
|---|---|---|
| Critical | Breaks a primary user journey or causes data loss or a security issue | Fixed before the next merge to `main` |
| Major | A feature works incorrectly but has a workaround | Fixed within the current sprint |
| Minor | Cosmetic or low-impact issue | Planned in the backlog |
