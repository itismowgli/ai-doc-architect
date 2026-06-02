# Changelog

All notable changes to `ai-doc-architect` are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [2.2.0] - 2026-06-03

### Added

- **Folder-based output as the default standard** (SKILL.md `Default Output Format`) -
  all generated documentation is now a folder of Markdown files, not a single file;
  single-file output is the explicit fallback for short one-off requests only; includes
  naming rules, folder structure template, cross-linking rules, comparison table
  (single file vs folder), and portal compatibility notes for Mintlify, Docusaurus,
  GitBook, Notion, and Confluence

- **Slash commands** (SKILL.md `Slash Commands`, `commands/` folder) - four direct
  invocation commands:
  - `/generate-manual` - full workflow: read code + fetch specs + map journeys + output folder
  - `/prd-to-manual` - reconcile PRD against shipped code, output manual folder + gap report
  - `/scan-codebase` - inventory only, returns documentation plan and folder structure before writing
  - `/gap-report` - audit existing docs against current code, produce prioritised stale/missing list

- **Section 23: Folder-Based Documentation Output** (`references/writing-guide.md`) -
  complete reference covering: standard folder structure, file naming rules, index.md
  templates (root and section), cross-linking with relative paths, portal mapping table,
  user-guide/admin-guide separation as parallel folder trees, and when a single file
  is acceptable

- **examples/08-folder-structure-output.md** - complete folder output showcase for a
  generic SaaS product ("Workspace"): 26-file folder tree, actual content for 5 key
  files (root index, section index, who-this-is-for, invite-members workflow,
  admin roles-and-access), cross-link examples, portal sidebar mapping, and single-file
  vs folder comparison; the "start here" example for new users

### Changed

- **examples/README.md** - `08-folder-structure-output.md` is now the primary "start
  here" example; `07-output-showcase.md` listed second for single-section detail

---

## [2.4.0] - 2026-06-03

### Changed

- **`SKILL.md` rewritten for token efficiency** - cut from 599 lines / 31KB to ~200 lines
  / ~11KB (65% reduction); content removed from SKILL.md was not deleted - it lives in
  reference files and commands/ that are read on demand; specific removals:
  - Requirements Coverage matrix (never used during execution; 40 lines)
  - "What This Skill Produces" section (redundant with slash commands; 15 lines)
  - Documentation Standards full prose (Rules 0-14 with examples) replaced by a
    compact rule table (one line per rule); full detail remains in `references/writing-guide.md`
    Sections 20-22 which are read when generating documentation
  - PRD to User Manual full workflow block replaced by a 5-line pointer to
    `commands/prd-to-manual.md` and `examples/03-prd-to-manual.md`
  - One-Shot Generation Modes detail replaced by a compact 6-row mode table
  - Verbose "How to Approach a Request" context replaced by a compact match table
  - Core Principles compressed from paragraph form to 6 one-line bullets
  - Laravel/PHP Specific section removed from SKILL.md; already covered in
    `references/architecture.md` Laravel/PHP Stack Ingestion

- **GitHub templates updated** to reflect current skill structure:
  - `bug_report.md` - file list updated to include all 6 commands/; bug types updated to
    include "Generated folder structure was wrong"; removed "Additional context" (redundant)
  - `feature_request.md` - contribution types updated; `commands/` added as a primary
    contribution target; description updated from "scenario/platform/anti-pattern" to
    "command/example/prompt/anti-pattern/integration"
  - `PULL_REQUEST_TEMPLATE.md` - stale checklist item "New examples follow the 5-section
    structure" removed and replaced with "New examples are command-oriented (walkthrough,
    showcase, or prompt library entry)"; `commands/` added to Type of change; new checklist
    item "New commands follow the phase structure (scan → ask → preview → generate)";
    "Token efficiency improvement" added as a change type

---

## [2.3.0] - 2026-06-03

### Added

- **`examples/00-prompt-library.md`** - complete ready-to-paste prompt library
  organized by user situation: starting fresh, codebase with no docs, specs in
  various tools (Linear/Confluence/Jira/Notion), existing docs that may be stale,
  preview-only mode, format-specific output, and power user / modifier prompts;
  the first thing a new user should read

- **`examples/01-wizard-walkthrough.md`** - full `/wizard` session from a single
  `/wizard` prompt to 43 generated files; shows the silent scan findings table,
  every question with real answers and the reason each question is asked, the
  pre-generation folder tree preview, generation progress output, and completion
  summary with confidence tiers and terminology choices

- **`examples/02-autodoc.md`** - `/autodoc .` on a Laravel fintech codebase with
  zero specs; shows what is ingested (routes, policies, tests, validation rules),
  the full generated folder structure (38 files), and the complete inference report
  with high/medium confidence tiers, terminology choices, and could-not-infer items

- **`examples/03-prd-to-manual.md`** - `/prd-to-manual` reconciling a Django
  codebase against Confluence PRD + Linear acceptance criteria; shows the
  three-source reconciliation table, what was shipped vs specified, the generated
  folder output, and the full gap report with P1/P2/P3 priorities

- **`examples/04-gap-report.md`** - `/gap-report` catching 6 sprints of drift;
  shows the stale steps, missing documentation, and partially outdated files with
  specific fix instructions per file; includes the patch prompt pattern for
  surgical single-file updates

- **`examples/05-output-showcase.md`** - complete rendered output for a workflow
  file (invite-members.md) with every required element: entry trigger, overview,
  scenario, two task blocks, error handling per task, best practices, limits table,
  system messages table, what-to-do-next connectors; maps every output element
  back to its code source; shows how the folder renders as portal sidebar

### Changed

- **`examples/README.md`** - fully rewritten; organized by user situation not file
  number; `00-prompt-library.md` is the primary "start here" entry

### Removed

- `examples/01-startup-mvp.md` - organized around company archetype, not command;
  replaced by `01-wizard-walkthrough.md`
- `examples/02-enterprise-full.md` - same reason; gap report and multi-source
  reconciliation now covered by `03-prd-to-manual.md` and `04-gap-report.md`
- `examples/03-api-docs-only.md` - API-first prompt now in `00-prompt-library.md`
- `examples/04-healthcare-hipaa.md` - healthcare prompt now in `00-prompt-library.md`
- `examples/05-prd-to-manual.md` - replaced by new `03-prd-to-manual.md` with
  folder output, three-source reconciliation table, and gap report
- `examples/06-clinical-healthcare-saas.md` - stub file, removed
- `examples/06-existing-codebase-zero-docs.md` - replaced by `02-autodoc.md`
- `examples/07-output-showcase.md` - merged into new `05-output-showcase.md`
- `examples/08-folder-structure-output.md` - merged into new `05-output-showcase.md`

---

## [2.2.0] - 2026-06-03

### Added

- **`/wizard` slash command** (`commands/wizard.md`) - guided documentation setup with
  six phases: silent deep scan, findings summary, context-aware questions, pre-generation
  preview, generation, and completion summary; every question is derived from actual scan
  findings (uses real module names, real role names, real spec source names) - never
  generic; questions are conditional and skipped when the answer is obvious from the code;
  covers scope selection, audience strategy, spec reconciliation, confidence handling for
  untested modules, and output location; shows an exact folder tree preview before writing
  any file; generates in confidence order (high-coverage modules first); outputs
  `inference-report.md` and optional `gap-report.md` alongside the manual

- **`/wizard` added to SKILL.md slash commands table** as the first entry - the
  recommended starting point for any new user

---

## [2.1.0] - 2026-06-02

### Added

- **`/autodoc` slash command** (`commands/autodoc.md`) - zero-context documentation
  generation: point at any codebase with no PRD, no specs, and no existing docs; the
  skill infers product name, modules, audiences, user journeys, permission matrix, and
  terminology entirely from routes, tests, validation rules, and controller code;
  supports `--module`, `--audience`, and `--output` flags; outputs the standard two-tree
  folder structure plus an `inference-report.md` flagging what was generated with high vs
  medium confidence and what could not be inferred; works with Laravel, Rails, Django,
  Express, FastAPI, and Next.js (and any other stack via the generic patterns)

- **`/autodoc` added to SKILL.md slash commands table** as the first entry - the
  zero-context entry point before `/generate-manual`

---

## [2.0.0] - 2026-06-01

### Added

- **Documentation Standards** (writing-guide.md Sections 20, 21, 22) - the complete
  framework for generating production-ready documentation:
  - Section 20: mandatory section list, per-section structure template (9 required
    subsections), role-specific reading guide format, troubleshooting section format,
    glossary rules, scenario-based writing format, limits table derivation, PHI/privacy
    section for healthcare, and 25-point completeness checklist
  - Section 21: 10 critique-proven rules covering section naming, audience separation,
    terminology locking, no internal field names, information architecture by frequency,
    Quick Start requirement, UX pattern brevity, implementation detail exclusion, tone,
    and visual hierarchy in Markdown
  - Section 22: 8 process-oriented documentation rules covering journey mapping,
    "what to do next" connectors, entry point triggers, progressive disclosure, decision
    forks, contextual inline links, waiting state documentation, and the minimum journey
    standard checklist

- **Documentation Standards enforcement in SKILL.md** - 15 numbered rules (0-14) plus
  split rule and glossary rule applied to every generated manual; Rules 0 and 0b
  establish journey mapping and process-orientation before any writing begins

- **One-Shot Generation Modes** (SKILL.md) - full system scan, module-by-module,
  feature-by-feature, PRD-driven, and controller-grouped modes with prompt templates,
  ingestion sources, and expected output structures

- **Laravel / PHP Stack Ingestion** (architecture.md) - complete ingestion sequence
  for Laravel applications: route files, FormRequests, Eloquent Models, Policy files,
  Pest feature tests, console commands, migrations, config files, CLAUDE.md as domain
  glossary; module-by-module strategy with source mapping template

- **Healthcare documentation guidance** (writing-guide.md Sections 18-19) - PHI-aware
  writing rules, regulatory context (ICH E6 GCP, 21 CFR Part 11, HIPAA), clinical
  terminology standards, role-gated documentation, audit trail template, dynamic forms
  documentation pattern, i18n guidance, and 8 healthcare-specific anti-patterns

- **PRD-to-manual workflow** (SKILL.md + examples/05) - three-source reconciliation
  (any spec tool + any issue tracker + code), Specification Reconciler, gap report
  generation, tracked issue creation; source discovery step asks the user which tools
  they use before fetching anything

- **examples/06-existing-codebase-zero-docs.md** - universal four-step pattern for
  any existing codebase with zero documentation: inventory from routes, build permission
  matrix first, read tests as specification, generate section by section; stack-agnostic
  (Laravel, Django, Rails, Node.js, Spring, Next.js)

- **examples/07-output-showcase.md** - full rendered documentation output for a
  fictional SaaS "Workspace" product showing Team and Member Management; demonstrates
  every required element: scenarios, step-by-step tasks, outcomes, error states, best
  practices, permission matrix, limits table, system messages index, "what to do next"
  connectors; maps every output element back to the exact code source that produced it

- **evals** expanded to 18 test cases covering: MVP systems, MCP selection, healthcare
  on-prem, agent design, anti-patterns, enterprise architecture, tech stack decisions,
  PRD-to-manual workflow, writing quality, plain language, Laravel SaaS module-by-module,
  team invitation PRD-driven, and full-platform system scan

### Changed

- **Section naming** - "Chapter" replaced with descriptive workflow names throughout
  all files; PART structure introduced for frequency-of-use ordering (Introduction,
  Daily Workflows, Operational Features, Analytics and Administration, Troubleshooting
  and Reference)

- **"Book Standard" renamed to "Documentation Standards"** - removes academic/textbook
  connotation; example 07 renamed from `07-book-standard-showcase.md` to
  `07-output-showcase.md`

- **Audience separation enforced** - User Guide and Admin Guide are always separate
  documents; a single document combining all audiences is no longer acceptable output

- **SKILL.md trimmed** - removed four architecture-design sections (Designing the
  Agent System, Designing the Workflow, Output Format Guidance, Quick Reference: Key
  Design Decisions); these are now referenced via `references/architecture.md` pointer

- **Spec sources made fully flexible** - PRDs, URDs, and functional requirements can
  live in Linear, Confluence, Jira, Notion, Docmost, GitHub, Asana, ClickUp, Google
  Drive, or pasted directly; no tool is prescribed as primary

- **All Resonance/patients-api-specific references removed** - skill is now globally
  usable; DrugInventory, AppointmentController, Care Insight, patients-api, and
  Resonance-specific terminology replaced with universally relatable generic examples
  (billing module, team invitation flow, workspace/owner/ticket terminology)

- **examples/README.md restructured** - examples 06 and 07 listed first with clear
  "start here" guidance; table updated with "read when" column

- **CONTRIBUTING.md** - clone URL updated to `itismowgli/ai-doc-architect`

- **README** - all `your-org` placeholders replaced with `itismowgli`; skills.sh badge
  points to `https://skills.sh/itismowgli/ai-doc-architect`

- **evals/evals.json** version updated to match current skill version

### Fixed

- All 186+ em dash instances removed and held at 0 across all files
- `writing-guide.md` Section 20 role reading guide genericised (was Care Insight-specific)
- `writing-guide.md` Section 21 terminology lock table genericised (was clinical terms)
- All `.fuse_hidden` OS filesystem artifacts excluded from packaged `.skill` file via rsync
- `.gitignore` added to exclude `.fuse_hidden*`, `.DS_Store`, and `*.skill` build artifacts

---

## [1.2.0] - 2026-05-29

### Added

- Requirements coverage matrix in `SKILL.md` mapping all 11 primary requirements to
  specific sections of the skill
- System types coverage in `architecture.md` - explicit table of supported system types
- Five-Persona Writing Model in `architecture.md` and `writing-guide.md` - PM, technical
  writer, support engineer, QA engineer, developer lenses applied to every artifact
- 6 new proactive intelligence alerts: Unclear Workflow, Simplification Opportunity,
  Onboarding Gap, Duplicate Content, Edge Case Undocumented, Missing FAQ Entry
- 4 new Skills/Modules: Embedded Walkthrough Builder, Training Material Generator,
  FAQ Synthesizer, Feature Walkthrough Builder
- AI-Assisted Editing workflow and Interactive Documentation Agent workflow in
  `architecture.md`
- Trello added to MCP Priority Matrix in `references/mcps.md`
- Five-Persona review checklist in `writing-guide.md` Section 16

### Changed

- `SKILL.md` description expanded with all missing document and system type triggers
- `README.md` updated to v1.2.0

---

## [1.1.0] - 2026-05-29

### Added

- `references/writing-guide.md` - 7-step user manual creation process, plain language,
  second person style, task-oriented writing, cross-linking, screenshot policy, analytics,
  and prompting ADUMAS agents for quality output
- `examples/05-prd-to-manual.md` - full PRD-to-manual walkthrough
- PRD-to-manual workflow and 3-source reconciliation in `SKILL.md`
- 7 new eval test cases (IDs 11-15)

### Fixed

- All 186 em dash instances removed across all files

---

## [1.0.0] - 2026-05-28

### Added

- Initial release of `ai-doc-architect` skill
- `SKILL.md` - core skill with triggers, principles, and usage guidance
- `references/architecture.md` - five-tier architecture, 7-agent roster, workflow
  designs, database schemas, and automation pipeline specifications
- `references/tech-stack.md` - tech stack recommendations, integration architecture,
  scalability, security, enterprise deployment, MVP vs enterprise breakdown
- `references/mcps.md` - MCP integration guide for 10 platforms
- `references/decision-guide.md` - 10 decision trees for major architectural choices
- `references/anti-patterns.md` - 20 anti-patterns across planning, build, operations
- `examples/` - four scenario walkthroughs: startup MVP, enterprise full platform,
  API-docs-only, healthcare/HIPAA
- `evals/evals.json` - 8 test cases
- `.github/` issue templates and PR template
- `CONTRIBUTING.md`, `LICENSE`, `CHANGELOG.md`

---

## [Unreleased]

### Planned

- `examples/09-open-source-project.md` - documentation automation for public OSS
  repositories
- `references/observability.md` - monitoring, alerting, and debugging a running
  documentation pipeline
- Agent prompt templates in `scripts/prompts/` - ready-to-use LLM system prompts for
  each of the 7 agents
- Screenshot automation integration guide
- Video script generation workflow example
