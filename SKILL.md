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


## Documentation Standards

**A user manual is organized around user intentions, not product features.**

The difference between good documentation and great documentation is the unit of
organization. Good docs say "here is Case Creation, here is Case Management, here is
Reassignment." Great docs say "here is how to get a specialist's opinion on a complex
patient case" - and every step flows into the next, every completed action points
forward, every page knows why the user arrived there.

Study how Linear (linear.app/docs), Notion, and Stripe document their products.
Their pages follow user journeys. Sections connect. Steps link forward.
That is the standard.

Every generated manual must meet the Documentation Standards (Sections 20, 21, and 22 of
`references/writing-guide.md`) and pass the completeness checklist before delivery.
A manual with any unchecked item is a draft.

### Rule 0: Map user journeys before writing a single section

Before generating any documentation, produce a journey map for each primary persona.
A journey map is a numbered sequence from the user's trigger (why they need the product
today) to their completed outcome. Each step in the journey map becomes a page or section.
Steps that connect to other steps get explicit forward links.

```
Example journey map - New team member joining a SaaS product:
1. TRIGGER: Received invite email, needs to start contributing
2. Accept invitation, create account or log in
3. Understand the workspace layout and navigation
4. Find the team they were added to
5. WAITING STATE: understand their role and what they can do
6. Complete their first task (varies by product)
7. Invite a colleague to the team
8. Manage their notification settings

Each numbered step = one documentation section.
Step 5 is a waiting state - document it explicitly.
```

Replace this example with the actual primary persona journey for the product
you are documenting. The format stays the same for any product or industry.

The journey map is the manual's spine. Write it first.

### Rule 0b: Process-oriented, not feature-oriented

Every section answers "what is the user trying to accomplish?" not "what does this
feature do?" The entry point, steps, outcome, and next steps all follow from the user's
intention, not from the feature's capabilities.

**Feature-oriented (wrong):**
> "The Reassign function allows panel leaders and case owners to transfer case
> responsibility to another panel member."

**Process-oriented (right):**
> "You are here because a case needs to be handled by a different specialist - either
> because the current reviewer is unavailable, or because the case requires a different
> specialty. Here is how to reassign it and what happens next."

### Rule 1: Never say "Chapter" - Use descriptive workflow names

"Chapter" sounds like a textbook. Use descriptive names that tell the reader what
they can do or understand. Use a PART structure to group sections by frequency:

```
PART 1 - Introduction
  Introduction
  Quick Start          <- always second, always present
  Roles and Access Control

PART 2 - Daily Workflows
  [Primary workflow 1]
  [Primary workflow 2]
  ...

PART 3 - Operational Features
  [Less-frequent features]

PART 4 - Analytics and Administration
  [Admin-only content]

PART 5 - Troubleshooting and Reference
  Troubleshooting
  Reference and Appendix
```

Section naming guide: "Case Creation Workflow" not "Creating a Case Review".
"Panel Management" not "Panels". "Roles and Access Control" not "Roles and Permissions".

### Rule 2: Always generate separate documents for separate audiences

One document for all audiences is a specification, not a user manual. Always produce:

- **User Guide** - daily tasks, what buttons do, what happens next, common workflows.
  Zero admin content. Zero configuration. Zero internal details.
- **Admin and Configuration Guide** - panel settings, timeout configuration, permission
  management, audit logs, analytics dashboards, assignment logic.

The test: "Would a new end user on their first day need to read this?" If no, it belongs
in the Admin Guide.

### Rule 3: Lock terminology before writing

Before the first section, create a terminology lock table. Choose one term per concept.
Use the term that appears in the UI. Never use synonyms or alternate names.

```
| Concept | Use this | Never use |
|---|---|---|
| A shared team space | Workspace | Organization, Account, Team |
| The person who created it | Owner | Creator, Admin, Founder |
```

Replace these with the actual concepts from the product you are documenting.
Use the term visible in the UI - never a synonym from the codebase.

If you catch yourself reaching for a synonym, the terminology is not locked. Fix it.

### Rule 4: Quick Start is always the second section

Immediately after the Introduction, before any detailed sections. 10 steps or fewer
per persona. Steps only - no explanations, no prerequisites, no error states.

### Rule 5: Structure by frequency of use

Daily workflows first. Exceptions second. Admin operations third. Reference last.
A new user should be able to skip PART 4 entirely and still use the product effectively.

### Rule 6: No internal field names in user documentation

Never let database column names, config flags, or code identifiers appear in user docs.
Translate every technical identifier to plain language before writing.

- Never: "If `prevent_non_matched_specialty` is enabled..."
- Always: "If the panel is configured to restrict cases to matching specialties..."

### Rule 7: Keep standard UX patterns brief

Typing indicators, message likes, draft auto-save, pagination - one or two sentences.
Full task blocks only for features with product-specific behavior, permission gates,
irreversible consequences, or multi-step branching workflows.

### Rule 8: No implementation details in user documentation

Email subject templates, API endpoints, database field names, cache durations, template
engine references, cron schedules - these belong in developer docs, not user manuals.

### Every feature section must contain

- Audience preamble (who this is for) and outcome statement
- Overview paragraph - narrative, not a bullet list
- At least one scenario in blockquote format
- All core tasks: step-by-step, second-person active voice, one action per step
- Outcome after each task ("What happens next")
- Error handling for each task ("If something goes wrong")
- Best practices (3-5 specific, actionable items)
- Limits and constraints table
- System messages and toasts reference table
- Related topics links

### Rule 9: "What to do next" after every completed action

After every task completion, include 2-4 forward paths. Link to the exact section for
each. Order by frequency - most common next action first. Always include the "something
went wrong" path. This is what connects isolated pages into a flow.

### Rule 10: Entry point triggers

Every section opens with one sentence explaining what situation brings a user here.
"You are here because..." or woven into the overview paragraph. A user arriving from
search must immediately know they are in the right place.

### Rule 11: Progressive disclosure

Layer 1 = simple first-time path only. Layer 2 = common variations. Layer 3 = edge
cases and admin config (Admin Guide only, never in User Guide Layer 1).

### Rule 12: Decision forks at branching points

When different roles or configurations produce different screens, fork the steps
explicitly: "If you see X... / If you see Y..." Never write steps that silently fail
for half the audience.

### Rule 13: Waiting states are documented

Every workflow has a waiting period. Document it. Tell the user what to do, what to
watch for, and what to do if the wait is too long.

### Rule 14: Inline links woven into steps

Links belong at the moment they are needed - inside step 4, not in a "Related topics"
footer after step 10. If a user needs a link at step 4, put it at step 4.

### The split rule

More than 3 major tasks or its own permission model = its own section, not a sub-section.

### The glossary rule

Define every product-specific term on first use AND in the Reference glossary.
A reader skipping to any section must understand every term without reading from the start.

---

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
| Module-by-module | "document the billing module", "generate docs for user management" | Per-audience docs for that module |
| Feature-by-feature | "document the export feature", "generate docs for the invite flow" | One feature guide + gap report |
| PRD-driven | "read this PRD and generate docs", "compare this PRD to the code" | User manual + gap report + tracked issues |
| Controller-grouped | "document everything under PaymentController" | One guide covering all endpoints in that controller |

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
Prompt: "Document the billing module. Generate guides for end users,
         admins, and support staff. Output as Markdown files."

Ingestion sources: controller files, service layer, models, policies, feature tests, any PRDs
Outputs:
  docs/billing/overview.md
  docs/billing/user-guide.md
  docs/billing/admin-guide.md
  docs/billing/support-guide.md
  docs/billing/permissions.md
```

### Feature-by-Feature

Use when a specific feature just shipped and needs documentation immediately.

```
Prompt: "Document the team invitation flow that just shipped.
         The PRD is in Confluence. The Linear epic is INV-142."

Ingestion sources: spec from Confluence/Linear + InvitationController + DB migration + tests
Outputs:
  docs/teams/invite-members.md
  docs/teams/gap-report-invitations-v1.md
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
See `examples/06-existing-codebase-zero-docs.md` for generating from any existing codebase with no docs.
See `examples/07-output-showcase.md` for what the output actually looks like.

### Laravel / PHP Specific

For Laravel applications, use the ingestion sequence in `references/architecture.md`
under "Laravel / PHP Stack Ingestion". Key sources: route files, FormRequests
(validation rules = field docs), Eloquent Models (fillable = editable fields),
Policy files (permission matrix), Pest feature tests (behavioral specification),
PRD files in repo root, CLAUDE.md as domain glossary.


## Designing an ADUMAS System

When the user wants to design a documentation automation system (agents, pipelines,
database schema, CI/CD integration) rather than generate documentation directly, read
`references/architecture.md`. It covers the full agent roster, five-tier architecture,
workflow design, version control strategy, database schema, automation pipelines, and
the 15-section architecture document structure.

For tech stack decisions, deployment options, scalability, and the MVP vs enterprise
breakdown, read `references/tech-stack.md`.

For the MCP integration setup (GitHub, Confluence, Linear, Jira, Slack, Notion, Figma),
read `references/mcps.md`.

For common planning, build, and operations mistakes, read `references/anti-patterns.md`.

For the 10 key architecture decisions (LLM choice, database, versioning strategy,
deployment model, human review triggers), read `references/decision-guide.md`.
