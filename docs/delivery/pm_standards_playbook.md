# PM Standards

## Playbook — What I expect from every project manager

**Owner:** Isaac Welch, VP of Delivery
**Last updated:** May 7, 2026
**Audience:** All project managers

---

## Why this exists

Project managers are the operating system of every engagement. When Project Managers (PM) run well, clients feel confident, teams stay focused, and engagements stay healthy. When they don't, the symptoms show up everywhere: surprise status updates, missed risks, scattered information, scope creep, eroded margins.

This document lays out what I expect from every PM. It is not a rigid script. It is the set of principles, behaviors, and standards that define how we operate as a delivery agency. Use the templates and examples to adapt to your client's context, but hold the core standards as non-negotiable.

The five standards below answer one question: how do we deliver software for clients without surprises?

---

## The Five PM Standards

### 1. Status Visibility

**Principle:** I should never be surprised by a project's status. Surface issues before they become recoveries.

**What this looks like:**

- A weekly written status update lives in the document repository every Friday by EOD, regardless of whether I asked.
- The PROJECTS database always reflects current state — health (Green / Yellow / Red), % complete, % timeline burned, current sprint, top risks.
- If health changes from green to yellow or yellow to red, I get a Teams message within 24 hours of the change. Don't wait for the weekly update.
- "We thought we'd catch up next sprint" is not a status update. If you're behind, say it the day you know.

**Examples:**

- **Good:** "VFW Command Post slipped from 65% to 58% complete this sprint. Root cause: discovery on the auth integration revealed a third-party API limitation. Revised plan and 2-week impact attached."
- **Weak:** "Sprint went well overall, working through some blockers, will update next week."

### 2. Information Stewardship

**Principle:** Information lives in one place per type. PMs are the librarians.

**What this looks like:**

- New meeting → MEETING note within 24 hours, with action items linked to task tracker.
- New client touchpoint (call, important email, decision) → INTERACTION documented.
- New deliverable → SharePoint, in the right client folder (`/Clients/[Client]/[Project]/`).
- New work item → ADO, sized, labeled.
- Decision made → captured in the project's NOTES or in the relevant meeting record. If it's worth remembering, it's worth writing down.

**Examples:**

- **Good:** Client emails about a scope change → reply, log INTERACTION, create TASK or ADO ticket, update PROJECTS health if material.
- **Weak:** Decision made on a Teams call, lives only in chat history.

### 3. Client Communication

**Principle:** Be ahead of the client. Direct, structured, no fluff.

**What this looks like:**

- Every client gets a weekly written update in their preferred channel (default: email, Smart Brevity format).
- Every meeting has a recap within 24 hours: decisions, action items, owners, due dates.
- Bad news is delivered before the client asks. Always with the plan.
- Pushback is part of the job. If a client decision puts the engagement at risk, surface it directly — not in a passive footnote.

**Examples:**

- **Good:** "Three updates this week: sprint goal hit (auth flow shipped to staging), one risk emerging (data migration timing tight for Phase 2), one decision needed (final UX review by Thursday). Details below."
- **Weak:** "Hi team, hope you're well! Just wanted to circle back and share some updates from the past week…"

### 4. Scope & Risk Management

**Principle:** Scope and risk are PM jobs. Don't outsource them to the client or the engineering team.

**What this looks like:**

- A risk register lives in the project's files, updated weekly. Top three risks always visible, with owner and mitigation.
- Scope changes are documented. Use the scope change memo template. Quantify impact (effort, timeline, budget). Get a written decision before any work starts.
- "Can we just add…" gets a process, not a yes. Even small additions go through the memo. Pattern recognition matters.
- Risks are surfaced with options, not as problems. Bring the trade-off, not the panic.

**Examples:**

- **Good:** "Client requested feature X mid-sprint. Effort ~3 days. Trade-off options: (A) defer feature Y to next sprint, (B) extend sprint by 3 days, (C) absorb risk and recommend in Phase 2. Recommendation: B. Awaiting client decision."
- **Weak:** "Client wants X, I told them yes, we'll figure it out."

### 5. Delivery Discipline

**Principle:** Run the ceremonies. Watch the margins. Hit the bar.

**What this looks like:**

- Sprint planning, review, and retro happen on schedule. No exceptions.
- Definition of Done is applied consistently. Code reviewed, tested, deployed, demoed, documented.
- Margins reviewed monthly via ATP export and the KPI report. PM owns the conversation if a project is below margin target.
- Engagements end cleanly. Final deliverable, retrospective, lessons learned in NOTES, formal handoff or closeout.

---

## Source of Truth Map

One place per type of information. The recommendation below is what every PM should default to.

| Type of information | Lives in | Why |
|---|---|---|
| Project record (status, health, scope, % complete) | PROJECTS | Single executive view |
| Engineering work (epics, stories, bugs, tasks) | Azure DevOps | Sprint execution and team velocity |
| Meeting notes | MEETINGS | Searchable and linkable |
| Client touchpoints (calls, decisions, escalations) | INTERACTIONS | Relationship history |
| Files, deliverables, contracts | SharePoint `/Clients/[Client]/` | Formal artifacts |
| Time and utilization | ATP* → KPI report | Margin and capacity |
| Client email | Outlook (key items logged in INTERACTIONS) | External channel of record |
| Internal team comms | Teams | Day-to-day coordination only |

\* ATP time tracking to be replaced with alternative tool.

**Rule of thumb:** if two people might need it later, it doesn't live in a Teams chat.

---

## Communication Cadence

The minimum cadence below applies to every engagement. You can add more, never less. Adjust client-facing rhythm to match the engagement, but never drop below weekly.

| Cadence | What | Where |
|---|---|---|
| Daily | ADO updates current; blockers visible | ADO + Teams |
| Weekly (Friday EOD) | Internal status update | PROJECTS + email to Isaac & Jason |
| Weekly | Client status update | Email, Smart Brevity format |
| Sprint | Planning, Review, Retro | Zoom + MEETINGS |
| Monthly | Margin and health review | KPI report + 1:1 with Isaac & Jason |
| Quarterly | Client Business Review (QBR) | In person or Zoom |

---

## Escalation Triggers

Don't wait for a weekly update. Send a Teams message within 24 hours when any of these occur:

- Project >10% over timeline OR progress <80% of planned
- Margin within 5 points of target threshold
- Client escalates to anyone above the PM (CEO, partner, executive sponsor)
- Resource constraint will affect delivery (illness, departure, capacity)
- Scope change request >10% of total effort
- Trust event — a moment that could affect the client relationship, positive or negative

**Format:** situation, impact, what you're doing about it, what you need from me.

---

## Definition of Done

### A user story is done when

- Acceptance criteria met
- Code reviewed and merged
- Tests passing (unit and integration)
- Deployed to staging and QA approved
- Demoed to client (if client-facing)
- Documentation updated

### A sprint is done when

- Sprint goal achieved or formally renegotiated
- Burndown reflected in tracker
- Review completed with client
- Retro completed with team
- Notes captured in MEETINGS

### A project is done when

- All committed deliverables shipped
- Client signoff documented in INTERACTIONS
- Final deliverables filed in SharePoint
- Retrospective completed
- Lessons learned captured in NOTES
- Formal handoff or closeout email sent
- ATP and KPI updated

---

## Onboarding Checklist

For new PMs joining the practice, the first 90 days follow this arc.

### First 30 days

- Shadow one full sprint cycle on a steady-state project
- Read every active project record in database
- Meet every client stakeholder on assigned engagements
- Review the last 90 days of INTERACTIONS for assigned clients
- Set up ADO, ATP, and SharePoint access; verify everything works

### First 60 days

- Run a sprint review and retro independently
- Write a weekly status update Isaac doesn't need to revise
- Submit first scope change memo (real or simulated)
- Complete first monthly margin review for assigned project

### First 90 days

- Own a project end-to-end through one major milestone
- Lead a client status meeting with no Isaac safety net
- Identify and surface a real risk before it became visible to others
- Submit one improvement to PM standards (this document evolves)

---

## Templates

Start from the templates below. They live in `/Users/isaac/Documents/_Templates/PM/` — see Isaac if any are missing.

- Weekly status update — Smart Brevity format
- Scope change memo — effort, trade-offs, recommendation
- Risk register — Notion template
- Project closeout — checklist + email template
- Meeting recap — Notion MEETINGS

---

## A note on this document

This is version 1. It evolves. If you find a standard that doesn't serve the client or the engagement, raise it. Pushback is part of the job.

---

*© 2026. All rights reserved.*
