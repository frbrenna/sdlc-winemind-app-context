## Non-Functional Requirements – WineMind

### 1. Performance

#### NFR-001: API Response Time – Core Pairing Flow
**Description:**  
The backend must return a complete pairing result (dish identification + wine recommendations + explanations) within acceptable time for end users.

**Measurable Criteria:**  
- 95th percentile end-to-end response time for the “photo → pairing result” flow ≤ **7 seconds** under normal load (≤ 50 concurrent active users).  
- 99th percentile response time ≤ **10 seconds**.  
- Dish recognition sub-step (image upload received to dish classification result) ≤ **5 seconds** at 95th percentile.  

**Priority:** Critical  

---

#### NFR-002: API Response Time – Non-AI Endpoints
**Description:**  
Non-AI user interactions (login, preference updates, history retrieval) must be snappy.

**Measurable Criteria:**  
- 95th percentile latency for non-AI REST endpoints (e.g., `/api/v1/auth/*`, `/api/v1/preferences/*`, `/api/v1/history/*`) ≤ **300 ms** server-side.  
- 99th percentile latency for these endpoints ≤ **500 ms**.  

**Priority:** High  

---

#### NFR-003: Frontend Load Performance
**Description:**  
The web UI must load quickly on modern mobile and desktop devices.

**Measurable Criteria:**  
- Time to first meaningful paint (TTFMP) ≤ **2.5 seconds** on a 4G connection and mid-range smartphone (Lighthouse “Simulated Mobile”) for the main pairing page.  
- Total JavaScript payload (compressed) for initial load ≤ **300 KB**.  
- Lighthouse performance score ≥ **80** on the primary pairing page.  

**Priority:** High  

---

#### NFR-004: Throughput Capacity
**Description:**  
The system must handle a baseline level of concurrent usage without degradation of SLAs.

**Measurable Criteria:**  
- System sustains **100 image-based pairing requests per minute** and **300 non-AI API requests per minute** concurrently while still meeting NFR-001 and NFR-002.  
- CPU utilization on production application nodes under this load averages ≤ **70%** over 15 minutes.  

**Priority:** High  

---

### 2. Scalability

#### NFR-005: Horizontal Scalability of Backend
**Description:**  
The backend must scale horizontally to support traffic growth without major redesign.

**Measurable Criteria:**  
- Application services (FastAPI) deployed on AWS must support **scaling out to at least 5× baseline capacity** (requests per minute) by adding instances only, without code changes.  
- Adding or removing application instances must not require downtime (zero-downtime deployments supported).  

**Priority:** High  

---

#### NFR-006: AI/Bedrock Scaling
**Description:**  
AI workloads using AWS Bedrock must scale with demand while maintaining acceptable latency.

**Measurable Criteria:**  
- System must support at least **5 concurrent AI image-analysis jobs per application instance** without exceeding NFR-001 latency thresholds.  
- AI call rate-limiting and queuing must ensure that under sudden 3× traffic spikes, **no more than 2%** of AI requests are rejected due to throttling, with graceful user messaging when limits are hit.  

**Priority:** High  

---

#### NFR-007: Data Storage Scalability
**Description:**  
User profiles, preferences, and history must scale to support growth in active users.

**Measurable Criteria:**  
- Data layer (database) must support at least **100,000 registered users** and **10 pairing history records per user on average** without requiring schema redesign.  
- Under this data volume, typical read queries (fetch preferences, fetch last 10 history items) must complete in ≤ **100 ms** at the database level for 95th percentile.  

**Priority:** Medium  

---

### 3. Security

#### NFR-008: Authentication and Session Security
**Description:**  
User authentication and sessions must be secure and aligned with industry best practices.

**Measurable Criteria:**  
- All authenticated API endpoints require valid tokens (e.g., JWT or equivalent) with expiration ≤ **24 hours**.  
- Failed login attempts must be rate-limited to **≤ 5 attempts per 5 minutes per IP/user account**.  
- Session tokens must be stored using HTTP-only and Secure flags when cookies are used.  

**Priority:** Critical  

---

#### NFR-009: Transport Encryption
**Description:**  
All communication between clients, backend services, and external APIs must be encrypted.

**Measurable Criteria:**  
- 100% of external traffic uses **HTTPS/TLS 1.2 or higher**; HTTP requests are redirected to HTTPS.  
- Internal service-to-service calls within AWS use encrypted channels (e.g., TLS, private networking) for all sensitive data (user identifiers, preferences, history).  

**Priority:** Critical  

---

#### NFR-010: Data Protection & Privacy
**Description:**  
User data (including preferences, history, and any PII) must be protected at rest and in use.

**Measurable Criteria:**  
- All persistent storage (databases, backups, object storage) uses encryption at rest (e.g., AWS-managed KMS keys).  
- No raw password values are stored; passwords (if used) are hashed with a modern algorithm (e.g., bcrypt/argon2) with per-user salts.  
- Production database snapshots/backups are restricted to authorized roles only (IAM policies) and access is logged 100% of the time.  

**Priority:** Critical  

---

#### NFR-011: Security Logging and Monitoring
**Description:**  
Security-relevant events must be logged and monitored for anomalies.

**Measurable Criteria:**  
- Authentication events (logins, failed logins, password reset, social login linkage), privilege changes, and major profile changes are logged with user ID, timestamp, and source IP.  
- Security logs are retained for at least **90 days**.  
- Alerts are generated within **5 minutes** for: >50 failed logins from same IP in 10 minutes, or unusual spike in 4xx/5xx related to auth endpoints.  

**Priority:** High  

---

#### NFR-012: Third-Party & API Security
**Description:**  
Integrations with external wine search and pricing sources must be secured.

**Measurable Criteria:**  
- All outbound API calls to third parties use API keys or OAuth tokens stored in a secure secrets manager (e.g., AWS Secrets Manager) with rotation at least every **180 days**.  
- No third-party API credentials are committed to source code repositories (verified by CI checks).  

**Priority:** High  

---

### 4. Reliability

#### NFR-013: Service Availability
**Description:**  
The production environment must provide high availability for end users.

**Measurable Criteria:**  
- Monthly service availability (excluding scheduled maintenance) ≥ **99.5%** for the core pairing API.  
- No single planned maintenance event causes more than **15 minutes** of downtime; maintenance is scheduled during off-peak hours.  

**Priority:** Critical  

---

#### NFR-014: Fault Tolerance & Graceful Degradation
**Description:**  
The system must handle failures of dependencies (AI, external wine APIs) gracefully.

**Measurable Criteria:**  
- If AI image recognition fails or times out, the system presents a manual dish-entry fallback within **2 seconds** of error detection.  
- If external wine search APIs fail, the system returns at least a generic pairing explanation and fallback wine styles (not vendor-specific) in ≥ **95%** of such incidents.  
- Circuit breaker or retry logic limits retries to **≤ 3 attempts** with exponential backoff, and prevents cascading failures.  

**Priority:** High  

---

#### NFR-015: Backup and Recovery
**Description:**  
User data must be backed up and recoverable in the event of data loss.

**Measurable Criteria:**  
- Automated backups of primary databases performed at least **every 24 hours**, with point-in-time recovery (PITR) where supported.  
- Recovery Point Objective (RPO) ≤ **24 hours** and Recovery Time Objective (RTO) ≤ **4 hours** for critical data stores in case of major incident.  
- Quarterly restore tests validate that backups are complete and restorable in a non-production environment.  

**Priority:** High  

---

#### NFR-016: Error Handling and User Feedback
**Description:**  
System errors must be handled consistently and communicated clearly to users.

**Measurable Criteria:**  
- 100% of API endpoints return structured error responses (e.g., `{ "success": false, "error": { "code": "...", "message": "..." } }`) matching the API contract.  
- User-facing error messages for transient issues (timeouts, third-party failures) provide clear next steps and do not expose stack traces or internal details.  
- Unhandled exceptions in production are logged with full stack trace and correlation ID in ≥ **99%** of cases.  

**Priority:** Medium