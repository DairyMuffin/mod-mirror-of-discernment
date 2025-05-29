# Security Review — Mirror of Discernment (MoD)

## 1. Threat Model
- **MITM Attacks:** Intercepting API calls between AR client, Gateway, and CAA.  
- **Malicious Content Injection:** Adversaries submitting crafted text to bypass gates.  
- **API Abuse:** Rate-limit exhaustion or brute-forcing VIP_API_KEY.

## 2. Authentication & Authorization
- **API Keys:**  
  - Rotate VIP_API_KEY quarterly.  
  - Enforce strict scope (read-only `/scan` access).  
- **Role-Based Access:**  
  - Dashboard users: viewer vs. admin roles.  
  - Use JWTs with short TTLs.

## 3. Network Security
- **TLS Everywhere:**  
  - Enforce HTTPS for all endpoints.  
  - HSTS header on Gateway & CAA.  
- **CORS Policies:**  
  - AR client allowed origins only.  
  - Deny all others.

## 4. Input Validation & Sanitization
- **CAA `/scan`:**  
  - Reject non-UTF8 or excessively large payloads (>10KB).  
  - Sanitize text to prevent injection in any downstream

