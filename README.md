# Education Management System — SENG 321 (Team 23)
## Modular Student Information System (OBS) · API-First · Modular Monolith

![Status](https://img.shields.io/badge/sprint-2-blue) ![License](https://img.shields.io/badge/license-MIT-green)

**Repository:** https://github.com/K1llly/SENG-321-PROJECT · **Releases:** [sprint-1](https://github.com/K1llly/SENG-321-PROJECT/releases/tag/sprint-1) · [sprint-2](https://github.com/K1llly/SENG-321-PROJECT/releases/tag/sprint-2)

---

## Sprint 2 Deliverables (Requirements, UI/UX & Architecture)

Sprint 2 is a specification sprint — no product code is delivered in it.

| # | Deliverable | Document | Owner |
| :--- | :--- | :--- | :--- |
| 1 | Software Requirements Specification (SRS) + refined backlog (US-01 … US-07) | [HTML](<docs/Sprint-2/srs/SRS (Software Requirements Specification).html>) · [PDF](<docs/Sprint-2/srs/Software Requirements Specification (SRS).pdf>) | Alperen Gökay Göktaş |
| 2 | Non-Functional Requirements | [NFR_Specification.md](docs/Sprint-2/NFR_Specification.md) | Reyhan Çetin |
| 2 | Quality Measures & Verification Plan | [Quality_Measures_Specification.md](docs/Sprint-2/Quality_Measures_Specification.md) | Serhat Eroğlu |
| 3 | UI/UX prototypes + user-flow diagrams (Figma) | [UI_UX_Prototypes.md](docs/Sprint-2/UI-UX/UI_UX_Prototypes.md) | Muhammed Ökmen |
| 4 | Software architecture — C4 models, UML, ADRs | [Architecture.md](docs/Sprint-2/architecture/Architecture.md) | Berkay Kervan |
| — | API contract (OpenAPI 3.1) | [api/seng321-project.yaml](api/seng321-project.yaml) | Muhammed Ökmen / Berkay Kervan |

Requirement IDs, traceability (`US-01` … `US-07` → `FR-*`), the scope phasing (MVP vs final) and the institution-type menu matrix live in the SRS.

---

## Sprint 1 Deliverables (Project Foundation)

### Team Members and Roles (RACI Matrix)

| Name Surname | Student ID | Corporate Email | Role | Sprint 1 Responsibilities |
| :--- | :--- | :--- | :--- | :--- |
| Alperen Gökay Göktaş | 220201040 | s220201040@ankarabilim.edu.tr | Product Owner | Project vision, Backlog (A), Acceptance criteria (A) |
| Yağmur Deniz Biçer | 230201051 | s230201051@ankarabilim.edu.tr | Scrum Master | Process management, Repo rules (A) |
| Berkay Kervan | 230204007 | s230204007@ankarabilim.edu.tr | Lead Software Architect | Architecture design, OpenAPI contract (A) |
| Serhat Eroğlu | 260201311 | s260201311@ankarabilim.edu.tr | QA & Test Automation | Sprint 1 evidence verification (A) |
| Reyhan Çetin | 230201056 | s230201056@ankarabilim.edu.tr | DevOps & CI/CD Engineer | DevOps infrastructure (R) |
| Muhammed Ökmen | 260204316 | s260204316@ankarabilim.edu.tr | Implementation Engineer | Repo setup, OpenAPI writing (R) |

*(R: Responsible, A: Accountable, C: Consulted, I: Informed)*

---

### 1. Project Proposal

**Problem Definition:**
In educational institutions (language courses, tutoring centers), classroom capacities, teacher schedules, and students' pass/fail statuses based on various exam types (midterm/final/makeup) are often kept in disconnected Excel files. This leads to double-booking and data integrity errors during end-of-term report card generation.

**Intended Outcome:**
An API-first education management system where managers can assign classes, teachers can enter attendance and grades, and the system can automatically calculate report cards based on configured exam weights (e.g., 40% midterm, 60% final).

**First Release Scope:**
*   **Included:** Classroom/capacity management, teacher assignments, local authentication, student registration, attendance tracking, flexible exam type configuration, grade weighting, and digital report card generation.
*   **Excluded:** Accounting/payment tracking, SMS/WhatsApp parent notifications, and online video class infrastructure.

**Assumptions and Risks:**
*   *Assumption:* The system will handle secure authentication via username/password on its own database without needing an external Identity Provider (SSO).
*   *Risk:* Concurrent registration requests might lead to capacity conflicts. This will be isolated and managed through integration testing (QA).

**License:**
*   MIT License

---

### 2. System Vision and Boundaries

**System Vision:**
The Education Management System automates assignment processes to provide reliable reservation rules for administrative staff, allows teachers to centrally monitor grade data, and enables students to view their weighted report cards. The initial release is designed as API-first.

**System Boundary:**
*   **Inside the Boundary:** Capacity catalog, student assignment lifecycle, local authentication, exam configurations, pass/fail algorithms, and report card generation.
*   **Outside the Boundary:** Physical classroom maintenance operations, financial/billing processes.

**System Interactions:**
*   **Manager:** Creates classes, defines capacities and exam weights.
*   **Teacher:** Enters grades and attendance strictly for assigned classes.
*   **Student/System:** Authenticates and retrieves weighted averages.

---

### 3. Git-Flow & Repository Rules

**Repository URL:** https://github.com/K1llly/SENG-321-PROJECT

**Branching Strategy:**
*   `main`: Stable release tags intended for production or evaluation.
*   `develop`: Integration branch for stable development code during the sprint.
*   `feature/<issue-id>-<name>`: Short-lived branches created to resolve specific issues.

**Rules:**
1. Direct pushes to `main` and `develop` branches are restricted (Protected branches).
2. Every code change must be linked to an existing Issue and submitted via a Pull Request (PR).
3. PRs require at least 1 peer review approval before being merged into the target branch.
---

### 4. Initial Product Backlog

| ID | User Story | Acceptance Criteria | Effort (SP) | Issue |
| :--- | :--- | :--- | :---: | :--- |
| US-01 | As a user, I want to log in with my username and password to access the system. | Successful login generates a role-based token; invalid credentials return a 401 error. | 3 | [#9](https://github.com/K1llly/SENG-321-PROJECT/issues/9) |
| US-02 | As a manager, I want to create a class and define its maximum capacity. | Capacity cannot be negative and must be saved to the DB. | 5 | [#2](https://github.com/K1llly/SENG-321-PROJECT/issues/2) |
| US-03 | As a teacher, I want to enter exam types and grades for students in my assigned classes. | Grades must be between 0-100; unauthorized access must be blocked. | 8 | [#3](https://github.com/K1llly/SENG-321-PROJECT/issues/3) |
| US-04 | As the system, I want to calculate the weighted grade average and determine the pass/fail status. | Weights (midterm/final) are summed up; below-threshold scores yield a "fail" status. | 8 | [#4](https://github.com/K1llly/SENG-321-PROJECT/issues/4) |

Sprint 2 adds **US-05** (hybrid auto-scheduling, [#21](https://github.com/K1llly/SENG-321-PROJECT/issues/21)), **US-06** (setup wizard, [#22](https://github.com/K1llly/SENG-321-PROJECT/issues/22)) and **US-07** (mock exams, [#23](https://github.com/K1llly/SENG-321-PROJECT/issues/23)); their full user stories and acceptance criteria are in [SRS §4](<docs/Sprint-2/srs/SRS (Software Requirements Specification).html>).

---

### 5. Repository Structure

```
.
├── api/                  # OpenAPI 3.1 contract (API-first design)
├── docs/
│   ├── Sprint-2/         # SRS, NFR, quality measures, UI/UX, architecture
│   └── Sprint_1_Project_Foundation_Team23.pdf
├── src/                  # Application source code (Sprint 3)
├── tests/                # Automated tests (Sprint 3)
├── CONTRIBUTIONS.md      # Per-member contribution log
├── LICENSE               # MIT License
└── README.md
```

### 6. Sprint Documents

* **Sprint 1 – Project Foundation:** [docs/Sprint_1_Project_Foundation_Team23.pdf](docs/Sprint_1_Project_Foundation_Team23.pdf)
* **Sprint 2 – Requirements, UI/UX & Architecture:** see the [deliverables table](#sprint-2-deliverables-requirements-uiux--architecture) above (SRS, NFR, quality measures, Figma prototypes, C4/UML architecture).

### License

This project is licensed under the [MIT License](LICENSE).
