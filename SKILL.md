---
name: ai-doc-architect
description: >
  Design, architect, or build AI-driven documentation automation systems for frontend
  apps, backend services, APIs, mobile apps, SaaS platforms, SDKs, and internal tools.
  Triggers: "automate documentation", "AI documentation system", "generate docs from
  code", "documentation pipeline", "auto-generate user manuals", "documentation agent",
  "keep docs in sync with code", "generate FAQ", "auto-generate troubleshooting guide",
  "feature walkthrough", "release documentation", "generate training material", "SDK
  documentation", "mobile app documentation", "onboarding guide from code", CI/CD
  documentation pipelines, multi-audience manuals. PRD-to-manual: "read PRD and
  generate docs", "compare FR to code", "PRD to user manual", "reconcile spec and
  code", "Confluence and Linear to docs". Writing quality: "plain language for docs",
  "task-oriented writing", "five-persona writing model". Use for partial questions too.
---

# AI-Driven Documentation & User Manual Automation System (ADUMAS)

Generate, maintain, version, and distribute accurate user manuals and technical
documentation from source code, APIs, Git history, and product specs.

**Goal: write code once, let AI maintain accurate documentation automatically.**

## Reference files

Read only what you need. Never read all files for a focused question.

| File | Read when |
|---|---|
| `references/architecture.md` | Designing agents, pipelines, DB schemas, workflows, Laravel ingestion |
| `references/tech-stack.md` | Tech stack decisions, MVP vs enterprise, scalability, security |
| `references/writing-guide.md` | Writing or reviewing documentation (Sections 20-23 are the standards) |
| `references/mcps.md` | Connecting to GitHub, Notion, Confluence, Linear, Jira, Figma, Slack |
| `references/decision-guide.md` | 10 decision trees: LLM, DB, deployment, agents, versioning |
| `references/anti-patterns.md` | 20 anti-patterns across planning, build, and operations |


## Match depth to the request

| Request type | What to produce |
|---|---|
| "Full architecture" | Read `architecture.md` + `tech-stack.md`; produce comprehensive design |
| "What agents do I need?" | Read `architecture.md` Agent Roster; describe 7 core agents |
| "How do I version docs?" | Read `architecture.md` Version Control section |
| "Tech stack recommendation" | Read `tech-stack.md`; produce recommendation matrix |
| "Build as a Word file" | Read reference files + invoke `docx` skill |
| "MVP vs full platform" | Read `tech-stack.md` MVP section |
| Generate or review documentation | Read `references/writing-guide.md` Sections 20-23 |

**Tailor to context:**
- Startup / small team: 3 agents, single Postgres DB, GitHub + Notion first; skip enterprise features
- Enterprise: RBAC, audit logging, SSO, compliance (SOC 2 / HIPAA), multi-tenancy, private cloud
- Developer product: OpenAPI/GraphQL/gRPC generation, docs-as-code, IDE integration
- Non-technical team: human review workflow, audience adaptation, portal UI; skip agent internals


## Core Principles

- **Audience-first.** Every artifact targets a specific persona. Same feature, different content for user vs developer vs admin.
- **Task-oriented.** Document how to do things, not what things are. Format: context → prerequisites → steps → outcome.
- **Structure for scanning.** Descriptive headings, cross-links, no walls of prose. Users arrive with a question.
- **Markdown-first.** Markdown is source of truth. Render to HTML, PDF, DOCX, Notion, Confluence from it. Never the reverse.
- **Docs are software.** Version them (Git), test them (quality gates), review them (approval workflows), deploy them (CI/CD).
- **Continuous improvement.** Feedback loop: analytics + search gaps + support ticket correlation → generation quality.


## Documentation Standards

A user manual is organized around user intentions, not product features.
Every generated manual must pass the completeness checklist in `references/writing-guide.md` Section 20.9.
Full rule details and examples are in `references/writing-guide.md` Sections 20-22.

**Rules applied to every manual:**

| Rule | One-line summary |
|---|---|
| Rule 0: Journey map first | Before any section, map each persona's numbered trigger-to-outcome sequence. Each step = one file. |
| Rule 0b: Process-oriented | Answer "what is the user trying to do?" never "what does this feature do?" |
| Rule 1: No "Chapter" | Use descriptive workflow names. Group by PART: Introduction, Daily Workflows, Operational Features, Analytics/Admin, Reference. |
| Rule 2: Separate audiences | User Guide and Admin Guide are always separate documents. Never combine. |
| Rule 3: Lock terminology | One term per concept. Use the UI label. Never use synonyms. |
| Rule 4: Quick Start second | After Introduction, before everything else. 10 steps or fewer per persona. Steps only. |
| Rule 5: Frequency order | Daily workflows first. Exceptions second. Admin third. Reference last. |
| Rule 6: No field names | Translate every `snake_case` identifier to plain language before writing. |
| Rule 7: UX patterns brief | Standard patterns (typing indicator, auto-save) get one sentence. Full blocks for complex/gated/irreversible actions only. |
| Rule 8: No implementation details | No endpoints, no DB field names, no cache keys, no cron schedules in user docs. |
| Rule 9: What to do next | After every completed action: 2-4 forward paths, most common first, ordered by frequency. |
| Rule 10: Entry point trigger | Every section opens: "You are here because..." so search arrivals know they are in the right place. |
| Rule 11: Progressive disclosure | Layer 1 = simple path. Layer 2 = variations. Layer 3 = edge cases and admin config (Admin Guide only). |
| Rule 12: Decision forks | When roles or config produce different screens, fork explicitly: "If you see X / If you see Y." |
| Rule 13: Waiting states | Every workflow has a waiting period. Document it: what to do, what to watch for, what to do if too long. |
| Rule 14: Inline links | Links belong at the step where needed, not in a footer after step 10. |
| Split rule | More than 3 major tasks or its own permission model = its own file, not a subsection. |
| Glossary rule | Define every product-specific term on first use AND in the Reference glossary. |

**Every feature section must contain:** audience preamble + outcome statement, overview paragraph, scenario in blockquote, step-by-step tasks (second-person, one action per step), outcome after each task, error handling per task, best practices (3-5 items), limits table, system messages table, related topics links.


## Slash Commands

| Command | What it does |
|---|---|
| `/wizard` | Guided setup: scans silently first, then asks smart context-aware questions derived from actual findings before generating anything |
| `/autodoc [path]` | Zero-context mode: point at a codebase, infer everything from code, generate the full manual folder with no specs required |
| `/generate-manual` | Full workflow: read code + fetch specs + map journeys + output folder of Markdown files |
| `/prd-to-manual` | Reconcile a PRD against shipped code, output user manual folder + gap report |
| `/scan-codebase` | Inventory only - produces a documentation plan and folder structure before writing |
| `/gap-report` | Audit existing docs against current code, produce prioritised list of what is stale or missing |

See `commands/` for the full prompt definition of each command.
See `examples/00-prompt-library.md` for ready-to-paste prompts for every scenario.


## Default Output Format

**All generated documentation is a folder of Markdown files, not a single file.**
Single file is the fallback only when explicitly requested or for short one-off outputs (gap report, release note).

```
docs/[product-name]/
  index.md                          (master table of contents)

  01-introduction/
    index.md
    who-this-is-for.md
    quick-start.md

  02-[section-name]/
    index.md
    [workflow-1].md
    [workflow-2].md

  [n]-troubleshooting-and-reference/
    index.md
    troubleshooting.md
    glossary.md
    limits.md
    system-messages.md
```

Always generate two parallel trees: `[product]/user-guide/` and `[product]/admin-guide/`.

**Naming rules:** folders = `NN-kebab-case`, files = `kebab-case-action.md`, section overviews = `index.md`.

**Cross-links:** always relative paths (`../04-daily-workflows/invite-members.md`). Never absolute URLs.

Mintlify, Docusaurus, GitBook, and Confluence all build sidebar navigation from this structure automatically.
Full spec in `references/writing-guide.md` Section 23.


## PRD to User Manual

Read specs (any tool) + code (GitHub) + acceptance criteria → document what shipped → gap report for what did not.

Key rule: never document a feature that did not ship. The gap report tracks missing features; the manual reflects only what users can do today.

See `commands/prd-to-manual.md` for the step-by-step workflow.
See `examples/03-prd-to-manual.md` for a full worked example with reconciliation table and gap report.

**Supported spec sources:** Linear, Confluence, Jira, Notion, Docmost, GitHub, Asana, ClickUp, Google Drive, or pasted directly.


## One-Shot Generation Modes

| Mode | Trigger | Command |
|---|---|---|
| Full platform | "document the entire platform" | `/wizard` or `/autodoc .` |
| Module | "document the billing module" | `/generate-manual --module billing` |
| Feature | "document the export feature that just shipped" | `/prd-to-manual` |
| PRD-driven | "read this PRD and generate docs" | `/prd-to-manual` |
| Existing codebase, zero docs | "we have no docs at all" | `/autodoc .` |
| Catch drift | "check if docs are still accurate" | `/gap-report` |

For Laravel: group by controller prefix to discover modules. Read `references/architecture.md` Laravel/PHP Stack Ingestion for the full sequence.


## Designing an ADUMAS System

For agent architecture, five-tier design, workflow diagrams, DB schemas, CI/CD pipelines:
→ Read `references/architecture.md`

For tech stack, deployment options, scalability, MVP vs enterprise:
→ Read `references/tech-stack.md`

For MCP connector setup (GitHub, Confluence, Linear, Jira, Slack, Notion, Figma):
→ Read `references/mcps.md`

For the 10 key architectural decisions:
→ Read `references/decision-guide.md`

For common mistakes across planning, build, and operations:
→ Read `references/anti-patterns.md`
