---
description: Compare existing documentation against the current codebase and identify what is stale, missing, or wrong. Produces a prioritised gap report without regenerating the whole manual.
---

# Gap Report

You are auditing existing documentation against the current state of the codebase. You are not rewriting docs. You are finding what needs to be updated and prioritising the work.

This command is re-validation. A claim verified six months ago is an assumption today, so
nothing in the existing docs is trusted because it was once correct.

## Step 1: Locate the existing docs and their evidence

Ask the user:
- **Where are the existing docs?** (path to the docs folder, or paste the doc directly)
- **Where is the codebase?** (current directory or specify a path)

Then look for `validation-report.md` beside the docs.

**If it exists**, you have the previous claim ledger and the commit SHA it was validated
against. Diff that commit against HEAD to narrow the search to what actually changed, then
re-verify every claim - including the ones that changed to Unverified because the constant
they cited was deleted.

**If it does not exist**, the docs were written without an evidence trail. Say so, then
build the ledger from scratch by extracting every factual claim from the existing prose.
This is slower and it is the main reason to generate a validation report in the first
place.

## Step 2: Read the codebase

Read the route files, controllers, models, and feature tests. Build a current map of:
- All endpoints that exist today
- All actions that are available per role
- All validation rules and constraints currently enforced
- All system messages and notifications currently sent

## Step 3: Re-validate every claim in the existing docs

Extract every factual claim from the existing prose - limits, timeouts, expiries, role
restrictions, required fields, error messages, state names, defaults, orderings - and give
each one a fresh verdict with a current file reference. Same verdicts as generation:
Verified, Contradicted, Unshipped, Unverified.

A claim that is now **Unverified** is as serious as one that is **Contradicted**: the
manual states something nobody can confirm, which is how a wrong number survives three
releases.

Alongside the claim check, for each documented section:
- Does the documented endpoint/feature still exist in the code?
- Has the behavior changed? (compare steps to current controller logic)
- Are the permission rules still accurate? (compare to current policy files)
- Are the error messages still correct? (compare to current validation and test assertions)
- Are the limits still accurate? (compare to current config and validation)
- Are there new features in the code that are not yet documented?
- Do the screenshots still match? Run `.captures/capture.mjs` if it exists. A selector that
  no longer matches means both the image and the step text that names that element are
  stale (see `references/screenshots.md`).

## Step 4: Produce the gap report

```markdown
# Documentation Gap Report
Generated: [date]
Docs reviewed: [path]
Codebase scanned: [path/commit]

## Summary
- Claims re-validated: N against commit [sha]
- Verified: N | Contradicted: N | Unshipped: N | Unverified: N
- Stale sections: N (documented behavior no longer matches code)
- Missing sections: N (features in code with no documentation)
- Removed features: N (documented features no longer exist)
- Screenshots broken: N (selector no longer matches)

## Contradicted claims (fix these first - the manual is actively wrong)
| Claim in docs | File | Code says now | Evidence |
|---|---|---|---|
| Invitations expire after 7 days | teams/invite-members.md | 14 days | config/invitation.php:12 |
| ...

## Unverified claims (nobody can confirm these)
| Claim in docs | File | Why it cannot be verified |
|---|---|---|
| Invite links expire | teams/invite-members.md | No expiry constant, no test, no config |
| ...

## Stale sections (behaviour changed, not just one value)
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

## Recommended update order
1. [Contradicted claims - the manual is actively misleading users]
2. [Removed features still documented - users follow steps that cannot work]
3. [Unverified claims - either pin the value in code or remove the claim]
4. [Missing sections for features users are likely using]
5. [Broken screenshots]
```

## Step 5: Offer to fix

After producing the gap report, ask the user:
- "Would you like me to update the stale sections now?"
- "Should I generate the missing sections?"
- "Do you want to create tracked issues for each gap?"

Do not automatically update anything. The gap report is the deliverable. Updates require explicit confirmation.

If the user accepts the fixes, write the refreshed ledger back to `validation-report.md`
with the new commit SHA, so the next audit starts from evidence rather than from prose.
