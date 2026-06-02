---
description: Fetch a PRD or spec from any tool (Linear, Confluence, Jira, Notion, etc.), reconcile it against the shipped code, generate a user manual folder for what actually shipped, and a gap report for what was specified but not built.
---

# PRD to Manual

You are running the three-source reconciliation workflow to generate documentation from specifications and code.

## Step 1: Discover sources

Ask the user:
- **Where is the PRD/URD?** (Linear document, Confluence page, Jira epic, Notion page, GitHub, or paste directly)
- **Where are the acceptance criteria?** (may be the same tool or a separate issue tracker)
- **Where is the code?** (current directory, or specify a path)
- **Which feature or milestone?** (to scope which code to read)
- **Output location:** where should the docs folder be created?

## Step 2: Fetch the spec

Use the appropriate MCP connector for the user's tool:
- Linear MCP: `search_documentation`, `get_document`, `list_issues`
- Atlassian MCP: `getConfluencePage`, `searchConfluenceUsingCql`, `getJiraIssue`
- Notion MCP: `notion-fetch`, `notion-search`
- GitHub MCP: issues, discussions
- No MCP: ask user to paste the content directly

Extract: goals, user stories, acceptance criteria, scope, non-goals, implementation decisions.

## Step 3: Read the code

Read the controllers, models, policies, tests, and migrations that implement this feature. Feature tests are the most precise specification of what actually shipped - read every test file.

## Step 4: Reconcile

Compare spec against code:
- For each acceptance criterion: is there code evidence it shipped?
- For each code behavior: is it covered by the spec?
- Note deviations: where the code differs from the spec

Produce a structured reconciliation:
```
Specified and shipped:   document in the user manual
Specified, not shipped:  add to gap report, create tracked issue
Shipped, not specified:  document in user manual (it shipped)
Deviation:               document actual behavior, flag in gap report
```

**Key rule:** Never document a feature that did not ship. The user manual reflects only what users can do today.

## Step 5: Generate the folder output

Create the user manual as a folder of Markdown files (see `/generate-manual` for the full structure). Each section is one file. The master `index.md` links everything.

Create the gap report as a separate file:
```
docs/[product-name]/gap-report.md
```

The gap report covers: deviations, missing features, unspecified shipped behaviors, and a tracked issue for each gap.

## Step 6: Create tracked issues for gaps

If the user has an issue tracker connected (Linear MCP, Atlassian MCP), create one issue per gap automatically. Label with `docs-gap`. Link back to the spec and the relevant code.

## Output

Report:
- User manual files created
- Gaps found: deviations, missing features, unspecified behaviors
- Issues created in the tracker
