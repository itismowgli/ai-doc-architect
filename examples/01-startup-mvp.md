# Example 1: Startup MVP - 6 Engineers, GitHub + Notion, 90 Days

## Context

A 6-person B2B SaaS startup building a project management tool. They have:
- One GitHub monorepo with a Next.js frontend and a FastAPI backend
- Notion as their primary internal knowledge base
- Slack for team communication
- No dedicated technical writer
- Investors asking "where is your documentation?"

Their product ships every 2 weeks. Features ship faster than docs. Support tickets are increasing because users can't find how to do basic things. The CTO has 10% of one engineer's time to invest in documentation automation.

They have no existing Confluence, no Jira, no Linear - just GitHub, Notion, and Slack.

---

## What They Asked Claude

```
We're a small startup - 6 engineers, GitHub monorepo, Notion for docs.
We keep shipping features but never have time to write documentation.
Customers are confused and support tickets are piling up.

We have maybe 10% of one engineer's time to set this up.
What's the simplest version of an AI documentation system that will actually work?
Give me a 90-day build plan.
```

---

## What the Skill Produces

### Recommended Architecture (MVP Only)

**3 components, not 7 agents:**

```
GitHub Webhook
     ↓
Commit Intelligence (single LLM call)
     ↓
Documentation Generator (Markdown output)
     ↓
Notion MCP → Notion Workspace
     ↓
Slack MCP → #releases notification
```

Skip for now: Knowledge Graph, Quality Agent, Audience Adaptation, Elasticsearch, Neo4j, multi-version docs, translation, compliance features.

**Tech stack (minimal):**

| Component | Choice | Why |
|---|---|---|
| Database | Supabase (managed Postgres + pgvector) | Zero ops; free tier covers MVP |
| LLM | Claude Sonnet via Anthropic API | Best prose quality for documentation |
| Queue | Supabase Edge Functions + pg_cron | No Kafka; overkill at this scale |
| Distribution | Notion MCP | Already using Notion |
| Notifications | Slack MCP | Already using Slack |
| Hosting | Vercel (free tier) | Serverless; no servers to manage |

### 90-Day Build Plan

**Week 1–2: The trigger**
- Set up GitHub webhook on the monorepo to fire on `pull_request.closed` (merged) events.
- Build a simple API endpoint (Vercel serverless function) that receives the webhook payload.
- Store the event in Supabase: `change_events` table with commit SHA, diff summary, changed files.
- Test: merge a PR, verify the event is captured in Supabase.

**Week 3–4: The brain**
- Build the Commit Intelligence step: pass the PR diff + commit message to Claude Sonnet.
- Prompt: "Analyze this code diff. What product features or user workflows changed? List only changes that would be visible to end users or administrators. Output as JSON."
- Store the change manifest in Supabase.
- Test: merge a feature PR, verify the change manifest is accurate.

**Week 5–6: The writer**
- Build the Documentation Generator step: pass the change manifest to Claude.
- Prompt template: audience = "end user", format = task-oriented Markdown, sections = [context, prerequisites, steps, expected outcome].
- Store the generated Markdown draft in Supabase.
- Test: review a generated draft manually. Adjust the prompt until the output is usable.

**Week 7–8: The publisher**
- Connect the Notion MCP.
- Create the Notion workspace structure manually: Getting Started, User Guide, Admin Guide, Release Notes.
- Build the Distribution step: on draft approval, call `notion-create-pages` or `notion-update-page`.
- Store the Notion `page_id` in Supabase to avoid duplicate page creation.
- Test: approve a draft, verify the Notion page is created/updated correctly.

**Week 9–10: The review workflow**
- Add a simple approval step: after generation, send a Slack message with the draft link and an "Approve" button (Slack interactive message).
- Approving triggers the Notion sync.
- For now, the "reviewer" is whoever merged the PR. One click to approve.

**Week 11–12: The notification**
- Connect the Slack MCP.
- On Notion page publication, send a Slack message to `#product` or `#releases` with the documentation link.
- Add a weekly "documentation health" message listing PRs merged in the last week with no documentation draft.

### Notion Workspace Structure

```
📚 [Product Name] Docs
├── 🚀 Getting Started
│   ├── Quick start guide
│   └── Account setup
├── 👤 User Guide
│   ├── [Feature A]           ← auto-populated by pipeline
│   ├── [Feature B]           ← auto-populated by pipeline
│   └── ...
├── ⚙️ Admin Guide
│   ├── Team management
│   └── Billing & plans
└── 📋 Release Notes
    ├── v1.3.0 - May 2026     ← auto-generated on each PR merge
    └── v1.2.0 - April 2026
```

---

## Key Decisions Made

**No Kafka, no Knowledge Graph, no Elasticsearch.** At 6 engineers and one repo, these are pure overhead. Supabase handles the queue (pg_cron + Postgres queues). Postgres full-text search handles documentation search until you exceed 10k pages. Neo4j is added when "which docs are impacted?" becomes a slow query, which won't happen at startup scale.

**Notion over GitHub Pages.** The team already lives in Notion. Getting engineers and non-engineers to both contribute to and read documentation in Notion requires zero behavior change. A new documentation portal would require driving traffic to a new destination - not worth the effort at this stage.

**Single reviewer, Slack approval.** A formal approval workflow (with routing, deadlines, email notifications) is premature. The PR author is the right first reviewer because they know what changed. A Slack button is the lowest-friction approval mechanism possible.

**Claude Sonnet, not Haiku.** Documentation quality matters more than cost at MVP scale. At 20–30 PR merges per week, Claude Sonnet API costs are under $50/month. That is not a meaningful expense compared to the cost of poor documentation.

---

## What to Watch Out For

**Notion rate limits.** If you merge many PRs in rapid succession (e.g., a release day), the Notion sync will be rate-limited (3 requests/second). Add a simple queue with a 400ms delay between Notion API calls from day one - don't add it after you hit the rate limit in production.

**Diff size.** Some PRs contain enormous diffs (dependency updates, file moves, generated code). The Commit Intelligence step will waste tokens and produce irrelevant documentation for these. Add a filter: skip any file matching `package-lock.json`, `*.generated.*`, `dist/`, `node_modules/`, or any diff with > 5,000 changed lines.

**Monorepo complexity.** If the frontend and backend are both in one repo, the pipeline will detect changes in both on every full-stack PR. Train the Commit Intelligence Agent to distinguish frontend changes (user-facing) from backend changes (API-facing) and generate persona-appropriate documentation for each.

**Don't show customers the raw AI output.** The first few weeks of output will be rough. Approve only documentation you've personally read before publishing. Quality improves as you tune the prompt - but never auto-publish without review in the first 30 days.
