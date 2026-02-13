# LLM and AI Application Security Reference

## Overview

As organizations integrate Large Language Models and Generative AI into applications,
a new class of threats has emerged that doesn't fit traditional vulnerability categories.
The OWASP Top 10 for LLM Applications addresses these risks.

## Key LLM Security Risks

### Prompt Injection
- **Threat:** Attacker crafts input that tricks the model into executing unintended commands,
  leaking confidential data, or bypassing safety controls
- **Direct injection:** Malicious instructions in user-facing input
- **Indirect injection:** Malicious instructions embedded in external data the model processes
  (emails, documents, web pages)
- **Mitigation:**
  - Input filtering and classification to detect high-risk prompts
  - Separate data and control planes — never mix instructions with user content
  - Apply output validation before acting on model responses
  - Limit the model's access to sensitive tools and data

### Training Data Poisoning
- **Threat:** Attacker manipulates training data to introduce backdoors, biases, or
  vulnerabilities into the model
- **Mitigation:**
  - Verify provenance and integrity of all training datasets
  - Implement data validation and anomaly detection during training
  - Use diverse data sources to reduce single-point-of-failure risk
  - Monitor model behavior for unexpected changes post-training

### Excessive Agency
- **Threat:** Model is given too many permissions to interact with sensitive tools,
  databases, or external services — and uses them in unintended ways
- **Mitigation:**
  - Restrict tool access by default (least privilege for AI agents)
  - Require human-in-the-loop validation for sensitive operations
  - Implement guardrails that limit action scope (e.g., read-only access to databases)
  - Log all tool invocations for audit and review

### Sensitive Information Disclosure
- **Threat:** Model reveals PII, trade secrets, or system details in its responses
- **Mitigation:**
  - Filter training data for sensitive content before training
  - Implement output scanning for PII, credentials, and internal identifiers
  - Apply differential privacy techniques where appropriate
  - Test for memorization of training data

### Insecure Output Handling
- **Threat:** Model output is passed to downstream systems without validation,
  enabling injection attacks (SQL injection via model output, XSS via model-generated HTML)
- **Mitigation:**
  - Treat all model output as untrusted input
  - Apply the same validation and encoding to model output as to user input
  - Sandbox model output rendering

### Model Denial of Service
- **Threat:** Crafted inputs cause excessive resource consumption (long generation times,
  memory exhaustion, high API costs)
- **Mitigation:**
  - Set token limits, timeouts, and rate limits on model API endpoints
  - Monitor resource consumption per request
  - Implement circuit breakers for anomalous usage patterns

## AI Security in the Development Lifecycle

### IDE as Attack Surface
AI-powered code assistants can inadvertently suggest vulnerable code patterns.
- Review AI-suggested code with the same rigor as human-written code
- Configure assistants with security-aware prompts and rules
- Track the ratio of AI-suggested code that introduces vulnerabilities
- Use SAST to validate all code regardless of origin

### Defense-in-Depth for LLM Applications
1. **Input layer:** Classify and filter prompts; detect injection patterns
2. **Model layer:** Fine-tune for safety; apply alignment techniques
3. **Output layer:** Validate, filter, and sanitize model responses
4. **Action layer:** Gate all tool use behind authorization and human approval
5. **Monitoring layer:** Log all interactions; detect anomalous behavior patterns

### Security Requirements for AI Features
When adding AI/LLM capabilities to an application, require:
- Threat model that includes AI-specific threats (prompt injection, data poisoning, agency abuse)
- Input validation layer between users and the model
- Output validation layer between the model and downstream systems
- Rate limiting and cost controls on model API usage
- Audit logging of all prompts and responses (with PII redaction)
- Human-in-the-loop for any action that modifies data or has side effects
- Regular red-team exercises specifically targeting AI features
