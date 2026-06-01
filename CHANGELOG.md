# Changelog

All notable changes to `ai-doc-architect` are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/).

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

- `examples/08-open-source-project.md` - documentation automation for public OSS
  repositories
- `references/observability.md` - monitoring, alerting, and debugging a running
  documentation pipeline
- Agent prompt templates in `scripts/prompts/` - ready-to-use LLM system prompts for
  each of the 7 agents
- Screenshot automation integration guide
- Video script generation workflow example
