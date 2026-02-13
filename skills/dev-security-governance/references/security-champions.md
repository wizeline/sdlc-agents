# Security Champions Program Reference

## Overview

Security Champions are active members of development, QA, or DevOps teams who act
as the "voice" of security within their groups. They bridge the 100:10:1 gap
(developers : operations : security staff) by decentralizing security expertise.

## The Six-Step Implementation Playbook

### Step 1: Identify Teams
- Map the organizational structure to identify which teams need a champion
- Understand each team's tech stack, deployment model, and data sensitivity
- Establish a baseline of current security practices per team
- Prioritize teams handling the most sensitive data or highest-risk applications

### Step 2: Define the Role
Clearly document expectations so champions and their managers agree on scope:
- **Time commitment:** ~20% of working time dedicated to security activities
- **Core responsibilities:**
  - Promote security best practices within the team
  - Participate in or facilitate threat modeling during design
  - Help the team use security testing tools effectively
  - Triage security findings and coordinate remediation
  - Serve as liaison between the team and the central AppSec team
- **What it is NOT:** The champion is not solely responsible for the team's security;
  they are a force multiplier, not a single point of failure

### Step 3: Nominate Champions
- Champions should volunteer or be nominated — forced assignment breeds resentment
- Look for developers with curiosity about security, attention to detail, and influence within their team
- Obtain explicit management buy-in for the time commitment
- Ensure champions are supported, not penalized, for spending time on security

### Step 4: Set Up Communication Channels
- Create a dedicated Slack/Teams channel for all champions
- Schedule monthly champions meetups (cross-team knowledge sharing)
- Establish escalation paths to the central security team
- Create a shared knowledge base (Confluence, Notion, wiki) for findings and patterns

### Step 5: Build a Solid Knowledge Base
- Provide advanced training beyond what all developers receive
- Recommended training platforms: Snyk Learn, Checkmarx secure coding, SANS AppSec courses
- Create internal training modules specific to your tech stack and common vulnerability patterns
- Build a curated library of secure coding patterns, anti-patterns, and architectural decisions

### Step 6: Maintain Interest and Momentum
- Gamification: Martial arts-style belt levels (white → green → black belt)
- Recognition: Internal security awards, conference attendance sponsorship
- Competitions: CTF events, bug bounties (internal), security hackathons
- Career growth: Security Champion experience as a credential for promotion
- Community: Annual champions summit, guest speakers, cross-company meetups

## Measuring Champion Program Effectiveness

### Leading Indicators (predictive)
- Number of threat models completed per sprint/quarter
- Security training hours per champion per quarter
- Number of security-related code review comments
- Champion engagement in community channels (messages, questions answered)

### Lagging Indicators (outcome)
- Reduction in vulnerabilities found in production
- Decrease in mean time to remediate (MTTR) for security findings
- Percentage of projects with completed threat models
- Reduction in security-related incidents per team

### Program Health Metrics
- Champion retention rate (aim for >80% year-over-year)
- Percentage of teams with an active champion
- Champion satisfaction survey scores
- Manager satisfaction with champion impact

## Common Pitfalls and Mitigations

**Pitfall: Champions become bottlenecks**
- Mitigation: Champions enable and teach, not gate. The whole team owns security.

**Pitfall: Management doesn't allocate time**
- Mitigation: Get executive sponsorship before launch; track and report value delivered.

**Pitfall: Burnout from dual responsibilities**
- Mitigation: Clear scope boundaries; rotate responsibilities; celebrate contributions.

**Pitfall: Champions lack authority**
- Mitigation: Give champions a formal role with documented authority to block releases
  for critical security issues; back them up when they exercise it.

**Pitfall: Program fizzles after initial enthusiasm**
- Mitigation: Regular cadence of events, recognition, and new content; connect to
  career development; refresh the champion cohort annually.
