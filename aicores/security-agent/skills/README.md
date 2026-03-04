# devsec-governance → 6 Granular Skills

## The New Structure

| Skill                              | Trigger Persona                        | Reference Files                                                                                              |
| ---------------------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `devsec-reviewing-code-for-security`    | Developer reviewing/writing code       | owasp-top10-2025, secure-coding-practices, asvs-verification, secure-code-review-checklist                   |
| `devsec-conducting-threat-modeling`     | Architect/designer at design phase     | threat-model-template, owasp-top10-2025                                                                      |
| `devsec-hardening-devsecops-pipelines`  | DevOps/platform engineer               | cicd-supply-chain, agent-implementation                                                                      |
| `devsec-managing-compliance-frameworks` | Compliance/GRC, pre-audit              | compliance-mapping, compliance-verification-kpis, nist-ssdf, asvs-verification, security-assessment-template |
| `devsec-designing-security-architecture` | Architect designing APIs/AI systems    | api-cloud-security, llm-ai-security                                                                          |
| `devsec-building-security-programs`     | AppSec lead or CISO building a program | samm-maturity, security-champions, nist-ssdf, security-assessment-template                                   |

---

### ✅ Where granular wins

**Precision**: Each skill's trigger description is specific enough that Claude selects
the right skill without ambiguity. "Review this code for XSS" → `secure-code-review`.
"Set up SAST in GitHub Actions" → `devsecops-pipeline`. No routing table needed.

**Reduced context load**: A code reviewer loads ~600 lines of reference (owasp,
secure-coding, asvs). The monolith would have also surfaced guidance on Security
Champions, SAMM, LLM security, and CI/CD — none of which help answer the question.

**Better trigger descriptions**: Each description now targets a specific *persona with
a specific task*, not a sprawling keyword list. This improves routing accuracy.

**Maintainability**: Updating LLM security guidance only touches `05-security-architecture`.
Previously you'd have to audit the monolith to check for impacts across all sections.

---

## Recommendation

Deploy all 6 skills and retire the monolith. If the skill system supports tagging or
grouping, tag all 6 as `security` so they appear together in skill discovery UIs.

For the rare cross-domain "build our entire security program" query, skills 04 + 06
together (or 03 + 06) will naturally produce the right output. Claude synthesizes
across active skills effectively when their scopes are well-defined.
