# Code Review

Code review is not a gate. It's a conversation. The goal is better software, not checking a box before merge. A good review catches real problems, spreads knowledge across the team, and holds the quality bar without creating bottlenecks.

---

## The Basics

Every change merged to a main branch goes through a pull request. No exceptions — including hotfixes, small changes, and "I'll clean it up later" commits.

**Minimum requirements to open a PR:**

- CI passes (build, lint, unit tests)
- Self-review completed — read your own diff before requesting review
- PR description explains what changed and why
- Linked to the work item (story, bug, or task) it addresses

A PR that fails CI, has no description, or is obviously incomplete should be sent back before review begins. Don't review broken work.

---

## PR Description

The description does the thinking so the reviewer doesn't have to start cold. A good description answers three questions:

**What changed?** A brief summary of the change — not a list of files, but a human explanation of what the code now does differently.

**Why?** The context behind the change. What problem does it solve? What decision was made? If there's a relevant ticket or design document, link it.

**How to test it?** Instructions for verifying the change manually (if needed) or a note confirming automated test coverage.

For large or complex PRs, add a short walkthrough of the key areas to focus on. This makes the reviewer's job faster and better.

---

## Review Standards

**Response time.** Reviewers respond within one business day. If you're assigned a review and can't get to it, say so — don't silently sit on it and block someone's work.

**Required approvals.** Every PR needs at least one approval from a developer who didn't author the change. Senior changes (architecture decisions, security-relevant code, database migrations) require approval from the Tech Lead or a designated senior reviewer.

**What reviewers look for:**

- Correctness — does the code do what the ticket requires?
- Edge cases — what happens with empty input, null values, unexpected state?
- Error handling — are failures caught and surfaced appropriately?
- Security — are there injection risks, improper access controls, or sensitive data exposure?
- Performance — are there N+1 queries, unbounded loops, or unnecessary computation?
- Tests — is there adequate test coverage for the change? Are the tests actually testing the right thing?
- Standards adherence — does the code follow our naming, structure, and pattern standards?
- Readability — will the next developer be able to understand this in six months?

Not every review will catch something. That's fine. The discipline of looking is still valuable.

---

## Feedback Style

**Be specific.** "This is confusing" is not actionable. "This method does three different things — consider splitting it so each has a single responsibility" is.

**Distinguish blocking from non-blocking feedback.** Use prefixes to signal intent:

- No prefix — required change, must be addressed before merge
- `nit:` — minor style or preference, author's discretion
- `suggestion:` — optional improvement worth considering
- `question:` — genuine curiosity, not necessarily a problem

**Assume positive intent.** The author made a choice for a reason. Ask about the reasoning before assuming it's wrong: "Why did you go with X here rather than Y?" is better than "This should be Y."

**Praise what's good.** If you see a clean solution or an approach you'd steal, say so. Reviews don't have to be purely corrective.

---

## What Authors Do with Feedback

**Address every comment.** Either fix it, explain why you're not fixing it, or ask a clarifying question. Don't silently ignore feedback — even `nit:` comments deserve a response.

**Don't argue in the PR.** If you disagree with feedback, a short explanation is fine. If it escalates, take it offline. PR comments are not the place for a lengthy debate.

**Re-request review after changes.** When you've addressed feedback, re-request review from the original reviewer. Don't assume they're watching for a commit and will re-review automatically.

---

## Merging

The author merges after approval, not the reviewer. The author knows whether there are other comments still outstanding or other reviewers who need to sign off.

**Before merging:**

- All required approvals obtained
- All blocking comments resolved
- CI is passing on the latest commit
- Branch is up to date with the target branch (rebase or merge main in)

We use squash merges by default to keep the main branch history readable. The squash commit message should be a clean summary of the PR, not a dump of every commit message.

---

## Special Cases

**Hotfixes.** A hotfix still gets a PR, still gets a review, and still goes through CI. The difference is that we find a reviewer fast rather than waiting for the normal queue. If the fix is genuinely time-critical and no reviewer is available, the Tech Lead can approve it directly — and a follow-up review happens within 24 hours.

**WIP / Draft PRs.** Use GitHub/ADO draft PR status for work that's not ready for review. Don't open a real PR and write "WIP" in the title — it creates ambiguity about whether review is expected.

**Large PRs.** A PR that touches more than 400–500 lines of net-new logic is usually too big. If a change is large by nature (a migration, a new feature), break it into a stack of smaller PRs that can be reviewed and merged incrementally. Large PRs get superficial reviews — they're too hard to hold in your head at once.

---

## Using Section Codes in Reviews

Our engineering standards documents assign stable codes to every rule (e.g., `[DOTNET-NAME-03]`, `[FE-A11Y-02]`, `[DB-NAME-04]`). When citing a standard in a review comment, use the code. It's faster than explaining the rule from scratch and creates a shared vocabulary across the team.
