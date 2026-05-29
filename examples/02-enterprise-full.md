# Example 2: Enterprise Full Platform - Confluence + Jira + Slack, 200 Engineers

## Context

A 200-person company building an enterprise SaaS platform. They have:
- 12 GitHub repositories (monorepo split into services over the past year)
- Confluence as the primary documentation platform
- Jira for all engineering and product work
- Slack for communication
- Linear for product roadmap tracking
- A dedicated technical writing team of 3 people (overwhelmed)
- SOC 2 Type II certification in progress
- A platform engineering team that can own the documentation pipeline infrastructure

Their pain: features ship with no documentation, or with documentation written 3 weeks later by a technical writer who wasn't in the room when the feature was built. Documentation is inconsistent across teams. Support costs are high.

---

## What They Asked Claude

```
We're a larger company - about 200 engineers across 12 microservice repos.
We use Confluence, Jira, Slack, and Linear. We have 3 technical writers but they're
swamped. We're going through SOC 2 so audit logging is important.

We want the full AI documentation system - agents, knowledge graph, the works.
Give us the production architecture and a 12-month implementation roadmap.
We'll generate the full Word doc too.
```

---

## What the Skill Produces

### Production Architecture

**All 7 agents, phased over 12 months. Full enterprise infrastructure.**

```
[INGESTION]
GitHub MCP (12 repos) ──────────────────────┐
Linear MCP (feature context) ───────────────┤
Figma MCP (UI annotations) ─────────────────┤→ Kafka (doc.changes topic)
Atlassian MCP (Jira issue context) ─────────┘
                                                       │
[INTELLIGENCE]                              ┌──────────┘
Neo4j Knowledge Graph ◄─────────────────────┤
PostgreSQL + pgvector ◄─────────────────────┤
                                             │
  7 AI Agents consume from Kafka:            │
  1. Codebase Analyst ──────────────────────►│
  2. Commit Intelligence ───────────────────►│
  3. Documentation Generator ───────────────►│
  4. Quality Auditor ────────────────────────►│
  5. Audience Adapter ───────────────────────►│
  6. Distribution & Sync ────────────────────►│
  7. Feedback Intelligence ──────────────────►│
                                             │
[DISTRIBUTION]                              └──→ Atlassian MCP (Confluence)
                                                  Slack MCP (#docs-review, #releases)
                                                  Linear MCP (documentation tasks)
                                                  GitHub Pages (public API reference)
```

**Full tech stack:**

| Component | Choice |
|---|---|
| API Gateway | FastAPI (Python) |
| Agent Orchestration | LangGraph |
| Primary LLM | Claude claude-sonnet-4-6 (Anthropic API, zero-retention) |
| Embedding | text-embedding-3-large (Azure OpenAI, same region) |
| Primary DB | PostgreSQL 16 + pgvector (RDS) |
| Knowledge Graph | Neo4j Enterprise (EC2, 4 nodes, 2 read replicas) |
| Event Bus | Confluent Cloud Kafka (Standard tier) |
| Search | Elasticsearch 8.x (Elastic Cloud) |
| Object Storage | AWS S3 (us-east-1, versioning enabled) |
| Cache | Redis (Elasticache) |
| Orchestration | Kubernetes (EKS) |
| Task Queue | Temporal Cloud |
| Secret Management | AWS Secrets Manager |
| Observability | Datadog (APM + logs + metrics) |
| IaC | Terraform + Helm |
| CI/CD | GitHub Actions |

### Confluence Structure

```
Confluence: [Company] Documentation
├── Space: PROD-DOCS (Product Documentation)
│   ├── Getting Started
│   ├── User Guide
│   │   ├── [Feature pages - auto-populated]
│   ├── Admin Guide
│   ├── Integrations
│   └── Release Notes (auto-generated per release)
│
├── Space: API-DOCS (Developer API Reference)
│   ├── Authentication
│   ├── Endpoints (auto-generated from OpenAPI)
│   └── SDKs
│
├── Space: INT-DOCS (Internal Engineering Docs)
│   ├── Architecture
│   ├── Runbooks
│   └── SOPs
│
└── Space: ARCHIVE (Versioned historical docs)
    ├── v2.x
    └── v1.x
```

### Jira Documentation Project

```
Jira Project: DOCS
Workflow: Identified → Triage → In Progress → In Review → Done

Ticket types:
- Documentation Gap (auto-created by Quality Agent)
- Documentation Update (auto-created by Commit Intelligence)
- Documentation Review (auto-created for human approval queue)

Labels:
- auto-generated    (created by pipeline, not manually)
- breaking-change   (requires two reviewers)
- compliance        (requires legal/compliance review)
- stale             (staleness score > 0.7)
```

### RBAC Model

| Role | Permissions |
|---|---|
| Viewer | Read all published documentation |
| Contributor | Read + suggest edits (via feedback UI) |
| Reviewer | Read + approve/reject drafts |
| Editor | Read + approve + directly edit published documentation |
| Admin | Full access + configure personas, quality thresholds, platform connections |
| System (agents) | Read all + write drafts + publish approved content |

### Audit Logging (SOC 2)

Every action is appended to the `audit_logs` table (append-only, never updated or deleted):

| Event | Logged Data |
|---|---|
| Documentation draft created | Agent ID, change event ID, generated content hash, timestamp |
| Human review started | Reviewer user ID, documentation node ID, timestamp |
| Documentation approved | Approver user ID, documentation node ID, timestamp |
| Documentation published | Distribution target, documentation node ID, publish hash, timestamp |
| Documentation edited manually | Editor user ID, before/after content hash, timestamp |
| MCP credential accessed | Service name, actor, timestamp |

Audit logs are exported nightly to S3 in JSON format and indexed in Elasticsearch for SOC 2 evidence collection.

---

## 12-Month Implementation Roadmap

### Phase 1: Foundation (Months 1–3)
- Core infrastructure provisioned (Postgres, Redis, Kafka, S3) via Terraform.
- GitHub webhook integration live for all 12 repos.
- Commit Intelligence Agent operational.
- Documentation Generator Agent live (Markdown output).
- Confluence (Atlassian MCP) distribution live for 2 pilot services.
- Single-reviewer approval workflow via Jira ticket.
- Basic Slack notifications (`#docs-review`).

### Phase 2: Intelligence (Months 4–6)
- Neo4j Knowledge Graph deployed and populated from codebase analysis.
- Codebase Analyst Agent live (nightly full scan of all 12 repos).
- RAG pipeline with pgvector for semantic documentation retrieval.
- Quality Auditor Agent live (quality scoring, contradiction detection).
- Multi-tier approval workflow (breaking changes → two reviewers + tech lead).
- Full Confluence distribution across all services.

### Phase 3: Scale & Governance (Months 7–9)
- Audience Adapter Agent live (end user, admin, developer personas - separate documentation views per persona).
- Full RBAC model implemented.
- SOC 2 audit logging complete and verified.
- SSO integration (Okta) for documentation portal access.
- Elasticsearch full-text search live on documentation portal.
- Figma MCP integration for UI annotation extraction.

### Phase 4: Intelligence & Loop Closure (Months 10–12)
- Feedback Intelligence Agent live (search gap analysis, support ticket correlation).
- Vector (semantic) search via pgvector in documentation portal.
- Documentation Health Dashboard for engineering and product leadership.
- Translation pipeline for top 3 languages (if applicable).
- Full automation: 80%+ of documentation updates auto-approved without human review.

---

## Key Decisions Made

**Neo4j from Phase 2, not Phase 1.** At 12 repos with complex cross-service dependencies, the Knowledge Graph is genuinely needed - but only after the basic pipeline is proven. Adding it in Phase 1 would delay the first working documentation update by 6–8 weeks. Prove the pipeline first; add intelligence in Phase 2.

**Temporal for task queue, not Celery.** At 200 engineers with 12 repos generating significant daily commit volume, Temporal's durability (tasks survive pod restarts, full history replay) is worth the additional complexity. Celery is fine at startup scale; Temporal is the right choice when the pipeline is business-critical.

**Zero-retention LLM agreement mandatory.** Because this company is going through SOC 2, all LLM API calls must use providers with a zero-retention data processing agreement (Anthropic and Azure OpenAI both offer this). Verify the agreement is in place before the pipeline processes any production code.

**Technical writers own the review workflow, not engineers.** The 3 technical writers become the documentation review team, not the engineers. Engineers write code, the AI generates drafts, and technical writers review and approve. This is a role change, not a headcount reduction - technical writers shift from writing from scratch to curating and quality-assuring AI output.

---

## What to Watch Out For

**Kafka partition strategy.** With 12 repos all publishing to the same `doc.changes` topic, partition by repository to ensure changes from the same repo are processed in order. Out-of-order processing can cause the system to generate documentation for an older version of a file over a newer one.

**Confluence page version conflicts.** If two documentation updates for the same page are processed concurrently, Confluence's optimistic locking will reject one of them. Implement per-page locking in Redis before calling the Confluence API. Check for `409 Conflict` responses and implement a retry with the latest page version.

**Linear-Jira duplication.** This team uses both Linear (product roadmap) and Jira (engineering tickets). Don't create documentation tasks in both. Decide: Linear for product-facing documentation gaps, Jira for engineering-facing documentation tasks. Document this routing rule explicitly in the pipeline configuration.

**SOC 2 scope creep.** The audit logging requirement will grow. Start with the 6 events listed above. Do not add more event types until a compliance auditor specifies what else is needed. Premature logging expansion adds storage cost and noise without compliance benefit.
