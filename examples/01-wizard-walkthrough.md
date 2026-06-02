# Example 01: /wizard Walkthrough

A full session from start to finish. What the wizard asks, why it asks it, and
what comes out the other side.

**Product:** Nexus - a fictional SaaS project management tool. Node.js + Express
backend, React frontend. 18 months old. GitHub repo, Linear for issues, no
existing documentation.

**Trigger:** Developer runs `/wizard` with no other context.

---

## The prompt

```
/wizard
```

That is the complete prompt.

---

## Phase 1: Silent scan (no output yet)

The wizard reads the codebase without printing anything. Here is what it finds:

**From `routes/` files:**
- `/api/workspaces` - 6 endpoints (CRUD + archive)
- `/api/members` - 8 endpoints (invite, roles, remove, pending)
- `/api/projects` - 11 endpoints (CRUD + archive + duplicate)
- `/api/tasks` - 14 endpoints (CRUD + assign + status + comments)
- `/api/notifications` - 3 endpoints (list, mark-read, preferences)
- `/api/billing` - 7 endpoints (plan, upgrade, payment method, invoices)
- `/api/admin/users` - 5 endpoints (all behind `/admin` prefix)
- `/api/admin/audit` - 3 endpoints (all behind `/admin` prefix)

**From middleware/auth.js and policies/:**
- Three roles: `OWNER`, `MEMBER`, `GUEST`
- OWNER: all endpoints
- MEMBER: workspaces, projects, tasks, notifications (own resources only)
- GUEST: read-only access to invited projects

**From `__tests__/` directory:**
- Workspaces: 18 tests
- Members: 22 tests
- Projects: 31 tests
- Tasks: 29 tests
- Notifications: 4 tests
- Billing: 14 tests
- Admin: 9 tests

**From `package.json`:** `"name": "nexus-api"`

**MCP check:** Linear connected. GitHub connected. Confluence: not connected.

**Existing docs:** `README.md` present (42 lines, setup instructions only). No `docs/` folder.

---

## Phase 2: Findings summary

The wizard prints:

```
## What I found in Nexus

**Modules (7 total)**

| Module | Routes | Type | Test coverage |
|---|---|---|---|
| Tasks | 14 | Mixed | High (29 tests) |
| Projects | 11 | Mixed | High (31 tests) |
| Members | 8 | Action-heavy | High (22 tests) |
| Workspaces | 6 | Mixed | High (18 tests) |
| Billing | 7 | Action-heavy | Medium (14 tests) |
| Notifications | 3 | Read-heavy | Low (4 tests) |
| Admin (Users + Audit) | 8 | Mixed | Medium (9 tests) |

**Audiences detected from permission layer**

| Role | Access level | Modules |
|---|---|---|
| OWNER | Full access | All |
| MEMBER | Create/edit own resources | Workspaces, Projects, Tasks, Notifications |
| GUEST | Read-only | Invited projects only |

**Spec sources**

- Linear: connected (I can search for relevant epics)
- GitHub: connected (I can read merged PRs)
- Confluence: not connected
- Spec files in repo: none found

**Existing docs**

- README.md: present (setup only, not a user manual)
- docs/ folder: not found

---

Based on this, I have a few questions before I start generating.
```

---

## Phase 3: Questions

### Question 1: Scope

```
I found 7 modules. What do you want documented?

A  All 7 modules
B  Core modules only (Tasks, Projects, Members, Workspaces)
C  Let me pick
```

**User answers:** A

---

### Question 2: Audiences

```
I found 3 roles: OWNER, MEMBER, GUEST.

OWNER has full access including admin features (user management, audit log).
MEMBER and GUEST are end users with different permission levels.

How do you want to handle this?

A  Two separate guides: User Guide (MEMBER + GUEST workflows) and Admin Guide
   (OWNER-only features) — recommended, roles are meaningfully different
B  One combined guide with role callouts ("Owners can also...")
C  User Guide only — skip OWNER/admin content for now
D  Admin Guide only
```

**User answers:** A

---

### Question 3: Spec reconciliation

```
Linear is connected. Should I check for relevant epics before generating?

A  Yes — search Linear for epics related to these modules, reconcile against
   code, include a gap report
B  No — generate from code only
```

**User answers:** B  *(they just want the manual fast)*

---

### Question 4: Notifications module

```
The Notifications module has only 4 tests. I can generate it, but behavior
will be inferred from controller code, not tests. It will be less reliable.

A  Generate it anyway, flag low-confidence sections with a warning
B  Skip it for now, add it later
C  Tell me about it and I'll fill in the gaps
```

**User answers:** A

---

### Question 5: Output location

```
No existing docs/ folder found. Where should I save the documentation?

A  docs/nexus/  (recommended)
B  Custom path
```

**User answers:** A

---

## Phase 4: Pre-generation preview

```
## Here is what I will generate

docs/nexus/
  user-guide/
    index.md
    01-introduction/
      index.md
      who-this-is-for.md
      quick-start.md
    02-workspaces/
      index.md
      create-a-workspace.md
      manage-workspace-settings.md
      archive-a-workspace.md
    03-projects/
      index.md
      create-a-project.md
      duplicate-a-project.md
      archive-a-project.md
    04-tasks/
      index.md
      create-a-task.md
      assign-a-task.md
      update-task-status.md
      comment-on-a-task.md
    05-members/
      index.md
      invite-members.md
      manage-member-roles.md
      remove-a-member.md
    06-notifications/
      index.md
      manage-notification-preferences.md  [LOW CONFIDENCE]
    07-troubleshooting-and-reference/
      index.md
      troubleshooting.md
      glossary.md
      limits.md
      system-messages.md

  admin-guide/
    index.md
    01-introduction/
      index.md
      admin-responsibilities.md
    02-user-management/
      index.md
      manage-users.md
    03-audit-log/
      index.md
      reading-the-audit-log.md
    04-billing/
      index.md
      manage-subscription.md
      update-payment-method.md
      download-invoices.md
    05-troubleshooting-and-reference/
      index.md
      troubleshooting.md
      audit-log-reference.md

  inference-report.md

Total: 43 files across 16 folders

Anything you want to change before I start?

A  Looks good — generate
B  Change output path
C  Remove a module
D  Start over
```

**User answers:** A

---

## Phase 5: Generation (progress output)

```
Generating docs/nexus/...

  user-guide/index.md
  user-guide/01-introduction/ (3 files)
  user-guide/02-workspaces/ (4 files)
  user-guide/03-projects/ (4 files)
  user-guide/04-tasks/ (5 files)
  user-guide/05-members/ (4 files)
  user-guide/06-notifications/ (2 files) [low confidence flagged]
  user-guide/07-troubleshooting-and-reference/ (5 files)

  admin-guide/index.md
  admin-guide/01-introduction/ (2 files)
  admin-guide/02-user-management/ (2 files)
  admin-guide/03-audit-log/ (2 files)
  admin-guide/04-billing/ (4 files)
  admin-guide/05-troubleshooting-and-reference/ (2 files)

  inference-report.md
```

---

## Phase 6: Completion summary

```
## Done — 43 files generated in docs/nexus/

**High confidence** (routes + tests + policies all present)
- Workspaces, Projects, Tasks, Members, Billing, Admin

**Flagged for review**
- Notifications: 4 tests only — verify that manage-notification-preferences.md
  accurately describes the preference options. A warning callout appears at the
  top of that file.

**Terminology choices made**
- "workspace" used for model: Organization — verify this matches your UI
- "owner" used for role: OWNER — verify this matches what the UI calls it
- "task" used for model: Issue — your code uses Issue internally but the UI
  label appears to be Task based on route names

**Next steps**
- Open docs/nexus/user-guide/index.md and scan the master TOC
- Check inference-report.md for the full confidence breakdown
- Commit docs/ alongside your code — it versions with every change
- Run /gap-report after your next sprint to catch any drift
```

---

## Why this example matters

The wizard asked 5 questions. It did not ask:
- "What framework are you using?" (read it from package.json)
- "What are your modules?" (read from routes)
- "Who are your users?" (read from policies)
- "Is there an existing docs folder?" (checked the filesystem)

Every question was about a real choice - scope, audience strategy, spec reconciliation,
confidence handling, output location. The user answered in seconds. The wizard did the
rest.

Total time from `/wizard` to 43 generated files: one short conversation.
