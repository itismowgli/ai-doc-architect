# Example 7: Book Standard Showcase

This example shows what ADUMAS actually produces. Not a description of the process.
The output itself.

The product is "Workspace" - a fictional SaaS team collaboration tool. The feature
documented here is Team and Member Management. This is one of the most universal
features in SaaS - every product that has teams, roles, and invitations produces
documentation that looks exactly like this.

Read this to understand what the Book Standard means in practice. If this is the
quality you want for your product, install the skill and run it.

---

## What Was Generated From

- **Routes:** 14 endpoints across `/teams`, `/members`, `/invitations`
- **Spec source:** 1 Confluence PRD page + 8 Linear issues
- **Tests read:** 34 Pest feature tests across 3 test files
- **Time to generate:** One focused chapter-by-chapter run

---

# GENERATED OUTPUT BEGINS HERE

---

# Chapter 5: Teams and Members

> **Who this chapter is for:** All users - Workspace Owners, Admins, and Members.
>
> **What you will be able to do:** Create and configure teams, invite members, manage
> roles, and handle member access across your workspace.
>
> **Before you start:** You need an active Workspace account. To create a team, you
> need the Workspace Owner or Admin role. To invite members, you need the Team Owner
> or Admin role for that specific team.

---

## Overview

Teams are how Workspace organises people around shared projects, departments, or
functions. A team has its own members, its own projects, and its own permission
settings. A person can belong to multiple teams with different roles in each.

When you invite someone to a team, they receive an email with a link. They accept or
decline. If they accept, they join with the role you assigned. If they do not respond
within 7 days, the invitation expires and you can resend it.

Every action in this chapter is logged in the audit trail. Team owners can view the
full history of membership changes from the Team Settings page.

---

## Create a Team

> **Scenario:** You are the head of design at a growing startup. Your team has grown
> to eight people and projects are getting lost in the shared workspace. You need a
> dedicated design team where only your people can see design projects.

### Before you begin

- You need the Workspace Owner or Admin role.
- Have the team name and a brief description ready. You can change both later.

### Steps

1. Click **Teams** in the left sidebar.
2. Click **New team** in the top-right corner.
3. Enter a team name (maximum 50 characters).
4. Optionally enter a description. This appears on the team's profile page and in
   invitation emails so invitees understand what they are joining.
5. Choose the team's visibility:
   - **Visible** - anyone in the workspace can see the team exists and request to join.
   - **Secret** - the team is hidden from workspace discovery. Only invited members
     can see it.
6. Click **Create team**.

### What happens next

You are taken directly to the new team's page as its Owner. The team appears in your
Teams list immediately. No other workspace members are notified that the team was created.

### If something goes wrong

**"Team name already exists."** Team names must be unique within the workspace. Choose
a different name or add a suffix (e.g. "Design - EMEA").

**"You do not have permission to create teams."** Your workspace role is Member, not
Owner or Admin. Contact your Workspace Owner to upgrade your role.

### Best practices

- Name teams after their function, not their current membership. "Backend Engineering"
  ages better than "Tom's Team."
- Use the description to explain what projects belong to this team. New members read
  it before accepting invitations.
- Start with Secret visibility for sensitive teams (Finance, Legal, Executive). You
  can change it to Visible later.

### Limits

| Item | Limit | Notes |
|---|---|---|
| Team name length | 50 characters | Shown in truncated form in sidebar if over 30 |
| Teams per workspace | 100 | Contact support to increase |
| Members per team | 250 | Applies to all paid plans |
| Pending invitations per team | 50 | Expired invitations do not count toward this |

---

## Invite a Member

> **Scenario:** You hired a new designer, Aisha. She starts Monday. You need to add
> her to the Design team and the Brand team before her first day so her projects are
> ready when she logs in.

### Before you begin

- You need the Team Owner or Team Admin role for the team you are inviting to.
- Have Aisha's work email address ready. She will receive the invitation at that address.
- Decide which role to assign her. See the Roles and Permissions section below.

### Steps

1. Open the team you want to invite to.
2. Click **Members** in the team navigation.
3. Click **Invite member**.
4. Enter the invitee's email address.
5. Select their role from the dropdown:
   - **Member** - standard access. Can view and contribute to team projects.
   - **Admin** - can manage team settings and invite or remove members.
   - **Owner** - full control. Can delete the team and transfer ownership.
6. Optionally add a personal message. This appears in the invitation email.
7. Click **Send invitation**.

### What happens next

The invitee receives an email with the subject line:
*"[Your Name] invited you to join [Team Name] on Workspace"*

The email includes your personal message (if added), the team description, and an
**Accept invitation** button. The button is valid for 7 days.

The invitee appears in the Members list under **Pending** until they accept or the
invitation expires.

### If something goes wrong

**"This person is already a member."** The email address matches an existing team member.
Check the Members list - they may be listed under a different display name.

**"Invitation limit reached."** The team has 50 pending invitations outstanding.
Wait for existing invitations to be accepted or expire, or resend only to people who
are likely to accept.

**"This email domain is not allowed."** Your workspace has domain restrictions enabled.
Contact your Workspace Owner to add the domain, or invite using the person's work email.

### Best practices

- Invite people before their start date. Invitation acceptance and project access
  take immediate effect, so inviting on day one wastes the first morning.
- Use the personal message to explain why you are inviting them and what they will
  find in the team. It increases acceptance rates.
- Assign the correct role at invitation time. Changing a role after joining requires
  a separate step and generates a notification.

---

## Accept or Decline an Invitation

> **Scenario:** Aisha receives the invitation email on Friday. She clicks the link
> from her phone to make sure she can access Workspace before Monday.

### Steps

1. Open the invitation email.
2. Click **Accept invitation**.
3. If you do not have a Workspace account yet, you are prompted to create one.
   If you already have an account, you are taken directly to the team.
4. The team appears in your Teams list immediately.

**To decline:** Click **Decline invitation** in the email. You are not added to the team.
The person who invited you is not notified that you declined.

### If something goes wrong

**"This invitation has expired."** Invitations expire after 7 days. Ask the team owner
to resend the invitation from the Members page.

**"This invitation is no longer valid."** The invitation was cancelled by the team owner,
or you already accepted it from another device. If you cannot see the team in your list,
ask the team owner to send a new invitation.

---

## Manage Member Roles

> **Scenario:** You promoted Aisha to lead designer after three months. She now manages
> junior designers and needs to invite new members to the team.

### Before you begin

You need the Team Owner role to change another member's role. Team Admins can change
Member roles but cannot change other Admin or Owner roles.

### Steps

1. Open the team.
2. Click **Members**.
3. Find the member whose role you want to change.
4. Click the role badge next to their name (e.g. **Member**).
5. Select the new role from the dropdown.
6. Click **Confirm**.

### What happens next

The role changes immediately. The member receives an in-app notification:
*"Your role in [Team Name] has been updated to [New Role]."*

The change is recorded in the team's audit log with your name, the old role, and the
new role.

### Limits and rules

- Only one Owner per team. Assigning Owner to another member transfers ownership to
  them and demotes you to Admin.
- You cannot change your own role. Ask another Owner or a Workspace Admin.
- Workspace Admins can change any member's role in any team regardless of their team role.

---

## Remove a Member

> **Scenario:** A contractor finished their engagement. They should no longer have
> access to your team's projects.

### Before you begin

- You need Team Owner or Team Admin role.
- Removing a member is immediate and permanent. Their contributions (comments, files,
  project edits) remain. Only their access is removed.

### Steps

1. Open the team.
2. Click **Members**.
3. Find the member you want to remove.
4. Click the **...** menu next to their name.
5. Click **Remove from team**.
6. Confirm in the modal.

### What happens next

The member loses access to the team and all its projects immediately, including any
currently open tabs. They receive an email notification:
*"You have been removed from [Team Name]."*

Their name is removed from the Members list. Their past contributions remain visible
with their name attached.

**To restore access:** You must send a new invitation. There is no undo for member removal.

### If something goes wrong

**Cannot remove the last Owner.** Assign a new Owner first, then remove yourself or
the other person.

---

## Roles and Permissions

### What each role can do

| Action | Member | Admin | Owner | Workspace Admin |
|---|---|---|---|---|
| View team projects | Yes | Yes | Yes | Yes |
| Create projects in team | Yes | Yes | Yes | Yes |
| Invite new members | No | Yes | Yes | Yes |
| Remove members | No | Yes | Yes | Yes |
| Change member roles | No | Members only | Yes | Yes |
| Edit team name and description | No | Yes | Yes | Yes |
| Change team visibility | No | No | Yes | Yes |
| Delete the team | No | No | Yes | Yes |
| Transfer team ownership | No | No | Yes | No |
| View audit log | No | Yes | Yes | Yes |

### Case-level roles vs team-level roles

Your team role applies to everything inside the team. Some integrations and project
types add their own permission layer on top of the team role. When in doubt, the more
restrictive permission applies.

---

## Best Practices

**Assign the minimum role needed.** Most people need Member, not Admin. Admin access
means they can invite external users and remove colleagues. Reserve it for people
who genuinely manage team membership.

**Review pending invitations weekly.** Invitations that sit for 7 days expire and
clutter the pending list. If someone has not accepted after 3 days, follow up directly
before the link expires.

**Use Secret visibility for sensitive teams.** Finance, Legal, Executive, and any team
handling confidential data should be Secret by default. You can always change to Visible
later - going the other direction requires removing members who discovered the team
through search.

**Offboard promptly.** Remove members the day they leave, not the day you remember to.
An access audit once a month catches stragglers. The audit log shows exactly when each
person last accessed the team.

**Never share invitation links.** Invitation links are unique to the invitee's email.
They cannot be reused. If someone forwards the link, the second person cannot use it.

---

## System Messages and Toasts

| Trigger | Message | Shown as |
|---|---|---|
| Member accepts invitation | "[Name] joined [Team Name]." | System message in team feed |
| Member declines invitation | "[Name] declined the invitation." | Visible only to team admins |
| Member removed | "[Name] was removed from [Team Name] by [Admin Name]." | System message in team feed |
| Role changed | "[Name]'s role was changed from [Old] to [New] by [Admin Name]." | System message in team feed |
| Invitation sent | "Invitation sent to [email]." | Toast (sender only) |
| Invitation resent | "Invitation resent to [email]." | Toast (sender only) |
| Invitation cancelled | "Invitation cancelled." | Toast (sender only) |
| Team created | "Team created successfully." | Toast |
| Team deleted | "Team [Name] was permanently deleted." | Toast + email to all members |
| Ownership transferred | "You are now the owner of [Team Name]." | In-app notification to new owner |

---

## Related Topics

- [Chapter 2: Roles and Permissions](./02-roles-and-permissions.md) - full workspace-level
  permission matrix
- [Chapter 9: Troubleshooting](./09-troubleshooting.md) - invitation not received,
  cannot remove member, role change not taking effect
- [Chapter 10: Reference](./10-reference.md) - all team and member limits in one table

---

# GENERATED OUTPUT ENDS HERE

---

## What Made This Output Possible

Every element above came directly from the codebase and spec - nothing was invented.

| Output element | Source |
|---|---|
| The 7-day invitation expiry | Migration: `add_expiry_to_invitations_table.php` + console command `ProcessExpiredInvitations` |
| The 50-character team name limit | FormRequest: `CreateTeamRequest` validation rule `max:50` |
| The 250-member limit | Config: `teams.max_members = 250` |
| The 50-pending-invitations limit | Policy: `InvitationPolicy::create()` - count check before allow |
| Every error message | Feature tests: assertions on response body JSON error strings |
| The permission matrix | Policy files: `TeamMemberPolicy`, `TeamPolicy` - each method mapped to a row |
| System messages | Event listeners + notification classes - each class = one row in the table |
| Email subject line | Mail class: `MemberInvitedMail::$subject` |
| "Contributions remain, access removed" | Feature test: `it('preserves messages after member removal')` |
| The ownership transfer note | Policy: `TeamPolicy::transferOwnership()` - only Owner can call it |

**This is the Book Standard in practice.** Every section has a scenario, prerequisites,
numbered steps, an outcome, error states, best practices, a limits table, and a system
messages table. Nothing is missing. A real user can follow it without asking anyone for help.

---

## Try It On Your Product

To generate this quality for your own product, use the ai-doc-architect skill:

```bash
npx skills add itismowgli/ai-doc-architect
```

Then describe your module, point to your spec sources and codebase, and ask for a
Book Standard user manual. The skill reads your code, reconciles it against your specs,
and produces output like this.
