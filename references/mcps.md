# MCP Integration Guide

This file answers the three questions that matter most when connecting MCPs to an AI documentation system:
**Which MCPs do I need? When do I connect them? And exactly how do they fit into the architecture?**

## Table of Contents

1. [MCP Priority Matrix](#mcp-priority-matrix)
2. [Source Ingestion MCPs](#source-ingestion-mcps-feeding-data-in)
3. [Distribution MCPs](#distribution-mcps-publishing-docs-out)
4. [Project Management MCPs](#project-management-mcps-tracking-work)
5. [Design MCPs](#design-mcps-capturing-ui-context)
6. [Communication MCPs](#communication-mcps-notifying-teams)
7. [MCP Wiring: How They Connect](#mcp-wiring-how-they-connect)
8. [What NOT to Connect](#what-not-to-connect)
9. [MCP Setup Checklist](#mcp-setup-checklist)

---

## MCP Priority Matrix

Read this before connecting anything. Not every MCP is worth the integration cost at every stage.

| MCP | Phase | Reason to Connect | Skip If |
|---|---|---|---|
| **GitHub MCP** | 🔴 MVP | Source of all code and commit intelligence - the system cannot work without this | You use a self-hosted Git system - use webhooks directly instead |
| **Notion MCP** | 🟡 MVP if using Notion | Primary destination for published documentation | Your team uses Confluence instead |
| **Atlassian MCP** (Confluence + Jira) | 🟡 MVP if using Confluence | Confluence = docs destination; Jira = create doc tickets | Your team uses Notion instead |
| **Trello MCP** | 🟢 Optional | Create documentation cards in Trello boards; track doc tasks on teams that use Trello for project management | Your team uses Linear, Jira, or Asana instead |
| **Linear MCP** | 🟡 Recommended | Create documentation tasks, link docs to issues | Your PM workflow is entirely Jira-based |
| **Slack MCP** | 🟡 Recommended | Notify teams on updates, staleness alerts, release notes | You use Teams - use the Teams webhook directly |
| **Figma MCP** | 🟢 Phase 2 | Extract UI component names and annotations for accurate screen docs | You document APIs only; no UI documentation needed |
| **Google Drive MCP** | 🟢 Optional | Export documentation as shareable Drive files | Your team lives in Notion or Confluence already |
| **Asana / ClickUp MCP** | 🟢 Optional | Create documentation work items in your PM tool | You use Linear or Jira |
| **Intercom MCP** | 🟢 Phase 3 | Sync help articles from the documentation system | You don't have an in-product support widget |
| **Fireflies / Otter MCP** | 🟢 Phase 3 | Extract documentation requirements from recorded product meetings | Meeting transcripts aren't a primary input |

---

## Source Ingestion MCPs (Feeding Data In)

These MCPs provide the raw signals the system analyzes to generate documentation.

### GitHub MCP

**Role in ADUMAS:** The single most critical integration. This is the source of commits, PR diffs, release tags, file trees, and linked issue descriptions. Every documentation update starts here.

**What it provides:**
- Commit metadata (author, message, timestamp, changed files)
- PR diffs (what code actually changed)
- Release tags (triggers the changelog and release documentation workflow)
- File tree (allows the Codebase Analyst Agent to traverse the repo structure)
- Linked issues (provides feature context that commit messages alone miss)

**Required OAuth scopes:**
```
repo          - read commits, PRs, file contents, issues
read:org      - identify team members for author attribution
```

**How to wire it:**

The GitHub MCP replaces both webhook setup and manual API calls. Use it in the Commit Intelligence Agent to:

```
1. On PR merge → get_diff(pr_number) → extract changed files and diff
2. On release tag → get_release(tag) → extract release notes and linked PRs
3. Nightly → list_commits(since=yesterday) → scan for undocumented changes
```

**What NOT to do with it:**
- Don't use the GitHub MCP as a polling mechanism (check every N minutes). Use webhooks to trigger the pipeline; use the MCP only to fetch data after a trigger.
- Don't pull entire file contents for every commit scan. Fetch only the files in the diff.

**GitLab equivalent:** Use the GitLab REST API via webhook. There is no native GitLab MCP in the Cowork ecosystem at time of writing - wire the GitLab webhook directly to the Ingestion Orchestrator instead.

---

## Distribution MCPs (Publishing Docs Out)

These MCPs are where generated documentation ends up.

### Notion MCP

**Role in ADUMAS:** The primary distribution target for teams that use Notion as their internal knowledge base. The Distribution & Sync Agent uses this MCP to create and update Notion pages from approved documentation drafts.

**What it provides:**
- Create and update pages with rich block content
- Manage databases (use Notion databases for documentation indexes and version matrices)
- Two-way sync capability (detect pages deleted in Notion and handle gracefully)

**Required OAuth scopes:**
```
read_content     - read existing pages to detect changes and avoid duplicate creation
update_content   - write and update documentation pages
insert_content   - create new pages and blocks
```

**How to wire it:**

Map each documentation section to a Notion page hierarchy:

```
Notion Workspace
└── [Product Name] Documentation
    ├── Getting Started        ← maps to getting-started/* documentation nodes
    ├── User Guide             ← maps to user-guide/* documentation nodes
    ├── Admin Guide            ← maps to admin/* documentation nodes
    ├── API Reference          ← maps to api-reference/* documentation nodes
    └── Release Notes          ← maps to changelog/* documentation nodes
```

On each approved documentation update:

```
1. Search Notion for the page matching the documentation node title
2. If found → update_page() with new Markdown-converted blocks
3. If not found → create_page() under the correct parent
4. Store the Notion page_id in the documentation_nodes table for future updates
```

**What NOT to do:**
- Don't create a new Notion page on every documentation update. Maintain a `page_id` mapping in your database to update existing pages in place.
- Don't push raw Markdown to Notion - convert to Notion block format first. The Notion MCP handles this, but ensure your Markdown is clean (no custom MDX components).
- Don't sync draft documentation. Only publish approved content.

---

### Atlassian MCP (Confluence + Jira)

**Role in ADUMAS:** For enterprise teams using Atlassian products, this single MCP covers both Confluence (documentation destination) and Jira (documentation task tracking).

**Confluence - what it provides:**
- Create and update Confluence pages in structured spaces
- Manage page hierarchies and labels
- Attach files (PDF and DOCX exports)

**Jira - what it provides:**
- Create documentation tickets automatically when breaking changes are detected
- Link documentation pages to related Jira stories
- Update ticket status when documentation is approved and published

**Required scopes:**
```
read:confluence-content.all    - read existing pages
write:confluence-content       - create and update pages
read:jira-work                 - read issues for linking
write:jira-work                - create documentation tickets
```

**How to wire Confluence:**

```
Confluence Space: [PRODUCT]-DOCS
├── Getting Started
├── User Guide
│   ├── Feature A
│   └── Feature B
├── Admin Guide
├── API Reference
└── Release Notes
    ├── v2.1.0
    └── v2.0.0
```

Use Confluence's `ancestor` parameter to create pages under the correct parent:

```
1. On documentation approval → createConfluencePage(spaceKey, title, body, ancestor_id)
2. Map documentation node IDs → Confluence page IDs in your database
3. On subsequent updates → updateConfluencePage(page_id, new_body, version+1)
```

**How to wire Jira:**

Create a documentation ticket automatically when:
- A breaking API change is detected with no corresponding documentation update
- A PR is merged that modifies a feature with documentation_coverage_score < 0.7
- A scheduled audit detects documentation staleness above the threshold

```
createJiraIssue(
  project="DOCS",
  type="Task",
  summary="Update docs: [Feature Name] changed in PR #[number]",
  description="[Auto-generated context from change manifest]",
  labels=["auto-generated", "documentation"]
)
```

**PRD-to-Manual workflow with Confluence:**

This is a key ADUMAS workflow: read the product specification from Confluence,
compare it against what shipped in code, and generate a user manual in Markdown.

```
1. Fetch the PRD → getConfluencePage(page_id or CQL search by title)
2. Extract: goals, user stories, acceptance criteria, scope, non-goals
3. Fetch merged PRs from GitHub MCP (linked to the same Jira epic or milestone)
4. Reconcile PRD intent vs shipped code (Specification Reconciler agent)
5. Generate user manual as Markdown (default format, always)
6. Generate gap report as Markdown
7. Create Jira issues for each gap: type="Task", project="DOCS", label="spec-gap"
8. Publish approved manual to Confluence via createConfluencePage or updateConfluencePage
```

**Trigger options:**
- Manual: user types "generate docs for the Export feature" and provides the Confluence page URL
- Automatic: when a Jira epic transitions to "Done," trigger reconciliation for all linked stories

**What NOT to do:**
- Don't create a Jira ticket for every commit. Only create tickets for changes that exceed the configured staleness or coverage threshold.
- Don't let Jira become a backlog graveyard. Close tickets automatically when the documentation is updated and published.
- Don't document what did not ship. The user manual reflects reality; the gap report tracks the rest.

---

## Project Management MCPs

### Linear MCP

**Role in ADUMAS:** The recommended PM integration for teams that use Linear. Used by the Quality Agent to surface documentation gaps as actionable work items, and by the CI/CD gate to block PRs with insufficient documentation.

**What it provides:**
- Create issues in documentation-specific teams or projects
- Link documentation issues to the product issues they relate to
- Update issue status automatically when documentation is approved

**Required scopes:**
```
issues:create    - create documentation gap tickets
issues:read      - check existing tickets before creating duplicates
issues:update    - close tickets on documentation completion
```

**How to wire it:**

```
Linear Team: Documentation
├── Project: Documentation Backlog
├── Project: Release Docs (created per release)
└── Project: API Documentation

Workflow states: Identified → In Progress → In Review → Done
```

On documentation gap detection:
```
1. Check if an open Linear issue already exists for this documentation gap
   (search by title pattern to avoid duplicates)
2. If none → create issue with:
   - Title: "Missing docs: [feature/API name]"
   - Description: [auto-generated context]
   - Label: "auto-generated"
   - Linked issue: [product issue ID if available]
3. On documentation approval → transition issue to Done
```

**PRD-to-Manual workflow with Linear:**

When the user wants to generate documentation from Linear functional requirements:

```
1. Fetch the Linear epic and all linked issues: list_issues(epic_id or milestone)
2. For each issue: extract title, description, acceptance criteria, user-facing behaviors
3. Separate user-facing behaviors from implementation details
4. Feed extracted behaviors to the Specification Reconciler
5. Reconciler compares against GitHub diff for what actually shipped
6. Documentation Generator produces Markdown user manual from reconciled spec
7. Gap issues created in Linear for deviations: label="docs-gap", link to original issue
```

**Acceptance criteria extraction prompt:**

```
From this Linear issue, extract only the user-facing acceptance criteria.
Ignore implementation details, technical constraints, and internal behavior.
Output as a bulleted list of: "The user can [action] and sees [result]."

Issue title: {title}
Issue body: {body}
```

**What NOT to do:**
- Don't create Linear issues for every minor staleness flag. Set a staleness threshold (e.g., score > 0.6) before creating tickets.
- Don't assign auto-generated tickets automatically. Let the team triage and assign.
- Don't treat Linear issue descriptions as final truth. Compare against what shipped in code.

---

## Design MCPs

### Figma MCP

**Role in ADUMAS:** Used by the Codebase Analyst Agent (Phase 2) to extract UI component names, screen annotations, and flow descriptions from Figma designs. This makes generated documentation accurate for UI-heavy features without requiring manual screenshot annotation.

**What it provides:**
- Read component names and descriptions from Figma files
- Get design annotations and specification notes
- Export component screenshots for embedding in documentation

**Required scopes:**
```
file:read     - read Figma file content and component metadata
```

**How to wire it:**

Maintain a mapping between product features and Figma file URLs in your feature manifest:

```sql
-- In feature_manifest table
figma_url TEXT    -- e.g. "https://figma.com/file/abc123/Product-Screens"
```

When generating documentation for a UI feature:
```
1. Look up figma_url from feature manifest
2. get_design_context(figma_url) → extract component names and descriptions
3. get_screenshot(frame_id) → fetch annotated screen image
4. Embed screenshot + extracted descriptions into the documentation draft
```

**What NOT to do:**
- Don't fetch entire Figma files - only the specific frames relevant to the feature being documented.
- Don't use Figma screenshots as the primary documentation for simple workflows. Screenshots go stale; step-by-step text instructions age better. Use screenshots as supplements, not the main content.
- Don't require Figma access to generate documentation. If the Figma URL is absent, the system should generate text-only documentation and flag the gap, not fail.

---

## Communication MCPs

### Slack MCP

**Role in ADUMAS:** Keeps the team informed about documentation events without requiring them to check a dashboard. The Distribution Agent uses this MCP to send targeted notifications.

**What it provides:**
- Post messages and formatted blocks to channels
- Send direct messages to specific users (e.g., PR authors)
- React to messages (useful for acknowledging resolved alerts)

**Required scopes:**
```
chat:write          - post messages to channels
users:read          - look up PR authors by email to send DMs
channels:read       - verify channel existence before posting
```

**When to send Slack notifications (and when not to):**

| Event | Notify? | Channel | Format |
|---|---|---|---|
| Release documentation published | ✅ Yes | `#releases` or `#engineering` | Rich block with version, link, summary |
| Breaking change docs queued for review | ✅ Yes | `#docs-review` + DM to PR author | Link to draft + review deadline |
| Documentation staleness alert (high severity) | ✅ Yes | `#docs-alerts` | Affected section + staleness score |
| Minor documentation update published | ❌ No | - | Too noisy - use digest instead |
| Every PR comment added | ❌ No | - | Engineers already get GitHub notifications |
| Weekly documentation health digest | ✅ Yes | `#engineering` or `#product` | Summary of health score, top gaps, recent updates |

**What NOT to do:**
- Don't notify on every documentation change. Slack fatigue is real - engineers will mute the channel and miss important alerts. Set a severity threshold.
- Don't use Slack as a documentation destination. Slack messages are ephemeral. Always publish to Notion, Confluence, or GitHub Pages as the permanent record.

---

## MCP Wiring: How They Connect

Here is the complete data flow showing where each MCP sits in the pipeline:

```
[INGESTION]
  GitHub MCP ──────────────────────┐
  Figma MCP (Phase 2) ────────────┤
  Atlassian MCP (Jira, read) ─────┤→  Ingestion Orchestrator
  Fireflies MCP (Phase 3) ────────┘       │
                                           ▼
[INTELLIGENCE]                     Event Bus (Kafka)
  (No MCPs - internal agents)             │
                                           ▼
[GENERATION]                       AI Agents
  (No MCPs - internal LLM calls)          │
                                           ▼
[MANAGEMENT]                       Approval Workflow
  Linear MCP ──────────────────── create/close tasks
  Atlassian MCP (Jira) ─────────  create/close tickets
                                           │
                             ┌─────────────┘
                             ▼
[DISTRIBUTION]
  Notion MCP ──────────────  publish/update pages
  Atlassian MCP (Confluence) publish/update pages
  Slack MCP ───────────────  notify teams
  Google Drive MCP ─────────  export PDF/DOCX (optional)
  Asana / ClickUp MCP ──────  create work items (optional)
```

**The key rule:** MCPs that read data belong in the Ingestion tier. MCPs that write data belong in the Distribution or Management tier. Never let a distribution MCP read source data - that creates coupling that makes the system fragile.

---

## What NOT to Connect

Some integrations seem useful but create more problems than they solve at each phase.

| Integration | Why to Avoid (or Delay) |
|---|---|
| **Direct database MCP** (PostgreSQL/MySQL) | Gives the AI agent write access to production data. Use a read-only replica for schema introspection only, via controlled query - not an open MCP connection. |
| **Salesforce MCP** | Customer data should not flow into documentation generation pipelines. Sales content (contracts, deal notes) is not documentation source material. |
| **Gmail / Outlook MCP** | Email is too unstructured and sensitive to use as a documentation source. Support tickets from a dedicated help desk (Zendesk, Intercom) are far more reliable. |
| **Calendar MCP** | Scheduling context has no meaningful input to documentation generation. |
| **Analytics MCP (raw event data)** | Raw analytics events are noisy. Use a pre-aggregated reporting layer (Amplitude summaries, Mixpanel exports) rather than direct event stream access. |

---

## MCP Setup Checklist

Use this checklist when wiring MCPs for the first time:

### Before connecting any MCP

- [ ] All MCP credentials stored in HashiCorp Vault or AWS Secrets Manager - never in `.env` files committed to Git.
- [ ] Each MCP connection uses the minimum OAuth scopes required. Reviewed and documented in this file.
- [ ] A test connection verified with a read-only call (e.g., `list_channels`, `search_files`) before wiring into production pipeline.
- [ ] Rate limits documented for each MCP. Backoff and retry logic implemented in the Distribution Agent.
- [ ] A circuit breaker configured per MCP - if a platform is down, the pipeline degrades gracefully instead of failing entirely.

### Notion MCP
- [ ] Notion workspace connected with `read_content`, `update_content`, `insert_content` scopes.
- [ ] Page hierarchy structure created manually before first sync (the agent writes to existing pages, not invents new hierarchies).
- [ ] `page_id` mapping table created in PostgreSQL to avoid duplicate page creation.

### GitHub MCP
- [ ] GitHub App or OAuth token with `repo` and `read:org` scopes.
- [ ] Webhook configured to fire on `push`, `pull_request`, and `release` events.
- [ ] Webhook secret configured and verified (prevents spoofed events).

### Atlassian MCP (Confluence + Jira)
- [ ] Confluence space and page hierarchy pre-created.
- [ ] Jira project `DOCS` (or equivalent) created with workflow states: Identified → In Progress → In Review → Done.
- [ ] Service account created with minimal permissions (not admin account).

### Linear MCP
- [ ] Linear team "Documentation" created.
- [ ] Workflow state mapping documented: `Identified` = created by agent, `In Progress` = assigned to human, `Done` = closed by agent on publication.
- [ ] Duplicate detection logic implemented before ticket creation.

### Slack MCP
- [ ] Channels created: `#docs-review`, `#docs-alerts`, `#releases` (or equivalent per team convention).
- [ ] Notification severity thresholds configured - tested to confirm minor updates do NOT trigger alerts.
- [ ] Weekly digest scheduled (Sunday evening or Monday morning, before the week's standup).

### Figma MCP (Phase 2)
- [ ] Figma file URL field added to `feature_manifest` table.
- [ ] Graceful fallback implemented: if Figma URL is absent, generate text-only documentation without failing.
- [ ] Screenshot storage configured in S3 with expiry policy (Figma screenshots go stale - refresh on each documentation generation run).
