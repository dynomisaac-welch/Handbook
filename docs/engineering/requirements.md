# Requirements & Work Items

Clear requirements are the foundation of predictable delivery. When a work item is ambiguous, developers guess, testers can't verify, and reviewers can't tell if the work is done. These standards exist so every story that enters a sprint is ready to build.

These standards apply across all project types and tools (Azure DevOps, Jira, Linear, etc.). The fields and structure below are the baseline — individual projects may add to them but not remove them.

---

## Who Owns This

Writing requirements is primarily the responsibility of the **Product Owner (PO)** or **Business Analyst (BA)** on the engagement. But requirements are a team sport:

- Developers read requirements and flag gaps during refinement
- Testers use acceptance criteria to verify work — if they can't, the criteria aren't done
- The DM ensures stories are Ready before they enter a sprint

---

## Core Fields — Every Work Item

Every work item must have these fields before it leaves the backlog:

**Title** — One clear line. If you can't describe the work in a single short sentence, the item is probably too large. Break it up.

**Description** — A short paragraph explaining the purpose. What problem does this solve? Why does it matter?

**Type** — User Story, Bug, Task, or Enhancement.

**Priority** — High, Medium, or Low. Base this on business impact, not urgency to someone's personal preference.

**Status** — Where it sits in the workflow: Backlog, Ready, In Progress, In Review, Done.

---

## User Story Format

When the work item is a User Story, use the standard format:

> **As a** [user role], **I want** [what], **so that** [benefit / why].

This format keeps the focus on the person using the feature and the value it delivers. All three parts are required. A story missing the "so that" is a story with no justification — which is a story that shouldn't be built.

**Example:**
> As an administrator, I want to receive an email summary after the nightly data sync completes, so that I can confirm the sync succeeded or investigate failures before business hours.

If a story can't be expressed in this format, it's often a Task (implementation detail) rather than a User Story. Tasks don't need the user story format, but they do need a clear description of the work and the expected outcome.

---

## Acceptance Criteria

Acceptance criteria define what "done" means. A story without acceptance criteria isn't a story — it's a wish.

**Format:** Use Given / When / Then (also called Gherkin or BDD):

- **Given** — the starting condition or context
- **When** — the action or event
- **Then** — the expected result

**Example:**
> Given the nightly sync has completed,  
> When all records are processed successfully,  
> Then the system sends a summary email to the admin distribution list by 6:00 AM.

**Coverage:** Acceptance criteria must cover three scenario types:

1. **Happy path** — the feature works as expected with valid input
2. **Edge cases** — unusual but valid situations (empty results, maximum values, slow networks)
3. **Failure scenarios** — what the system does when something goes wrong

A story is not ready for development until all three are covered. If you can't write edge and failure criteria, you don't fully understand the requirement yet.

---

## Definition of Ready

A work item is Ready for a sprint when all of the following are true:

- Title is clear and fits on one line
- Description explains the purpose and the "why"
- Type, Priority, and Status are set
- User story is written (if applicable)
- Acceptance criteria cover happy path, edge cases, and failures
- Dependencies are identified and resolved or tracked
- Any required design assets are attached or linked
- Story points are estimated

If a story doesn't meet this bar, it stays in the backlog. Don't pull half-formed work into sprints — it generates churn, rework, and frustration.

**Story point sizing:** We use the Fibonacci sequence (1, 2, 3, 5, 8, 13). Points measure relative complexity, not hours. If a story is 13 points, split it. Stories over 8 points are high-risk and frequently slip.

---

## Functional and Non-Functional Requirements

Most stories have both types. Both belong in the work item.

**Functional requirements** describe what the system does:

- What inputs the system accepts
- What the system does with each input
- Validation rules and error messages
- Business rules that apply

**Non-functional requirements** describe how the system behaves:

- Performance — "The page must load in under 2 seconds for 95% of requests"
- Security — authentication, authorization, data protection
- Scalability — expected load and growth
- Availability — uptime requirements
- Accessibility — WCAG 2.1 AA for all user-facing features

Non-functional requirements are easy to skip and expensive to retrofit. Capture them during refinement, not during QA.

---

## Technical Details

Add a technical notes section when the story involves:

- External APIs — endpoints, request/response shapes
- Data model changes — schema updates, migrations
- Known dependencies — other features, third-party services
- Security considerations — data sensitivity, access control, audit requirements

This section is optional for simple stories and required for anything touching infrastructure, integrations, or sensitive data. If the developer can't figure out what to build from the requirements alone, add more here.

---

## UI and Design

For any change that affects the user interface:

- Wireframes or mockups must be attached or linked (Figma preferred)
- Reference existing design patterns where applicable
- Include accessibility notes: color contrast, keyboard navigation, screen reader support

**Rule:** Don't start UI development without an approved design. Unilateral design decisions during development generate rework.

---

## Test Cases

List the test scenarios QA will use to verify the story. Work with the test team to make this complete before the story enters the sprint.

- Positive tests — valid inputs that should succeed
- Negative tests — invalid inputs that should fail safely
- Edge cases — boundary values, empty inputs, special characters

For user-facing features, include at least one accessibility check.

---

## Dependencies and Risks

Every story that depends on another team, system, or external service should call that out explicitly:

- **Dependencies** — other stories, backend APIs, third-party services
- **Risks** — unknowns, integration concerns
- **Blockers** — anything that prevents work from starting

If a dependency isn't resolved, the story isn't ready. Don't pull blocked stories into a sprint.

---

## Links and References

Attach or link anything the team needs to understand the context:

- Parent epic
- Related bugs or follow-on stories
- Design documents or Figma files
- Relevant meeting notes or decisions
- Architecture documents (ADRs)

Good links save 20 minutes of context-gathering at the start of every development session.

---

## Quick Checklist

Before marking a work item Ready for Development:

- [ ] Title is clear and fits on one line
- [ ] Description explains the purpose and why it matters
- [ ] Type, Priority, and Status are set
- [ ] User story is written (if a User Story type)
- [ ] Acceptance criteria cover happy path, edge cases, and failures
- [ ] Functional and non-functional requirements are documented
- [ ] Technical details captured (APIs, data models, security considerations)
- [ ] Design assets attached or linked (if UI changes)
- [ ] Test cases listed
- [ ] Dependencies and blockers identified
- [ ] Related items linked
- [ ] Story points estimated and item assigned
