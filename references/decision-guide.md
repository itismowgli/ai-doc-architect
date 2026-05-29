# Decision Guide

When multiple valid options exist, this guide provides decision trees and rationale to help choose the right path. Each tree ends with a clear recommendation, not "it depends."

## Table of Contents

1. [What to Build First (MVP Scope)](#what-to-build-first-mvp-scope)
2. [Which LLM to Use](#which-llm-to-use)
3. [Which Database Strategy](#which-database-strategy)
4. [Which Documentation Destination](#which-documentation-destination)
5. [Which Deployment Model](#which-deployment-model)
6. [How Many Agents to Start With](#how-many-agents-to-start-with)
7. [When to Require Human Review](#when-to-require-human-review)
8. [Docs-as-Code vs Separate Docs Repo](#docs-as-code-vs-separate-docs-repo)
9. [Which Search Strategy](#which-search-strategy)
10. [When to Version Documentation](#when-to-version-documentation)

---

## What to Build First (MVP Scope)

```
How many repositories do you need to document?
├── 1–3 repos
│   └── Start with: Git webhook → Commit Intelligence → Generator → one platform (Notion or GitHub Pages)
│       Skip: Knowledge Graph, Quality Agent, Audience Adaptation, translation
│       Timeline: 6–8 weeks to working MVP
│
└── 4+ repos or a monorepo
    └── Is the monorepo structured by service/package with clear boundaries?
        ├── Yes → treat each service as an independent documentation unit
        │          Add Codebase Analyst Agent in Week 3 (structural complexity warrants it)
        └── No (tightly coupled) → start with full-repo analysis, Knowledge Graph from day one
                                    Budget 12–16 weeks for MVP
```

**The 90-day MVP build sequence (for most teams):**

| Week | What to Build |
|---|---|
| 1–2 | Git webhook receiver → Commit Intelligence Agent (single LLM call on diff) |
| 3–4 | Documentation Generator Agent → Markdown output to GitHub Pages or Notion |
| 5–6 | PostgreSQL database → store nodes, change events, publish history |
| 7–8 | Basic quality gate on PRs (pass/fail if impacted docs have no draft) |
| 9–10 | Human review workflow (single reviewer, email notification) |
| 11–12 | Distribution Agent → Slack notifications on publication |

---

## Which LLM to Use

```
Is the primary task long-form documentation writing (manuals, guides, SOPs)?
├── Yes → Use Claude (Sonnet or Opus)
│          Reason: best long-context reasoning, strongest prose quality, handles
│                  large codebases in a single pass
│
└── No - is it structured data extraction (API schemas, JSON output, table generation)?
    ├── Yes → Use GPT-4o (structured outputs mode)
    │          Reason: reliable JSON generation, lower hallucination on constrained formats
    │
    └── No - is it a deployment where NO data can leave the network?
        ├── Yes → Use a local model via Ollama or vLLM
        │          Best options: Llama 3.1 70B (quality), Mistral 7B (speed)
        │          Accept: 30–50% quality reduction vs. Claude
        │
        └── No - is it a high-volume, low-stakes task (staleness scoring, classification)?
            └── Use a smaller/cheaper model (Claude Haiku, GPT-4o mini)
                Reason: cost; these tasks don't require frontier model quality
```

**Recommended multi-model setup for production:**

| Task | Model | Why |
|---|---|---|
| Codebase analysis + full manual generation | Claude Opus or Sonnet | Long context, best prose |
| API schema extraction (structured JSON) | GPT-4o (structured outputs) | Reliable schema compliance |
| Staleness scoring, classification | Claude Haiku or GPT-4o mini | Low cost, adequate quality |
| On-premises / air-gapped | Llama 3.1 70B (Ollama) | No external calls |
| Embedding generation | text-embedding-3-large | Best semantic accuracy |

---

## Which Database Strategy

```
Do you already run PostgreSQL in production?
├── Yes → Use PostgreSQL + pgvector extension for everything at MVP
│          Single database, minimal ops overhead, vector search built in
│
└── No - what is your team's primary database expertise?
    ├── PostgreSQL (most common) → use it; adding pgvector is a one-line extension
    │
    └── Other (MySQL, MongoDB, etc.)
        └── Do you have resources to run a second database in production?
            ├── No → use Supabase (managed Postgres + pgvector) - zero ops overhead
            └── Yes → use PostgreSQL + pgvector for docs metadata,
                       your existing DB for application data
```

**When to add Neo4j (Knowledge Graph):**

Add Neo4j when all three of these are true:
1. You have 5+ repositories or a complex monorepo.
2. You need to answer "which documentation is impacted by this code change?" accurately at scale (Knowledge Graph makes this O(graph traversal) instead of O(scan all docs)).
3. You have engineering capacity to operate a second database.

If only 1 or 2 are true, skip Neo4j and model relationships as JSONB arrays in PostgreSQL. Upgrade to Neo4j when the PostgreSQL approach becomes a performance bottleneck (typically > 50k documentation nodes or > 1,000 daily change events).

**When to add Elasticsearch:**

Add Elasticsearch when:
- Full-text search with exact phrase matching across > 10k documentation pages is slow on PostgreSQL.
- You need faceted search (filter by product version, audience, documentation type).
- You are building a public documentation portal where search is a primary UX feature.

Skip Elasticsearch in the MVP. PostgreSQL full-text search (`tsvector`/`tsquery`) handles the first 10k–100k documents adequately.

---

## Which Documentation Destination

```
Where does your team primarily consume documentation today?
│
├── Notion → Connect Notion MCP. Structure: one Workspace, one Database per doc type.
│             No public portal needed unless you have external users.
│
├── Confluence → Connect Atlassian MCP. Map spaces to product areas, pages to doc nodes.
│                Add Jira integration from the same MCP for task tracking.
│
├── GitHub (internal OSS or dev-focused team) → Use GitHub Pages with Next.js + MDX.
│   Keep docs in the same repo as code. Use GitHub Actions to publish on merge.
│
├── Public documentation portal (external users, SaaS help center) → 
│   Build with Next.js + MDX. Host on Vercel or Cloudflare Pages. 
│   Add Algolia for search (free tier covers most early-stage products).
│
└── Multiple destinations → Start with ONE. Add others after the pipeline is stable.
    Priority: whichever destination your support team links to most often.
    That is where users actually look for help.
```

**Never use as the primary destination:**

- Slack - ephemeral, unsearchable, not the right UX for documentation.
- Google Docs - no version control, no structured navigation, poor search.
- SharePoint - poor developer experience, no Git integration.
- Email newsletters - documentation needs to be pullable, not pushed.

---

## Which Deployment Model

```
Does your organization have data residency or compliance requirements
(HIPAA, GDPR data residency, FedRAMP, SOC 2)?
├── Yes → Does compliance require data to never leave your network?
│         ├── Yes → On-premises deployment. Use Ollama/vLLM for local LLM inference.
│         │         No external API calls permitted.
│         └── No → Dedicated Cloud (single-tenant, your chosen region).
│                   External LLM API calls permitted with zero-retention agreement.
│
└── No - What is your team size?
    ├── < 20 engineers → Cloud SaaS (multi-tenant). Fastest setup, lowest cost.
    │
    ├── 20–200 engineers → Cloud SaaS or Dedicated Cloud.
    │   Dedicated Cloud if you need custom SSO, white-labeling, or SLAs.
    │
    └── 200+ engineers → Dedicated Cloud or BYOC (Bring Your Own Cloud).
        BYOC if you have a strong platform engineering team and want full control.
```

---

## How Many Agents to Start With

```
Team size?
├── 1–5 engineers → 2 agents: Commit Intelligence + Generator
│   (Distribution is just a script, not a full agent at this scale)
│
├── 6–20 engineers → 3 agents: Commit Intelligence + Generator + Distribution
│   Add Quality Agent only when documentation volume exceeds 200 pages
│
└── 20+ engineers → All 7 agents, phased over 12 months
    Phase 1 (MVP): Commit Intelligence, Generator, Distribution
    Phase 2: Quality Auditor, Codebase Analyst
    Phase 3: Audience Adapter, Feedback Intelligence
```

**Single most important agent if you can only build one:**

The **Commit Intelligence Agent**. Without it, you have no way to automatically detect that documentation needs to change. The Generator Agent is useless without a reliable input telling it what changed and why. Build Commit Intelligence first, even before the Generator - output the change manifest to a log file and manually generate documentation initially. That workflow is still 10x better than nothing.

---

## When to Require Human Review

Not every documentation change needs a human reviewer. Requiring review on everything kills adoption - engineers will route around the system.

```
Is this a breaking API change (removed endpoint, changed parameter, new required field)?
├── Yes → Require two reviewers + tech lead sign-off. Block distribution until approved.
│
└── No - Does this change affect a compliance-sensitive area (auth, billing, data deletion)?
    ├── Yes → Require single reviewer. Flag for legal/compliance if applicable.
    │
    └── No - Is this a new feature (first documentation for something that didn't exist)?
        ├── Yes → Require single reviewer (PM or tech lead).
        │
        └── No - Is this an update to existing documentation (feature changed behavior)?
            ├── Change is major (new workflow, new permissions, deprecated path)?
            │   → Require single reviewer.
            │
            └── Change is minor (clarification, typo fix, screenshot update)?
                → Auto-approve and publish. Log for audit trail but no human gate.
```

**Calibrating the auto-approve threshold:**

Start conservative (require review for most changes). After 30 days, look at your review queue:
- If > 80% of reviewed PRs are approved without changes → loosen the threshold.
- If reviewers are missing SLA because of volume → loosen the threshold.
- If you find published errors in auto-approved content → tighten the threshold.

The goal is that human reviewers only see documentation changes that genuinely need judgment, not rubber-stamping.

---

## Docs-as-Code vs Separate Docs Repo

```
Does your documentation change at roughly the same pace as your code?
├── Yes (feature docs updated with every PR) → Docs-as-Code in the same repo
│   Rationale: a single PR contains both code and documentation changes.
│               Reviewers see the full picture. Documentation can't lag behind.
│
└── No (docs updated on release schedule, not per-commit) → Separate docs repo
    Rationale: documentation lifecycle is decoupled from code lifecycle.
               Easier for non-engineers to contribute without touching the app repo.
               Allows content writers to work without understanding Git branch strategy.
```

**Hybrid approach (most common in practice):**

- API reference docs → in the code repo (generated from code comments / OpenAPI spec).
- User-facing manuals and guides → in a separate docs repo.
- The ADUMAS pipeline pulls from both repos and publishes to a single documentation portal.

---

## Which Search Strategy

```
Is your documentation portal public-facing (external users)?
├── Yes → Use Algolia DocSearch (free for open-source, affordable for SaaS)
│          Crawls your documentation site and provides instant semantic search.
│          Best-in-class UI components available for React and Vue.
│
└── No - Is it internal (team-only)?
    ├── < 10k documentation pages → PostgreSQL full-text search (tsvector)
    │   No additional infrastructure. Fast enough. Easy to maintain.
    │
    └── > 10k pages OR need faceted search (filter by version, audience, type)?
        └── Add Elasticsearch or OpenSearch.
            Host it yourself (one node is fine for internal use)
            or use Elastic Cloud / OpenSearch Service.
```

**When to add vector (semantic) search:**

Add vector search (pgvector or Pinecone) when:
- Users frequently search with natural language questions ("how do I reset a password") rather than keywords ("password reset endpoint").
- You want to support an AI Q&A interface over documentation (RAG-based chat).
- Users miss relevant content because they use different terminology than the documentation.

Vector search is a Phase 2 addition for most teams. Full-text search solves 80% of search problems with 20% of the complexity.

---

## When to Version Documentation

```
Do you maintain multiple supported versions of your product simultaneously?
├── Yes (e.g., v2.x and v3.x are both in production) → Version documentation.
│   Each version gets its own documentation branch and URL path (/v2/, /v3/).
│   Use SemVer: major.minor.patch maps to breaking/feature/fix documentation changes.
│
└── No - Do you ship frequently (weekly or more) and immediately sunset old versions?
    └── Is your product API-based with external consumers?
        ├── Yes (public API with external developers) → Version API reference docs.
        │   Version user-facing guides only on breaking changes.
        │
        └── No (internal tool or single-tenant) → Don't version documentation.
            Keep one current version. Archive on major releases.
            Versioning without a reason adds maintenance overhead with no user benefit.
```

**Version naming convention:**

```
Documentation version matches product version:
- v1.0 → v1.0 documentation (created when v1.0 ships)
- v1.1 → v1.1 documentation (minor additions, no breaking changes)
- v2.0 → v2.0 documentation + migration guide from v1.x

URL structure:
  /docs/latest/  → always points to current stable version
  /docs/v2.0/   → v2.0 documentation
  /docs/v1.1/   → v1.1 documentation (archived but accessible)
  /docs/v1.0/   → v1.0 documentation (archived)
```
