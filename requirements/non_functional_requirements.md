## Non-Functional Requirements – WineMind

### 1. Performance

---

**NFR-001: Dish Image Analysis Latency**

- **Category:** Performance  
- **Description:** The system shall analyze an uploaded dish image (including AI recognition + pairing computation) within an acceptable time for interactive use.  
- **Measurable Criteria:**  
  - 95th percentile end-to-end latency (from image upload completion to dish identification result displayed) ≤ **5 seconds** under normal operating load (≤ 50 concurrent active users).  
  - 99th percentile latency ≤ **8 seconds** under normal operating load.  
- **Priority:** Critical  

---

**NFR-002: Recommendation Generation Latency**

- **Category:** Performance  
- **Description:** The system shall generate complete wine recommendations (including personalization and explanations) fast enough to feel responsive.  
- **Measurable Criteria:**  
  - 95th percentile time from confirmed dish to full recommendation list (with explanations) ≤ **4 seconds** under normal load.  
  - 99th percentile ≤ **7 seconds** under normal load.  
- **Priority:** Critical  

---

**NFR-003: Real-Time Wine Search Latency**

- **Category:** Performance  
- **Description:** The system shall execute web search for currently available wines and enrich results with pricing within a bounded time.  
- **Measurable Criteria:**  
  - 95th percentile external search + enrichment duration ≤ **3 seconds** per recommendation session.  
  - Timeout for any external wine catalog/search API ≤ **2 seconds** per call, with graceful degradation (e.g., fewer results) rather than user-visible errors.  
- **Priority:** High  

---

**NFR-004: API Response Time for Core Backend Endpoints**

- **Category:** Performance  
- **Description:** REST APIs shall return responses promptly for standard operations (excluding AI inference time, which is covered separately).  
- **Measurable Criteria:**  
  - 95th percentile response time for non-AI endpoints (e.g., auth, preference CRUD, history retrieval) ≤ **300 ms** under normal load.  
  - 99th percentile response time ≤ **600 ms** under normal load.  
- **Priority:** High  

---

**NFR-005: Throughput Capacity**

- **Category:** Performance  
- **Description:** The system shall support a minimum baseline request throughput without performance degradation beyond defined SLAs.  
- **Measurable Criteria:**  
  - Support at least **100 requests/second** aggregated across all API endpoints with CPU utilization ≤ **70%** on provisioned capacity.  
  - Under this load, all latency requirements in NFR-001 to NFR-004 remain within stated bounds.  
- **Priority:** High  

---

### 2. Scalability

---

**NFR-006: Horizontal Scalability of Backend**

- **Category:** Scalability  
- **Description:** The backend architecture shall scale horizontally to handle increased traffic without major redesign.  
- **Measurable Criteria:**  
  - Ability to increase capacity by adding instances/containers (e.g., AWS autoscaling groups or ECS/EKS tasks) with **no code changes** and only configuration changes.  
  - Doubling the number of stateless backend instances yields at least **70% improvement in max sustainable throughput** under identical test conditions.  
- **Priority:** High  

---

**NFR-007: AI Inference Scalability**

- **Category:** Scalability  
- **Description:** The AI/agent layer (AWS Bedrock) usage shall scale with demand while controlling cost and maintaining latency SLAs.  
- **Measurable Criteria:**  
  - System supports at least **5x peak daily concurrent AI requests** observed at launch (configurable) by adjusting Bedrock concurrency and rate limits without code change.  
  - Under projected 5x peak load, 95th percentile image analysis latency (NFR-001) not degraded by more than **30%**.  
- **Priority:** High  

---

**NFR-008: Data Storage Scalability**

- **Category:** Scalability  
- **Description:** Persistence layers (user profiles, preferences, history, logs) shall scale with growth in user base and usage.  
- **Measurable Criteria:**  
  - Support at least **1 million registered users** and **100 million pairing records** without schema redesign.  
  - Typical read/write operations maintain 95th percentile latency ≤ **50 ms** at the database level under expected peak load.  
- **Priority:** Medium  

---

**NFR-009: Elastic Scaling Policy**

- **Category:** Scalability  
- **Description:** The system shall automatically adjust capacity in response to traffic patterns.  
- **Measurable Criteria:**  
  - Autoscaling rules (CPU and/or request rate) adjust compute capacity within **5 minutes** of sustained metric breach (e.g., CPU > 70% for 5 minutes).  
  - During autoscaling events, error rate for core APIs remains ≤ **1%** and user-facing downtime = **0**.  
- **Priority:** Medium  

---

### 3. Security

---

**NFR-010: Authentication & Authorization**

- **Category:** Security  
- **Description:** All authenticated actions shall be protected by secure authentication and role-based authorization.  
- **Measurable Criteria:**  
  - 100% of authenticated API endpoints require a valid JWT or equivalent token, validated on each request.  
  - Role/permission checks are implemented for all admin or privileged operations; no direct access to admin functions by standard users verified by automated tests with **100% coverage of protected routes**.  
- **Priority:** Critical  

---

**NFR-011: Data in Transit Protection**

- **Category:** Security  
- **Description:** All data in transit between clients, backend services, and external integrations shall be encrypted.  
- **Measurable Criteria:**  
  - 100% of external-facing endpoints served over **HTTPS (TLS 1.2 or higher)**; HTTP requests are redirected to HTTPS.  
  - All outbound calls to third-party APIs (including wine search and Bedrock) use HTTPS/TLS with certificate validation enabled.  
- **Priority:** Critical  

---

**NFR-012: Data at Rest Protection**

- **Category:** Security  
- **Description:** Sensitive data (user accounts, preferences, tokens) shall be protected when stored.  
- **Measurable Criteria:**  
  - All databases, object storage buckets (for images), and backups are encrypted at rest using **AES-256** or AWS-managed KMS keys.  
  - Passwords stored only as salted, adaptive hashes (e.g., bcrypt/Argon2) with **zero plaintext password storage** verified by code review.  
- **Priority:** Critical  

---

**NFR-013: Access Control & Secrets Management**

- **Category:** Security  
- **Description:** Infrastructure and application secrets shall be centrally managed and access-limited.  
- **Measurable Criteria:**  
  - 100% of secrets (API keys, DB credentials, tokens) stored in a secure secrets manager (e.g., AWS Secrets Manager/SSM Parameter Store), not in source code or config files.  
  - IAM roles enforced such that each service has least-privilege permissions; quarterly access review ensures no role grants wildcard `*` permissions in production.  
- **Priority:** High  

---

**NFR-014: Logging, Auditing & Security Monitoring**

- **Category:** Security  
- **Description:** Security-relevant events shall be logged and monitored for suspicious activity.  
- **Measurable Criteria:**  
  - 100% of authentication events (login, logout, failed login, password reset) and privilege changes logged with user ID, timestamp, and IP where available.  
  - Security alerts (e.g., repeated failed logins, abnormal traffic) generated and surfaced within **5 minutes** of detection for at least **95%** of such events.  
- **Priority:** High  

---

**NFR-015: OWASP & Input Validation**

- **Category:** Security  
- **Description:** The system shall mitigate common web application vulnerabilities and validate all inputs.  
- **Measurable Criteria:**  
  - No **High** or **Critical** severity issues in periodic OWASP Top 10 vulnerability scans or penetration tests before production releases.  
  - 100% of external inputs (API payloads, query params, image metadata) validated via centralized validation (e.g., Pydantic) with automated tests covering typical and boundary cases.  
- **Priority:** High  

---

### 4. Reliability

---

**NFR-016: Service Availability**

- **Category:** Reliability  
- **Description:** Core user-facing functionality shall maintain high availability.  
- **Measurable Criteria:**  
  - Overall monthly uptime for core APIs and frontend ≥ **99.5%** (excluding planned maintenance windows).  
  - Planned maintenance windows announced at least **24 hours** in advance and limited to **4 hours/month**.  
- **Priority:** Critical  

---

**NFR-017: Fault Tolerance & Graceful Degradation**

- **Category:** Reliability  
- **Description:** The system shall handle partial failures (e.g., external wine APIs, AI provider issues) gracefully without full service outage.  
- **Measurable Criteria:**  
  - When external wine search fails, system still returns **fallback recommendations or educational content** in ≥ **95%** of such cases rather than an error page.  
  - Circuit breaker patterns or equivalent in place for all external dependencies with retry policies; no single dependency failure yields > **5%** global error rate.  
- **Priority:** High  

---

**NFR-018: Error Rate and Resilience Under Load**

- **Category:** Reliability  
- **Description:** The system shall maintain a low error rate under expected peak load.  
- **Measurable Criteria:**  
  - HTTP 5xx rate ≤ **0.5%** over any 1-hour window under normal and expected peak load.  
  - During controlled stress tests up to **2x expected peak**, the system continues to serve at least **90%** of valid requests successfully (2xx/3xx) while applying backpressure (e.g., 429) rather than failing with 5xx.  
- **Priority:** High  

---

**NFR-019: Backup and Recovery**

- **Category:** Reliability  
- **Description:** Critical data shall be backed up and recoverable within defined objectives.  
- **Measurable Criteria:**  
  - Automated backups of production databases at least **every 24 hours** with point-in-time recovery (PITR) where supported.  
  - Recovery Time Objective (RTO) ≤ **4 hours** and Recovery Point Objective (RPO) ≤ **1 hour** for the primary database in case of critical failure, validated via at least **2 disaster recovery tests per year**.  
- **Priority:** High  

---

**NFR-020: Incident Detection and Response**

- **Category:** Reliability  
- **Description:** Operational issues shall be detected, alerted, and resolved in a timely manner.  
- **Measurable Criteria:**  
  - Monitoring dashboards for latency, error rate, and resource utilization with alerts configured for key thresholds (e.g., error rate > 1%, CPU > 80%).  
  - Mean Time to Detect (MTTD) for critical incidents ≤ **10 minutes** and Mean Time to Resolve (MTTR) ≤ **2 hours**, measured quarterly.  
- **Priority:** Medium