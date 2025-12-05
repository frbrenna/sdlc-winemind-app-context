## Non-Functional Requirements – WineMind

### 1. Performance

---

**NFR-001: Dish Analysis Response Time**

- **Category:** Performance  
- **Description:** The system must analyze an uploaded dish image, identify the dish, and return the identification (including confidence score and candidate dishes if applicable) within an acceptable time.  
- **Measurable Criteria:**
  - P95 end-to-end latency (from image upload request to dish identification result) ≤ **5 seconds** under normal load (≤ 50 concurrent users).
  - P99 latency ≤ **8 seconds** under normal load.
  - For images ≤ 8 MB, upload + processing must not exceed **10 seconds** end-to-end in 95% of requests.  
- **Priority:** Critical  

---

**NFR-002: Recommendation Response Time**

- **Category:** Performance  
- **Description:** The system must generate personalized wine recommendations (including web search for availability and prices) fast enough to feel “real-time” to users.  
- **Measurable Criteria:**
  - P95 latency from “Get recommendations” click to complete recommendation list (min 3 wines) ≤ **7 seconds** under normal load.
  - P99 latency ≤ **10 seconds** under normal load.
  - In case of slow external APIs, system must return partial recommendations with a timeout of **5 seconds per external call**, and total response within **12 seconds** in 99% of requests.  
- **Priority:** Critical  

---

**NFR-003: Throughput Capacity (Steady State)**

- **Category:** Performance  
- **Description:** The backend must support a baseline volume of concurrent users and requests without degradation of performance.  
- **Measurable Criteria:**
  - Support at least **100 concurrent active users** performing image uploads and recommendation requests.
  - Sustain at least **20 completed recommendation flows per minute** (image upload → dish ID → recommendations) with all P95 latency targets still met.
  - Average CPU utilization on application nodes under this load ≤ **70%** and memory utilization ≤ **75%**.  
- **Priority:** High  

---

**NFR-004: Frontend Perceived Performance**

- **Category:** Performance  
- **Description:** The web UI must feel responsive on modern devices and typical network conditions.  
- **Measurable Criteria:**
  - Time to Interactive (TTI) for the main pairing page ≤ **3 seconds** on a 4G connection and mid-range device.
  - Largest Contentful Paint (LCP) ≤ **2.5 seconds** on 75th percentile of users.
  - All client-side UI actions (opening modals, switching steps in wizards) respond within **100 ms** for 95% of interactions.  
- **Priority:** High  

---

### 2. Scalability

---

**NFR-005: Horizontal Scalability**

- **Category:** Scalability  
- **Description:** The system must handle growth in traffic by scaling horizontally without code changes.  
- **Measurable Criteria:**
  - Application and API tiers can be scaled out to at least **5×** baseline capacity (from 100 to 500 concurrent active users) by adding instances and/or adjusting autoscaling policies, with no code modifications.
  - Under scaled-out configuration, P95 latency degradation is ≤ **30%** compared to baseline while still meeting business response time requirements (dish ID ≤ 5s, recommendations ≤ 7s).
  - Autoscaling triggers based on CPU and request latency, with scale-out time ≤ **5 minutes** from threshold breach.  
- **Priority:** High  

---

**NFR-006: AI/Bedrock Call Scalability**

- **Category:** Scalability  
- **Description:** The AI/agent components using AWS Bedrock must scale to handle peak inference demand without significant queuing delays.  
- **Measurable Criteria:**
  - Support at least **300 AI inference calls per hour** without P95 Bedrock API response time exceeding **3 seconds**.
  - Queuing delay (time waiting before AI invocation) ≤ **1 second** for 95% of calls under peak expected load.
  - System must be configurable to route or throttle AI workloads per tenant/feature without restart.  
- **Priority:** High  

---

**NFR-007: Data Store Scalability**

- **Category:** Scalability  
- **Description:** The persistence layer must support growth in users, preferences, and history without performance degradation.  
- **Measurable Criteria:**
  - Support at least **100,000 user accounts** and **5 million recommendation records** while maintaining query P95 latency for user profile and history reads ≤ **200 ms**.
  - Database write latency for new recommendations/history entries ≤ **300 ms** at **50 writes/second** sustained.
  - Ability to scale vertically and/or horizontally (read replicas) without downtime greater than **15 minutes** for planned changes.  
- **Priority:** Medium  

---

### 3. Security

---

**NFR-008: Authentication & Authorization**

- **Category:** Security  
- **Description:** All protected endpoints must enforce strong authentication and role-based access control.  
- **Measurable Criteria:**
  - 100% of non-public APIs require authentication (JWT/OAuth2/session) except the explicitly whitelisted guest-mode endpoints.
  - Role/permission checks are enforced for any admin/analytics endpoints with 0 known bypass paths in security testing.
  - All authentication failures return HTTP 401/403 without disclosing implementation details or user existence.  
- **Priority:** Critical  

---

**NFR-009: Data in Transit Protection**

- **Category:** Security  
- **Description:** All data exchanged between clients, backend, and external services must be encrypted in transit.  
- **Measurable Criteria:**
  - 100% of external traffic uses **HTTPS/TLS 1.2+**; HTTP requests are automatically redirected or blocked.
  - TLS configurations achieve at least an **A rating** on SSL Labs test for public endpoints.
  - No plaintext transmission of authentication tokens, passwords, or secrets verified through code review and automated security scans.  
- **Priority:** Critical  

---

**NFR-010: Data at Rest Protection**

- **Category:** Security  
- **Description:** Sensitive user data and secrets must be protected at rest.  
- **Measurable Criteria:**
  - All persistent storage (databases, object storage for images, backups) use **AES-256** or AWS-managed encryption.
  - No plaintext secrets (API keys, DB passwords) are stored in code repositories; 100% of secrets are stored in AWS Secrets Manager/SSM parameter store.
  - Nightly automated scan confirms absence of hard-coded secrets in the main branches.  
- **Priority:** High  

---

**NFR-011: Access Logging and Auditability**

- **Category:** Security  
- **Description:** Security-relevant events must be logged for audit and incident investigation.  
- **Measurable Criteria:**
  - 100% of login attempts, password resets, and changes to user preferences are logged with timestamp, user ID (if available), and source IP.
  - Logs are retained for at least **90 days** in a tamper-resistant storage (e.g., CloudWatch with restricted access).
  - Security logs are searchable within **5 minutes** of event occurrence and can be correlated by request ID.  
- **Priority:** High  

---

**NFR-012: Vulnerability Management**

- **Category:** Security  
- **Description:** The system must minimize known vulnerabilities in dependencies and code.  
- **Measurable Criteria:**
  - Automated dependency scanning (e.g., GitHub Dependabot/Snyk) runs on every main-branch push and nightly.
  - No known **Critical** vulnerabilities and no **High** vulnerabilities older than **30 days** in production dependencies.
  - Application undergoes at least **annual** penetration testing, and all Critical/High findings are remediated or risk-accepted within **60 days**.  
- **Priority:** High  

---

### 4. Reliability

---

**NFR-013: Service Availability**

- **Category:** Reliability  
- **Description:** The WineMind service must maintain high availability for end users.  
- **Measurable Criteria:**
  - Monthly service uptime (core pairing functionality) ≥ **99.5%**, excluding scheduled maintenance.
  - No single unplanned outage exceeds **60 minutes**.
  - Scheduled maintenance windows are announced at least **24 hours** in advance and limited to **4 hours/month**.  
- **Priority:** Critical  

---

**NFR-014: Fault Tolerance & Graceful Degradation**

- **Category:** Reliability  
- **Description:** The system must handle failures of external dependencies gracefully.  
- **Measurable Criteria:**
  - If external wine search or pricing APIs fail/time out, the system:
    - Returns at least **1 fallback recommendation** (e.g., generic wine style) in ≥ **80%** of such incidents.
    - Presents clear error messaging without application crashes.
  - Circuit breakers or retry policies are in place for all external calls, with maximum **3 retries** and exponential backoff.
  - No more than **5%** of total user requests during an external outage result in unhandled errors (HTTP 5xx without user-friendly explanation).  
- **Priority:** High  

---

**NFR-015: Backup and Recovery**

- **Category:** Reliability  
- **Description:** User data and configuration must be restorable in case of data loss or corruption.  
- **Measurable Criteria:**
  - Automated backups of primary databases occur at least **every 24 hours** with **point-in-time recovery** enabled where supported.
  - Recovery Time Objective (RTO): **4 hours** to restore the service after a total data store failure.
  - Recovery Point Objective (RPO): Data loss limited to maximum **15 minutes** for critical user data (profiles, preferences, history).
  - Backups are tested via restoration drills at least **twice per year**.  
- **Priority:** High  

---

**NFR-016: Error Handling and Monitoring**

- **Category:** Reliability  
- **Description:** The system must detect, log, and surface errors for operational response.  
- **Measurable Criteria:**
  - 100% of unhandled exceptions in backend services are captured and logged with stack traces and correlation IDs.
  - Centralized monitoring (e.g., CloudWatch, Prometheus + Grafana) tracks key metrics: error rate, latency, throughput, AI call failures.
  - Alerting thresholds:
    - Error rate (5xx) > **2%** over 5 minutes triggers a high-priority alert.
    - AI/Bedrock failure rate > **5%** over 10 minutes triggers an alert.
  - Mean Time to Detect (MTTD) for critical incidents ≤ **10 minutes** (time from occurrence to alert).  
- **Priority:** High