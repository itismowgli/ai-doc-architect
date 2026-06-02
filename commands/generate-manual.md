---
description: Generate a complete user manual as a folder of Markdown files. Reads the codebase, fetches specs from any tool (Linear, Confluence, Jira, Notion, etc.), maps user journeys, and outputs one file per section ready to drop into any docs portal.
---

# Generate Manual

You are generating a complete user manual using the ai-doc-architect Documentation Standards.

## Step 1: Gather context

Ask the user for:
- **What to document:** module name, product area, or "the entire platform"
- **Codebase location:** path to the project, or confirm it is the current directory
- **Spec sources:** where are PRDs, URDs, and acceptance criteria stored? (Linear, Confluence, Jira, Notion, GitHub, or paste directly)
- **Audiences:** who will read this? (end users, admins, developers, support staff)
- **Output location:** where should the docs folder be created? (default: `docs/[product-name]/` inside the current project)

If the user has already provided this context in their message, skip asking and proceed directly.

## Step 2: Map user journeys

Before writing anything, produce a journey map for each primary persona. A journey map is a numbered sequence from the user's trigger to their completed outcome. Each step becomes one file in the output.

## Step 3: Ingest sources

Read the relevant code: routes, controllers, validation rules, models, policies, feature tests, config files, migrations. Fetch specs from the user's tools using the appropriate MCP connector. Build the permission matrix from policy files before writing any section.

## Step 4: Generate the folder output

Create the documentation as a folder of Markdown files following this structure:

```
docs/[product-name]/
  index.md                    (master table of contents)
  01-introduction/
    index.md
    who-this-is-for.md
    quick-start.md
  02-[section-name]/
    index.md
    [workflow-1].md
    [workflow-2].md
  ...
  [n]-troubleshooting-and-reference/
    index.md
    troubleshooting.md
    glossary.md
    limits.md
    system-messages.md
```

Generate **two separate guides** for each major module:
- `[product]/user-guide/` - for end users performing daily tasks
- `[product]/admin-guide/` - for administrators and configuration

## Step 5: Apply Documentation Standards

Every section file must follow the Documentation Standards (references/writing-guide.md Sections 20-22):
- Entry point trigger at the top (why the user is here)
- Scenario in blockquote format
- Step-by-step tasks in second-person active voice
- "What to do next" connector at the end of every completed action
- Decision forks where roles or config produce different screens
- Waiting states documented explicitly
- No internal field names or implementation details

## Step 6: Run the completeness checklist

Before saving any file, verify it passes the checklist in writing-guide.md Section 20.9. A file with unchecked items is a draft.

## Output

Save all files to the output location the user specified. Report:
- Total files created
- Total word count
- Any gaps found (spec items with no code evidence)
- Any features found in code with no spec coverage
