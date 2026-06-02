# Example 02: /autodoc — Zero Specs, Infer Everything

The most direct path from codebase to documentation. No PRDs, no issue trackers,
no context provided. The skill reads the code and figures everything out.

**Product:** Finlo - a fictional fintech SaaS, Laravel + PHP backend, 2 years old,
zero documentation. The team is preparing for an enterprise sales call and needs a
user manual in 24 hours.

---

## The prompt

```
/autodoc .
```

The entire prompt. Current directory. No flags.

---

## What it ingested

### From `routes/api.php`

```
Route::prefix('accounts')->group(...)         → Module: Account Management
Route::prefix('transactions')->group(...)     → Module: Transactions
Route::prefix('transfers')->group(...)        → Module: Transfers
Route::prefix('payees')->group(...)           → Module: Payees
Route::prefix('statements')->group(...)       → Module: Statements
Route::prefix('cards')->group(...)            → Module: Cards
Route::prefix('admin')->middleware('admin')   → Admin module (role-gated)
```

### From `app/Policies/`

```
AccountPolicy.php   → roles: account_owner, account_member, viewer
TransactionPolicy.php → same role structure
CardPolicy.php      → account_owner only for card management
AdminPolicy.php     → admin role only
```

### From `tests/Feature/`

```
AccountTest.php       34 tests
TransactionTest.php   41 tests
TransferTest.php      28 tests
PayeeTest.php         19 tests
StatementTest.php     11 tests
CardTest.php          22 tests
AdminTest.php         17 tests
```

### From `app/Http/Requests/`

Validation rules translated to plain-language field documentation. Example:

```php
// TransferRequest.php
'amount'          => 'required|numeric|min:0.01|max:999999.99'
'payee_id'        => 'required|exists:payees,id'
'scheduled_date'  => 'nullable|date|after:today'
'memo'            => 'nullable|string|max:255'
```

Becomes: "Amount (required), Payee (required), Schedule for a future date (optional),
Memo (optional, 255 characters max)"

---

## What was generated

```
docs/finlo/
  user-guide/
    index.md
    01-introduction/
      index.md
      who-this-is-for.md
      quick-start.md
    02-accounts/
      index.md
      add-an-account.md
      manage-account-members.md
      account-settings.md
    03-transactions/
      index.md
      view-transactions.md
      filter-and-search.md
      export-transactions.md
    04-transfers/
      index.md
      send-a-transfer.md
      schedule-a-transfer.md
      cancel-a-transfer.md
    05-payees/
      index.md
      add-a-payee.md
      edit-a-payee.md
      remove-a-payee.md
    06-statements/
      index.md
      download-a-statement.md
    07-cards/
      index.md
      view-card-details.md
      freeze-or-unfreeze-a-card.md
      report-a-lost-card.md
    08-troubleshooting-and-reference/
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
      suspend-an-account.md
    03-troubleshooting-and-reference/
      index.md
      audit-log.md

  inference-report.md

Total: 38 files
```

---

## The inference report

Every `/autodoc` run produces `inference-report.md` alongside the documentation.
This is what it contained for Finlo:

```markdown
# Inference Report — Finlo

## High confidence (routes + tests + policies present)

- Account Management: AccountTest.php (34 tests) covers create, update, member
  invite, and deactivation. All paths tested.
- Transactions: 41 tests, strong behavioral coverage.
- Transfers: 28 tests including scheduled transfer and cancellation flows.
- Cards: 22 tests including freeze/unfreeze and loss reporting.

## Medium confidence (fewer than 15 tests)

- Payees: 19 tests — standard CRUD covered but edge cases may be missing.
  Verify: what happens when you try to delete a payee with pending transfers?

- Statements: 11 tests — download flow covered but date range filtering
  not explicitly tested. Verify steps in download-a-statement.md.

## Terminology choices made

The following translations were applied. Verify these match your UI labels:

| Code term | Used in docs | Where to check |
|---|---|---|
| `account_owner` | "Account owner" | Roles description in who-this-is-for.md |
| `account_member` | "Account member" | Same |
| `payee` | "Payee" | Payees section - verify label in UI |
| `scheduled_date` | "Schedule for a future date" | send-a-transfer.md step 4 |
| `memo` field | "Memo" | Same |

## Could not infer

- Notification system: no routes found for notifications. Either not yet built
  or handled entirely on the frontend. No documentation generated.
- Two-factor authentication: referenced in AdminTest.php but no routes or
  policy found. Skipped.

## Recommended next step

Read docs/finlo/user-guide/04-transfers/send-a-transfer.md first - it is the
most complex workflow and the most likely to need human verification.
```

---

## What `/autodoc` does not do

It does not know what you intended to build - only what is built. If a feature
exists in a PRD but was never shipped, it will not appear in the documentation
(and will not appear in the inference report - that is what `/prd-to-manual` is for).

If your codebase has a feature with no tests and no clear route naming, the
documentation for it will be less accurate. The inference report flags these.

Use `/prd-to-manual` when you want reconciliation against specs.
Use `/gap-report` after you have docs to catch drift.
Use `/autodoc` when you just want docs from code, fast.
