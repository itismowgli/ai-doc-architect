---
description: Inventory a codebase and produce a documentation plan - folder structure, section list, audience map, and effort estimate - before writing a single word. Use this to align with the team before generating full documentation.
---

# Scan Codebase

You are inventorying a codebase to produce a documentation plan. You are not writing any documentation yet. You are producing the plan that will guide the generation run.

## Step 1: Identify the scope

Ask the user:
- **What to scan:** the entire platform, or a specific module/area
- **Codebase location:** current directory or a specific path
- **Audiences:** who will read these docs? (end users, admins, developers, support)

## Step 2: Sweep for specs

Before reading code, search for a URD, PRD, and feature brief - in the repo
(`docs/specs/`, `docs/prd/`, `.planning/`, files named URD, PRD, spec, requirements,
brief) and in any connected MCP (Linear **documents** as well as issues, Docmost spaces,
Confluence, Jira, Notion).

Open what you find. The plan needs to say which sections have a spec behind them and which
will be inferred from code alone, because those two carry different confidence and the
team should know which is which before approving the plan.

If nothing is found, say so explicitly. No spec is a finding, not a default.

## Step 3: Read the route files

Read the route/router files first. Group all endpoints by resource or feature area. This grouping becomes the section structure of the manual.

```
For each route group:
  - Name the feature area (plain language, not controller name)
  - Count the endpoints
  - Identify the primary HTTP verbs (GET-heavy = read workflow, POST/PUT-heavy = action workflow)
  - Note any auth/permission middleware
```

## Step 4: Read the permission layer

Read policy, permission, or auth middleware files. Map every action to the roles that can perform it. This becomes the permission matrix.

## Step 5: Count the tests

Read the feature/integration test files. Count test cases per module. Modules with many tests have well-specified behavior and will generate faster. Modules with few tests need more manual investigation.

## Step 6: Produce the documentation plan

Output a structured plan:

```
## Documentation Plan: [Product/Module Name]

### Proposed folder structure
docs/[product-name]/
  index.md
  01-introduction/
    who-this-is-for.md
    quick-start.md
  02-[feature-area]/
    [workflow].md
  ...

### Sections (one file per row)
| File | Feature area | Audience | Endpoints | Tests | Spec coverage | Complexity |
|---|---|---|---|---|---|---|
| user-management.md | User Management | Admin | 8 | 24 | URD §3 | Medium |
| billing.md | Billing | User, Admin | 12 | 31 | none - inferred from code | High |
| ...

Sections with no spec coverage will have terminology chosen rather than given. Flag them
here so the team can supply a spec or accept the inference before generation starts.

### Audiences identified
| Audience | Sections they need | Guide type |
|---|---|---|
| End users | Introduction, Daily workflows | User Guide |
| Admins | All sections + Admin Guide | Admin Guide |

### Gaps flagged
- [Module]: no test coverage found - claims will be Unverified and left out of the manual
- [Module]: no policy file found - permissions unclear
- [Module]: no spec coverage - terminology will be inferred from UI labels and code

### Estimated effort
- Total sections: N
- Estimated generation runs: N (one per section)
- Recommended order: [fastest/highest-value first]

### Recommended next step
Run /generate-manual for [highest-value module] first.
```

Show the plan to the user and ask for approval before generating anything.
