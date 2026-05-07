# Status Reporting

Every active project sends a written status report to the client every Friday. This is not optional and not contingent on whether the sprint was good or bad. Consistent reporting is how we build trust — clients who hear from us regularly are clients who feel in control.

This page covers what goes in a status report, how to write one, and how we handle escalations.

---

## Why We Report in Writing

A weekly written report does three things a meeting can't:

It creates a paper trail. The client has a dated record of what we said the status was, what risks we flagged, and what decisions were pending. This protects both sides.

It forces clarity. You can't write a coherent status update if you don't actually know where the project stands. The report is a forcing function that surfaces problems before they become surprises.

It respects the client's time. Not everyone who needs to stay informed wants to be in a meeting every week. A well-written report lets stakeholders consume information on their schedule.

---

## What Goes in Every Report

Every weekly status report covers these eight elements:

**Sprint accomplishments.** What stories were completed and accepted this sprint. Reference the sprint goal and whether it was met. Be specific — list features or fixes by name, not just story counts.

**Next sprint plan.** What we're committing to in the upcoming sprint. This gives the client visibility into what's coming and an opportunity to reprioritize before work begins.

**Budget burn vs. plan.** Actual hours or spend to date against the budgeted amount. Include the percentage consumed and the percentage of scope delivered. If these are diverging, flag it — don't hide it.

**Velocity trend.** Points or stories completed per sprint over the last 3–4 sprints. A downward trend is a risk signal. An upward trend is worth calling out.

**Backlog health.** How many sprints of Ready work are in the backlog. If we're running thin (less than two sprints of groomed work), note what's being done about it.

**Top risks.** No more than three. Each risk gets one line: what it is, the likelihood, the impact, and the mitigation plan. If a risk from last week was resolved, say so.

**Decisions needed.** Any open questions that require client input before work can proceed. Be explicit about the deadline if timing matters.

**Notable wins.** One or two things that went particularly well. This is not cheerleading — it's helping the client understand what's working so we can protect it.

---

## Format and Length

Reports go out as a brief email with a structured body — no attachment, no deck. The client should be able to read the full update in three minutes.

Use this structure:

```
Subject: [Project Name] — Weekly Status | Week of [Date]

STATUS: [Green / Yellow / Red]

[One sentence on overall health]

SPRINT [N] RESULTS
...

NEXT SPRINT PLAN
...

BUDGET
...

RISKS
...

DECISIONS NEEDED
...
```

**Status colors:**

- **Green** — On track. No significant issues.
- **Yellow** — At risk. A concern exists that could affect timeline, budget, or quality if not addressed. Describe it.
- **Red** — Off track. A concrete problem is affecting delivery. Describe it and the recovery plan.

Never call a project Green when it's Yellow, and never call it Yellow when it's Red. Clients do not punish honesty — they punish being blindsided.

---

## Tone

Status reports are written for a mixed audience: the day-to-day contact, their manager, and sometimes executives who see it secondhand. Write accordingly.

**Plain language.** Avoid technical jargon. If a technical issue needs to be described, explain it in terms of user impact, not architecture.

**Direct.** State the status, then the reason. Don't bury a risk in the fourth paragraph of a dense block of text.

**Confident, not defensive.** When something went wrong, own it and describe the fix. A report that sounds like a legal brief makes clients nervous.

**No surprises.** If something significant happened during the week — a production issue, a scope gap, a resource change — the client should already know. The report confirms and summarizes; it's never the first time they're hearing something important.

---

## Escalation Reporting

When a project moves to **Yellow or Red**, the Delivery Manager must also notify the Portfolio Manager before the report goes to the client. The PM needs to know before the client does, not after.

If the project is Red, the report must include:

- What went wrong
- When it was identified
- The impact on timeline and budget
- The specific recovery plan with dates
- What the client needs to do (if anything)

A Red report without a recovery plan is an incomplete report.

---

## Report Timing

Reports go out by **3:00 PM local time on Fridays.** If a holiday or unusual circumstance means Friday isn't viable, send it Thursday. Never let a week go without a report.

If a project is in Hypercare (the 2–4 weeks post-launch), reports switch to daily check-ins rather than weekly summaries. See the [Project Lifecycle](project-lifecycle.md) page for Hypercare specifics.

---

## Templates

Report templates live in the `_Templates/` folder in the shared drive. Use the correct template for the engagement type (fixed-price vs. T&M) — the budget section differs.

If you're a DM creating a report for the first time on a new engagement, copy the template and customize the header. Don't start from a blank email.
