# CWE/MITRE Reference — Common Weakness Enumeration

## What is CWE?

The Common Weakness Enumeration (CWE™) is a community-developed list maintained by MITRE. CWE provides a standardized language and taxonomy for describing security weaknesses at the **root-cause level** — before they become exploitable vulnerabilities (CVEs).

> **Key distinction:** A **weakness** describes the underlying condition (e.g., "missing authentication check"). A **vulnerability** describes an exploitable instance in a specific product (e.g., CVE-2024-XXXXX). Map to CWE to describe *what went wrong*, not *what an attacker can do*.

---

## CWE Abstraction Hierarchy

CWE organizes weaknesses from most abstract (Pillar) to most specific (Variant):

| Level | Example | Mapping Guidance |
|-------|---------|-----------------|
| **Pillar** | CWE-664: Improper Control of a Resource | **PROHIBITED** — never use for vulnerability mapping |
| **Class** | CWE-20: Improper Input Validation | Use only when no Base or Variant applies |
| **Base** | CWE-89: SQL Injection | **PREFERRED** — correct level for most mappings |
| **Variant** | CWE-564: SQL Injection: Hibernate | **PREFERRED** — most precise, use when language/context known |
| **Compound** | CWE-352: CSRF | Context-dependent; consult mapping notes |

**Never map to CWE Categories** (labeled "Category" in the CWE list) — they are organizational groupings, not weaknesses.

---

## Mapping Labels

Every CWE entry carries a label indicating whether it may be used for vulnerability mapping:

| Label | Meaning |
|-------|---------|
| **ALLOWED** | Safe to use for root-cause mapping |
| **ALLOWED (with careful review)** | Consult the CWE entry's mapping notes first |
| **DISCOURAGED** | Avoid — ambiguous or insufficiently specific |
| **PROHIBITED** | Never use — typically a Category or Pillar entry |

Always check the mapping label before citing a CWE in a security report.

---

## 2024 CWE Top 25 Most Dangerous Software Weaknesses

Calculated from real-world CVE data, scored by frequency × severity of exploitation.

| Rank | CWE-ID | Name | Secure Coding Domain |
|------|--------|------|----------------------|
| 1 | CWE-79 | Cross-site Scripting (XSS) | Output Encoding |
| 2 | CWE-787 | Out-of-bounds Write | Memory Management |
| 3 | CWE-89 | SQL Injection | Input Validation / Database |
| 4 | CWE-352 | Cross-Site Request Forgery (CSRF) | Authentication / Session |
| 5 | CWE-22 | Path Traversal | File / Resource Management |
| 6 | CWE-125 | Out-of-bounds Read | Memory Management |
| 7 | CWE-78 | OS Command Injection | Input Validation |
| 8 | CWE-416 | Use After Free | Memory Management |
| 9 | CWE-862 | Missing Authorization | Access Control |
| 10 | CWE-434 | Unrestricted Upload of Dangerous Files | File / Resource Management |
| 11 | CWE-94 | Code Injection | Input Validation |
| 12 | CWE-20 | Improper Input Validation | Input Validation |
| 13 | CWE-77 | Command Injection | Input Validation |
| 14 | CWE-287 | Improper Authentication | Authentication / Session |
| 15 | CWE-269 | Improper Privilege Management | Access Control |
| 16 | CWE-502 | Deserialization of Untrusted Data | Input Validation |
| 17 | CWE-200 | Exposure of Sensitive Information | Data Protection |
| 18 | CWE-863 | Incorrect Authorization | Access Control |
| 19 | CWE-918 | Server-Side Request Forgery (SSRF) | Communication Security |
| 20 | CWE-119 | Improper Restriction of Buffer Operations | Memory Management |
| 21 | CWE-476 | NULL Pointer Dereference | Memory Management |
| 22 | CWE-798 | Hard-coded Credentials | Authentication / Session |
| 23 | CWE-190 | Integer Overflow or Wraparound | Memory Management |
| 24 | CWE-400 | Uncontrolled Resource Consumption | System Configuration |
| 25 | CWE-306 | Missing Authentication for Critical Functions | Authentication / Session |

Source: [2024 CWE Top 25](https://cwe.mitre.org/top25/archive/2024/2024_cwe_top25.html)

---

## OWASP Top 10 (2025) ↔ CWE Cross-Reference

| OWASP Category | Primary CWE IDs (Base/Variant preferred) |
|----------------|------------------------------------------|
| A01: Broken Access Control | CWE-862, CWE-863, CWE-269, CWE-284, CWE-285 |
| A02: Security Misconfiguration | CWE-16, CWE-732, CWE-1188 |
| A03: Software Supply Chain Failures | CWE-1035, CWE-937, CWE-494 |
| A04: Cryptographic Failures | CWE-327, CWE-326, CWE-311, CWE-312, CWE-330 |
| A05: Injection | CWE-79, CWE-89, CWE-78, CWE-77, CWE-94 |
| A06: Insecure Design | CWE-209, CWE-770, CWE-841 |
| A07: Authentication Failures | CWE-287, CWE-306, CWE-798, CWE-352, CWE-384, CWE-613 |
| A08: Software & Data Integrity Failures | CWE-502, CWE-494, CWE-829 |
| A09: Logging & Alerting Failures | CWE-778, CWE-223, CWE-117 |
| A10: Mishandling of Exceptional Conditions | CWE-754, CWE-755, CWE-390 |

---

## 14 Secure Coding Domains ↔ CWE Cross-Reference

| Domain | Key CWE IDs |
|--------|-------------|
| Input Validation | CWE-20, CWE-89, CWE-79, CWE-78, CWE-77, CWE-94, CWE-502 |
| Output Encoding | CWE-79, CWE-116, CWE-838 |
| Authentication / Session | CWE-287, CWE-306, CWE-798, CWE-352, CWE-384, CWE-613 |
| Access Control | CWE-862, CWE-863, CWE-269, CWE-284, CWE-285 |
| Cryptography | CWE-327, CWE-326, CWE-311, CWE-312, CWE-330, CWE-338 |
| Error Handling | CWE-209, CWE-390, CWE-755, CWE-754 |
| Data Protection | CWE-200, CWE-312, CWE-311, CWE-359 |
| Communication Security | CWE-295, CWE-918, CWE-319, CWE-326 |
| System Configuration | CWE-16, CWE-400, CWE-732, CWE-1188 |
| Database Security | CWE-89, CWE-943, CWE-564 |
| File / Resource Management | CWE-22, CWE-434, CWE-73, CWE-36 |
| Memory Management | CWE-787, CWE-125, CWE-416, CWE-119, CWE-476, CWE-190 |
| Business Logic | CWE-840, CWE-841, CWE-770 |
| Dependency Management | CWE-1035, CWE-937, CWE-494 |

---

## How to Reference CWE in Security Findings

When reporting a finding, always:

1. **Use Base or Variant level** — e.g., `CWE-89: SQL Injection`, not `CWE-664 (Pillar)`
2. **Use weakness language** — describe the root cause, not the impact:
   - ✅ `Missing authentication check (CWE-306)` — weakness language
   - ❌ `Allows unauthenticated access` — impact language, not a CWE description
3. **Combine with OWASP** — e.g., `A05:2025 – Injection (CWE-89)`
4. **Combine with ASVS** — map to the ASVS requirement that would have caught this
5. **Check the mapping label** — verify the CWE is ALLOWED before citing it

### Canonical Finding Header Format

```
Severity:   Critical
Title:      SQL Injection in user login query
CWE:        CWE-89 – SQL Injection  [Base, ALLOWED]
OWASP:      A05:2025 – Injection
ASVS:       5.0 §5.3.4 (Parameterized queries required)
Root Cause: User-supplied input concatenated directly into a SQL string
Impact:     Unauthenticated full read/write access to the database
```

---

## CWE Navigation Views

| View | Use Case |
|------|----------|
| **Developer View (CWE-699)** | Finding the right CWE for a code-level weakness |
| **Research View (CWE-1000)** | Comprehensive research and root-cause analysis |
| **NVD View (CWE-1350)** | Mapping vulnerabilities that appear in NVD/CVE records |
| **CWE Top 25** | Prioritizing the highest-frequency, highest-severity weaknesses |

Search tool: https://cwe.mitre.org/cgi-bin/jumpmenu.cgi

---

## What NOT to Do

| Mistake | Why It's Wrong | Correct Approach |
|---------|----------------|-----------------|
| Map to a CWE Category | Categories are groupings, not weaknesses | Use a Base or Variant CWE |
| Use a Pillar-level CWE | Too abstract to be actionable | Drill down to Base/Variant |
| Describe the exploit, not the weakness | CWE describes root cause, not attacker action | Use weakness language |
| Use a PROHIBITED or DISCOURAGED CWE | Inaccurate; rejected by NVD/tooling | Check mapping label first |
| Invent a new CWE ID | CWEs are a curated enumeration | Use existing CWE or report a gap to MITRE |

---

## References

- CWE Official Site: https://cwe.mitre.org
- CWE Usage Guidance: https://cwe.mitre.org/documents/cwe_usage/guidance.html
- 2024 CWE Top 25: https://cwe.mitre.org/top25/archive/2024/2024_cwe_top25.html
- CWE Full List: https://cwe.mitre.org/data/index.html
- CWE Developer View: https://cwe.mitre.org/data/definitions/699.html
