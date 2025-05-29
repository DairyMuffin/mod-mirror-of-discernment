# Security Review — Mirror of Discernment (MoD)

## 1. Threat Model

- **Man-in-the-Middle (MITM) Attacks**  
  Risk of interception or tampering of API calls between the AR Client, Gateway, and CAA.

- **Malicious Content Injection**  
  Adversaries submitting crafted text designed to bypass or exploit gate logic.

- **API Abuse**  
  Rate-limit exhaustion or brute-force attempts against the `VIP_API_KEY`.

## 2. Authentication & Authorization

- **API Keys**  
  - Rotate `VIP_API_KEY` at least quarterly.  
  - Enforce strict scoping (e.g. read-only `/scan` access).

- **Role-Based Access Control (RBAC)**  
  - Define clear roles for Dashboard users (viewer, admin).  
  - Use short-TTL JWTs for session management and authorization.

## 3. Network Security

- **TLS Everywhere**  
  - Enforce HTTPS for all endpoints (including service-to-service).  
  - Apply HSTS headers on Gateway and CAA services.

- **CORS Policies**  
  - Allow only trusted AR Client origins.  
  - Deny all other cross-origin requests by default.

## 4. Input Validation & Sanitization

- **CAA `/scan` Endpoint**  
  - Reject non-UTF-8 or oversized payloads (>10 KB).  
  - Sanitize all incoming text to prevent injection in downstream processing.  
  - Validate each field for expected type, length and range.

## 5. Additional Recommendations

- **Rate Limiting**  
  - Enforce per-IP and per-API-key limits at the Gateway.

- **Audit Logging**  
  - Log authentication attempts, failed logins, and anomalous API usage; review regularly.

- **Dependency Management**  
  - Continuously monitor and update third-party libraries and container images for security patches.

- **Incident Response**  
  - Define procedures for key revocation, user lockout, rapid patch deployment, and post-mortem reporting.

## 6. Compliance & Best Practices

- **Data Protection**  
  - Encrypt sensitive data at rest (e.g. database volumes, S3) and in transit.  
  - Retain logs and data only as long as operational/compliance needs dictate.

- **Security Testing**  
  - Integrate automated vulnerability scans (SAST/DAST) into your CI/CD pipeline.  
  - Schedule regular third-party penetration tests and quarterly SOC-2 audits.
