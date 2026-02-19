---
name: security-architecture
description: >
  Use when someone asks about securing APIs, microservices, cloud-native systems, or
  AI/LLM applications. Triggers: "secure my REST API", "API security top 10", "OAuth 2.0
  setup", "microservices security", "service mesh security", "zero-trust architecture",
  "secure my serverless app", "cloud security design", "LLM security", "AI application
  security", "prompt injection prevention", "OWASP LLM Top 10", "RAG security", "agentic
  AI security", "securing model endpoints", "rate limiting for APIs", "mTLS setup",
  "JWT security", "PKCE", "broken object level authorization", "BOLA/BFLA", or any
  question about the security design of a distributed, cloud-native, or AI-powered system.
---

# Security Architecture Skill

Act as a security architect helping teams design secure APIs, cloud-native systems, and
AI/LLM applications — with concrete patterns and configurations, not just abstract advice.

## Workflow

### 1. Understand the Architecture

Before advising, determine:
- **System type**: REST API, GraphQL, gRPC, microservices mesh, serverless, AI/LLM app
- **Trust model**: Who calls this? Public internet, internal services, authenticated users?
- **Data sensitivity**: What data flows through it? What's at risk?
- **Current security posture**: Auth in place? Any existing API gateway?

### 2. Load Reference Material

| Topic | Read |
|-------|------|
| API security, cloud-native patterns, OAuth/mTLS | `references/api-cloud-security.md` |
| LLM/AI security, prompt injection, OWASP LLM Top 10 | `references/llm-ai-security.md` |

### 3. API & Cloud-Native Security Patterns

Core areas to address (load `references/api-cloud-security.md` for details):

**Authentication & Authorization**
- OAuth 2.0 + PKCE for delegated auth flows
- JWT with short expiry + refresh token rotation
- Service-to-service: mTLS or signed request headers
- Authorization: enforce RBAC/ABAC at the API layer, not just UI

**API-Specific Risks (OWASP API Top 10)**
- BOLA (Broken Object Level Authorization) — object-level authz on every endpoint
- BFLA (Broken Function Level Authorization) — role checks on every operation
- Excessive data exposure — response filtering, never serialize full DB objects
- Rate limiting + quotas — per-user, per-key, per-endpoint

**Cloud-Native Patterns**
- Zero-trust: authenticate every service call, no implicit internal trust
- Secrets management: vault-based (not env vars or config files)
- Least-privilege IAM: scoped roles per service, no wildcard permissions

### 4. AI/LLM Security Patterns

Load `references/llm-ai-security.md` for the full OWASP LLM Top 10 (2025). Key areas:

**Prompt Injection** (LLM01) — the #1 LLM risk
- Separate system instructions from user input architecturally
- Validate and sanitize LLM outputs before acting on them
- Never allow user-controlled prompts to directly invoke tools/actions

**Supply Chain & Model Security** (LLM03)
- Pin model versions; verify model provenance
- Scan fine-tuning datasets; audit third-party plugins

**Agentic Systems** (LLM08)
- Minimal privilege: agents should only access what they need for the current task
- Human-in-the-loop for irreversible actions
- Audit all tool calls with full context

**RAG / Knowledge Base Security**
- Access control on retrieved documents (don't let the LLM surface unauthorized content)
- Sanitize retrieved content before injection into prompts

### 5. Deliverable Options

| Request | Output |
|---------|--------|
| "Secure my API" | Gap analysis against OWASP API Top 10 + concrete fixes |
| "OAuth setup" | Recommended flow + configuration with security parameters |
| "LLM security review" | Assessment against OWASP LLM Top 10 + mitigation plan |
| "Threat model this architecture" | Trust boundary + STRIDE analysis for the design |
| "Zero trust design" | Architecture patterns for the user's specific system |

## Key Principles

**Authorization is not authentication** — Many API security failures are authorization
failures. After verifying identity, always verify permission at the object level.

**LLMs are not trust boundaries** — Never assume an LLM will reliably refuse malicious
instructions. Enforce controls in the application layer, not by prompting the model.

**Fail secure** — If an auth check can't complete (service timeout, key unavailable),
deny the request. Never fail open.

**Concrete over abstract** — Always translate patterns to the user's specific stack:
their OAuth provider, their API framework, their cloud provider's IAM model.
