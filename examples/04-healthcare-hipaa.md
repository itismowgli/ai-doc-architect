# Example 4: Healthcare SaaS - HIPAA Constraints, On-Prem Option

## Context

A 30-engineer healthcare SaaS company building a clinical trial management platform. They have:
- A GitHub Enterprise (self-hosted) installation - code cannot leave their network boundary
- Confluence for internal documentation (self-hosted Confluence Data Center)
- Jira (self-hosted) for all work tracking
- Slack for communication
- A compliance officer who reviews all documentation before it's published externally
- HIPAA Business Associate Agreement (BAA) in place with certain cloud vendors
- Active SOC 2 Type II audit

Their specific constraints:
1. Source code must never leave their on-premises network.
2. Patient data (PHI) must never touch the documentation pipeline.
3. The LLM used must operate under a zero-retention policy or run locally.
4. All actions must be auditable for HIPAA and SOC 2 compliance.
5. External documentation (for clinical sites and sponsors) must pass compliance review before publication.

---

## What They Asked Claude

```
We're a healthcare company - 30 engineers, GitHub Enterprise on-prem,
self-hosted Confluence and Jira. We're HIPAA-regulated and going through SOC 2.

Our code can't leave our network. We need AI to help automate documentation
but we're worried about sending code to external LLM APIs.

What are our options? Can this work without cloud LLMs?
What's the right architecture given our constraints?
```

---

## What the Skill Produces

### The HIPAA-Safe Documentation Architecture

Two deployment options, evaluated against the constraints:

**Option A: Hybrid (Recommended for most healthcare teams)**

Use external LLM with a zero-retention BAA, air-gapped ingestion.

```
GitHub Enterprise (on-prem)
     ↓ [Git webhook, stays on-prem]
Ingestion Agent (on-prem Docker container)
     ↓ [extracts only non-PHI metadata: function signatures, comments, diffs]
     ↓ [NEVER sends raw patient data fields, database records, or PHI]
Claude API (Anthropic - zero-retention BAA signed)
     ↓ [processes only sanitized code metadata]
Documentation Generator (on-prem)
     ↓
Confluence Data Center (on-prem) via self-hosted Atlassian MCP
     ↓
Compliance Review Queue → Compliance Officer approves
     ↓ [only after approval]
External Publication (if applicable)
```

**Option B: Fully On-Premises (for highest-sensitivity environments)**

Use a local LLM via Ollama or vLLM. No external API calls.

```
GitHub Enterprise (on-prem)
     ↓
Ingestion Agent (on-prem)
     ↓
Local LLM: Llama 3.1 70B via Ollama (on-prem GPU server)
     ↓
Documentation Generator (on-prem)
     ↓
Confluence Data Center (on-prem)
     ↓
Compliance Review Queue
```

**Tradeoffs:**

| Criterion | Option A (Hybrid) | Option B (Fully On-Prem) |
|---|---|---|
| Documentation quality | High (Claude Sonnet) | Moderate (Llama 70B ~60% of Claude quality) |
| Data leaves network? | Sanitized code metadata only | Never |
| Setup complexity | Moderate | High (GPU server required) |
| Cost | Anthropic API costs | GPU server capital cost |
| BAA required? | Yes (Anthropic provides) | No |
| Recommended for | Most healthcare SaaS | Air-gapped clinical systems |

### PHI Detection and Exclusion

The most critical safety component: the pipeline must detect and exclude any PHI before processing.

The Ingestion Agent scans every code diff for patterns matching PHI categories:

```python
PHI_PATTERNS = [
    r'\b\d{3}-\d{2}-\d{4}\b',           # SSN
    r'\b\d{10}\b',                        # NPI (provider ID)
    r'\b[A-Z]{2}\d{6}\b',               # Medical record number pattern
    r'patient_name|patient_id|dob|',     # Variable names suggesting PHI
    r'diagnosis_code|icd_10|icd_11',     # Diagnosis code references
]
```

If PHI patterns are detected in a code comment or docstring, the system:
1. Redacts the matching content before sending to the LLM.
2. Logs the detection event (file, line range, pattern matched - not the PHI itself).
3. Alerts the compliance officer via Slack.
4. Flags the documentation draft as requiring manual PHI review before publication.

**What counts as PHI in documentation context:**
- Patient names, dates of birth, medical record numbers embedded in code examples.
- Real participant IDs used in test fixtures committed to the repo.
- Database column names or schema definitions that reveal the structure of PHI tables (requires case-by-case judgment - column names like `patient_first_name` are not PHI themselves, but context matters).

### Compliance Review Workflow

External-facing documentation (for clinical sites, sponsors, and regulatory bodies) has an additional compliance gate beyond the standard technical review:

```
Documentation Draft Generated
     ↓
Technical Review (Engineering Lead) - accuracy and completeness
     ↓
Compliance Review (Compliance Officer) - regulatory language, PHI absence verification
     ↓ [only after both approvals]
External Publication
     ↓
Audit Record Created (immutable, timestamped, both approvers logged)
```

Internal documentation (for engineers and internal ops) follows the standard single-reviewer workflow.

### Self-Hosted MCP Configuration

Because this team runs GitHub Enterprise and Confluence Data Center on-premises, the MCPs must be configured to point to self-hosted instances, not cloud endpoints.

**GitHub MCP (self-hosted):**
```
GITHUB_API_URL=https://github.your-company.internal/api/v3
GITHUB_TOKEN=[service account PAT with repo scope]
```

**Atlassian MCP (self-hosted Confluence Data Center):**
```
CONFLUENCE_BASE_URL=https://confluence.your-company.internal
CONFLUENCE_USERNAME=[service account]
CONFLUENCE_API_TOKEN=[token from Confluence admin]
```

**Important:** Verify that your MCP connector supports self-hosted endpoints before selecting it. Some MCP connectors are hardcoded to cloud endpoints and do not support enterprise self-hosted instances. In that case, use the REST API directly (Confluence Cloud API and self-hosted API use the same structure; only the base URL differs).

### Audit Log Requirements

For HIPAA and SOC 2, the following events must be logged with full traceability:

| Event | Required Data |
|---|---|
| PHI pattern detected in code diff | File path, line range, pattern type (not the PHI), timestamp, actor (pipeline) |
| Documentation draft generated | Source commit SHA, agent ID, model used, content hash, timestamp |
| Compliance review started | Reviewer user ID, documentation node ID, timestamp |
| Compliance review completed | Reviewer user ID, outcome (approved/rejected), notes, timestamp |
| Documentation published | Target (Confluence page ID), content hash, publisher, timestamp |
| Documentation accessed (external) | User ID (or external token), documentation node ID, timestamp |

Audit logs stored in: Postgres (append-only table) + exported nightly to a separate, access-restricted S3 bucket (or on-prem object store). Retain for 6 years (HIPAA minimum).

---

## Key Decisions Made

**Option A (Hybrid) over Option B (Fully On-Prem) for most teams.** The quality difference between Claude Sonnet and Llama 70B is significant for technical writing tasks. If a zero-retention BAA is available from Anthropic (it is), and if only sanitized code metadata (not PHI) is sent to the API, Option A is safe for HIPAA compliance and produces materially better documentation.

**No cloud Confluence or Jira.** This team uses self-hosted Atlassian products. Do not attempt to mirror data to Atlassian Cloud to use the cloud MCP - that would move data off-premises in violation of their compliance posture. Use the self-hosted REST API directly.

**Compliance Officer in the approval chain for external docs.** HIPAA requires that any documentation disclosing system capabilities (which could indirectly reveal PHI handling) is reviewed by a qualified compliance person. This is not optional. Build it into the workflow as a hard gate, not a suggestion.

**Separate audit log storage from application storage.** The `audit_logs` table lives in the main application database for operational queries, but is also replicated to a separate, access-restricted storage location (S3 bucket or on-prem equivalent) that cannot be modified by application credentials. This ensures the audit trail remains intact even if application credentials are compromised.

---

## What to Watch Out For

**Test data with real PHI.** Healthcare engineering teams sometimes use real (or realistic) patient data in test fixtures - especially if the product was built by clinical domain experts who thought it was "just test data." Run the PHI detector against the full repository history, not just future commits, before onboarding the repo to the pipeline.

**Llama 70B infrastructure requirements (Option B).** Running Llama 3.1 70B locally requires an NVIDIA A100 or H100 GPU (or equivalent). Budget approximately $2,000–$5,000/month in cloud GPU costs if using a managed service, or $30,000–$100,000 in capital for on-prem hardware. This is the real cost of "no external API calls" - factor it in when comparing options.

**Self-hosted MCP version drift.** Self-hosted Confluence and GitHub Enterprise versions lag behind cloud releases. Check the MCP connector's minimum version requirements against your installed versions before committing to this integration. If the MCP requires Confluence 8.x but you're on 7.x, plan the upgrade first.

**The compliance officer bottleneck.** If every external documentation update requires compliance review, and the compliance officer has 10 hours/week for documentation, you have at most 10 hours of review capacity per week. Design the workflow to batch reviews: the pipeline accumulates approved technical drafts and presents them for compliance review in a single weekly session, not as individual requests throughout the week.
