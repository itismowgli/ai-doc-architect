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
- Linear MCP available? If so, also check for **documents** (`search_documentation`), not just issues - URDs, PRDs, and feature briefs are usually Linear documents
- Docmost MCP available? If so, `list_spaces()` to see which spaces the authorising account can reach
- Confluence / Jira MCP available?
- Notion MCP available?
- GitHub MCP available?

Store: **available_mcps[]**, **spec_docs_found[]**

### 1g. Check for spec files in the repo, and read them

Look for:
- Files named PRD, URD, spec, requirements, brief, RFC in any format (`.md`, `.txt`, `.pdf`, `.docx`)
- A `docs/specs/`, `docs/prd/`, `docs/requirements/`, or `.planning/` directory
- A `CLAUDE.md` (acts as domain glossary)

**Open what you find.** Phase 2 has to name real document titles and say what each one
covers. "Spec files in repo: 3 found" tells the user nothing they can act on; "URD covering
participants and queries, last updated March" tells them whether it is worth using.

Store: **spec_files[]** with title, scope summary, and last-modified date

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
| Source | Document | Covers | Last updated |
|---|---|---|---|
| Linear (document) | "Study Manager URD" | Personas, participant workflows | 12 Mar |
| Linear (document) | "Query handling feature brief" | Queries only | 2 Aug |
| Docmost | space "Product", 14 pages under "Specs" | Mixed | 28 Aug |
| Repo | none found | | |

If nothing is found anywhere, say so explicitly - "no URD, PRD, or feature brief found in
the repo or in any connected tool" - rather than leaving the section blank. No spec is a
finding, not a default.

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

Name what you actually found, including document titles.

```
I can see [Linear is connected / Docmost space "[name]" / spec files in your repo].
Named documents I found: [list the URD, PRD, and feature brief titles]

Should I reconcile the documentation against these?

A  Yes - the specs define scope, terminology, and personas; the code defines
   behaviour. You get the manual plus a gap report of what was specified but
   not shipped (recommended)
B  No - generate from code only, skip the gap report
```

If multiple sources are available:
```
I found specs in more than one place. Which should I use?

A  Linear documents - [list titles: URD, PRD, feature brief]
B  Docmost - space "[name]", [N] pages under "[parent]"
C  Files in repo - [list filenames]
D  All of them
E  None - code only
```

If the user picks several and they disagree on a user-facing behaviour, the code
settles it and the conflict goes in the gap report. Within the spec tier: feature
brief beats PRD beats URD for a specific feature; URD beats both for personas and
terminology.

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

Then ask whether to publish, but only if a destination MCP is reachable:

```
I can also publish the finished manual to [Docmost / Confluence / Notion].

A  Yes - Markdown in the repo stays the source of truth, published as a page tree
B  No - just write the files
```

If Docmost and they say yes, confirm the space and the sharing model:

```
Two things:

1. Which space? I can create "[Product] User Guide" and "[Product] Admin Guide",
   or publish into spaces you already have.
2. The User Guide space can be shared publicly with subpages included, so one link
   covers the whole manual. The Admin Guide stays private. Is that what you want?
```

Store: **publish_to**, **publish_spaces[]**, **public_share** (true/false)

---

### Question 6: Screenshots

**Ask only if Phase 1 found a frontend** (a `package.json` with a dev server, a Blade or
Inertia view layer, or an existing Playwright or Cypress config). Skip it for API-only and
library repos.

```
I can capture screenshots by driving your running app with Playwright, so every image
is regenerated by a script instead of pasted in by hand.

A  Yes - the app runs at http://localhost:3000 with seeded data
B  Yes - but at a different URL (I'll type it)
C  No screenshots - words only
```

If A or B, also ask what you cannot infer:

```
Two things I need:

1. Login for each audience I detected ([audience list]). A storageState file, test
   credentials, or a seed command - whichever you already have.
2. Is the data seeded and stable? Screenshots of a shared dev environment produce a
   diff on every run, and a docs repo with noisy diffs stops getting reviewed.
```

If they have no seeded fixture environment, recommend C and move on. Capturing against
live data is how personal information ends up in a manual.

Store: **capture_url**, **capture_auth**, **capture_enabled**

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

  validation-report.md    (every claim, its evidence, its verdict, the commit SHA)
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
Apply Documentation Standards (`references/standards.md` Sections 20-22) to every file.

**Before writing any prose, run the validation pass.** List every factual claim the manual
would make and give each a verdict against code (`SKILL.md` Source Precedence and
Validation). Save it as `validation-report.md` with the commit SHA. A section may not be
written until every claim in it has a verdict. Verified claims become prose; Contradicted
claims become prose describing the code plus a gap report row; Unshipped and Unverified
claims never become prose.

Then generate in this order:
1. Root `index.md` (master TOC - can be written now since we know the full structure)
2. `01-introduction/` files (quick start, who-this-is-for)
3. Each module section, highest confidence first
4. Troubleshooting and reference files
5. Admin guide (if separate)
6. Screenshots, if **capture_enabled** - build `.captures/manifest.json` from the journey
   map, run the capture, embed each image after the step it confirms
   (see `references/screenshots.md`)
7. The plain-language pass over every file (`references/plain-language.md`) - this is the
   last edit before saving
8. `validation-report.md` and `inference-report.md`
9. `gap-report.md` (if spec reconciliation was selected)
10. Publish to **publish_to**, if set. Follow the two-pass transform in
    `references/publishing.md`: create every page first (parents before children),
    upload attachments, then rewrite cross-links, image paths, and callouts in memory
    and push content. One pass leaves dead links wherever a link points at a page
    created later in the walk. Persist the path-to-page-id map to
    `.publish/[destination].json` or the next run builds a duplicate tree. Never
    auto-delete a published page that no longer has a file.

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

### Validation
- [N] claims checked against commit [sha]
- Verified: [N] | Contradicted: [N] | Unshipped: [N] | Unverified: [N]
- Unverified claims were left out of the manual and listed in the inference report

### High confidence
- User Management: routes, tests, and policies all present
- Billing: routes and tests present

### Flagged for review
- Notifications: no tests found - verify steps are accurate

### Screenshots
- Captured: [N] images across [M] pages
- Failed: [shot id] on [page] - selector did not match, step is written in words only

### Terminology used
- "workspace" for [model: Organization] - update in index.md if incorrect
- "owner" for [role: ROLE_ADMIN] - verify this matches your UI labels

### Next steps
- Open docs/[product-name]/validation-report.md first - it shows what was left out and why
- Open docs/[product-name]/user-guide/index.md to review
- Run /gap-report if specs change in future to catch drift
- Commit docs/ folder alongside code so they version together
- Wire docs/[product-name]/.captures/capture.mjs into CI so a UI change fails the build
  instead of leaving a stale screenshot behind
```
