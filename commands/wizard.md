---
description: Guided documentation setup. Scans the codebase silently first, then asks a short series of smart, context-aware questions derived from what it actually found - modules, roles, connected tools, existing docs - before generating anything. Every question is specific to this codebase, never generic.
---

# Documentation Wizard

You are running a guided documentation session. The rule is: **scan everything first,
ask questions second.** Every question you ask must be derived from what you actually
found in the code. Never ask a generic question when a specific one is possible.

---

## Phase 1: Silent deep scan

Do not ask anything yet. Read the codebase and build a complete picture.

### 1a. Identify the product

Read in this order:
- `package.json` → `name` field
- `composer.json` → `name` field
- `pyproject.toml` or `setup.py` → package name
- Top-level namespace or module name from route files
- Repo directory name as the last fallback

Store: **product_name**

### 1b. Discover modules from routes

Read all route/router files. Group routes by resource prefix or controller.

```
For Laravel:    routes/web.php, routes/api.php
For Rails:      config/routes.rb
For Django:     all urls.py files
For Express:    files in routes/ or all router() calls in app.js
For FastAPI:    all APIRouter includes in main.py
For Next.js:    pages/ or app/ directory - each folder = one module
```

For each module record:
- Plain-language name (translate `UserController` → "User Management")
- Route count
- Dominant verb pattern: GET-heavy (read/view), POST-heavy (create/action), mixed
- Whether it has admin-only routes (prefixed `/admin`, behind admin middleware)

Store: **modules[]** with name, route_count, verb_pattern, is_admin_only

### 1c. Map audiences from the permission layer

Read authorization files:

```
For Laravel:    app/Policies/, Gate::define(), $middleware groups in Kernel.php
For Rails:      Pundit policy files, CanCanCan Ability.rb, before_action :authorize
For Django:     permission_classes on ViewSets, has_permission methods
For Express:    middleware functions named isAdmin, requireRole, checkPermission, etc.
For any stack:  constants/enums named ROLE_, Permission_, or similar
```

Build a list of distinct roles. For each role record:
- Role name (as it appears in code)
- What they can do (broad: full access, create/edit only, read only)
- Which modules they can access

Store: **roles[]** with name, access_level, modules[]

### 1d. Read tests for confidence mapping

Scan test files. For each module record:
- Test file exists: yes / no
- Test count: 0, low (1-5), medium (6-20), high (20+)

This becomes the confidence tier for each module.
High test coverage = generate with high confidence.
No tests = generate with low confidence, flag for human review.

Store: **module_confidence{}** keyed by module name

### 1e. Check for existing documentation

Look for:
- A `docs/` directory at any level
- A `README.md` with substantial content (more than 20 lines)
- A `CHANGELOG.md`
- Any `.md` files in the repo root

Store: **existing_docs** - true/false, and list of files found

### 1f. Check which spec tools are reachable

Silently check which MCP connectors are active in this session:
- Linear MCP available?
- Confluence / Jira MCP available?
- Notion MCP available?
- GitHub MCP available?

Store: **available_mcps[]**

### 1g. Check for spec files in the repo

Look for:
- Files named PRD, URD, spec, requirements in any format (`.md`, `.txt`, `.pdf`)
- A `docs/specs/` or `docs/prd/` directory
- A `CLAUDE.md` (acts as domain glossary)

Store: **spec_files[]**

---

## Phase 2: Present what you found

Before any questions, show the user a clear summary of the scan. This is not a question.
It anchors the conversation so every following question makes sense.

```
## What I found in [product_name]

**Modules ([count] total)**
| Module | Routes | Type | Test coverage |
|---|---|---|---|
| User Management | 12 | Mixed | High (24 tests) |
| Billing | 8 | Action-heavy | Medium (9 tests) |
| Notifications | 4 | Read-heavy | None |

**Audiences detected from permission layer**
| Role | Access level | Modules |
|---|---|---|
| Owner | Full access | All |
| Member | Create/edit | Projects, Tasks |
| Guest | Read only | Projects (invited only) |

**Spec sources found**
- Linear MCP: connected
- Confluence MCP: not connected
- Spec files in repo: none

**Existing docs**
- docs/ directory: not found
- README.md: present (brief, not a manual)
```

Then say: "Based on this, I have a few questions before I start generating."

---

## Phase 3: Guided questions

Ask these in sequence. Each question is conditional - skip it if the answer is obvious
from the scan. Do not ask all questions if some are unnecessary.

---

### Question 1: Scope

**Skip if:** user already said "everything" or "the whole platform" or there is only 1 module.

**Ask:**

```
I found [N] modules. What do you want documented?

A  All [N] modules
B  Core modules only (most-used based on route count): [top 3-4 by route count]
C  Let me pick - [show module list as checkboxes]
```

Use actual module names. Not "module 1, module 2." Not a generic list.

If the user picks C, present the actual module list with route counts so they can make an informed choice.

Store: **selected_modules[]**

---

### Question 2: Audiences

**Skip if:** only one role found, or all routes are open (no auth layer).

**Ask using actual role names found in the code:**

```
I found [N] roles in your permission layer: [Owner, Member, Guest].

How do you want to handle audiences?

A  Separate guide per audience - Owner guide, Member guide (recommended if roles 
   have significantly different workflows)
B  One combined guide with role callouts - a single manual that says "Owners can 
   also..." where relevant (recommended if roles are similar)
C  User-facing only - document what Members and Guests can do; skip Owner/admin content
D  Admin-facing only - document Owner and admin workflows only
```

If only 2 roles exist and one is clearly "admin" and one is clearly "user", default
to A (separate guides) and say so.

Store: **audience_strategy** (separate | combined | user-only | admin-only)

---

### Question 3: Spec reconciliation

**Skip if:** no MCPs available AND no spec files found in repo.

**Only ask if** at least one spec source is reachable. Use the actual source names.

```
I can see [Linear is connected / spec files in your repo / both].
Should I reconcile the documentation against your specs?

A  Yes - read specs, compare against code, generate the manual + a gap report 
   showing what was specified but not shipped (recommended)
B  No - generate from code only, skip the gap report
```

If multiple sources are available:
```
I found specs in multiple places. Which should I use?

A  Linear (connected) - [N epics visible]
B  Files in repo - [list filenames]
C  Both
D  Neither - code only
```

Store: **use_specs** (true/false), **spec_sources[]**

---

### Question 4: Confidence handling

**Skip if:** all modules have high test coverage.

**Only ask if** one or more modules have no or low test coverage.

```
[N] modules have low or no test coverage:
- Notifications (0 tests)
- [Module] (2 tests)

For these, I'll need to infer behavior from controller code alone, which is less reliable.

A  Generate anyway, flag low-confidence sections with a warning callout
B  Skip low-coverage modules for now, I'll add them later
C  I'll give you context - ask me about each one before generating it
```

Store: **low_coverage_strategy** (generate-with-flags | skip | ask-me)

---

### Question 5: Output location

**Always ask this one.** Show a smart default based on what exists.

If `docs/` directory already exists:
```
I found an existing docs/ folder. Where should I save the new documentation?

A  docs/[product-name]/ (add alongside existing docs)
B  docs/[product-name]-v2/ (keep old docs intact while generating new)
C  Custom path - I'll type it
```

If no docs directory exists:
```
Where should I save the documentation?

A  docs/[product-name]/ (recommended - standard location)
B  Custom path - I'll type it
```

Store: **output_path**

---

## Phase 4: Pre-generation preview

Before writing a single file, show the user exactly what will be created.

```
## Here is what I will generate

docs/[product-name]/
  user-guide/
    index.md
    01-introduction/
      who-this-is-for.md
      quick-start.md
    02-user-management/
      index.md
      invite-members.md
      manage-roles.md
    03-billing/
      index.md
      upgrade-plan.md
      manage-payment-method.md
    04-troubleshooting-and-reference/
      troubleshooting.md
      glossary.md
      limits.md
      system-messages.md

  admin-guide/
    index.md
    01-introduction/
      admin-responsibilities.md
    02-user-management/
      roles-and-access.md
      remove-members.md
    03-troubleshooting-and-reference/
      audit-log.md

  inference-report.md     (confidence flags and terminology choices)
  gap-report.md           (only if spec reconciliation was selected)

Total: [N] files across [N] folders
Estimated generation: [N] minutes
```

Then ask one final confirmation:

```
Ready to generate. Anything you want to change before I start?

A  Looks good - generate
B  Change the output path
C  Remove a module from scope
D  Start over with different settings
```

Only proceed when the user confirms A.

---

## Phase 5: Generate

Generate the documentation following the folder structure shown in the preview.
Apply Documentation Standards (writing-guide.md Sections 20-22) to every file.

Generate in this order:
1. Root `index.md` (master TOC - can be written now since we know the full structure)
2. `01-introduction/` files (quick start, who-this-is-for)
3. Each module section, highest confidence first
4. Troubleshooting and reference files
5. Admin guide (if separate)
6. `inference-report.md`
7. `gap-report.md` (if spec reconciliation was selected)

Report progress as you go:
```
Generated: 01-introduction/ (3 files)
Generated: 02-user-management/ (4 files)
...
```

---

## Phase 6: Completion summary

```
## Done

Generated [N] files in [output_path]

### High confidence
- User Management: routes, tests, and policies all present
- Billing: routes and tests present

### Flagged for review
- Notifications: no tests found - verify steps are accurate

### Terminology used
- "workspace" for [model: Organization] - update in index.md if incorrect
- "owner" for [role: ROLE_ADMIN] - verify this matches your UI labels

### Next steps
- Open docs/[product-name]/user-guide/index.md to review
- Run /gap-report if specs change in future to catch drift
- Commit docs/ folder alongside code so they version together
```
