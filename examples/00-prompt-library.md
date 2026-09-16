# Prompt Library

Ready-to-paste prompts for every situation. Copy, adapt the bracketed parts, run.

Organized by where you are, not by command name.

---

## "I have no idea where to start"

The safest first move. The wizard scans everything and asks you smart questions
before writing a single word.

```
/wizard
```

That is the entire prompt. Run it in a session where your codebase is accessible.
It will find everything on its own.

---

## "I have a codebase with no documentation at all"

### Simplest possible - just go

```
/autodoc .
```

Point at the current directory. It reads routes, tests, policies, and validation
rules. Generates the full folder output plus an inference report.

### With a specific module

```
/autodoc . --module billing
```

### With a specific audience

```
/autodoc . --audience "end users"
```

### With a custom output location

```
/autodoc . --output docs/user-guide/
```

### When you want to see the plan before it writes anything

```
/scan-codebase

Codebase: .
Tell me what you find and what you would generate before you start writing.
```

---

## "I have specs somewhere and want to reconcile them against code"

### Specs in Linear

```
/prd-to-manual

Codebase: current directory
Specs: Linear - epic [EPIC-ID]
Output: docs/[product-name]/
```

### Specs in Confluence

```
/prd-to-manual

Codebase: current directory
Specs: Confluence - page "[Page Title]" in space [SPACE-KEY]
Output: docs/[product-name]/
```

### Specs in Jira

```
/prd-to-manual

Codebase: current directory
Specs: Jira - epic [PROJECT-KEY] "[Epic name]"
Output: docs/[product-name]/
```

### Specs in Notion

```
/prd-to-manual

Codebase: current directory
Specs: Notion - page "[Page URL or title]"
Output: docs/[product-name]/
```

### Specs pasted directly

```
/prd-to-manual

Codebase: current directory
Specs:

[paste your PRD or FR content here]

Output: docs/[product-name]/
```

### Multiple spec sources

```
/prd-to-manual

Codebase: github.com/[org]/[repo]
Specs:
  - Confluence: space [KEY], page "[PRD title]"
  - Linear: epic [EPIC-ID] (acceptance criteria)
  - GitHub: merged PRs from last 30 days
Output: docs/[product-name]/
```

---

## "I want full control - tell me exactly what to generate"

```
/generate-manual

Product: [product name]
Codebase: [path or repo URL]
Specs:
  - [tool]: [location]
Audiences: [end users, admins, developers - pick what applies]
Modules: [list modules, or "all"]
Output: docs/[product-name]/
```

### Example - single module, two audiences

```
/generate-manual

Product: Acme Platform
Codebase: .
Specs: Linear epic ACM-88
Audiences: end users, admins
Modules: user management only
Output: docs/acme/
```

### Example - full platform, no specs

```
/generate-manual

Product: Acme Platform
Codebase: github.com/acme/platform
Specs: none
Audiences: end users, admins, support staff
Modules: all
Output: docs/acme/
```

---

## "My docs exist but might be out of date"

### Full audit - find everything stale or missing

```
/gap-report

Codebase: .
Existing docs: docs/
Check for: stale sections, missing features, outdated screenshots, wrong steps
Output: gap-report.md
```

### After a specific feature shipped

```
/gap-report

Codebase: .
Existing docs: docs/
Scope: [feature or module name] only - we just shipped changes here
```

### After a release - compare against merged PRs

```
/gap-report

Codebase: .
Existing docs: docs/
Scope: everything merged in the last [2 weeks / sprint / release]
Compare against: GitHub merged PRs since [date]
```

---

## "I just want to see what you'd produce before committing"

```
/scan-codebase

Codebase: .
Show me the proposed folder structure and section list.
Do not write any documentation yet.
```

This gives you the full documentation plan - modules, audiences, file tree, effort
estimate - so you can approve it before a single file is written.

---

## Modifier prompts

Add these to any command to tune the output.

### Restrict to one audience
```
Only generate the end user guide. Skip admin content entirely.
```

### Skip low-coverage modules
```
Skip any module with no test coverage. Flag it in the inference report.
```

### Plain language only
```
Translate all technical terms to plain language. No field names, no controller
names, nothing from the code. Write as if the reader has never seen the UI.
```

### Capture screenshots from the running app
```
Capture screenshots with Playwright. The app runs at http://localhost:3000,
seed it with `npm run app:seed`. Admin login is in .env.test.

Only capture steps that need visual proof: gated actions, unlabeled controls,
visual outcomes, and role forks. Mask every element that can hold personal data.
```

### Words only, no screenshots
```
No screenshots. Every step must be complete in words.
```

### Run the plain-language pass on existing docs
```
Apply the Section 24 plain-language rules to docs/product/user-guide/.
Do not change any fact, limit, role, or error case - only how the sentences read.
Show me a table of what you changed and why, by rule number.
```

### Point at a Linear URD, PRD, or feature brief
```
/prd-to-manual

Codebase: current directory
Specs: Linear document "[URD title]" and feature brief "[title]"
Use the specs for scope, terminology, and personas. Verify every behaviour
against the code. Anything specified but not shipped goes in the gap report.
Output: docs/[product-name]/
```

### Specs in Docmost
```
/prd-to-manual

Codebase: current directory
Specs: Docmost - space "[space name]", page "[URD title]" and its child pages
Output: docs/[product-name]/
```

### Publish the finished manual to Docmost
```
After generating, publish to Docmost. Create "[Product] User Guide" and
"[Product] Admin Guide" as separate spaces. Folders become parent pages, files
become child pages. Upload the screenshots as attachments and rewrite the image
paths. Share the User Guide space publicly with subpages included; keep the
Admin Guide private. Markdown in the repo stays the source of truth.
```

### Include a gap report alongside the manual
```
After generating the manual, produce a gap-report.md listing anything in the
code that has no documentation coverage and anything in the docs that has no
code evidence.
```

### Output for a specific docs portal

For Mintlify:
```
Output for Mintlify. Use the standard folder structure. Do not add mint.json -
I will configure that separately.
```

For Docusaurus:
```
Output for Docusaurus. Use numeric folder prefixes for sidebar ordering.
```

For Confluence:
```
After generating Markdown, publish to Confluence space [KEY] using the
Atlassian MCP. Use the folder structure as the page hierarchy.
```

### Healthcare / HIPAA
```
This is a healthcare product. Apply Section 18 of the writing guide:
- No PHI examples in any steps
- Role-gate all clinical content
- Include audit trail callouts where actions are logged
- Regulatory context: [HIPAA / ICH E6 GCP / 21 CFR Part 11 - pick what applies]
```

### Multiple languages
```
Generate in English first. Then produce a second pass in [Spanish / French /
German]. Keep file names in English. Add the language code as a suffix:
invite-members-es.md, invite-members-fr.md.
```

---

## Power user prompts

### Document a single controller or route group
```
Document everything under [ControllerName / route prefix]. One file per
major action. End users only. Output to docs/[module]/
```

### Generate from a GitHub PR
```
Read the diff for PR #[number] in [org/repo]. Generate documentation for
exactly what shipped in this PR. Output one file: docs/[feature-name].md
```

### Terminology first
```
Before writing any documentation, output a terminology lock table for this
product. One term per concept. Use only what appears in the UI. I will approve
it before you generate anything.
```

### Journey map first
```
Before writing any documentation, produce a journey map for each audience you
detect. Show me the maps and wait for my approval before writing sections.
```

### Re-capture screenshots after a UI change
```
The Members panel was redesigned in PR #204. Re-run the captures for
docs/product/.captures/manifest.json, report any selector that no longer
matches, and update the step text where the UI labels changed.
```

### Patch one section without regenerating everything
```
The [section name] section in docs/[path]/[file].md is out of date.
The change was: [describe what changed].
Update only that file. Do not touch any other files.
```
