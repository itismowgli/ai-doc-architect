# Example 03: /prd-to-manual — Spec Reconciliation

What happens when you have specs in one place and code in another and they have
drifted. The command reads both, documents what actually shipped, and produces a
gap report for everything that did not.

**Product:** Relay - a fictional SaaS customer messaging tool. Django + Python.
PRD lives in Confluence. Acceptance criteria in Linear. Code in GitHub.

---

## The prompt

```
/prd-to-manual

Codebase: github.com/relay-hq/relay
Specs:
  - Confluence: space RELAY, page "Inbox v2 - Product Requirements"
  - Linear: epic REL-204 "Inbox v2"
Output: docs/relay/
```

---

## What it fetched

### From Confluence — "Inbox v2 - Product Requirements"

Extracted:
- Goal: unified inbox combining email, chat, and SMS into a single view
- User stories: 11 stories covering assignment, snooze, merge threads, bulk actions
- Out of scope: voice calls, social media channels

### From Linear — epic REL-204

Extracted 18 issues with acceptance criteria. Key ones:

```
REL-210  Assign conversation to team member
         AC: owner or assigned agent can reassign; notification sent to new assignee

REL-211  Snooze a conversation
         AC: snooze for 1h / 4h / tomorrow / custom date; reappears at snooze time

REL-212  Merge duplicate threads from same contact
         AC: merge up to 5 threads; merged threads show combined history

REL-215  Bulk close conversations
         AC: select up to 50 conversations; close all with one action

REL-218  Custom inbox views
         AC: filter by channel, assignee, tag, status; save as named view
         [STATUS: in progress - not merged]

REL-221  Conversation tags
         AC: add up to 10 tags per conversation; filter by tag in inbox
```

### From GitHub — merged PRs in last 60 days matching REL-*

```
PR #412  REL-210: conversation assignment  [merged]
PR #419  REL-211: snooze workflow           [merged]
PR #431  REL-212: thread merge             [merged - partial: max 3 threads, not 5]
PR #445  REL-215: bulk actions             [merged - close only, no bulk assign]
PR #447  REL-221: conversation tags        [merged]
REL-218: no merged PR found               [not shipped]
```

---

## Three-source reconciliation

The command compares spec intent, acceptance criteria, and code reality:

| Feature | Spec | Code | Status |
|---|---|---|---|
| Assign conversation | REL-210 | PR #412 | Shipped as specified |
| Snooze conversation | REL-211 | PR #419 | Shipped as specified |
| Merge threads | REL-212 | PR #431 | Shipped - max 3 (spec said 5) |
| Bulk close | REL-215 | PR #445 | Shipped - close only (spec included bulk assign) |
| Conversation tags | REL-221 | PR #447 | Shipped as specified |
| Custom inbox views | REL-218 | none | Not shipped |
| Bulk assign | part of REL-215 | none | Not shipped |

---

## What was generated

### User manual folder

```
docs/relay/
  user-guide/
    index.md
    01-introduction/
      index.md
      who-this-is-for.md
      quick-start.md
    02-inbox/
      index.md
      assign-a-conversation.md
      snooze-a-conversation.md
      merge-threads.md
      bulk-close-conversations.md
    03-tags/
      index.md
      add-tags-to-a-conversation.md
      filter-by-tag.md
    04-troubleshooting-and-reference/
      index.md
      troubleshooting.md
      glossary.md
      limits.md
      system-messages.md
```

The manual documents only what shipped. The merge threads file says "up to 3 threads"
(not 5 as the PRD stated). The bulk actions file documents close only (not assign).
Custom inbox views does not appear - it did not ship.

### Gap report

```
docs/relay/gap-report-inbox-v2.md
```

Contents:

```markdown
# Gap Report — Inbox v2

Generated: 2026-06-03
Spec source: Confluence RELAY / "Inbox v2 - Product Requirements"
Acceptance criteria: Linear epic REL-204

---

## Not shipped (in spec, not in code)

### Custom inbox views (REL-218)
**Spec:** Users can filter inbox by channel, assignee, tag, status and save as
named views.
**Code:** No merged PR. Linear issue status: In Progress.
**Impact:** High - mentioned in 3 user stories as a primary workflow.
**Recommended action:** Create Linear issue to track documentation as a
follow-up once REL-218 ships.

### Bulk assign conversations (part of REL-215)
**Spec:** Bulk action bar includes "Assign to..." option.
**Code:** PR #445 merged bulk close only. Assign button not present.
**Impact:** Medium.
**Recommended action:** Confirm with engineering if bulk assign is deferred
or cancelled.

---

## Shipped with differences from spec

### Merge threads (REL-212)
**Spec:** Merge up to 5 threads.
**Code (PR #431):** Maximum 3 threads.
**Doc impact:** merge-threads.md documents the 3-thread limit.
**Recommended action:** Update Linear REL-212 acceptance criteria to reflect
the shipped limit, or track as a known limitation.

---

## In code, not in spec

### Conversation priority flag
**Found in:** PR #445 - `POST /conversations/bulk` accepts a `priority` field.
**Spec coverage:** None found in Confluence or Linear.
**Doc impact:** Not documented.
**Recommended action:** Confirm if this is intentional or a side effect of
the bulk actions PR. If intentional, add to a future spec and document.

---

## Summary

| Category | Count |
|---|---|
| Not shipped | 2 items |
| Shipped with differences | 1 item |
| In code, not in spec | 1 item |
| Shipped as specified | 5 items |
```

---

## Key rules this demonstrates

**Document what shipped, not what was specified.** `merge-threads.md` says
"up to 3 threads" even though the PRD said 5. The gap report tracks the
discrepancy - the manual does not.

**Never document an unshipped feature.** Custom inbox views has a full PRD
entry and a Linear issue. It is not in the manual. It is in the gap report.

**Surface what code has that specs do not.** The priority flag in PR #445
was not in the spec. The gap report flags it. The team can decide whether
to document it, spec it retroactively, or remove it.
