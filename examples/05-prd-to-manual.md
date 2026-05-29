# Example 5: PRD + Functional Requirements + Code to User Manual

## Context

A 25-person SaaS product team uses Confluence for product requirements (PRDs) and
URDs (user requirements documents), Linear for functional requirements and issue
tracking, and GitHub for their codebase.

The team ships features that are well-specified in documents but the documentation
never catches up. Product managers write PRDs. Engineers write Linear issues with
acceptance criteria. Code gets merged. No one writes user documentation.

They want ADUMAS to read the PRD from Confluence, the functional requirements from
Linear, compare them against the actual code that shipped, identify gaps and
discrepancies, and produce a user manual as a Markdown file.

---

## What They Asked Claude

```
I want ADUMAS to read our PRDs from Confluence, pull the functional requirements
from Linear, compare them against the code in GitHub, find any gaps or things that
shipped differently than specified, and then generate a user manual in Markdown.

The default output should always be a Markdown file we can put directly into our
docs portal.
```

---

## What the Skill Produces

### Architecture for This Workflow

```
Confluence MCP                Linear MCP               GitHub MCP
(fetch PRD)                   (fetch FRs)              (fetch merged PRs)
     |                             |                         |
     v                             v                         v
PRD Parser                   FR Extractor             Commit Intelligence
(extract goals,              (extract acceptance      (extract what actually
 user stories,                criteria, user           shipped: diffs, tests,
 scope, non-goals)            flows, edge cases)       API changes)
     |                             |                         |
     +-----------------------------+-------------------------+
                                   |
                                   v
                        Specification Reconciler
                        (compare PRD intent vs FR detail
                         vs code reality; surface gaps,
                         deviations, missing coverage)
                                   |
                          +--------+--------+
                          |                 |
                          v                 v
                    Gap Report       Documentation Generator
                    (Markdown)       (user manual in Markdown)
                          |                 |
                          v                 v
                  Linear: create     Output: feature-name.md
                  issue for each     (Markdown, default format)
                  gap found
```

### Step-by-Step Workflow

**Trigger:** A Linear issue transitions to "Done" and is linked to a Confluence PRD.
Alternatively: manual trigger by typing a prompt like "Generate user manual for the
Export feature" to ADUMAS.

**Step 1: Fetch the PRD from Confluence**

The Confluence MCP fetches the linked PRD page. The PRD Parser extracts:

- Feature goals and success metrics
- Target user persona and use cases
- User stories (in "As a... I want... So that..." format)
- Scope: what is in and out
- Acceptance criteria stated in the PRD
- Known limitations or deferred decisions

**Step 2: Fetch functional requirements from Linear**

The Linear MCP fetches all issues linked to the PRD milestone or epic. The FR
Extractor pulls from each issue:

- Title and description
- Acceptance criteria (from the issue body or checklist)
- User-facing behavior vs implementation details (tagged separately)
- Edge cases and error states documented in the issue
- Any deviations from the original PRD noted in comments

**Step 3: Fetch what actually shipped from GitHub**

The Commit Intelligence Agent reads all merged PRs linked to the Linear milestone:

- Code diffs: what files changed, what functions were added or modified
- Test files: what behaviors are covered by tests (tests as specification)
- PR descriptions: what the engineer documented about the change
- Any UI changes visible in the diff (component names, labels, form fields)

**Step 4: Reconcile specification against reality**

The Specification Reconciler compares all three sources and produces:

```json
{
  "feature": "CSV Export",
  "prd_intent": "Users can export any dataset view as a CSV with all visible columns",
  "fr_detail": [
    "Exports up to 100,000 rows",
    "Includes all columns currently visible in the table",
    "Excludes computed columns unless explicitly included",
    "Email download link for exports over 10,000 rows"
  ],
  "shipped": [
    "Export button present in table header (confirmed via diff)",
    "Row limit: 50,000 (FR says 100,000 - DEVIATION)",
    "Computed columns excluded (matches FR)",
    "Email download not implemented (FR says yes - GAP)"
  ],
  "gaps": [
    {
      "type": "deviation",
      "fr": "Exports up to 100,000 rows",
      "shipped": "Exports up to 50,000 rows",
      "action": "Document the actual limit (50,000); create Linear issue to reconcile"
    },
    {
      "type": "missing",
      "fr": "Email download link for exports over 10,000 rows",
      "shipped": "Not found in codebase",
      "action": "Exclude from user manual; create Linear issue for follow-up"
    }
  ]
}
```

**Step 5: Generate the user manual in Markdown**

The Documentation Generator produces a Markdown file based on what actually shipped,
not what was specified. Gaps are flagged in the gap report but not included in the
user manual (do not document what does not work).

Output file: `docs/features/csv-export.md`

**Step 6: Create Linear issues for gaps**

For each gap found in Step 4, the system creates a Linear issue:

- Title: `[Docs gap] CSV Export: 50k row limit not in FR`
- Description: The shipped behavior differs from the FR. The manual documents the actual
  behavior (50k). The FR needs to be updated or a follow-up issue created to ship 100k.
- Labels: `documentation`, `spec-gap`
- Links: back to the original Linear issue and the Confluence PRD

---

## The Generated Markdown Output

The default output format for ADUMAS is always Markdown. Here is what the generated
`csv-export.md` looks like:

```markdown
# Export Data as CSV

Export any table view as a CSV file to analyze your data in Excel, Google Sheets,
or any tool that reads CSV format.

## Before you begin

- You need View access or higher to export data.
- Large exports (over 10,000 rows) may take a few minutes to process.

## Export a table view

1. Open the table you want to export.
2. Apply any filters or column selections you want reflected in the export.
3. Click **Export** in the table header.
4. Select **Export as CSV**.
5. For exports under 10,000 rows: the file downloads immediately.
6. For exports over 10,000 rows: a processing message appears.
   Your export will be ready within a few minutes. Refresh the page to check status.

## What is included in the export

The CSV export includes all columns currently visible in your table view.
To control which columns appear in the export, show or hide columns before exporting.

Computed columns (columns calculated from other fields) are not included in CSV exports.

## Export limits

You can export up to 50,000 rows in a single CSV file. To export larger datasets:

- Apply filters to reduce the result set below 50,000 rows
- Export in multiple filtered batches and combine the files

## Troubleshooting

**The Export button is not visible.**
The Export button requires at least View access. Ask your administrator to verify
your role has export permissions enabled.

**My export is missing some columns.**
Computed columns are not included in CSV exports. Verify that the columns you need
are not computed fields. You can check by hovering over the column header.

## Related topics

- [Filter and sort table views]
- [Import data from CSV]
- [Data access and permissions]
```

---

## Gap Report Output

Alongside the user manual, ADUMAS produces a gap report in Markdown:

```markdown
# Gap Report: CSV Export - v1.2.0

Generated: 2026-05-29
Sources: Confluence PRD (page 18273645), Linear Epic EXP-142, GitHub PRs #891 #903 #911

## Summary

- 1 deviation between FR and shipped behavior
- 1 missing feature (specified in FR, not shipped)
- 0 PRD goals without FR coverage
- 0 FR items without code evidence

## Deviations

### Row limit: 50,000 shipped vs 100,000 specified

**FR:** Exports up to 100,000 rows  
**Shipped:** Exports up to 50,000 rows (confirmed in PR #903, ExportService.MAX_ROWS = 50000)  
**Documentation:** Documented as 50,000 (actual behavior)  
**Action required:** Update FR or create follow-up issue to ship 100k limit  
**Linear issue created:** EXP-187

## Missing features

### Email download link for large exports

**FR:** Email download link for exports over 10,000 rows  
**Shipped:** Not found in codebase  
**Documentation:** Excluded from user manual  
**Action required:** Implement or remove from FR  
**Linear issue created:** EXP-188

## Coverage confirmed

- Export button in table header: documented
- Column visibility respected: documented
- Computed columns excluded: documented
- Access permission requirement: documented
```

---

## Key Decisions Made

**Markdown as default output, always.** The user manual is generated as a `.md` file
that can be dropped directly into any docs portal (Docusaurus, Mintlify, GitHub Pages,
GitBook, Notion via import). PDF and DOCX are rendered on demand from the Markdown source.

**Document what shipped, not what was specified.** A user manual that documents a feature
that does not exist is worse than no documentation. The Specification Reconciler separates
what is in the codebase from what is only in the FR. Only shipped behavior appears in the
user manual.

**Gaps create Linear issues, not manual todo notes.** Every gap is automatically tracked.
Nothing falls through the cracks. The product and engineering teams can triage the gap
issues in their normal workflow.

**Tests as additional specification source.** Unit tests and integration tests document
expected behavior precisely. When the FR is vague, the test suite often contains the
definitive answer. The Commit Intelligence Agent reads test files for behavioral specification.

**PRD, FR, and code are all required inputs.** If any one is missing, the reconciliation
is incomplete. The system prompts for the missing input before generating documentation.
If the Confluence page cannot be fetched (permissions), the user is asked to paste the
relevant section.

---

## What to Watch Out For

**Stale Confluence pages.** PRDs are often written before the feature is built and never
updated after. A Confluence PRD that is 6 months old may not reflect the final scope.
The Specification Reconciler should note the PRD's last-edited date and flag PRDs
not updated in the last 90 days as potentially stale.

**Linear issues without acceptance criteria.** Many engineers write Linear issues as
implementation notes, not behavioral specifications. If the issue body lacks acceptance
criteria, the FR Extractor will have limited material. Encourage teams to write Linear
issues with a "User behavior" section separate from the "Implementation" section.

**Ghost features in the codebase.** Sometimes code exists for a feature that was built
but never fully shipped (behind a feature flag, disabled in production). The Commit
Intelligence Agent will find the code and try to document it. Add a check: if a feature
flag is found in the code, mark the feature as "not yet available" in the user manual
and create a placeholder page.

**Conflicting terminology between PRD and code.** The PRD calls it "workspace"; the code
calls it "organization"; the UI calls it "team." The Terminology Normalizer should resolve
these against the product glossary before documentation is generated.
