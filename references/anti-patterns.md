# Anti-Patterns

Twenty documented mistakes teams make when building AI documentation systems. Organized by phase: planning, build, and operations. Each entry includes the root cause and how to fix it.

---

## Planning Anti-Patterns

### AP-01: Trying to automate everything on day one

**What it looks like:** The team scopes an MVP that includes the Knowledge Graph, all 7 agents, 10 platform integrations, multi-persona output, multilingual support, and a public portal - before writing a single line of pipeline code.

**Why it happens:** The architecture looks impressive on paper and stakeholders want to show ambition. The full system *is* the right long-term goal.

**Why it fails:** Six months of building, nothing in production, no user feedback, and the team has burned trust with stakeholders who expected earlier results.

**Fix:** Start with the 90-day MVP: Git webhook → Commit Intelligence → Generator → one distribution platform. Ship to one real repository. Iterate from there. The full system is built iteratively, not designed in full and then built.

---

### AP-02: Treating documentation automation as a one-time project

**What it looks like:** Engineering allocates a sprint, builds the pipeline, ships it, and moves on. No ongoing ownership. No monitoring. No iteration.

**Why it happens:** Documentation is viewed as a project, not a product. The pipeline is treated like a migration script - run it once and forget it.

**Why it fails:** Products change. The pipeline becomes stale, generates inaccurate documentation, and engineers lose trust in it. Within 6 months, the pipeline is disabled or ignored.

**Fix:** Assign explicit ownership. The documentation pipeline is a product with a roadmap, a backlog, and on-call responsibilities. Allocate 10–20% of one engineer's time for ongoing operations.

---

### AP-03: Designing for the documentation consumers, not the documentation authors

**What it looks like:** The system is designed around beautiful output formats and polished portals, but no thought is given to how engineers will review, approve, or correct AI-generated content.

**Why it happens:** External user experience is visible and impressive in demos. Internal author experience is invisible until someone has to use it.

**Why it fails:** Engineers find the review workflow painful, skip it, and the system publishes unchecked AI-generated content. Quality degrades quickly.

**Fix:** Before designing the output experience, design the review experience. The approval workflow, the inline editing interface, and the correction feedback loop are more important than the portal design.

---

### AP-04: Skipping persona definition

**What it looks like:** The system generates one version of documentation and distributes it to everyone - admins, end users, and developers all see the same content.

**Why it happens:** Persona modeling feels like overhead when you just want to ship something.

**Why it fails:** End users are confused by technical detail meant for developers. Developers are frustrated by simplified guides that skip the nuance they need. Support volume stays high because neither audience gets what they actually need.

**Fix:** Define at minimum two personas before building the Generator Agent: a technical persona (developer/admin) and a non-technical persona (end user). These two personas alone drive most of the meaningful content variation.

---

### AP-05: Choosing the destination before the source

**What it looks like:** The team spends weeks setting up the Notion workspace, Confluence space, and documentation portal before wiring any source integrations.

**Why it happens:** Distribution is more tangible and visually satisfying than ingestion plumbing.

**Why it fails:** When you finally wire the source integrations, you discover the destination structure doesn't match the natural shape of the documentation output. Everything gets reorganized.

**Fix:** Design the documentation taxonomy (what sections exist, how they're organized) from the source - from the feature manifest and code structure - before designing the destination hierarchy. Let the source drive the shape.

---

## Build Anti-Patterns

### AP-06: Polling Git instead of using webhooks

**What it looks like:** A cron job runs every 5 minutes, calls the GitHub API to check for new commits, and triggers the documentation pipeline.

**Why it happens:** Webhooks feel more complex to set up and require a public endpoint.

**Why it fails:** Polling is slow (5-minute delay minimum), wastes API rate limit, and is brittle (misses commits if the job fails). GitHub rate-limits heavy polling. Latency is the enemy of adoption - engineers expect documentation to update within minutes of a merge.

**Fix:** Use webhook events (`push`, `pull_request.closed`, `release.published`). The Ingestion Orchestrator needs a public endpoint - expose it via a simple API gateway. Latency drops from minutes to seconds.

---

### AP-07: Using the AI to generate documentation for ALL changed files

**What it looks like:** On every commit, the Generator Agent receives the full diff of every changed file and generates documentation updates for all of them.

**Why it happens:** It feels thorough.

**Why it fails:** A commit that changes 50 files might only impact 3 features worth documenting. Generating documentation for all 50 is expensive (token cost), slow, and produces irrelevant noise in the review queue.

**Fix:** The Commit Intelligence Agent must classify changes before calling the Generator. Only changes that affect public-facing features, APIs, user workflows, or configuration options need documentation. Internal refactors, test files, CI configuration, and style changes do not.

---

### AP-08: Storing generated documentation only in the destination platform

**What it looks like:** Generated documentation is published directly to Notion (or Confluence) and no copy is stored in the system's own database.

**Why it happens:** "Notion is the source of truth" sounds clean.

**Why it fails:** If Notion is down, you have no documentation. If someone deletes a page in Notion, it's gone. You can't roll back. You can't diff versions. You can't run quality checks against previous states.

**Fix:** Always store the canonical documentation in your own database (PostgreSQL + S3 for content). External platforms are distribution targets, not sources of truth. The ADUMAS database is the source of truth.

---

### AP-09: Generating documentation in binary formats first

**What it looks like:** The Generator Agent produces DOCX or PDF files as its primary output.

**Why it happens:** "The stakeholder wants a Word document."

**Why it fails:** You can't diff DOCX files in Git. You can't search PDF efficiently. You can't programmatically update section 3.2 of a DOCX without reparsing the whole file. Binary formats are read-only artifacts, not living documentation.

**Fix:** Generate Markdown first, always. Render Markdown to DOCX, PDF, HTML, or Notion blocks as needed. Markdown is your source format. Binary formats are export targets.

---

### AP-10: Building the Knowledge Graph before proving the pipeline works

**What it looks like:** Week 1 of the build is setting up Neo4j, designing the node/relationship schema, and writing graph queries.

**Why it happens:** The Knowledge Graph is architecturally elegant and intellectually interesting.

**Why it fails:** The Knowledge Graph is a Phase 2 optimization. It makes "which docs are impacted by this change?" fast at scale. At MVP scale (1–3 repos, < 500 documentation pages), a simple PostgreSQL query on the `impacted_doc_nodes` JSONB column answers the same question in milliseconds.

**Fix:** Prove the end-to-end pipeline works with PostgreSQL first. Add Neo4j when you hit a concrete performance wall, not before.

---

### AP-11: Letting the AI write documentation without a structured prompt

**What it looks like:** The Generator Agent prompt is "Write documentation for this code change: [diff]." The LLM outputs whatever it thinks is appropriate.

**Why it happens:** It works surprisingly well in demos.

**Why it fails:** Output format is inconsistent. Some sections have prerequisites listed, some don't. Some use numbered steps, some use prose. Quality varies dramatically based on the complexity of the diff. Engineers distrust AI-generated content they can't predict.

**Fix:** The Generator Agent prompt must specify the exact structure: audience, document type, required sections (context → prerequisites → steps → expected outcome → troubleshooting), and tone. Use few-shot examples in the prompt. Consistent structure makes the output reviewable and trustworthy.

---

### AP-12: No idempotency in the Distribution Agent

**What it looks like:** The Distribution Agent creates a new Notion page every time it runs for a documentation node, instead of updating the existing one.

**Why it happens:** Create is simpler than update. The code was written quickly.

**Why it fails:** After a week of operation, there are dozens of duplicate pages for the same feature. The Notion workspace becomes unusable.

**Fix:** Always maintain a mapping between documentation node IDs and destination platform IDs (Notion page IDs, Confluence page IDs). Before creating, check if a mapping exists. If it does, update. If not, create and store the mapping. This must be implemented before the first production run.

---

## Operations Anti-Patterns

### AP-13: No human feedback loop

**What it looks like:** The pipeline generates and publishes documentation automatically. When documentation is wrong, there is no mechanism to correct it without going directly into the database.

**Why it happens:** The feedback interface is considered a "nice to have" for later.

**Why it fails:** Engineers discover incorrect documentation, can't easily fix it, give up, and start writing documentation manually again - defeating the purpose of the system.

**Fix:** The management console must include a simple inline editor for published documentation. Any approved change via the editor should feed back into the system as a correction signal that improves future generation. This is not optional - it is the mechanism that keeps the system accurate over time.

---

### AP-14: Alerting on everything

**What it looks like:** The Slack integration sends a notification every time a documentation section is updated, a PR quality gate runs, a translation is queued, and a staleness score changes.

**Why it happens:** "More visibility is better."

**Why it fails:** Engineers mute the Slack channel within a week. The genuinely important alerts (breaking changes, critical staleness, failed syncs) are buried in noise and missed.

**Fix:** Default to silence. Only alert when human action is required: draft queued for review, sync failure after retries, documentation staleness exceeding the critical threshold, or a CI quality gate blocking a PR. Everything else goes into a weekly digest.

---

### AP-15: No rate limit handling in the Distribution Agent

**What it looks like:** On a release day with 50 documentation updates, the Distribution Agent makes 50 sequential Notion API calls without any throttling.

**Why it happens:** Rate limits are usually not hit in development (low volume).

**Why it fails:** Notion's API has rate limits (3 requests/second per integration). On release day, the sync fails halfway through. Some pages are updated, some are not. The documentation portal is in an inconsistent state.

**Fix:** Implement a queue with configurable concurrency and exponential backoff. For Notion: max 2 concurrent requests, 500ms minimum delay between requests. For Confluence: max 5 concurrent requests. Test with a volume simulation before going to production.

---

### AP-16: Treating auto-generated documentation as final

**What it looks like:** The team auto-approves all generated documentation because "the AI is good enough."

**Why it happens:** Review queues pile up and the team takes shortcuts.

**Why it fails:** AI-generated documentation has subtle errors - especially for complex permissions models, edge cases, and error states. These errors are worse than no documentation because users trust them and act on them.

**Fix:** Auto-approve only genuinely low-risk changes: minor text updates, screenshot refreshes, typo corrections. All new feature documentation and breaking change documentation must go through human review. The review burden is manageable when auto-approve handles the high-volume, low-risk cases.

---

### AP-17: Hardcoding documentation structure

**What it looks like:** The documentation taxonomy (which sections exist, what they're called, how they're organized) is hardcoded into the Generator Agent prompt and the distribution platform structure.

**Why it happens:** The initial taxonomy seemed obvious and permanent.

**Why it fails:** Products evolve. Sections get renamed, split, merged, or deleted. When the taxonomy changes, the system breaks - duplicate pages appear, orphaned sections accumulate, and engineers manually clean up the mess.

**Fix:** Store the documentation taxonomy in the database (`sections` table), not in code or prompts. The Generator Agent queries the taxonomy at runtime. When the taxonomy changes, update the database; the pipeline adapts automatically.

---

### AP-18: No monitoring on the pipeline itself

**What it looks like:** The documentation pipeline has no metrics, no error alerting, and no visibility into whether it's running.

**Why it happens:** Observability is added after "more important" features.

**Why it fails:** The pipeline silently fails for days. Nobody notices until a stakeholder asks why the documentation hasn't been updated in two weeks.

**Fix:** Before shipping to production, instrument these four metrics:
1. Events processed per hour (alerts if drops to zero for > 2 hours).
2. Documentation generation success rate (alerts if drops below 90%).
3. Distribution sync success rate per platform (alerts if drops below 95%).
4. Time from commit to published documentation (alerts if p99 exceeds 30 minutes).

Use whatever observability stack you already have (Datadog, Grafana, Cloudwatch). Don't introduce a new tool just for this.

---

### AP-19: Not testing with real repository commits

**What it looks like:** The pipeline is developed and tested against synthetic diffs written by the team. It is deployed to production with real commit history for the first time on launch day.

**Why it happens:** Synthetic diffs are predictable and controlled. Real commits are messy.

**Why it fails:** Real commits contain things synthetic diffs don't: merge commits with thousands of changed lines, rebases, commits with empty messages, commits touching binary files, commits in languages the parser doesn't expect.

**Fix:** Before launch, run the pipeline against the last 6 months of actual commit history in read-only mode (generate documentation drafts but do not publish). Identify and fix every parsing error, generation failure, and edge case. Launch only after the pipeline handles real history cleanly.

---

### AP-20: Assuming MCPs are always available

**What it looks like:** The Distribution Agent calls the Notion MCP and throws an unhandled exception if the MCP is unavailable. The entire documentation pipeline stalls.

**Why it happens:** Developers test on local machines where MCP connections are stable.

**Why it fails:** In production, MCPs experience downtime, rate limits, and transient errors. A Notion outage should not stop the documentation pipeline from processing commits and queuing approvals.

**Fix:** Implement the circuit breaker pattern per MCP. If a MCP call fails after 3 retries with exponential backoff, the documentation is queued for a retry (not discarded), the failure is logged and alerted, and the pipeline continues processing other events. Treat every external MCP as unreliable by design.
