# Project Lifecycle

Every engagement we run follows the same four-phase arc — from handoff through closeout. The phases are consistent; the depth and duration flex based on the project. What doesn't flex is the discipline inside each phase.

This page gives you the map. Detailed playbooks for each phase live in the Delivery Operations workspace in Notion.

---

## Pre-Phase: Sales to Delivery Handoff

Before delivery begins, a formal handoff from Sales to the assigned Delivery Manager must occur. This is not a courtesy — it's a gate. Delivery doesn't start until the DM is fully briefed.

**The handoff must complete within 5 business days of contract signature.**

What the DM receives:
- Signed SOW (scope, budget, timeline, billing model)
- Client stakeholder map (who decides, who's the day-to-day, any politics)
- Known constraints (technical, regulatory, hard deadlines)
- Resource plan (who's assigned, when they start)
- Sales discovery notes and any flags from the close process

What the DM does immediately after:
- Reviews the SOW line by line and flags ambiguities
- Confirms resource availability with team leads
- Schedules both the internal kickoff and client kickoff
- Sets up project infrastructure (ADO, Notion, Slack, shared drives)

**The client introduction email goes out within 48 hours of handoff completion.**

### Deliverables

**Required**
- Completed handoff checklist confirming DM received all inputs
- Project infrastructure live (ADO, Notion, Slack, shared drives)
- Client introduction email sent within 48 hours

**Optional**
- Handoff memo — use when the engagement carries unusual complexity, political risk, or prior client history that warrants a written summary beyond the checklist

---

## Phase 1: Intake & Kickoff

**Typical duration:** Week 1 (5 business days)  
**Goal:** Team aligned, client expectations set, project infrastructure ready, Project Canvas approved.

A strong kickoff prevents most of the problems that surface later. The investment here pays back in every subsequent sprint.

### Internal Kickoff (Days 2–3)
Happens before the client kickoff. The full delivery team — DM, Tech Lead, engineers, QA, designer — reviews the SOW together, aligns on scope and technical approach, establishes working agreements, and identifies early risks. The team should never meet a client before they've met each other on this engagement.

### Client Kickoff (Days 3–5)
The first formal meeting with client stakeholders. Agenda covers:
- Business objectives and what success actually looks like
- Explicit scope alignment — what's in, what's out, and what's gray
- How we'll work together (sprint cadence, demo cadence, communication rhythm, escalation path)
- Change request process
- Decision-making authority on the client side

### Phase 1 Exit: Project Canvas
The Project Canvas is a single-page overview of the engagement — objectives, scope, timeline, budget, risks, communication plan, and Definition of Done. **Client sign-off on the Project Canvas is the gate out of Phase 1.** Don't proceed without it.

### Deliverables

**Required**
- Project Canvas — client sign-off gates exit from this phase
- Initial risk register — top risks identified with owners

**Optional**
- RACI matrix — use when the engagement has multiple client stakeholders or ambiguous decision authority
- Communication plan — use when the client has formal reporting requirements or distinct audience tiers (executive vs. working team)
- Sprint 0 plan — use when proceeding to a discovery phase
- Sprint 1 plan — use when proceeding directly to build

---

## Phase 2: Discovery

**Typical duration:** 2–6 weeks depending on complexity  
**Applies when:** Scope isn't fully defined or there are significant technical unknowns.  
**Goal:** A ready-to-build product backlog, validated architecture, and client sign-off on scope and approach.

Discovery is structured, not open-ended. Each week has a focus:

- **Week 1:** Current state — requirements, pain points, technical environment, integrations
- **Week 2:** Future state — user personas, journey mapping, feature prioritization (MoSCoW)
- **Week 3:** Story mapping workshop and backlog population
- **Week 4:** Architecture finalization, roadmap planning, discovery closeout

Client involvement is high during discovery — typically 2–3 working sessions per week. If the client Product Owner isn't available or empowered to make decisions, escalate. Discovery without a real decision-maker produces a backlog nobody owns.

### Discovery Exit Criteria
- Product backlog populated (at minimum, 2 sprints Ready and 4 sprints groomed)
- Product roadmap with release milestones
- Technical architecture documented and validated
- Definition of Ready and Definition of Done established
- Client sign-off obtained before build begins

### Deliverables

**Required**
- Product backlog — minimum 2 sprints Ready, 4+ sprints groomed, with acceptance criteria
- Product roadmap with release milestones
- Technical architecture document
- Definition of Ready and Definition of Done
- Client sign-off on discovery artifacts — gates transition to build

**Optional**
- Story map or epic breakdown — use when backlog complexity warrants visual structure for client alignment
- Data models and API specifications — use when integration depth requires formal documentation
- Wireframes or design comps — use when UI/UX decisions need client validation before build begins
- Updated Project Canvas — use if scope changed materially during discovery

---

## Phase 3: Build & Delivery

**Typical duration:** Variable — runs until committed scope is delivered  
**Goal:** Consistent, predictable delivery of working software in short cycles.

We use a two-week sprint cadence by default. The ceremonies are standard:

**Sprint Planning** — Review sprint goal, pull Ready stories, commit as a team, decompose into tasks. No story enters a sprint without meeting the Definition of Ready.

**Daily Standup (15 min)** — What shipped yesterday, what's shipping today, what's blocked. Deep dives go offline.

**Backlog Refinement (mid-sprint)** — Groom upcoming stories, add acceptance criteria and estimates, maintain 2+ sprints of Ready work at all times.

**Sprint Demo** — Show completed work to client stakeholders, in a production-like environment, against acceptance criteria. We only demo stories that meet the Definition of Done.

**Sprint Retrospective (internal)** — What went well, what to improve, 1–2 action items with owners. No client present.

### Change Management
Scope changes happen. The process:
- Client requests a change → DM assesses impact (stories, sprints, budget, timeline)
- Under 10% budget impact and no timeline impact → DM approves, adds to backlog
- Over 10% budget or timeline impact → Escalate to Portfolio Manager
- Change order signed before work begins

### Escalation Triggers
Escalate to Portfolio Manager when any of these are hit:
- Budget variance exceeds 10% or $25K (whichever is lower)
- Project margin drops 15+ points below target
- Any client exec signals dissatisfaction, raises a formal complaint, or requests DM replacement
- Two consecutive sprints with client dissatisfaction indicators

### Weekly Status Report
Every Friday, the client receives a written status update covering: sprint accomplishments, next sprint plan, budget burn vs. plan, velocity trend, backlog health, top risks, and decisions needed. This is non-negotiable — it's the primary artifact of project transparency.

### Deliverables

**Required**
- Weekly status report — every Friday, non-negotiable
- Updated risk register — weekly, top 3 risks visible with owners
- Signed change orders — required before work begins on any approved scope change
- Build completion sign-off — all committed stories accepted by client, documentation complete, known issues logged with severity

**Optional**
- Monthly health check summary — use on engagements running 3+ months; reviewed with Portfolio Manager covering budget burn, margin, and velocity forecast
- Release notes — use when the client requires formal documentation of each production deployment
- Sprint demo script — use for high-stakes demos or executive-level audiences

---

## Phase 4: Closeout & Hypercare

**Typical duration:** 2–4 weeks post-launch  
**Goal:** Stable production launch, smooth handoff, clean engagement close.

### Pre-Launch
Before any production deployment: performance and load testing complete, security review passed, monitoring and alerting configured, user training delivered, rollback plan documented and tested. The launch checklist gates production deployment.

### Hypercare Period
The 2–4 weeks immediately after launch. The delivery team stays on with enhanced monitoring, priority response to production issues, and daily check-ins with the support team. The DM remains the primary escalation point. Weekly hypercare status reports go to the client.

**Hypercare exits when:** System is stable with no critical issues for 1+ consecutive weeks, the support team is handling incidents independently, and user adoption is trending positively.

### Client Closeout
- Final demo and formal acceptance sign-off
- Lessons learned session
- Financial reconciliation and final invoice
- Transition communication if moving to a maintenance team
- Satisfaction survey or feedback conversation

### Internal Closeout
- Post-mortem with the delivery team
- Lessons learned logged
- Project artifacts archived
- Final financial reconciliation (budget vs. actual, margin analysis)
- Team capacity released
- Wins recognized

### Deliverables

**Required**
- Signed pre-launch checklist — gates production deployment
- Final acceptance sign-off — client sign-off on all committed deliverables
- Post-mortem report — internal, completed before engagement closes
- Lessons learned document — logged for organizational reference
- Financial closeout report — budget vs. actual, margin analysis

**Optional**
- Launch report — use for significant or public launches where the client needs a formal record of go-live metrics and outcomes
- Hypercare summary report — use when transitioning to a maintenance team or when hypercare exceeded standard duration
- Transition/handoff package — required only if moving to a maintenance team; includes architecture documentation, access credentials, SLA definitions, support runbook, escalation procedures, and contact list
- Final product backlog state — use when remaining work or technical debt is being handed to another team or back to the client

---

## Delivery Manager Authority

The DM can approve without escalation:
- Sprint-level prioritization adjustments within milestone goals
- Process changes (ceremony timing, standup format)
- Minor scope clarifications under 5% effort impact
- Hypercare extension of up to 1 week

The DM must escalate to Portfolio Manager for:
- Scope changes over 10% effort or budget
- Timeline extension requests
- Budget overruns or margin compression
- Client escalations or satisfaction issues
- Phase gate approvals (discovery → build, build → maintenance)
- Launch delays or rollback decisions
