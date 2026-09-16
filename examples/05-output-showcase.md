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
    img/
      invite-members-01-members-panel.png
      invite-members-02-invite-dialog.png
      invite-members-03-pending-state.png
  04-troubleshooting-and-reference/
    index.md
    troubleshooting.md
    glossary.md
    limits.md
    system-messages.md

docs/workspace/.captures/
  manifest.json               ← one entry per screenshot, derived from the journey map
  capture.mjs                 ← regenerates every image above
  auth/
    member.json
    admin.json

docs/workspace/
  validation-report.md        ← every claim, its evidence, its verdict, the commit SHA
  inference-report.md         ← what could not be verified, and why
  gap-report.md               ← specified but not shipped
```

Three images for a whole workflow, not one per step. Every other step is complete in
words, which is the test a step has to pass before it is allowed a picture.

---

## `docs/workspace/user-guide/index.md`

```markdown
# Workspace User Guide

Workspace is a team collaboration tool for managing projects, tasks, and members
in one place.

## How to use this guide

**New to Workspace:** Start with [Who this guide is for](./01-introduction/who-this-is-for.md),
then follow the [Quick start](./01-introduction/quick-start.md). You will be set
up in under five minutes.

**Just got invited to a workspace:** Go straight to
[Create your first project](./02-getting-started/create-your-first-project.md).

**Looking for something specific:** Use the contents below or your portal's
search bar.

## Contents

### Introduction
- [Who this guide is for](./01-introduction/who-this-is-for.md)
- [Quick start](./01-introduction/quick-start.md)

### Getting started
- [Create your first project](./02-getting-started/create-your-first-project.md)

### Daily Workflows
- [Invite members](./03-daily-workflows/invite-members.md)
- [Assign tasks](./03-daily-workflows/assign-tasks.md)
- [Manage notifications](./03-daily-workflows/manage-notifications.md)

### Reference
- [Troubleshooting](./04-troubleshooting-and-reference/troubleshooting.md)
- [Glossary](./04-troubleshooting-and-reference/glossary.md)
- [Limits and constraints](./04-troubleshooting-and-reference/limits.md)
- [System messages](./04-troubleshooting-and-reference/system-messages.md)
```

Headings are sentence case (Section 24, rule 21). The product name and the fixed PART
names keep their capitals; nothing else does.

---

## `docs/workspace/user-guide/03-daily-workflows/invite-members.md`

This is one complete workflow file. Every required element is present.

```markdown
# Invite members

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

   ![The Members panel showing the current member list and the Invite member button in the top right](./img/invite-members-01-members-panel.png)

3. Select **Invite member**.
4. Enter the person's email address in the **Email** field.
5. Choose their role: **Contributor** (can create and edit tasks) or **Viewer**
   (read-only access).

   ![The invite dialog with an email field, a role dropdown set to Contributor, and a Send invitation button](./img/invite-members-02-invite-dialog.png)

6. Select **Send invitation**.

**What happens next:** An invitation email is sent immediately. It expires after
7 days. The person appears in your member list as **Pending** until they accept.

![The Members list with a new row marked Pending and a resend icon](./img/invite-members-03-pending-state.png)

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

## `docs/workspace/validation-report.md`

Written before a word of the manual existed. Every claim above traces to a row here.

```markdown
# Validation Report

Validated against: `a4f9c21` on 2026-09-14
Specs read: Linear document "Workspace URD" (+ 4 child pages),
            Linear document "Invitations feature brief"

| Claim | Source | Code evidence | Verdict |
|---|---|---|---|
| Invitations expire after 7 days | PRD §4.2 | config/invitation.php:12 | Verified |
| 20 pending invitations per project | (none) | InvitationPolicy.php:41 | Verified |
| 50 members per project | URD §2.4 | ProjectPolicy.php:18 | Verified |
| 10 invitations per hour | (none) | InvitationController.php:9 throttle | Verified |
| Roles are Contributor and Viewer | URD §2.4 | MemberRole enum:3 | Verified |
| One active invite link per project | (none) | InviteLink migration: unique index | Verified |
| Owners can bulk-invite by CSV | URD §3.1 | no route, no controller | Unshipped |
| Invite emails retry 3 times | brief §2 | SendInvite.php:28 retries twice | Contradicted |
| Invite links expire | (none) | no expiry constant, no test | Unverified |

## What this produced

- **Verified (6)** → written as prose in the manual
- **Contradicted (1)** → the manual says the retry behaviour the code implements;
  the conflict is row 1 of the gap report
- **Unshipped (1)** → CSV bulk invite appears nowhere in the manual. Gap report only,
  tracked as WS-402
- **Unverified (1)** → the manual says invite links can be revoked, and says nothing
  about them expiring. Listed in the inference report for a human to pin down
```

Look at the last two rows, because they are the ones that decide whether the manual can be trusted.

The URD promised CSV bulk invite. It reads like a shipped feature and it would have written itself into a clean, plausible section. No route implements it, so it does not exist in the manual at all — a reader who followed it would have hunted for a button that was never built.

Invite link expiry is subtler. Nothing in the code pins it. The tempting sentence is "invite links expire after a period of time", which reads like documentation and tells the reader nothing they can plan around. It is not written. It is flagged, and a human decides whether the answer is "they don't" or "nobody implemented it yet."

---

## The manifest that produced those three images

`docs/workspace/.captures/manifest.json`, abbreviated to the first shot:

```json
{
  "baseUrl": "http://localhost:3000",
  "viewport": { "width": 1280, "height": 800 },
  "deviceScaleFactor": 2,
  "theme": "light",
  "personas": {
    "owner": { "storageState": ".captures/auth/admin.json" }
  },
  "shots": [
    {
      "id": "invite-members-01-members-panel",
      "page": "03-daily-workflows/invite-members.md",
      "persona": "owner",
      "step": 2,
      "alt": "The Members panel showing the current member list and the Invite member button in the top right",
      "goto": "/projects/seed-project/members",
      "clip": { "selector": "[data-testid='members-panel']" },
      "mask": ["[data-testid='member-email']", "[data-testid='avatar']"]
    }
  ]
}
```

Two things are doing the real work here.

`mask` blacks out the email column and the avatars before the pixels are written. A
writer would never type a real address into a manual; a capture script will photograph
one without noticing.

`clip.selector` crops to the members panel rather than the full 1280x800 window. When the
sidebar gets a new item next quarter, this image does not change, so it does not appear
in the diff, so nobody has to review it.

The whole run is reproducible: `node docs/workspace/.captures/capture.mjs`. Wired into CI
on every frontend change, a renamed `data-testid` fails the build on the pull request
that renamed it. That is the entire point. The stale screenshot is the most common defect
in software documentation and the only one that never announces itself.

Full mechanics: `references/screenshots.md`.

---

## The plain-language pass

The draft that came out of generation was complete and unreadable. Section 24 is the
last edit before a file is saved. Here is what it changed on this page.

| Rule | Draft | Shipped |
|---|---|---|
| 13 Inflated significance | "Inviting members is a crucial part of your Workspace journey, unlocking seamless collaboration across your team." | Deleted. The page now opens with why the reader is here |
| 4 Run-up before the point | "Let's dive into the two ways you can bring someone onboard!" | "Inviting a member sends them an email with a link to join the project." |
| 12 Stock words | "Simply enter their email and Workspace will effortlessly handle the rest." | "Enter the person's email address in the **Email** field." |
| 9 Stacked qualifiers | "The invitation will typically expire after approximately a week or so." | "It expires after 7 days." |
| 19 Bold as decoration | "The **invitation** is sent to their **email address** and expires after **7 days**." | Bold kept only on **Email**, **Send invitation**, **Pending** - the things on screen |
| 26 Heading repeated in first sentence | "## Invite a member by email" + "This section explains how to invite a member by email." | The heading does the work; the section starts at **Before you begin** |
| 2 Closer that repeats | "And that's all there is to inviting a member!" | Ends on the **What to do next** links |
| 25 Knowledge-limit disclaimer | "While the exact limit is unclear, there appears to be a cap on pending invitations." | "20" in the limits table, read from `InvitationPolicy.php` |

Rule 9 and rule 25 are the two that matter most. Both are the same failure: a number the
generator did not look up, written as prose so the gap does not show. "Approximately a
week" reads like documentation and tells the reader nothing they can plan around. If the
value cannot be found in the source, it belongs in the inference report where a human
will see it, never in the manual dressed as a fact.

What survived the pass is as important as what went. Second person, bold on every UI
label, the entry-point trigger at the top, an outcome statement after each task, every
error case, every limit, and the "What to do next" connectors. A page that reads well but
lost the 7-day expiry is worse than the draft it replaced.

---

## How this renders in a docs portal

The folder structure maps to sidebar navigation automatically:

```
Workspace User Guide
  Introduction
    Who this guide is for
    Quick start
  Getting started
    Create your first project
  Daily Workflows
    Invite members          ← the file above
    Assign tasks
    Manage notifications
  Reference
    Troubleshooting
    Glossary
    Limits and constraints
    System messages
```

Mintlify, Docusaurus, GitBook, and Confluence all read the folder structure and
build this sidebar automatically. No configuration file needed.

Every section has its own URL. Search returns the exact file. Git diffs show
only the changed file. Two people can edit different sections without conflicts.
