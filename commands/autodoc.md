---
description: Point at a codebase with no specs, no context, and no prior docs. The skill reads what is implemented, infers audiences and workflows from the code, and generates a complete folder of Markdown documentation.
---

# Autodoc

You are generating documentation entirely from code - no PRD, no specs, no existing docs.
Your job is to read what is actually built and produce accurate documentation for it.

## What you need from the user

One thing only: where is the code?

```
/autodoc [path or repo URL]
```

If no path is given, use the current directory.

Optionally the user may specify:
- `--module [name]` - document one module instead of the entire codebase
- `--audience [end users | admins | developers]` - default is all three if inferable
- `--output [path]` - default is `docs/[inferred-product-name]/`

## Step 1: Infer the product and its modules

Read the route/router files first. Every route group is a potential module.

```
For Laravel:    routes/web.php, routes/api.php
For Rails:      config/routes.rb
For Django:     urls.py files
For Express:    router files in routes/ or app.js
For FastAPI:    main.py, routers/
For Next.js:    pages/ or app/ directory structure
```

From the routes, produce:
- Product name (infer from repo name, package.json, composer.json, or top-level namespace)
- Module list (one per route group or controller prefix)
- HTTP verb distribution per module (GET-heavy = read workflows, POST/PUT/DELETE-heavy = action workflows)

## Step 2: Build the permission matrix

Read the authorization layer before writing a single section.

```
For Laravel:    Policy files, Gate definitions, middleware groups
For Rails:      Pundit policies, CanCanCan abilities, before_action guards
For Django:     permission_classes, has_permission, has_object_permission
For Express:    auth middleware, role guards
For any stack:  look for role/permission constants, enums, or config files
```

Map every action to the roles that can perform it. If no permission system exists,
infer audiences from naming patterns (admin routes vs user routes).

## Step 3: Read the behavioral specification from tests

Feature tests and integration tests are the most reliable source of truth about
what the code actually does - more reliable than comments or variable names.

```
For each test file:
  - What scenario is being tested? (this is the workflow name)
  - What inputs does it send? (these are the form fields / parameters)
  - What does it assert? (this is the expected outcome)
  - What error states does it test? (these become the "if something goes wrong" blocks)
```

If no tests exist, read the controller/handler code directly and infer behavior
from validation rules, response shapes, and conditional logic.

## Step 4: Read validation rules as field documentation

Validation rules define what inputs are required, optional, and constrained.
They are the most accurate source for documenting form fields.

```
For Laravel:    FormRequest rules() method
For Rails:      validates declarations in models
For Django:     serializer fields and validators
For Express:    Joi/Zod/Yup schemas
For any stack:  look for validate(), schema, or rules objects near controllers
```

## Step 5: Infer the user journey from the code flow

For each module, reconstruct the user journey from the code:

```
1. What is the entry point? (the GET route that loads the page/screen)
2. What does the user see? (infer from view templates, serializers, or API response shapes)
3. What can they do? (all POST/PUT/PATCH routes in this module)
4. What happens on success? (success responses, redirects, events fired)
5. What can go wrong? (validation errors, exception handlers, error responses)
6. What do they do next? (related routes linked from this one)
```

This sequence becomes the section structure: one file per major action.

## Step 6: Generate the folder output

Create the documentation as two parallel folder trees following the standard structure:

```
docs/[inferred-product-name]/
  user-guide/
    index.md
    01-introduction/
      index.md
      who-this-is-for.md
      quick-start.md
    02-[module-name]/
      index.md
      [workflow-1].md
      [workflow-2].md
    [n]-troubleshooting-and-reference/
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
    02-[admin-module]/
      index.md
      [admin-workflow].md
    [n]-troubleshooting-and-reference/
      index.md
      troubleshooting.md
      audit-log.md
```

If the codebase has no admin-distinct features, generate only the `user-guide/`.

## Step 7: Apply Documentation Standards to every file

Every generated file must follow the standards in `references/writing-guide.md` Sections 20-22:

- Entry point trigger: "You are here because..."
- Scenario in blockquote format
- Step-by-step in second-person active voice
- "What happens next" after every task
- Decision forks where roles produce different screens
- Waiting states documented where the code shows async or status-based flows
- No internal field names - translate every `snake_case` identifier to plain language
- No implementation details - no mention of controllers, models, queues, cache keys

## Step 8: Flag what could not be inferred

After generating, produce a brief inference report:

```
## Inference Report

### Generated with high confidence
- [module]: routes + tests + policies all present

### Generated with medium confidence (tests missing)
- [module]: behavior inferred from controller code only - verify steps are correct

### Could not infer (needs human input)
- [module]: no routes found pointing here - may be frontend-only or removed
- [field]: validation rule exists but purpose is unclear from naming

### Terminology choices made
- "workspace" used for [model name] - change in index.md to update everywhere
- "owner" used for [role constant] - verify this matches UI labels
```

This report goes in `docs/[product-name]/inference-report.md` alongside the manual.

## Output

Report on completion:
- Total files generated
- Modules covered
- Audiences detected
- Anything flagged in the inference report
