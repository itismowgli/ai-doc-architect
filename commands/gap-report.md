---
description: Compare existing documentation against the current codebase and identify what is stale, missing, or wrong. Produces a prioritised gap report without regenerating the whole manual.
---

# Gap Report

You are auditing existing documentation against the current state of the codebase. You are not rewriting docs. You are finding what needs to be updated and prioritising the work.

## Step 1: Locate the existing docs

Ask the user:
- **Where are the existing docs?** (path to the docs folder, or paste the doc directly)
- **Where is the codebase?** (current directory or specify a path)
- **When were the docs last updated?** (if known - helps prioritise)

## Step 2: Read the codebase

Read the route files, controllers, models, and feature tests. Build a current map of:
- All endpoints that exist today
- All actions that are available per role
- All validation rules and constraints currently enforced
- All system messages and notifications currently sent

## Step 3: Read the existing documentation

For each documented section, check:
- Does the documented endpoint/feature still exist in the code?
- Has the behavior changed? (compare steps to current controller logic)
- Are the permission rules still accurate? (compare to current policy files)
- Are the error messages still correct? (compare to current validation and test assertions)
- Are the limits still accurate? (compare to current config and validation)
- Are there new features in the code that are not yet documented?

## Step 4: Produce the gap report

```markdown
# Documentation Gap Report
Generated: [date]
Docs reviewed: [path]
Codebase scanned: [path/commit]

## Summary
- Stale sections: N (documented behavior no longer matches code)
- Missing sections: N (features in code with no documentation)
- Outdated limits: N (limits have changed)
- Removed features: N (documented features no longer exist)

## Stale sections (update these first)
| Section | File | Issue | Severity |
|---|---|---|---|
| Invite a Member | teams/invite-members.md | Role dropdown now has 4 options, not 3 | High |
| ...

## Missing sections (add these next)
| Feature | Where it belongs | Evidence |
|---|---|---|
| Bulk invite via CSV | teams/invite-members.md | BulkInviteController added in recent migration |
| ...

## Removed features (delete these)
| Section | File | Why |
|---|---|---|
| Import from Slack | integrations/slack.md | SlackImportController removed |
| ...

## Outdated limits
| Section | Current doc says | Code says now |
|---|---|---|
| Team name length | 50 characters | 80 characters (changed in validation) |
| ...

## Recommended update order
1. [Highest severity stale sections - these are actively misleading users]
2. [Missing sections for features users are likely using]
3. [Outdated limits]
4. [Removed feature cleanup]
```

## Step 5: Offer to fix

After producing the gap report, ask the user:
- "Would you like me to update the stale sections now?"
- "Should I generate the missing sections?"
- "Do you want to create tracked issues for each gap?"

Do not automatically update anything. The gap report is the deliverable. Updates require explicit confirmation.
