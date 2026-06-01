# Examples

These scenarios show how the `ai-doc-architect` skill adapts the ADUMAS architecture to specific real-world contexts. Each example is a complete story: who the team is, what they asked, what the skill produced, and why those specific choices were made.

## Start here

**New to the skill?** Read [`07-output-showcase.md`](./07-output-showcase.md) first.
It shows the actual output - a complete, real-looking manual section - so you know exactly
what you are getting before you run anything.

**Have an existing codebase with no docs?** Read [`06-existing-codebase-zero-docs.md`](./06-existing-codebase-zero-docs.md).
It covers the universal pattern for any stack - Laravel, Django, Rails, Node, Spring.

**Have PRDs and specs you want to reconcile against code?** Read [`05-prd-to-manual.md`](./05-prd-to-manual.md).

## The Examples

| File | What it shows | Read when |
|---|---|---|
| [`07-output-showcase.md`](./07-output-showcase.md) | Actual rendered manual output - a complete section with scenarios, steps, permission matrix, toasts, limits | You want to see what the output looks like before installing |
| [`06-existing-codebase-zero-docs.md`](./06-existing-codebase-zero-docs.md) | Universal pattern for generating docs from any existing codebase with zero existing documentation | You have a product that works but has no manual |
| [`05-prd-to-manual.md`](./05-prd-to-manual.md) | Three-source reconciliation: Confluence PRD + Linear FRs + GitHub code - gap report and manual from specs | You have specs and want to check what shipped vs what was planned |
| [`04-healthcare-hipaa.md`](./04-healthcare-hipaa.md) | Healthcare SaaS with HIPAA constraints, PHI-aware documentation, potential on-prem deployment | Your product handles health data |
| [`03-api-docs-only.md`](./03-api-docs-only.md) | API-first product, OpenAPI-driven, developer-only audience | You need API reference docs, not end-user guides |
| [`02-enterprise-full.md`](./02-enterprise-full.md) | 200-person enterprise, Confluence + Jira + Slack, RBAC, SSO, compliance | You are building the full platform, not an MVP |
| [`01-startup-mvp.md`](./01-startup-mvp.md) | 6-person startup, GitHub + Notion, 90-day MVP timeline | You want to ship something fast with minimal infrastructure |

## How to read an example

Each example either shows **actual output** (examples 06, 07) or walks through a
**scenario with decisions** (examples 01-05). The scenario examples follow this structure:

1. Context - who the team is and what problem they have
2. What they asked - the prompt they used
3. What was produced - the result
4. Key decisions - why specific choices were made
5. What to watch out for - gotchas for this scenario

## Using examples as prompts

The "What they asked" sections in examples 01-05 are real prompts. Paste them into
Claude with this skill installed (adapt the context to your situation) to see the
skill respond live.
