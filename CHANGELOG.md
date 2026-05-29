# Changelog

All notable changes to `ai-doc-architect` are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [1.2.0] - 2026-05-29

### Added

- Requirements coverage matrix in `SKILL.md` - maps all 11 primary requirements to specific sections of the skill.
- System types coverage in `architecture.md` - explicit table of supported system types: frontend, backend, APIs, mobile apps, SaaS, enterprise tools, SDKs, automation workflows.
- Five-Persona Writing Model in `architecture.md` (Agent 3) and `references/writing-guide.md` - PM, technical writer, support engineer, QA engineer, developer lenses applied to every documentation artifact.
- 6 new proactive intelligence alerts: Unclear Workflow, Simplification Opportunity, Onboarding Gap, Duplicate Content, Edge Case Undocumented, Missing FAQ Entry.
- Audience Adaptation expanded with environment-specific documentation (dev/staging/prod) and 7-dimension adaptation table.
- 4 new Skills/Modules: Embedded Walkthrough Builder, Training Material Generator, FAQ Synthesizer, Feature Walkthrough Builder.
- AI-Assisted Editing workflow in `architecture.md` - how human reviewers work with AI suggestions without blank-page editing.
- Interactive Documentation Agent workflow - self-improving question-answering loop that converts unanswered questions into new documentation.
- Trello added to MCP Priority Matrix in `references/mcps.md`.
- Five-Persona review checklist in `references/writing-guide.md` (Section 16) with per-persona pass/fail criteria.
- Updated SKILL.md description triggers to include: FAQ generation, troubleshooting guide automation, feature walkthrough generation, release documentation, training material, SDK documentation, mobile app documentation.

### Changed

- `SKILL.md` description expanded to include all missing document and system type triggers.
- `README.md` versioned to v1.2.0.

---

## [1.1.0] - 2026-05-29

### Added

- `references/writing-guide.md` - comprehensive documentation writing guide covering the 7-step user manual creation process, plain language principles, second person style, task-oriented writing template, searchable heading design, table of contents guidance, cross-linking strategy, screenshot policy, analytics and continuous improvement, and how to prompt ADUMAS agents for quality output. Closes all Mintlify article coverage gaps.
- `examples/05-prd-to-manual.md` - full walkthrough of the PRD-to-manual workflow: Confluence PRD + Linear functional requirements + GitHub code = Markdown user manual + gap report + Linear issues for deviations.
- PRD-to-manual workflow in `SKILL.md` - new workflow section covering 3-source reconciliation (Confluence + Linear/Jira + GitHub), Specification Reconciler agent, and gap reporting.
- PRD-to-manual integration notes in `references/mcps.md` - detailed Atlassian MCP and Linear MCP wiring for the specification reconciliation workflow.
- 7 new eval test cases in `evals/evals.json` (IDs 11-15) covering PRD-to-manual workflow, writing quality questions, plain language, and hands-on generation prompts.
- Default output format declaration in `SKILL.md`: Markdown is always the primary output format.

### Changed

- `SKILL.md` description updated to include PRD-to-manual and writing quality trigger phrases.
- `SKILL.md` reference file list expanded to cover all 6 reference files with descriptions.
- `README.md` updated with v1.1.0 badge, new capabilities, new example prompts, updated structure map, and writing-guide.md in progressive disclosure table.
- All 186 em dash instances removed across all files (replaced with hyphens).

---

## [1.0.0] - 2026-05-28

### Added
- Initial release of `ai-doc-architect` skill.
- `SKILL.md` - core skill with triggers, principles, and usage guidance.
- `references/architecture.md` - five-tier architecture, 7-agent roster, workflow designs, DB schemas, and automation pipeline specs.
- `references/tech-stack.md` - tech stack recommendations, integration architecture, scalability, security, enterprise deployment, and MVP vs enterprise feature breakdown.
- `references/mcps.md` - comprehensive MCP integration guide covering 10 platforms with setup priority, required scopes, data flow, and what to avoid.
- `references/decision-guide.md` - decision trees for every major architectural choice (LLM selection, database strategy, deployment model, agent scope, MCP selection).
- `references/anti-patterns.md` - 20 documented anti-patterns organized by phase (planning, build, operations) with root causes and remediation paths.
- `examples/` - four complete scenario walkthroughs: startup MVP, enterprise full platform, API-docs-only, healthcare/HIPAA.
- `evals/evals.json` - 8 test cases covering the full range of skill triggers.
- `.github/` - issue templates and PR template.
- `CONTRIBUTING.md`, `LICENSE`, `CHANGELOG.md`.

---

## [Unreleased]

### Planned
- `examples/05-open-source-project.md` - documentation automation for public OSS repositories.
- `references/observability.md` - monitoring, alerting, and debugging a running documentation pipeline.
- Agent prompt templates in `scripts/prompts/` - ready-to-use LLM system prompts for each of the 7 agents.
- `evals/` expansion - 20 additional test cases covering edge scenarios.

## v1.3.0 (2026-05-29)

### Added

- Laravel/PHP Stack Ingestion section in architecture.md: full ingestion sequence
  for Laravel apps (routes, FormRequests, Models, Policies, Pest tests, PRD files,
  CLAUDE.md as domain glossary), module-by-module strategy, Laravel-specific
  documentation patterns (permissions tables, validation-to-field docs, dynamic forms)
- One-Shot Generation Modes section in SKILL.md: full system scan, module-by-module,
  feature-by-feature, PRD-driven, controller-grouped - each with prompt templates,
  ingestion sources, and expected output structure
- examples/06-clinical-healthcare-saas.md: full worked example for a Laravel + Vue.js
  clinical trial management platform; covers all four generation modes with patients-api
  as the reference project; includes drug inventory module deep-dive and PRD-driven
  appointment feature documentation
- Sections 18 and 19 in writing-guide.md: healthcare and clinical trial documentation
  rules covering PHI-aware writing, regulatory context (ICH E6 GCP, 21 CFR Part 11,
  HIPAA), clinical terminology standards, role-gated documentation, audit trail
  documentation template, dynamic forms field documentation pattern, i18n guidance,
  and 8 healthcare-specific anti-patterns
- Four new evals (IDs 16-18): Laravel drug inventory module, PRD-driven appointments
  feature, and full-platform clinical trial system scan

### Fixed

- Em dash sweep: 0 em dashes across all files (verified)

## v1.4.0 (2026-05-29)

### Added

- Book Standard manual structure in writing-guide.md Section 20: mandatory chapter
  list, per-chapter structure template (9 required sub-sections), role-specific reading
  guide format, troubleshooting chapter format, glossary rules, scenario-based writing
  format, limits/constraints table derivation, PHI/privacy section for healthcare, and
  25-point completeness checklist
- Book Standard declaration in SKILL.md: mandatory chapter list, split rule (3+ major
  tasks = own chapter), scenario rule, glossary rule - all non-negotiable for every
  generated manual

### Fixed

- README: all "your-org" placeholders replaced with real GitHub handle (itismowgli)
- README: skills.sh badge now points to https://skills.sh/itismowgli/ai-doc-architect
- README: version badge updated to v1.4.0
