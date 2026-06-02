# Example 05: Output Showcase

What the generated files actually look like. Every required element rendered in full.

**Product:** Workspace - a fictional SaaS team collaboration tool.
**Feature:** Invite Members - chosen because every SaaS product has some version
of this workflow, so the pattern is universally transferable.

This is generated output. Not a template. Not a description of what would be
generated. The actual files.

---

## The folder structure

```
docs/workspace/user-guide/
  index.md
  01-introduction/
    index.md
    who-this-is-for.md
    quick-start.md
  02-getting-started/
    index.md
    create-your-first-project.md
  03-daily-workflows/
    index.md
    invite-members.md         ← shown in full below
    assign-tasks.md
    manage-notifications.md
  04-troubleshooting-and-reference/
    index.md
    troubleshooting.md
    glossary.md
    limits.md
    system-messages.md
```

---

## `docs/workspace/user-guide/index.md`

```markdown
# Workspace User Guide

Workspace is a team collaboration tool for managing projects, tasks, and members
in one place.

## How to use this guide

**New to Workspace:** Start with [Who This Guide Is For](./01-introduction/who-this-is-for.md),
then follow the [Quick Start](./01-introduction/quick-start.md). You will be set
up in under five minutes.

**Just got invited to a workspace:** Go straight to
[Create Your First Project](./02-getting-started/create-your-first-project.md).

**Looking for something specific:** Use the contents below or your portal's
search bar.

## Contents

### Introduction
- [Who This Guide Is For](./01-introduction/who-this-is-for.md)
- [Quick Start](./01-introduction/quick-start.md)

### Getting Started
- [Create Your First Project](./02-getting-started/create-your-first-project.md)

### Daily Workflows
- [Invite Members](./03-daily-workflows/invite-members.md)
- [Assign Tasks](./03-daily-workflows/assign-tasks.md)
- [Manage Notifications](./03-daily-workflows/manage-notifications.md)

### Troubleshooting and Reference
- [Troubleshooting](./04-troubleshooting-and-reference/troubleshooting.md)
- [Glossary](./04-troubleshooting-and-reference/glossary.md)
- [Limits and Constraints](./04-troubleshooting-and-reference/limits.md)
- [System Messages](./04-troubleshooting-and-reference/system-messages.md)
```

---

## `docs/workspace/user-guide/03-daily-workflows/invite-members.md`

This is one complete workflow file. Every required element is present.

```markdown
# Invite Members

You are here because you want to bring a colleague into a project so they can
see tasks, contribute work, or be assigned items.

You need to be the project owner or a workspace owner to send invitations. If
you do not see the Invite option described below, ask your workspace owner to
add the person for you — they can manage all members from the Admin panel. See
[Roles and Access](../../admin-guide/03-member-management/roles-and-access.md)
for the full permissions breakdown.

## Overview

Inviting a member sends them an email with a link to join the project. They
accept the invitation, create an account if they do not have one, and immediately
appear in your project member list. You can set their role before sending the
invitation or change it afterward.

Alternatively, you can share an invite link — a URL anyone with the link can
use to join the project. Invite links are useful for onboarding multiple people
at once without sending individual emails.

> **Scenario:** Your team just hired a designer. You create an account for them
> by sending an invitation to their work email. They accept, log in, and
> immediately see all the open design tasks assigned to them.

## Invite a member by email

**Before you begin:** You need the person's work email address. Personal email
addresses work but are not recommended for team projects.

1. Open the project you want to add them to.
2. Select **Members** in the left sidebar.
3. Select **Invite member**.
4. Enter the person's email address in the **Email** field.
5. Choose their role: **Contributor** (can create and edit tasks) or **Viewer**
   (read-only access).
6. Select **Send invitation**.

**What happens next:** An invitation email is sent immediately. It expires after
7 days. The person appears in your member list as **Pending** until they accept.

**If something goes wrong:**

- *"This email is already a member"* — the person is already in the project.
  Check the member list; they may appear under a different display name.
- *"Invitation failed to send"* — check the email address for typos and try
  again. If the problem continues, ask your workspace owner to check the domain
  allow-list in
  [Workspace Settings](../../admin-guide/02-workspace-configuration/workspace-settings.md).
- *"Invitation limit reached"* — you have 20 pending invitations. Cancel some
  before sending more. See [Resend or cancel an invitation](#resend-or-cancel-an-invitation).

## Share an invite link

An invite link lets anyone with the URL join the project directly, without a
personal email invitation.

1. Open the project and select **Members**.
2. Select **Invite member**.
3. Select the **Invite link** tab.
4. Select **Copy link**.
5. Share the link with your team via Slack, email, or any other channel.

**What happens next:** Anyone who opens the link and does not have a Workspace
account is prompted to create one. Anyone who already has an account is added
to the project immediately.

**If something goes wrong:**

- *"Invite links are disabled for this workspace"* — your workspace owner has
  turned off invite links. Use email invitations instead, or ask the owner to
  re-enable links in
  [Workspace Settings](../../admin-guide/02-workspace-configuration/workspace-settings.md).

## Resend or cancel an invitation

If a person has not accepted after a few days:

1. Open the project and select **Members**.
2. Find the person in the **Pending** list.
3. Select the three-dot menu next to their name.
4. Select **Resend invitation** or **Cancel invitation**.

**What happens next:** A resent invitation resets the 7-day expiry. A cancelled
invitation removes the pending entry and frees up one slot in your invitation
limit.

## Best practices

- Set the correct role before sending. Changing a role after the person joins
  requires returning to the Members list — it does not re-send an email.
- If you are inviting several people to the same project, the invite link is
  faster than sending individual emails.
- If a person accepts but reports they cannot see certain tasks, their role may
  need adjusting. See
  [Manage Member Roles](./manage-member-roles.md).
- Revoke invite links when a hiring process ends or a contractor engagement
  closes. An active link can still be used by anyone who received it.

## Limits

| Item | Limit |
|---|---|
| Members per project | 50 |
| Pending invitations per project | 20 |
| Invitation expiry | 7 days |
| Invitations sent per hour | 10 |
| Active invite links per project | 1 |

## System messages

| Message | What it means |
|---|---|
| "Invitation sent" | Email delivered successfully |
| "This email is already a member" | Address is already in the project member list |
| "Invitation failed to send" | Delivery error — check the address and try again |
| "Invitation limit reached" | 20 pending invitations exist — cancel some first |
| "Invite links are disabled" | Workspace owner has turned off link-based invites |
| "Link copied" | Invite link is now in your clipboard |

## What to do next

- [Assign Tasks](./assign-tasks.md) — give the new member work immediately
- [Manage Notifications](./manage-notifications.md) — let them know how to
  configure alerts so they do not miss task assignments
- [Manage Member Roles](./manage-member-roles.md) — change a member's role
  after they have joined
- [Return to Daily Workflows](./index.md)
```

---

## What every element maps to in the code

| Element | Source |
|---|---|
| "project owner or workspace owner" | `MemberPolicy.php` — `invite()` method |
| Two invite paths (email + link) | Routes: `POST /invitations` and `POST /invitations/link` |
| 7-day expiry | `InvitationRequest.php` - config value `invitation.expiry_days` |
| 20 pending limit | `InvitationPolicy.php` - `canCreate()` guard |
| 10 per hour rate limit | `InvitationController.php` - throttle middleware |
| 50 members per project | `ProjectPolicy.php` - `memberLimit` constant |
| "Invitation failed to send" toast | `resources/lang/en/messages.php` |
| Domain allow-list reference | `AdminController.php` - workspace settings |

The documentation standards say: translate every technical identifier to plain
language. No field names, no controller names, nothing from the code. Only the
translated result appears in the output.

---

## How this renders in a docs portal

The folder structure maps to sidebar navigation automatically:

```
Workspace User Guide
  Introduction
    Who This Guide Is For
    Quick Start
  Getting Started
    Create Your First Project
  Daily Workflows
    Invite Members          ← the file above
    Assign Tasks
    Manage Notifications
  Troubleshooting and Reference
    Troubleshooting
    Glossary
    Limits and Constraints
    System Messages
```

Mintlify, Docusaurus, GitBook, and Confluence all read the folder structure and
build this sidebar automatically. No configuration file needed.

Every section has its own URL. Search returns the exact file. Git diffs show
only the changed file. Two people can edit different sections without conflicts.
