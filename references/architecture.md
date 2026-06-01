# Architecture Reference

This file is the primary architectural reference for ADUMAS. Read the section(s) relevant to the user's question.

## Table of Contents

1. [Five-Tier Architecture](#five-tier-architecture)
2. [Agent Roster](#agent-roster)
3. [Skills & Modules](#skills--modules)
4. [Workflow Design](#workflow-design)
5. [Version Control Strategy](#version-control-strategy)
6. [Documentation Intelligence System](#documentation-intelligence-system)
7. [Database & Schema Design](#database--schema-design)
8. [Automation Pipelines](#automation-pipelines)

---

## System Types Supported

ADUMAS is designed to generate accurate documentation for any software system type:

| System Type | Ingestion Method | Documentation Produced |
|---|---|---|
| Frontend web apps | UI scraping, component analysis, user flow tracing | End-user guides, feature walkthroughs, in-app help |
| Backend services | API scanning, route analysis, middleware inspection | Admin guides, SOPs, operational runbooks |
| REST / GraphQL / gRPC APIs | OpenAPI, GraphQL schema, proto file parsing | API references, SDK guides, integration tutorials |
| Mobile apps (iOS, Android) | Source code analysis, screen flow mapping | User manuals, onboarding guides, platform-specific guides |
| SaaS platforms | Multi-tenant config analysis, role/permission mapping | Role-based user manuals, admin guides, enterprise configs |
| Internal enterprise tools | Workflow analysis, integration mapping | SOPs, operational procedures, onboarding guides |
| SDKs and developer libraries | Docstring extraction, usage pattern analysis | API references, code examples, quickstart guides |
| Automation workflows | DAG/workflow graph analysis, trigger/action mapping | Runbooks, SOP guides, troubleshooting guides |

---

## Five-Tier Architecture

ADUMAS is organized into five tiers that together form an end-to-end documentation intelligence platform:

| Tier | Name | Role |
|---|---|---|
| 1 | Ingestion Layer | Collects raw signals: source code, Git history, API schemas, UI flows, commit messages, PR diffs, release notes, runtime logs |
| 2 | Intelligence Layer | AI agents analyze ingested signals, extract meaning, detect changes, understand business logic, build knowledge graph |
| 3 | Generation Layer | Transforms structured knowledge into documentation artifacts: manuals, guides, SOPs, API refs, FAQs, changelogs |
| 4 | Management Layer | Versioning, approval workflows, quality scoring, audience adaptation, translation, lifecycle management |
| 5 | Distribution Layer | Publishes and syncs documentation to all target platforms: Notion, Confluence, Slack, GitHub Pages, in-app widgets |

**Data flow:**

```
Source Systems (Git, APIs, UI, DBs, Logs)
  → Ingestion Orchestrator
    → Event Bus (Kafka)
      → Intelligence Agents (LLM + RAG + Knowledge Graph)
        → Generation Engine (audience-specific artifacts)
          → Management Layer (versioning, quality, approval)
            → Distribution Engine (platform adapters)
              → Analytics & Feedback (loop back to Intelligence Layer)
```

---

## Agent Roster

Seven specialized agents cover the full documentation lifecycle. For an MVP, start with agents 2, 3, and 6.

### Agent 1: Codebase Analyst Agent
- **Purpose:** Deep static analysis of source code repositories.
- **Capabilities:** AST parsing, dependency graph extraction, feature detection, API endpoint discovery, permission/role mapping, business logic inference from comments and naming conventions.
- **Inputs:** Git repository clone, language-specific parsers (Python, JS/TS, Go, Java, Ruby).
- **Outputs:** Structured feature manifest, API schema catalog, permission matrix, architecture summary.
- **Trigger:** Repository push, new PR opened, scheduled nightly scan.
- **MVP needed?** Add when codebase grows complex enough that commit-level analysis misses structural changes. Optional in MVP.

### Agent 2: Commit Intelligence Agent ⭐ MVP
- **Purpose:** Analyzes Git commit history and PR diffs to understand what changed and why.
- **Capabilities:** Semantic diff analysis, breaking-change detection, author intent inference from commit messages, issue/ticket cross-referencing (Jira, Linear, GitHub Issues).
- **Inputs:** Git log, PR diffs, linked issue descriptions.
- **Outputs:** Change manifest, impacted documentation list, release summary draft, migration guide triggers.
- **Trigger:** PR merge, tag creation, release branch creation.

### Agent 3: Documentation Generator Agent ⭐ MVP
- **Purpose:** Transforms structured knowledge into human-readable documentation artifacts.
- **Capabilities:** Multi-persona output generation (user/admin/developer/enterprise), format adaptation, style consistency enforcement, task-oriented writing structure, the five-persona writing model (see below).
- **Inputs:** Feature manifest, change manifest, audience profile, existing documentation for context.
- **Outputs:** User manuals, admin manuals, technical documentation, API references, SOPs, onboarding guides, troubleshooting guides, FAQs, release documentation, feature walkthroughs, changelogs, embedded walkthroughs, training material, video script drafts.
- **Trigger:** Change manifest update, new feature detection, scheduled generation run.

**The Five-Persona Writing Model**

Every generated documentation artifact is reviewed internally through five lenses before publication.
This produces documentation that reads as if a full cross-functional team wrote it together:

| Persona | Lens Applied |
|---|---|
| Product Manager | Is the "why" explained? Does this match the intended user goal? Are success criteria implicit in the guide? |
| Technical Writer | Is the structure task-oriented? Are steps numbered? Is there an outcome statement? Is the language plain and second-person? |
| Support Engineer | Are the most common failure modes covered? Are error messages included? Is the troubleshooting section complete? |
| QA Engineer | Are edge cases documented? Are prerequisites listed? Are known limitations stated? Is there a "what if this goes wrong" path? |
| Developer | Is the technical accuracy verified against the codebase? Are code examples correct? Are API parameters accurate? |

The Generator Agent applies this model via five sequential prompt passes, each refining the output
from a different perspective before a final consolidation pass produces the published artifact.

### Agent 4: Documentation Quality Agent
- **Purpose:** Continuously audits documentation for quality, completeness, and consistency.
- **Capabilities:** Contradiction detection, staleness scoring, missing documentation identification, terminology inconsistency, readability scoring (Flesch-Kincaid), duplicate content detection.
- **Inputs:** All documentation artifacts, feature manifest, terminology glossary.
- **Outputs:** Quality score reports, issue tickets, suggested edits, prioritized improvement backlog.
- **Trigger:** After each generation run, weekly scheduled audit, user search failure events.
- **MVP needed?** Add when documentation volume makes manual review impractical.

### Agent 5: Audience Adaptation Agent
- **Purpose:** Personalizes documentation based on user role, permissions, platform, environment, and product edition.
- **Capabilities:** Role-based content filtering, technical depth adjustment, language simplification, feature-flag-aware content hiding, multilingual translation orchestration, environment-aware documentation (dev/staging/production variants).
- **Inputs:** User role/permission profile, feature flags, locale/language, product edition config, deployment environment, platform (web/mobile/desktop).
- **Outputs:** Role-specific documentation views, translated documents, in-app contextual help snippets, environment-specific setup guides, platform-specific guides.
- **Trigger:** Documentation publication, user session context (for in-app delivery).
- **MVP needed?** Add when more than two personas need meaningfully different documentation.

**Adaptation Dimensions**

| Dimension | Example Variation |
|---|---|
| User role | Admins see SSO configuration; end users see login instructions |
| Permissions | Features unavailable to the user are hidden from their documentation |
| Platform | Mobile users see touch-based instructions; desktop users see keyboard shortcuts |
| Environment | Dev setup guide differs from staging and production deployment guide |
| Product edition | Free tier users see features available to them; enterprise users see advanced configs |
| Language | Full multilingual output via Translation Orchestrator; RTL language support |
| Feature availability | Feature-flagged features are hidden in documentation until the flag is enabled |

### Agent 6: Distribution & Sync Agent ⭐ MVP
- **Purpose:** Publishes and synchronizes approved documentation to all connected platforms.
- **Capabilities:** Platform-specific formatting (Notion blocks, Confluence wiki markup, Slack rich text, GitHub Markdown), conflict resolution, incremental sync, rollback on failure.
- **Inputs:** Approved documentation artifacts, platform connection configs, sync manifests.
- **Outputs:** Published pages, updated wiki entries, Slack notifications, GitHub Pages deployments.
- **Trigger:** Documentation approval, scheduled sync, platform webhook.

### Agent 7: Feedback Intelligence Agent
- **Purpose:** Learns from user behavior to continuously improve documentation quality.
- **Capabilities:** Search query analysis (gap identification), support ticket parsing, NPS/rating signal processing, heatmap analysis, conversational query answering with feedback capture.
- **Inputs:** Analytics events, search logs, support ticket exports, user ratings.
- **Outputs:** Documentation gap report, prioritized improvement recommendations.
- **Trigger:** Continuous streaming; weekly batch summarization.
- **MVP needed?** Add when you have enough documentation traffic to generate meaningful analytics.

---

## Skills & Modules

Composable AI capabilities that agents invoke on demand:

| Skill | Function | Invoked By |
|---|---|---|
| Code-to-Prose | Converts annotated code blocks into plain-English explanations tailored to target audience | Generator Agent |
| API Schema Extractor | Parses OpenAPI/GraphQL/gRPC schemas → structured API reference docs | Codebase Analyst |
| Diagram Generator | Produces Mermaid/PlantUML diagrams from architecture code, DB schemas, workflow configs | Generator Agent |
| Screenshot Automator | Uses headless browser to capture annotated UI screenshots for documentation | Generator Agent |
| Changelog Composer | Transforms commit manifests → user-facing changelogs per SemVer | Commit Intelligence |
| Migration Guide Builder | Detects breaking changes → auto-drafts migration guides for API consumers | Commit Intelligence |
| Glossary Enforcer | Detects and standardizes terminology across all documentation artifacts | Quality Agent |
| Translation Orchestrator | Routes documentation to translation APIs (DeepL, Google Translate) and validates output | Audience Adaptation |
| SOP Formatter | Structures raw workflow descriptions into standardized SOPs with numbered steps, roles, expected outcomes | Generator Agent |
| Search Index Builder | Generates semantic embeddings + Elasticsearch indices for all documentation artifacts | Distribution Agent |
| In-App Help Builder | Packages documentation snippets as contextual tooltips and walkthroughs for product UIs | Distribution Agent |
| Video Script Drafter | Converts step-by-step guides into narration scripts and storyboards for tutorial videos | Generator Agent |
| Embedded Walkthrough Builder | Packages documentation as interactive in-product step-by-step flows (tooltip sequences, product tours) using Shepherd.js or Intercom-compatible JSON | Distribution Agent |
| Training Material Generator | Converts user manuals and SOPs into structured training modules with knowledge-check questions, learning objectives, and completion criteria | Generator Agent |
| FAQ Synthesizer | Clusters support tickets and search queries into FAQ entries; maintains FAQ freshness as product evolves | Quality Agent |
| Feature Walkthrough Builder | Generates step-by-step guided tours of new features, narrated in plain language with screenshots and expected outcomes | Generator Agent |

---

## Workflow Design

### Primary Documentation Generation Workflow

1. Developer pushes code or merges a PR.
2. Git Webhook fires → Ingestion Orchestrator receives commit metadata and diff.
3. Ingestion Orchestrator publishes a `ChangeEvent` to Kafka topic `doc.changes`.
4. Commit Intelligence Agent consumes the event, performs diff analysis, publishes a `ChangeManifest`.
5. Codebase Analyst Agent (if structural changes detected) re-scans affected modules → updates Feature Manifest.
6. Documentation Generator Agent consumes both manifests → generates draft updates for affected sections.
7. Documentation Quality Agent scores drafts, flags issues.
8. If auto-approved (below risk threshold): Distribution Agent immediately syncs to platforms.
9. If high-impact: Draft queued for human review → reviewer notified → approval triggers distribution.
10. Feedback Intelligence Agent monitors post-publication metrics → reports improvements.

### PR-Based Documentation Validation Workflow

- When a PR is opened, the system analyzes the diff automatically.
- If documentation-impacting changes are detected, a Documentation Review comment is posted on the PR with a quality gate status.
- Quality gate configured as hard block (PR cannot merge without docs update) or soft warning.
- Documentation changes can be proposed directly in the PR as file diffs (docs-as-code model).

### AI-Assisted Editing Workflow

Human reviewers do not edit raw drafts. They work with an AI-assisted editor:

1. The Generator Agent produces a draft. Quality Agent scores it and annotates issues inline.
2. The reviewer opens the documentation in the editing interface.
3. For each Quality Agent annotation, the editor suggests a specific rewrite. The reviewer accepts, modifies, or rejects.
4. The reviewer can also invoke the Generator Agent on a specific section ("rewrite this in plain language" or "add a troubleshooting section for this step").
5. The accepted version is stored as the canonical draft. All AI suggestions and human decisions are logged in the audit trail.

This preserves human judgment while eliminating blank-page writing for every documentation update.

### Interactive Documentation Agent Workflow

An Interactive Documentation Agent is embedded in the documentation portal (and optionally in-product):

- Users ask questions in natural language: "How do I export data as a CSV for a filtered view?"
- The agent retrieves the most relevant documentation sections via semantic search.
- If the answer is found, it is returned with a citation and a link to the full page.
- If the answer is not found, the agent logs the query as a documentation gap and triggers the FAQ Synthesizer to draft a candidate answer for review.
- The agent can also walk users through multi-step tasks interactively, confirming each step before proceeding.

This creates a self-improving loop: unanswered questions become new documentation.

### Human Review & Approval Tiers

| Change Type | Approval Mode |
|---|---|
| Minor text updates (typo, clarification) | Auto-publish - no review required |
| New feature documentation | Single reviewer approval |
| API breaking changes documentation | Two-reviewer approval + tech lead sign-off |
| Release documentation | Product Manager + Engineering Lead |
| Compliance-sensitive documentation | Legal/compliance review gate added |

### Scheduled Audit Jobs

| Job | Schedule |
|---|---|
| Full Codebase Scan | Nightly at 2 AM UTC |
| Staleness Audit | Every 6 hours |
| Search Gap Analysis | Daily at 6 AM UTC |
| Support Ticket Sync | Every 4 hours |
| Platform Sync Health Check | Every 15 minutes |
| Translation Queue Processing | Every 2 hours |
| Archive Old Versions | Monthly |

---

## Version Control Strategy

### Docs-as-Code Model

All documentation source files live alongside application code in the same Git repository (or a dedicated docs repository linked via Git submodule). This means:

- Documentation changes are tracked in the same Git history as code changes.
- PRs can include both code and corresponding documentation updates.
- Documentation can be branched, tagged, and rolled back exactly like code.
- CI/CD pipelines can validate documentation quality as part of the build.

### Semantic Versioning for Documentation

| Version Change | Trigger |
|---|---|
| Major (v2.0.0) | Breaking API change, complete product redesign, major workflow overhaul |
| Minor (v1.1.0) | New feature addition, new guide section, significant enhancement |
| Patch (v1.0.1) | Typo fix, clarification, screenshot update, minor correction |

### Branch Strategy

- `main` - Production documentation, always published to primary documentation site.
- `release/vX.Y` - Version-specific documentation maintained in parallel for users on older versions.
- `feature/*` - Draft documentation for features in development, not yet published.
- `staging` - Pre-production documentation environment.

### Rollback

Because all documentation artifacts are stored in S3 with versioning enabled and tracked in Git, any documentation deployment can be rolled back in under 60 seconds via CLI or Management Console.

---

## Documentation Intelligence System

### Knowledge Graph (Neo4j)

Captures relationships between all system entities:

- Features ↔ code modules that implement them
- Code modules ↔ APIs they expose
- APIs ↔ documentation sections that describe them
- Documentation sections ↔ user personas that consume them
- Commit events ↔ features, APIs, and documentation they impact

This graph answers: *"Which documentation sections need updating because function X was modified in commit Y?"* - automatically, without human intervention.

### Retrieval-Augmented Generation (RAG)

All documentation generation uses RAG to ground LLM outputs in verified codebase facts:

- Source code snippets, function signatures, and docstrings are embedded and stored in pgvector.
- When generating documentation, the LLM retrieves the most semantically relevant code context before writing.
- This prevents hallucination and ensures technical accuracy.

### Staleness Detection

Each documentation node carries a staleness score, calculated from:

- Days since last review (weighted by documentation volatility category)
- Number of code changes in related modules since last documentation update
- User feedback signals (low ratings, repeated searches for the same term, high bounce rate)
- Support ticket frequency for topics covered by this documentation node

### Proactive Intelligence Alerts

| Alert Type | Description |
|---|---|
| Undocumented API Endpoint | New endpoint detected in codebase scan with no corresponding documentation |
| Contradictory Instructions | Two documentation sections contain mutually exclusive instructions for the same workflow |
| Terminology Drift | A term used in recent commits differs from the established glossary term |
| Documentation Desert | A product area with high support ticket volume has sparse documentation |
| Screenshot Staleness | A UI screenshot does not match the current rendered UI (visual diff) |
| Missing Prerequisite | A guide references a setup step not documented anywhere in the system |
| Unclear Workflow | A documented workflow has no outcome statement, no success criteria, or steps that cannot be followed sequentially without ambiguity |
| Simplification Opportunity | A section scores above grade 12 reading level for an end-user audience; specific sentences flagged for rewriting |
| Onboarding Gap | New users reach a product milestone (e.g., first export) but documentation coverage for that milestone scores below 0.6; suggests onboarding guide improvement |
| Duplicate Content | Two documentation sections cover the same topic with different instructions; one should be canonical with the other cross-linking to it |
| Edge Case Undocumented | A code path handles an error or boundary condition with no corresponding documentation entry in the troubleshooting guide |
| Missing FAQ Entry | Support tickets containing the same question appear three or more times in a 30-day window with no corresponding FAQ entry |

---

## Database & Schema Design

### Database Architecture

| Database | Role |
|---|---|
| PostgreSQL (primary) | Documentation metadata, user accounts, approval records, audit logs, quality scores, publish history |
| pgvector (extension) | Vector embeddings for semantic search across documentation, code, and support tickets |
| Neo4j | Knowledge graph: features, modules, APIs, documentation nodes, personas, and their relationships |
| Elasticsearch | Full-text search index across all published documentation artifacts |
| Redis | Session cache, real-time event state, feature flag cache, rate limiting |
| MinIO / S3 | Immutable object storage for all generated documentation files |

### Key PostgreSQL Tables

**documentation_nodes**
```sql
id UUID PRIMARY KEY,
title TEXT,
content_hash TEXT,
format TEXT,                    -- markdown, html, pdf, docx
persona_target TEXT,            -- end_user, admin, developer, enterprise
product_version TEXT,
status TEXT,                    -- draft, review, published, archived
staleness_score FLOAT,
last_reviewed_at TIMESTAMPTZ,
last_code_change_at TIMESTAMPTZ,
quality_score FLOAT,
parent_node_id UUID REFERENCES documentation_nodes(id),
created_by UUID,
approved_by UUID,
created_at TIMESTAMPTZ,
updated_at TIMESTAMPTZ,
published_at TIMESTAMPTZ
```

**change_events**
```sql
id UUID PRIMARY KEY,
event_type TEXT,                -- commit, pr_merge, deploy, release
source_system TEXT,             -- github, gitlab
repository TEXT,
branch TEXT,
commit_sha TEXT,
author TEXT,
commit_message TEXT,
diff_summary JSONB,
impacted_modules JSONB,
impacted_doc_nodes JSONB,
risk_level TEXT,                -- low, medium, high, critical
processed_at TIMESTAMPTZ,
created_at TIMESTAMPTZ
```

**feature_manifest**
```sql
id UUID PRIMARY KEY,
feature_name TEXT,
module_path TEXT,
description TEXT,               -- AI-generated
status TEXT,                    -- active, deprecated
api_endpoints JSONB,
permissions_required JSONB,
related_features JSONB,
first_detected_at TIMESTAMPTZ,
last_verified_at TIMESTAMPTZ,
documentation_coverage_score FLOAT
```

**personas**
```sql
id UUID PRIMARY KEY,
name TEXT,                      -- end_user, admin, developer, enterprise
description TEXT,
allowed_features JSONB,
documentation_depth_level INT,  -- 1-5
preferred_format TEXT,
active_users_count INT,
created_at TIMESTAMPTZ
```

**audit_logs** (append-only, never updated or deleted)
```sql
id UUID PRIMARY KEY,
actor_id UUID,
actor_type TEXT,                -- user, agent, system
action TEXT,
resource_type TEXT,
resource_id UUID,
before_state JSONB,
after_state JSONB,
ip_address TEXT,
session_id TEXT,
created_at TIMESTAMPTZ
```

---

## Automation Pipelines

### CI/CD Documentation Quality Gate

Steps integrated into the existing CI/CD pipeline:

1. **Code Commit / PR Open** - Git webhook fires to ADUMAS Ingestion API.
2. **Diff Analysis** - Commit Intelligence Agent analyzes changes (< 30 seconds).
3. **Impact Assessment** - Knowledge Graph query identifies affected documentation nodes.
4. **Documentation Quality Check** - Quality Agent validates existing docs against new code state.
5. **PR Status Update** - Pass/fail status posted back to GitHub/GitLab PR with actionable suggestions.
6. **On Merge** - Generator Agent produces updated draft documentation sections.
7. **Auto-publish or Queue** - Based on change risk level (configured per repository).
8. **Post-Deploy Verification** - After production deployment, Distribution Agent verifies all platform syncs succeeded.

---

## Laravel / PHP Stack Ingestion

When the source system is a Laravel (PHP) application, ADUMAS uses a specific ingestion strategy tailored to Laravel conventions. The signals available in a Laravel codebase are richer than a generic codebase because Laravel enforces predictable structure.

### Primary Ingestion Sources

| Source | Location | What to Extract |
|---|---|---|
| Route files | `routes/api.php`, `routes/V5/api.php`, `routes/web.php` | All HTTP verbs, URI patterns, controller@method bindings, middleware groups (auth, role gates) |
| Controllers | `app/Http/Controllers/**/*.php`, `Modules/*/Controllers/` | Public methods = user-facing actions; method signatures reveal parameters; docblocks reveal intent |
| Form Requests | `app/Http/Requests/**/*.php` | Validation rules = field constraints and business rules; `authorize()` = permission model |
| Eloquent Models | `app/Models/**/*.php` | `$fillable` = editable fields; `$casts` = field types; relationships = data model; scopes = filter logic |
| Policy files | `app/Policies/**/*.php` | `view`, `create`, `update`, `delete` methods reveal the permission matrix per resource |
| Service classes | `app/Services/**/*.php` | Business logic that does not live in controllers; domain operations that need to be documented as workflows |
| Pest feature tests | `tests/Feature/**/*.php` | Tests describe expected behavior precisely. Each `it()` block is a behavioral specification. Read tests before writing docs. |
| Database migrations | `database/migrations/` | Schema history; new columns added = new fields to document; nullable = optional fields |
| PRDs and URDs | Linear, Confluence, Jira, Notion, Docmost, GitHub Issues, Asana, ClickUp, or any wiki/doc tool the team uses | Product intent, user stories, acceptance criteria; always reconcile against what shipped in code |
| Functional requirements | Linear, Jira, Asana, ClickUp, GitHub Issues, or wherever the team tracks acceptance criteria | Acceptance criteria, user flows, edge cases, error states per issue |
| CLAUDE.md | Repo root (if present) | Domain glossary, architectural decisions, naming conventions; treat as the authoritative internal style guide |
| Blade / Vue components | `resources/views/`, `frontend/src/` | UI labels, form field names, error messages; these are the exact strings users see |
| i18n files | `lang/`, `frontend/src/locales/` | Canonical user-facing terminology; use i18n keys to find the correct label for any field or action |

### Ingestion Sequence for a Laravel Feature

```
1. Read routes/api.php and routes/V5/api.php
   - Map every endpoint: METHOD /uri -> Controller@method
   - Note middleware: auth:sanctum, role:study-admin, etc.

2. For each controller method:
   - Read the FormRequest (authorize = who can do this; rules = what data is required)
   - Read the Service method it calls (what business logic runs)
   - Read the Model's fillable/casts (what data is stored)

3. Read the Policy file for that Model
   - Document: who can view, create, update, delete this resource

4. Read Pest feature tests for that controller
   - Each it() block = one documented behavior or edge case

5. Fetch the PRD/URD from wherever the team stores specs
   - Ask the user: "Where do your PRDs live?" (Linear, Confluence, Jira, Notion, Docmost, GitHub, etc.)
   - Use the appropriate MCP connector for their tool
   - Extract user stories, acceptance criteria, scope, and non-goals
   - Fetch linked issues/tickets for per-story acceptance criteria and edge cases
   - Reconcile against what the code actually does (Specification Reconciler)

7. Read Vue components for the feature
   - Extract UI labels, field names, button text, validation messages
   - These are the exact words users see - use them verbatim in the manual
```

### Laravel-Specific Documentation Patterns

**Permission tables.** Laravel Policies map directly to role-based documentation. Generate a permissions table for every resource:

| Action | Minimum Role | Notes |
|---|---|---|
| View appointment list | Site Staff | Scoped to their assigned site |
| Create appointment | Site Staff | Requires at least one active schedule block |
| Delete appointment | Site Admin | Cannot delete completed appointments |

**Validation rules as field documentation.** A FormRequest rule like `'date_of_birth' => 'required|date|before:today'` documents itself: the field is required, must be a valid date, and must be in the past. Transform rules into plain-language field descriptions automatically.

**Soft deletes.** When a Model uses `SoftDeletes`, document that deleted records are recoverable by an administrator. Users need to know this behavior.

**Dynamic forms engine.** If the system uses a schema-driven form engine (modules table -> forms -> elements), generate documentation from the element definitions rather than from hardcoded UI. The element's `label`, `field_type`, `list_id`, and `is_required` fields drive the documentation.

**Artisan commands.** Any command registered in `app/Console/Commands/` that is relevant to operators or administrators should appear in the admin guide or operations runbook.

### Module-by-Module Strategy

For a multi-module Laravel application, generate documentation one module at a time:

```
Module: Billing
  Sources: BillingController, SubscriptionController, InvoiceController,
           PaymentController
           app/Services/Billing/SubscriptionService.php
           tests/Feature/Billing/
           PRD/URD: fetch from wherever the team stores specs (Linear, Confluence,
           Jira, Notion, Docmost, GitHub, etc.) using the appropriate MCP connector
  Audiences: End users (managing their plan), Admins (overrides, refunds), Support (read-only)
  Output: docs/billing/overview.md, subscriptions.md, invoices.md, payments.md, admin-guide.md
```

Run this pattern for every module: identify the controller cluster, the service layer, the test coverage, and any linked PRDs. Generate per-audience documents for each module before moving to the next.

---

### Documentation Quality Score (0–100)

| Dimension | Weight | Measured By |
|---|---|---|
| Completeness | 25 pts | Coverage of expected sections for the document type |
| Accuracy | 25 pts | Consistency with current codebase state via RAG verification |
| Clarity | 20 pts | Flesch-Kincaid readability score for the target persona |
| Freshness | 15 pts | Time since last verified update relative to code change frequency |
| User Satisfaction | 15 pts | Aggregate of ratings, search success rate, support ticket avoidance |
