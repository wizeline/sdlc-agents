# Architecture Documentation: {{system_name}}

> **SDLC Phase**: {{sdlc_phase}}
> **Diagrams**: {{diagram_levels}}
> **Last Updated**: {{date}}

---

## System Overview

{{2-3 sentences: what the system does, who it serves, its business context.}}

---

{{DIAGRAM_SECTIONS}}
<!-- Each diagram agent appends its completed section here in level order:
     L1 System Context → L2 Container → L3 Component → Deployment → Dynamic -->

---

## Key Architectural Decisions

| # | Decision | Rationale | Date |
|---|----------|-----------|------|
| 1 | {{decision}} | {{rationale}} | {{date}} |

---

## Change Log

| Date | Change | Author |
|------|--------|--------|
| {{date}} | Initial documentation | {{author}} |

---

## Diagram Review Checklist

- [ ] All nodes: label + type + 1-sentence description
- [ ] All relationships: verb label
- [ ] L2+: protocol/technology on every relationship
- [ ] System boundary clearly marked
- [ ] External systems use `System_Ext` / external notation
- [ ] No level mixing (L1 has no containers; L2 has no components; etc.)
- [ ] Technologies are accurate and current
- [ ] Appropriate detail for the stated SDLC phase and audience
