# API, Cloud-Native, and Microservices Security Reference

## OWASP API Security Top 10

### API1: Broken Object Level Authorization (BOLA)
- APIs expose object IDs directly in URLs — attackers enumerate IDs to access others' data
- **Mitigation:** Implement authorization checks at the object level for every function that
  accesses a data source using a user-supplied ID
- Use random, non-sequential IDs (UUIDs) to reduce predictability

### API2: Broken Authentication
- Weak token handling, missing rate limits on auth endpoints, credentials in URLs
- **Mitigation:** Use OAuth 2.0 with PKCE for public clients; validate tokens server-side;
  implement rate limiting on authentication endpoints

### API3: Broken Object Property Level Authorization
- APIs expose all object properties, including those users shouldn't modify
- **Mitigation:** Explicitly define which properties are readable/writable per user role;
  use DTOs to control serialization

### API4: Unrestricted Resource Consumption
- APIs called by automated scripts susceptible to resource exhaustion (DoS) or runaway cloud costs
- **Mitigation:** Implement rate limiting, throttling, and payload size caps at the API gateway;
  set quotas per-client; monitor for anomalous usage patterns

### API5: Broken Function Level Authorization
- Administrative functions accessible to regular users due to missing authorization checks
- **Mitigation:** Deny by default; require admin authorization checks on all admin endpoints;
  centralize authorization logic

### API6: Unrestricted Access to Sensitive Business Flows
- Automated abuse of business logic (ticket scalping, credential stuffing via API)
- **Mitigation:** Identify sensitive business flows; implement anti-automation controls
  (CAPTCHA, device fingerprinting, behavioral analysis)

### API7: Server-Side Request Forgery (SSRF)
- APIs fetch remote resources based on user-supplied URLs without validation
- **Mitigation:** Validate and sanitize all user-supplied URLs; use allowlists for
  remote resources; isolate resource-fetching functionality in separate network segments

### API8: Security Misconfiguration
- Same as web apps but amplified: missing CORS restrictions, verbose error messages,
  unnecessary HTTP methods enabled
- **Mitigation:** Automate API gateway configuration; restrict CORS to specific origins;
  disable unused HTTP methods; return generic error responses

### API9: Improper Inventory Management
- Shadow APIs, deprecated endpoints, and undocumented versions remain accessible
- **Mitigation:** Maintain an API registry; version all APIs; sunset deprecated versions
  aggressively; use API gateways for lifecycle management

### API10: Unsafe Consumption of APIs
- Applications trust third-party API responses without validation
- **Mitigation:** Treat all external API data as untrusted input; validate, sanitize,
  and rate-limit data received from third-party APIs

## Microservices Security (NIST SP 800-204)

### Service Mesh Architecture
A service mesh provides a dedicated infrastructure layer for handling service-to-service
communication, abstracting security concerns away from individual service code.

**Core capabilities:**
- **mTLS everywhere** — Automatic mutual TLS between all services; no plaintext internal traffic
- **Centralized policy** — Security policies defined centrally, enforced at the sidecar proxy
- **Observability** — Distributed tracing, access logging, and metrics across all service calls
- **Traffic management** — Circuit breaking, retries, and load balancing with security awareness

### Identity and Token Management
- Use a secure token service (STS) for internal service-to-service authentication
- Internal authorization tokens should never be exposed to end-users
- Implement token exchange patterns (OAuth 2.0 Token Exchange RFC 8693)
- Use short-lived tokens with automatic rotation

### Security Design Principles for Microservices
- **Lifecycle independence** — Each service can be deployed, updated, and scaled independently
- **Fault tolerance** — A compromised or failed service should not cascade to others
- **Minimal attack surface** — Each service exposes only the endpoints it needs
- **Defense at multiple layers** — Gateway (edge), sidecar (service), and application (code)

## Cloud-Native Security (NIST SP 800-210)

### Access Control Models
- **RBAC (Role-Based)** — Assign permissions to roles, assign roles to users; good for
  stable organizational structures
- **ABAC (Attribute-Based)** — Evaluate policies based on user attributes, resource attributes,
  and environmental conditions; better for dynamic cloud environments
- **Recommendation:** Use RBAC as the baseline; layer ABAC for fine-grained, contextual decisions

### Cloud Security Practices
- **IaC Security** — All infrastructure defined as code; scanned for misconfigurations before deployment
- **Multi-tenancy isolation** — Enforce network segmentation, separate databases or schemas per tenant
- **Continuous monitoring** — CloudTrail, GuardDuty, or equivalent for real-time threat detection
- **Immutable infrastructure** — Deploy new instances rather than patching in-place; reduces drift

### Zero Trust Principles for Cloud-Native
- Never trust, always verify — even for internal service-to-service calls
- Authenticate and authorize every request, regardless of network location
- Minimize blast radius — segment networks, limit permissions, use short-lived credentials
- Encrypt everything in transit and at rest
- Monitor and log continuously; assume breach and design for detection
