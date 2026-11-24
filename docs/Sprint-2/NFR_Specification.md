# Non-Functional Requirements (NFR) Specification — Sprint 2

## 1. Performance
- **NFR-01 (API Response Latency):** At least 95% of standard REST API requests (`p95`) must complete in less than `300ms` under nominal load, and 99% (`p99`) in less than `800ms`.
- **NFR-02 (Concurrent Load Handling):** The backend API must sustain at least `100` concurrent requests per second without exceeding a `1%` HTTP `5xx` error rate.

## 2. Security
- **NFR-03 (Token & Session Management):** Protected endpoints must require valid JWT bearer tokens with short-lived access tokens and explicit expiration validation.
- **NFR-04 (Password Hashing):** User passwords must be hashed using `bcrypt` with a minimum work factor (cost) of `10` before persistence.
- **NFR-05 (RBAC & Authorization Enforcement):** Role-Based Access Control (RBAC) must verify user permissions on every protected route and return `HTTP 403 Forbidden` when an authenticated user lacks required privileges.

## 3. Concurrency and Data Integrity
- **NFR-06 (Double-Booking & Capacity Prevention):** Classroom capacity checks and seat/room reservations must execute inside ACID database transactions with appropriate locking or uniqueness constraints so concurrent requests cannot overbook capacity or cause double-booking.
- **NFR-07 (Atomic State Transitions):** Multi-step write operations (such as grading/report card calculation or schedule updates) must commit atomically or roll back completely on failure.

## 4. Portability
- **NFR-08 (Single-Command Local Deployment):** All required application and database services must start locally in a clean environment using a single command (`docker compose up -d` or `docker-compose up`) with documented environment variables.
- **NFR-09 (Environment Parity):** Containerized runtime behavior must remain consistent across Linux, macOS, and Windows (WSL2) developer environments.
