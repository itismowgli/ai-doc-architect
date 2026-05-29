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

This skill guides the design and implementation of AI-driven documentation automation
systems - platforms that generate, maintain, version, and distribute high-quality user
manuals and technical documentation directly from source code, APIs, Git history, and
product behavior.

The goal is always the same: **write code once, let AI maintain accurate documentation automatically.**

Reference files in this skill:
- `references/architecture.md` - Five-tier architecture, agent roster, workflows, database schemas, automation pipelines
- `references/tech-stack.md` - Tech stack recommendations, integrations, scalability, security, enterprise deployment, roadmap, MVP vs enterprise breakdown
- `references/writing-guide.md` - How to write documentation that works: 7-step process, task-oriented writing, plain language, second person, testing with users, cross-linking, analytics, AI agent prompting guide
- `references/mcps.md` - MCP priority matrix, integration setup for GitHub, Notion, Atlassian, Linear, Figma, Slack
- `references/decision-guide.md` - 10 decision trees: MVP scope, LLM, database, destination, deployment, agent count, review triggers, versioning
- `references/anti-patterns.md` - 20 anti-patterns across planning, build, and operations phases

Read only what you need. For a full system design, read all reference files. For a focused question (e.g., "what agents do I need?"), read only the relevant section. For writing quality questions, read `references/writing-guide.md`. For the PRD-to-manual workflow, read `references/mcps.md` and `references/architecture.md`.


## Requirements Coverage

This skill provides full coverage of all 11 primary requirements for an AI-driven documentation system.

| Requirement | Coverage | Primary Reference |
|---|---|---|
| **1. Intelligent Documentation Generation** | All document types: user manuals, admin manuals, technical docs, API references, SOPs, onboarding guides, troubleshooting guides, FAQs, release docs, feature walkthroughs, changelogs. All system types: frontend, backend, API, mobile, SaaS, enterprise tools, SDKs, automation workflows. | `architecture.md` - System Types Supported, Agent 3 |
| **2. Smart Context Understanding** | Business logic, feature relationships, user personas, workflow dependencies, permissions/roles, edge cases, system limitations, configuration requirements, environment setup, deployment architecture, operational flows. The Five-Persona Writing Model applies PM, technical writer, support engineer, QA engineer, and developer perspectives to every artifact. | `architecture.md` - Agent 3 (Five-Persona Model), Agent 1 |
| **3. Automated Documentation Lifecycle Management** | Staleness detection, feature-level change tracking, Git/PR monitoring, breaking change identification, auto-regeneration, consistency enforcement, semantic versioning, release tagging, branch-based docs, rollback, archived versions, migration guides. | `architecture.md` - Version Control Strategy, Workflows |
| **4. Advanced Version Control Integration** | Git history understanding, commit-to-doc mapping, release summary generation, version comparison, multi-version manuals, staging/production separation, diff-based updates, who/what/why/impact tracking. | `architecture.md` - Version Control Strategy, Agent 2 |
| **5. AI Documentation Intelligence Layer** | Missing doc detection, undocumented API alerts, unclear workflow identification, simplification suggestions, screenshot/video recommendations, duplicate content detection, contradiction detection, onboarding gap alerts, support-heavy area identification, FAQ gap detection, edge case coverage alerts. | `architecture.md` - Documentation Intelligence System, Proactive Intelligence Alerts |
| **6. Multi-Format Documentation Output** | Markdown, HTML, PDF, DOCX, interactive knowledge bases, wiki pages, API references, step-by-step tutorials, embedded walkthroughs, in-app guides, video script drafts, training material. | `architecture.md` - Skills and Modules, `tech-stack.md` - Integration Architecture |
| **7. Dynamic User Manual Generation** | Adapts by user role, permissions, platform (web/mobile/desktop), environment (dev/staging/prod), product edition, language, and feature availability. | `architecture.md` - Agent 5 (Audience Adaptation) |
| **8. MCP / External Platform Integrations** | Notion, Linear, Asana, ClickUp, Confluence, Slack, Jira, GitHub, GitLab, Trello, Zendesk, Intercom, Figma, GitLab Wiki, Google Drive. | `references/mcps.md` - MCP Priority Matrix, `tech-stack.md` - Integration Architecture |
| **9. Workflow Automation Features** | CI/CD pipelines, auto-generation after deployment, PR-based validation, quality scoring, approval workflows, human review tiers, AI-assisted editing, translation workflows, screenshot automation, auto-diagram generation, API schema extraction, interactive documentation agents. | `architecture.md` - Workflow Design, Automation Pipelines, Skills and Modules |
| **10. Enterprise Capabilities** | RBAC, audit logs, compliance tracking (SOC 2, HIPAA, GDPR, ISO 27001, FedRAMP), documentation governance, multilingual, analytics, search optimization, AI semantic search, knowledge graph, usage insights. | `tech-stack.md` - Security, Enterprise Deployment, `architecture.md` - Database Schema |
| **11. Future AI Capabilities** | Conversational manuals, AI support agents, voice-driven documentation, autonomous onboarding agents, self-healing documentation, contextual in-app assistance, AI training simulations, personalized learning flows. | `tech-stack.md` - Future Roadmap |


## What This Skill Produces

Depending on what the user asks for, this skill can help produce:

- A complete system architecture document (use the `docx` skill to render it professionally)
- A focused architecture decision record (ADR) for a specific component
- An agent design for a documentation automation pipeline
- A workflow diagram or step-by-step pipeline specification
- A tech stack recommendation matrix
- An MVP feature breakdown and phased implementation roadmap
- A database schema for a documentation intelligence system
- An integration architecture for connecting to platforms like Notion, Confluence, or Slack


## How to Approach a Request

### 1. Clarify scope before diving in

Documentation systems vary widely in scale. Before designing anything, understand:

- **What is being documented?** (single repo, multi-repo, SaaS product, internal tools, APIs, mobile apps)
- **Who are the audiences?** (end users, developers, admins, enterprise customers)
- **What output formats matter?** (Markdown portal, PDF, DOCX, in-app help, Notion, Confluence)
- **What is the team's Git workflow?** (GitHub/GitLab, branch strategy, PR process)
- **What is the maturity level?** (MVP from scratch, improving existing docs, full enterprise platform)
- **What platforms need to receive documentation?** (Notion, Confluence, Slack, custom portal)

If the user's request is already specific (e.g., "design the agent architecture for my GitHub-connected docs system"), skip straight to the relevant section.

### 2. Match depth to the request

| Request type | What to produce |
|---|---|
| "Give me the full architecture" | Read both reference files; produce a comprehensive design across all 15 components |
| "What agents do I need?" | Read `architecture.md` Agent Roster section; describe the 7 core agents with tailored recommendations |
| "How do I version my docs?" | Read `architecture.md` Version Control section; explain docs-as-code + SemVer strategy |
| "What tech stack should I use?" | Read `tech-stack.md`; produce a recommendation matrix tailored to their constraints |
| "Build me the architecture doc as a Word file" | Read both reference files + invoke the `docx` skill to produce a professional .docx |
| "MVP vs full platform - what's the difference?" | Read `tech-stack.md` MVP section; produce a phased feature breakdown |

### 3. Always tailor to the user's context

The architecture in the reference files is the canonical blueprint. Adapt it:

- For a **startup or small team**: Lead with the MVP feature set. Simplify to 3 agents, a single Postgres DB, and GitHub + Notion as the first integrations. Skip enterprise-only features.
- For an **enterprise**: Emphasize RBAC, audit logging, SSO, compliance (SOC 2 / HIPAA), multi-tenancy, and the private cloud deployment option.
- For a **developer-focused product**: Emphasize API documentation generation (OpenAPI/GraphQL/gRPC), the docs-as-code model, and IDE/GitHub Copilot integration.
- For a **non-technical product team**: Emphasize the human review workflow, audience adaptation, and the documentation portal UI. De-emphasize agent internals.


## Core Principles to Always Apply

These principles come from how the best documentation teams in the industry (Stripe, Linear, Anthropic, Notion) operate. Weave them into every recommendation:

**Audience-first.** Every documentation artifact targets a specific persona. The same feature generates different content for an end user vs. a developer vs. an admin. Never design a "one size fits all" documentation system.

**Task-oriented content.** Documentation should explain how to do things, not just describe what things are. The generator agents should be instructed to write in this mode: context → prerequisites → numbered steps → expected outcome.

**Structure for scanning, not reading.** Users arrive with a specific question. The system should produce documentation with descriptive headings, searchable content, and cross-links - never walls of prose.

**Online-first, format-agnostic.** Markdown is the source of truth. From Markdown, render everything else (HTML, PDF, DOCX, Notion blocks, Confluence wiki markup). Never let a binary format (PDF, DOCX) be the canonical source.

**Documentation is software.** It should be versioned (Git), tested (quality gates), reviewed (approval workflows), deployed (CI/CD pipelines), and monitored (analytics + staleness alerts). Every component of the architecture reflects this.

**Continuous improvement over perfection.** The system does not need to generate perfect documentation on day one. It needs a feedback loop: usage analytics, search gap analysis, support ticket correlation, and user ratings should continuously feed back into generation quality.


## Default Output Format

**All generated user manuals are Markdown files by default.** This is non-negotiable.

Markdown is the source of truth. PDF, DOCX, Confluence wiki, Notion pages, and any
other format are rendered outputs derived from Markdown. Never design a system where
a binary format (PDF, DOCX) is the primary output.

When asked to generate documentation, always produce:
- A `.md` file as the primary deliverable
- A file named after the feature: `feature-name.md` or `how-to-goal.md`
- A file that can be dropped directly into any docs portal without modification

When the user explicitly requests a different format (Word, PDF, Confluence), render
from the Markdown source using the appropriate skill (docx, pdf) or the Atlassian MCP.


## PRD to User Manual Workflow

When a user wants to generate documentation from existing product specifications,
follow this workflow. See `examples/05-prd-to-manual.md` for a full worked example.

**Inputs required:**
- PRD/URD from wherever the team stores them (Linear, Confluence, Jira, Notion, Docmost, GitHub, Asana, ClickUp, or pasted directly)
- Functional requirements/acceptance criteria from whatever issue tracker the team uses (Linear, Jira, GitHub Issues, Asana, ClickUp, etc.)
- Merged code changes from GitHub (fetch via GitHub MCP)

**The three-source reconciliation:**

1. Read the PRD: extract goals, user stories, scope, acceptance criteria
2. Read the FRs: extract acceptance criteria, user flows, edge cases, error states
3. Read the code: extract what actually shipped (diffs, tests, API changes, UI labels)
4. Reconcile: compare specification against reality. Surface deviations and gaps.
5. Generate the user manual: document what shipped, not what was specified
6. Generate the gap report: document deviations and create tracking issues

**Key rule:** Never document a feature that did not ship. The gap report tracks missing
features. The user manual reflects only what users can actually do today.

**Outputs:**
- `feature-name.md` - User manual for the shipped feature (Markdown)
- `gap-report-feature-name.md` - Deviations and missing items (Markdown)
- Linear or Jira issues created for each gap automatically


## One-Shot Generation Modes

When a user wants to generate documentation for an existing codebase, choose the mode
that matches their scope. All modes produce Markdown files as the default output.

| Mode | Trigger phrase | Example output |
|---|---|---|
| Full system scan | "document the entire platform", "generate all docs for this repo" | One doc per module + overview index |
| Module-by-module | "document the drug inventory module", "generate docs for appointments" | Per-audience docs for that module |
| Feature-by-feature | "document the export feature", "generate docs for the follow-up flow" | One feature guide + gap report |
| PRD-driven | "read this PRD and generate docs", "compare this PRD to the code" | User manual + gap report + tracked issues |
| Controller-grouped | "document everything under AppointmentController" | One guide covering all endpoints in that controller |

### Full System Scan

Use when generating documentation for a codebase for the first time.

```
Step 1: Inventory all modules from route files
Step 2: For each module, identify controllers, services, models, policies, tests
Step 3: Run ingestion sequence per module (see architecture.md - Laravel/PHP Stack Ingestion)
Step 4: Generate per-module, per-audience Markdown files
Step 5: Generate an index (docs/overview.md) linking all module docs
```

For a Laravel application, group routes by controller prefix to discover modules.
For a Node.js application, group by router file or directory.
For a Python application, group by Blueprint or APIRouter.

### Module-by-Module

Use when one functional area needs complete documentation before moving to the next.

```
Prompt: "Document the drug inventory module. Generate guides for site staff,
         site admins, and sponsors. Output as Markdown files."

Ingestion sources: controller files, service layer, models, policies, feature tests, any PRDs
Outputs:
  docs/drug-inventory/overview.md
  docs/drug-inventory/staff-guide.md
  docs/drug-inventory/admin-guide.md
  docs/drug-inventory/sponsor-guide.md
  docs/drug-inventory/permissions.md
```

### Feature-by-Feature

Use when a specific feature just shipped and needs documentation immediately.

```
Prompt: "Document the appointment follow-up flow that just shipped.
         The PRD is prd-612-appointment-chains-followup-view.md."

Ingestion sources: PRD file + AppointmentController + DB migration + tests
Outputs:
  docs/appointments/follow-up-appointments.md
  docs/appointments/gap-report-followup-v1.md
  (Linear issues created for each gap found)
```

### PRD-Driven

Use when specs exist in any external tool and need to be reconciled against the codebase.

**Source discovery - always ask first:**
> "Where do your PRDs/requirements live? (e.g. Linear, Confluence, Jira, Notion, Docmost, GitHub, Asana, ClickUp, or paste them directly)"

Once the user tells you their tools, use the appropriate MCP connectors and adapt the workflow accordingly.

```
Step 1: Discover spec sources
        Ask: Where are PRDs/URDs stored? (Linear docs, Confluence pages,
        Jira epics, Notion databases, Docmost, GitHub Issues, Asana, ClickUp, etc.)
        Ask: Where are acceptance criteria / functional requirements tracked?
        (May be the same tool or a different one)

Step 2: Fetch the PRD/URD using the appropriate connector
        Linear MCP: search_documentation, get_document
        Atlassian MCP: getConfluencePage, searchConfluenceUsingCql, getJiraIssue
        Notion MCP: notion-fetch, notion-search
        GitHub MCP: issues, discussions
        Asana / ClickUp / other: use available MCP or ask user to paste content
        Extract: goals, user stories, acceptance criteria, scope, non-goals

Step 3: Fetch functional requirements / acceptance criteria
        Same tool or separate issue tracker - use whatever connector is available
        Extract: per-story acceptance criteria, user flows, edge cases, error states

Step 4: Map to code
        Identify controllers, routes, models, services, and DB migrations

Step 5: Run three-source reconciliation
        Spec intent + acceptance criteria + code reality

Step 6: Generate user manual for what shipped (Markdown, always)

Step 7: Generate gap report for what was specified but not shipped

Step 8: Create tracked issues for each gap in the team's issue tracker
```

**Supported spec sources:**

| Tool | MCP Connector | What to fetch |
|---|---|---|
| Linear | Linear MCP | Documents (PRDs/URDs), issues (acceptance criteria), epics |
| Confluence | Atlassian MCP | Pages (PRDs), spaces, inline comments |
| Jira | Atlassian MCP | Epics, stories, acceptance criteria fields |
| Notion | Notion MCP | Pages, databases, linked docs |
| Docmost | Docmost MCP (if available) | Pages, workspaces |
| GitHub | GitHub MCP | Issues, discussions, wiki pages, markdown files in repo |
| Asana | Asana MCP | Projects, tasks, descriptions |
| ClickUp | ClickUp MCP | Docs, tasks, descriptions |
| Google Drive | Google Drive MCP | Docs, spreadsheets |
| Plain text / paste | No MCP needed | User pastes content directly into the prompt |

See `examples/05-prd-to-manual.md` for the complete PRD-to-manual workflow.
See `examples/06-clinical-healthcare-saas.md` for a full clinical SaaS example using all four modes.

### Laravel / PHP Specific

For Laravel applications, use the ingestion sequence in `references/architecture.md`
under "Laravel / PHP Stack Ingestion". Key sources: route files, FormRequests
(validation rules = field docs), Eloquent Models (fillable = editable fields),
Policy files (permission matrix), Pest feature tests (behavioral specification),
PRD files in repo root, CLAUDE.md as domain glossary.


## Designing the Agent System

When helping a user design their documentation agent system, always start from the seven canonical agents and decide which ones are needed for their scope. See `references/architecture.md` for the full agent roster.

For a **minimal viable system** (MVP), three agents cover 80% of the value:

1. **Commit Intelligence Agent** - Watches Git, understands what changed, identifies documentation impact.
2. **Documentation Generator Agent** - Turns change manifests into draft documentation using LLM + RAG.
3. **Distribution Agent** - Syncs approved documentation to target platforms (Notion, Confluence, GitHub Pages).

Add agents as the system matures:

- Add **Codebase Analyst Agent** when the codebase grows complex enough that ad-hoc diff analysis misses structural changes.
- Add **Quality Agent** when documentation volume makes manual quality review impractical.
- Add **Audience Adaptation Agent** when more than two personas need meaningfully different documentation.
- Add **Feedback Intelligence Agent** when you have enough documentation traffic to generate meaningful analytics.


## Designing the Workflow

A documentation workflow always has three phases:

**Ingestion** - Something changes (code commit, PR merge, deploy, scheduled scan). The system detects it and queues it for processing.

**Processing** - Agents analyze the change, consult the Knowledge Graph, generate or update documentation drafts, score quality, and route for approval if needed.

**Distribution** - Approved documentation is published to all target platforms. Platform sync is verified. Failures trigger alerts and retry logic.

When designing a workflow for a user, always be explicit about:
- What triggers the workflow (Git events, scheduled jobs, manual trigger)
- Where human review fits in (auto-approve minor changes, require review for breaking changes)
- What happens on failure (retry, alert, rollback)


## Output Format Guidance

When producing a full architecture document for the user, use this structure:

1. Executive Summary (value propositions, core principles)
2. Five-Tier Architecture Overview (Ingestion → Intelligence → Generation → Management → Distribution)
3. Agent Roster (customized to their scope and team size)
4. Skills & Modules (composable capabilities each agent can invoke)
5. Workflow Design (primary generation, PR validation, scheduled audit, human review)
6. Version Control Strategy (docs-as-code, SemVer, branch strategy, rollback)
7. Documentation Intelligence (Knowledge Graph, RAG, staleness detection, proactive alerts)
8. Database & Schema Design (tailored to their scale)
9. Automation Pipelines (CI/CD gate, scheduled jobs, quality scoring)
10. Integration Architecture (source systems + distribution platforms)
11. Tech Stack Recommendations (with alternatives where relevant)
12. Scalability & Security (appropriate to their deployment model)
13. Enterprise Deployment Options (if relevant)
14. Roadmap (MVP → growth → enterprise phases)
15. MVP vs Enterprise Feature Breakdown

If the user wants a Word document, invoke the `docx` skill after designing the architecture. The reference files contain the full content for all 15 sections.


## Quick Reference: Key Design Decisions

| Decision | Recommended Default | Why |
|---|---|---|
| Source of truth format | Markdown (MDX) | Version-controllable, renders everywhere, format-agnostic |
| Primary database | PostgreSQL + pgvector | Relational integrity + vector search in one DB |
| Event bus | Apache Kafka | Ordered, high-throughput, replay-capable |
| Knowledge store | Neo4j | Native graph for feature-API-documentation relationships |
| LLM for generation | Claude (long context) | Best for large codebase analysis and long-form writing |
| Embedding model | text-embedding-3-large | High quality, widely supported |
| Documentation portal | Next.js + MDX | Fast, Git-native, Markdown-first |
| CI/CD integration | GitHub Actions / GitLab CI | Native Git integration, easy webhook setup |
| Human review trigger | Risk-level threshold | Auto-approve patches; require review for breaking changes |
| Versioning | SemVer (major/minor/patch) | Maps naturally to breaking changes, features, and fixes |
