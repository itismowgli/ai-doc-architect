# Example 6: Existing Codebase, Zero Documentation

## The Most Common Scenario

A team has been shipping for 18 months. The product works. Users love it.
The documentation does not exist.

No one sat down to write docs. Every new hire spends their first two weeks asking
senior engineers how things work. Support tickets repeat the same five questions every
week. The sales team cannot self-serve answers during demos. The CEO asks for a user
manual before a key enterprise deal and nobody knows where to start.

This is the starting point for most ADUMAS deployments. This example shows how to
go from zero to a complete, production-ready user manual for any existing codebase in a
single structured run.

---

## What You Have (Any Stack, Any Domain)

This pattern applies regardless of technology:

| Your stack | ADUMAS reads |
|---|---|
| Laravel / PHP | Routes, Controllers, FormRequests, Models, Policies, Pest tests, Blade templates |
| Node.js / Express | Routes, controllers, middleware, validators, Jest/Mocha tests |
| Django / Python | urls.py, views.py, serializers.py, permissions.py, pytest files |
| Rails / Ruby | routes.rb, controllers, models, policies (Pundit/CanCanCan), RSpec files |
| Spring / Java | Controllers, Services, DTOs, SecurityConfig, JUnit tests |
| Next.js / React | API routes, page components, middleware, auth config |
| Any stack | If it has routes, validation, permissions, and tests - ADUMAS can read it |

The inputs do not change. The output is always Markdown.

---

## The Four-Step Pattern

### Step 1: Inventory the product

Before writing a single word, understand what the product actually is.

```
Read the route file(s) and group endpoints by resource or feature area.
each group becomes one section in the manual.

Example inventory from a route file:

Feature Area          Endpoints                           Section
------------------------------------------------------------------------
Authentication        POST /login, POST /logout,          Section 3: Getting Started
                      POST /register, POST /password
User Management       GET/POST/PUT/DELETE /users          Section 4: Managing Users
Team & Members        GET/POST /teams, /invitations       Section 5: Teams and Members
Billing               GET/POST /subscriptions, /invoices  Section 6: Billing
Notifications         GET/PUT /notifications, /settings   Section 7: Notifications
Admin                 GET /admin/*, /reports              Section 8: Administration
```

Treat this inventory as the manual's table of contents before writing anything.

### Step 2: Read permissions before writing anything

The most common documentation mistake is writing steps that only some users can perform
without saying so. Read the permission/policy layer first and build the permission matrix
before touching feature sections.

```
For each resource in your inventory:
  - Read the policy or permission class
  - Map every action: who can do it, under what conditions
  - Build one row in the permission matrix per action

This matrix becomes Section 2 (Roles and Permissions) and informs every
subsequent section - which steps to show to which audience, which warnings
to include, which actions to mark as admin-only.
```

### Step 3: Read tests as specification

Feature tests are the most precise specification of behavior in any codebase. They
are written to fail if behavior changes. They document edge cases the PRD forgot.
They describe the exact error that fires when a validation fails.

```
For every test file:
  - Each it() / test() / describe() block = one documented behavior
  - Assertions on error messages = exact error text to document
  - Setup conditions = prerequisites to list in the manual
  - Data setup = field constraints and defaults

A test that says:
  it('rejects invitation when team is at member limit')
  
becomes a troubleshooting entry:
  Symptom: "Invite member" button is disabled
  Cause: Team has reached its member limit
  Resolution: Upgrade your plan or remove an inactive member first
```

### Step 4: Generate one section at a time

Do not try to generate the entire manual in one pass. Generate section by section.
Each section is self-contained and follows the Documentation Standards structure:

```
For each section:
  1. Identify the relevant controllers, models, policies, and tests
  2. Read them
  3. Write: overview -> scenario -> tasks (step-by-step) -> outcomes ->
     error states -> best practices -> limits table -> system messages table
  4. Review against the completeness checklist before moving on
```

---

## What Gets Generated

For a typical SaaS product with 8 feature areas, ADUMAS produces:

```
docs/
  user-manual/
    00-about-this-manual.md
    01-what-is-[product].md
    02-roles-and-permissions.md
    03-getting-started.md
    04-[feature-area-1].md
    05-[feature-area-2].md
    06-[feature-area-3].md
    07-[feature-area-4].md
    08-[feature-area-5].md
    09-troubleshooting.md
    10-reference.md          <- glossary, limits, notifications, system messages
```

Every file is Markdown. Drop the folder into Docusaurus, Mintlify, GitBook, Notion,
or Confluence. No reformatting required.

---

## The Gap Report

When specs exist (in Linear, Confluence, Jira, or any tool), the reconciliation step
produces a gap report alongside the manual:

```
docs/
  user-manual/
    [all manual sections]
  gap-report.md              <- features specified but not shipped
                             <- features shipped but not specified
                             <- terminology mismatches between spec and code
                             <- fields in code with no validation documented
```

The gap report drives the follow-up work. Gaps become Linear/Jira issues automatically
via MCP connector if configured.

---

## Common Mistakes to Avoid

**Starting with writing instead of reading.** Read all the code first. Build the
permission matrix. Build the feature inventory. Only then open a blank document.

**Generating the whole manual at once.** One section at a time produces better output.
A 10,000-word generation run loses context halfway through. A focused section run
is focused and checkable.

**Skipping the tests.** Tests are not optional reading. They contain the most precise
behavioral specification in the entire codebase. A manual written without reading the
tests will miss edge cases, error messages, and boundary conditions.

**Writing for the average user.** There is no average user. Write for each role
separately. The admin guide is not the user guide with more steps. It is a different
document for a different person with different goals.

**Treating the first draft as the deliverable.** Run the completeness checklist.
A manual that passes the checklist is a deliverable. A manual that does not is a draft.

---

## See Also

- `examples/05-prd-to-manual.md` - when you have specs to reconcile against code
- `examples/07-output-showcase.md` - what production-ready output actually looks like
- `references/architecture.md` - Laravel/PHP-specific ingestion sequence
- `references/writing-guide.md` - Section 20: the full Documentation Standards definition
