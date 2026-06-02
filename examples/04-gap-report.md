# Example 04: /gap-report — Catching Documentation Drift

Documentation written today is accurate today. Three sprints later, a button
moved, a field was renamed, a workflow gained a new step. The docs say the old
thing. Nobody updated them.

`/gap-report` finds everything that has drifted between the current code and
the existing documentation.

**Product:** Nexus (same product as Example 01). Documentation was generated 6
weeks ago. Three sprints have shipped since then.

---

## The prompt

### Basic - full audit

```
/gap-report

Codebase: .
Existing docs: docs/nexus/
```

### Targeted - after a specific sprint

```
/gap-report

Codebase: .
Existing docs: docs/nexus/
Scope: everything merged in the last 3 weeks
Compare against: GitHub merged PRs since 2026-05-12
```

---

## What it compared

The command read the existing documentation files and compared them against the
current code. It also fetched merged PRs from GitHub to understand what changed.

**Merged PRs in scope:**

```
PR #201  Tasks: added subtask support (new endpoint, new UI section)
PR #208  Members: invite link flow (replaces email-only invite)
PR #214  Billing: new "pause subscription" option added to plan page
PR #219  Tasks: "blocked by" dependency field added
PR #225  Workspaces: archived workspaces now browsable (was permanent)
PR #231  Notifications: preference UI completely redesigned
```

---

## The gap report output

```markdown
# Gap Report — Nexus
Generated: 2026-06-03
Docs scanned: docs/nexus/ (43 files)
PRs compared: GitHub merged 2026-05-12 to 2026-06-03 (6 PRs)

---

## Priority 1 — Stale steps (will mislead users right now)

### invite-members.md
**What changed (PR #208):** Invite flow now has two paths: email invite (existing)
and invite link (new). Users can copy a shareable link from the Members page.
**What the doc says:** Only email invite described.
**Fix needed:** Add "Share an invite link" as a second task block. Update the
overview paragraph to mention both paths.
**File:** docs/nexus/user-guide/05-members/invite-members.md

### manage-subscription.md
**What changed (PR #214):** Plan page now has a "Pause subscription" option
alongside Upgrade and Cancel.
**What the doc says:** Only Upgrade and Cancel described.
**Fix needed:** Add "Pause your subscription" task block. Add pause to the
system messages table. Update limits table (pause available once per 12 months).
**File:** docs/nexus/admin-guide/04-billing/manage-subscription.md

---

## Priority 2 — Missing documentation (feature shipped, no docs)

### Subtasks (PR #201)
**What shipped:** Full subtask system under tasks. New endpoint `POST /tasks/:id/subtasks`.
Tests: 18 new test cases.
**Existing docs:** No mention of subtasks anywhere.
**Fix needed:** New file `docs/nexus/user-guide/04-tasks/add-subtasks.md`.
Update create-a-task.md to reference subtasks. Update limits.md (max 20 subtasks).
Update system-messages.md with subtask-related toasts.

### Task dependencies — "blocked by" field (PR #219)
**What shipped:** Tasks can be marked as blocked by other tasks. New field
visible on task detail view.
**Existing docs:** No mention.
**Fix needed:** New file `docs/nexus/user-guide/04-tasks/mark-task-as-blocked.md`.
Update update-task-status.md to describe behavior when a task is blocked.

---

## Priority 3 — Partially outdated (accurate but incomplete)

### manage-notification-preferences.md
**What changed (PR #231):** Notification preferences UI redesigned. Granular
per-event toggles replaced the old "mute all / mute mentions" model.
**What the doc says:** Describes old mute model. Steps reference UI elements
that no longer exist.
**Fix needed:** Full rewrite of manage-notification-preferences.md. The
existing content will mislead users if they follow the steps.
**File:** docs/nexus/user-guide/06-notifications/manage-notification-preferences.md

### Archived workspaces (PR #225)
**What changed:** Archiving a workspace is now reversible. Archived workspaces
appear in a new "Archived" section and can be restored.
**What the doc says:** archive-a-workspace.md implies archiving is permanent.
**Fix needed:** Update the "What happens next" section. Add a "Restore an archived
workspace" task block.

---

## No action needed

The following modules had no PRs in scope and passed a spot-check against current code:
- Accounts (Workspaces create/settings)
- Projects
- Admin — user management
- Admin — audit log
- All glossary and limits files (except the subtask addition noted above)

---

## Summary

| Priority | Items | Action |
|---|---|---|
| P1 — Stale steps | 2 files | Fix before next publish |
| P2 — Missing docs | 2 new files needed | Create new files |
| P3 — Partially outdated | 2 files | Rewrite or update |
| No action needed | 37 files | Current |
```

---

## Running gap-report regularly

The most useful cadence: run `/gap-report` at the end of every sprint before
releasing. It takes seconds and surfaces exactly what needs updating.

A documentation pipeline (see `references/architecture.md`) can run this
automatically on every merge to main and post the results to Slack or Linear
as a documentation debt tracker.

### Patch prompt after reading the gap report

Once you have the gap report, patch each file individually:

```
The invite-members.md section in docs/nexus/user-guide/05-members/ is out of date.

PR #208 added an invite link flow. Users can now either:
1. Send an email invite (existing flow)
2. Copy a shareable invite link from the Members page

Update invite-members.md to add the invite link as a second task block.
Do not change the existing email invite steps.
```

This keeps changes surgical - one file, one change, reviewable diff.
