---
description: Generate a complete user manual as a folder of Markdown files. Reads the codebase, fetches specs from any tool (Linear, Confluence, Jira, Notion, etc.), maps user journeys, and outputs one file per section ready to drop into any docs portal.
---

# Generate Manual

You are generating a complete user manual using the ai-doc-architect Documentation Standards.

## Step 1: Gather context

Ask the user for:
- **What to document:** module name, product area, or "the entire platform"
- **Codebase location:** path to the project, or confirm it is the current directory
- **Spec sources:** where are URDs, PRDs, feature briefs, and acceptance criteria stored? (Linear, Docmost, Confluence, Jira, Notion, GitHub, or paste directly). Ask, but do not rely on the answer - Step 2 searches for specs regardless. See `SKILL.md` Source Precedence and Validation.
- **Audiences:** who will read this? (end users, admins, developers, support staff)
- **Output location:** where should the docs folder be created? (default: `docs/[product-name]/` inside the current project)

If the user has already provided this context in their message, skip asking and proceed directly.

## Step 2: Discover and read the sources

Read `SKILL.md` Source Precedence and Validation before this step. Passes 1 to 3 happen here.

**Discover.** Always search for a URD, PRD, and feature brief, even if the user named none. Check connected MCPs (Linear **documents** as well as issues, Docmost spaces, Confluence, Jira, Notion) and the repo (`docs/specs/`, `docs/prd/`, files named URD, PRD, spec, requirements, brief). If you find a spec the user did not mention, tell them before going further. Never generate from code alone while a spec sits unread.

**Read the specs.** Fetch each one **including its child pages**. Extract scope, personas, terminology, acceptance criteria, and non-goals. Discard implementation notes.

**Read the code.** Routes, controllers, validation rules, models, policies, feature tests, config files, migrations. Build the permission matrix from the policy layer. Record the commit SHA you are reading.

## Step 3: Map user journeys

Now that you know what exists, produce a journey map for each primary persona: a numbered sequence from the user's trigger to their completed outcome. Each step becomes one file.

The journey map comes **after** ingestion, not before. A journey invented before reading the routes and policies is a guess about the product, and every file in the output inherits it.

## Step 4: Validate every claim

Pass 4. Do not write prose yet.

List every factual claim that would appear in the manual - limits, timeouts, expiries, role restrictions, required fields, error messages, state names, defaults, orderings, permissions - and give each one a verdict against code:

| Verdict | What happens |
|---|---|
| **Verified** | Write it |
| **Contradicted** | Write what the code does, log the conflict in the gap report |
| **Unshipped** | Never write it. Gap report only |
| **Unverified** | Never write a number or a rule. Flag in the inference report |

Save the ledger as `docs/[product-name]/validation-report.md` with the commit SHA.

**The gate: a section may not be written until every claim in it has a verdict.**

## Step 5: Generate the folder output

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

## Step 6: Apply Documentation Standards

Every section file must follow the Documentation Standards (`references/standards.md` Sections 20-22):
- Entry point trigger at the top (why the user is here)
- Scenario in blockquote format
- Step-by-step tasks in second-person active voice
- "What to do next" connector at the end of every completed action
- Decision forks where roles or config produce different screens
- Waiting states documented explicitly
- No internal field names or implementation details

## Step 7: Capture screenshots (optional, needs a running app)

Only if the user can give you a URL for a running instance with seeded fixture data.
If they cannot, skip this step and generate the manual without images. Do not ask them
to install a browser mid-generation.

Read `references/screenshots.md` for the full mechanics. In short:

1. Derive a capture manifest from the journey map. One entry per step that passes the
   four tests in `screenshots.md` Section 2: gated action, unnameable element, visual
   outcome, or role fork. Most steps fail all four and get no image.
2. Write `docs/[product-name]/.captures/manifest.json` and `capture.mjs`.
3. Run the capture. Pin viewport, colour scheme, timezone, locale, and reduced motion so
   reruns are byte-stable. Mask every element that can hold personal data.
4. Embed each image **after** the step it confirms, with alt text describing the contents.
   The step must read correctly with the image removed.
5. Any shot that fails: omit the image, keep the words, log it in the inference report
   under `Screenshots not captured`. Never emit a broken link or a placeholder.

## Step 8: Run the plain-language pass

Last edit before any file is saved. Read `references/plain-language.md` and apply the 28
numbered rules to every page.

Rules 1-5 justify an edit on a single sighting: not-X-but-Y contrasts, closers that
repeat the section, sayings that sound deep, run-ups before the point, and arguments with
no one. Then sweep for the tells that survive rewrites: stock words ("seamlessly",
"robust", "leverage", "simply"), bold used as decoration, Title Case headings, hedged
facts where a real number exists, and chatbot residue.

What must survive: second person, bold on UI element names, entry-point triggers, outcome
statements, "What to do next" connectors, and every limit, role restriction, error case,
and system message. A page that reads well but lost the timeout value is worse than the
one it replaced.

## Step 9: Run the completeness checklist

Before saving any file, verify it passes the checklist in `references/standards.md` Section 20.9. A file with unchecked items is a draft.

## Output

Save all files to the output location the user specified. Report:
- Total files created
- Specs discovered and read, including any the user did not mention
- Claims validated, broken down by verdict
- Any gaps found (spec items with no code evidence)
- Any features found in code with no spec coverage
- Screenshots captured, and any capture that failed
- The commit SHA the manual was validated against
