# Source Control

We use Git on every engagement. These standards cover how we branch, commit, and submit work for review. The goal is a history that's readable, a process that prevents accidents, and a workflow that scales across teams.

---

## 1. Branching Strategy [SCM-BRANCH]

### 1.1 Branch Structure [SCM-BRANCH-01]

We use a simplified trunk-based model with short-lived feature branches:

- `main` — production-ready code at all times. Protected. Direct commits are not allowed.
- `develop` — integration branch for active development (used on longer-running engagements). Optional; some projects merge directly to main.
- `feature/<short-description>` — one feature or story per branch, branched from main (or develop)
- `bugfix/<short-description>` — bug fixes, branched from main
- `hotfix/<short-description>` — urgent production fixes, branched from main, merged to main and develop
- `release/<version>` — release preparation (optional, for versioned projects)

### 1.2 Branch Naming [SCM-BRANCH-02]

Branch names are lowercase and kebab-case. They include the work item ID where possible:

```
feature/12345-add-retention-notification
bugfix/12346-fix-email-formatting
hotfix/fix-auth-timeout
```

No vague names like `fix-stuff`, `my-branch`, or `wip`. If the branch name doesn't describe the work, it's wrong.

### 1.3 Branch Lifetime [SCM-BRANCH-03]

Feature branches are short-lived — merged within one sprint (two weeks) in most cases. A branch that lives longer than two weeks has grown too large or has a dependency problem. Either break the work into smaller PRs or address the dependency.

Delete branches immediately after merge. Stale branches create confusion about what's active.

---

## 2. Commit Standards [SCM-COMMIT]

### 2.1 Commit Often [SCM-COMMIT-01]

Commit small, coherent units of work. A commit should represent a single logical change that leaves the codebase in a working state. Don't commit half-finished work to a shared branch — if you need a checkpoint, use a local commit or a draft PR.

### 2.2 Commit Message Format [SCM-COMMIT-02]

We follow a simplified Conventional Commits style:

```
<type>: <short summary>

<optional body explaining why, not what>

<optional footer: refs #12345>
```

**Type values:**

| Type | Use for |
|---|---|
| `feat` | New feature or user-visible capability |
| `fix` | Bug fix |
| `refactor` | Code change with no behavior change |
| `test` | Adding or updating tests only |
| `docs` | Documentation only |
| `chore` | Build, tooling, dependency updates |
| `perf` | Performance improvement |

**Examples:**

```
feat: add email notification after retention cleanup

fix: prevent null reference when user has no assigned org

refactor: extract RecordingArchiveService from controller

chore: upgrade EF Core to 9.0.4
```

### 2.3 Commit Message Rules [SCM-COMMIT-03]

- Subject line: 50 characters or fewer, imperative mood ("add" not "adds" or "added"), no period at the end
- Body: wrap at 72 characters, explain **why** the change was made (not what — the diff shows what)
- Reference the work item ID in the footer: `refs #12345` or `fixes #12345`
- Never commit merge commits on feature branches — rebase instead

### 2.4 Don't Commit [SCM-COMMIT-04]

Never commit:

- Secrets, API keys, or credentials (use environment variables or a secrets manager)
- Build artifacts, compiled output, or generated files (cover with `.gitignore`)
- IDE-specific files (`.vs/`, `.idea/`, `.DS_Store`) — use a global `.gitignore`
- `TODO` comments that should be tickets — open a ticket instead
- Commented-out code — remove it; version control is the history

---

## 3. Pull Requests [SCM-PR]

### 3.1 PR Size [SCM-PR-01]

Keep PRs small. A PR that touches more than 400 lines of net-new logic is usually too big. Large PRs get superficial reviews. If a feature requires a large change, stack smaller PRs that can be reviewed and merged independently.

The right size for a PR: a reviewer can hold the change in their head at once and give it the attention it deserves in 30–45 minutes.

### 3.2 PR Process [SCM-PR-02]

1. Branch off main (or develop), make your changes
2. Self-review your diff before opening the PR
3. Open the PR with a complete description (see [Code Review](code-review.md) for description standards)
4. Request review from at least one developer — senior changes require the Tech Lead
5. Address all feedback; re-request review when ready
6. Author merges after approval — not the reviewer

### 3.3 CI Requirements [SCM-PR-03]

No PR merges with a failing CI pipeline. The pipeline must pass on the latest commit at the time of merge. This is enforced via branch protection rules — bypasses are not permitted outside of declared emergencies.

The CI pipeline runs at minimum: build, lint, unit tests, and integration tests (where applicable).

### 3.4 Merge Strategy [SCM-PR-04]

We use **squash merges** by default. The squash commit message is the PR title (clean it up if it's not descriptive). This keeps the main branch history readable — one commit per PR.

Merge commits are used only for long-lived branch integration (e.g., release → main). Rebase merges are permitted for engineers who prefer them on personal branches but are not the standard.

---

## 4. Protected Branches [SCM-PROTECT]

### 4.1 Main Branch Rules [SCM-PROTECT-01]

The `main` branch (and `develop`, if used) has the following protections enforced in the repository settings:

- Require pull request before merging
- Require at least one approval
- Dismiss stale reviews when new commits are pushed
- Require CI status checks to pass
- Restrict who can push directly (Tech Lead and above for hotfixes only)

These rules are configured in the repository, not enforced by convention. Don't create repositories without them.

### 4.2 Hotfix Exception [SCM-PROTECT-02]

A genuine hotfix (production down or critical data issue) can bypass the standard review queue with Tech Lead approval. The bypass is temporary — the PR still exists, CI still runs, and a follow-up review happens within 24 hours of the merge.

Document the bypass and the reason in the PR description.

---

## 5. Repository Setup [SCM-REPO]

### 5.1 Every Repository Includes [SCM-REPO-01]

- `README.md` — setup instructions, how to run, how to test
- `.gitignore` — appropriate for the language and tooling
- `.editorconfig` — consistent formatting across editors
- CI/CD pipeline configuration (GitHub Actions, Azure Pipelines, or equivalent)
- Branch protection rules on main

### 5.2 Secrets and Environment Config [SCM-REPO-02]

Repository secrets (API keys, connection strings, credentials) are stored in the CI/CD secrets store or a secrets manager — never in the repository, never in environment files committed to source control. `.env` files are in `.gitignore`. If a `.env.example` is committed, it contains only key names, never values.

If a secret is accidentally committed, treat it as compromised immediately — rotate it before doing anything else.
