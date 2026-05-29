# Examples

These scenarios show how the `ai-doc-architect` skill adapts the ADUMAS architecture to specific real-world contexts. Each example is a complete story: who the team is, what they asked, what the skill produced, and why those specific choices were made.

## How to Read These Examples

Each example follows the same structure:

1. **Context** - Who is this team and what problem are they solving?
2. **What They Asked Claude** - The actual prompt(s) the user typed.
3. **What the Skill Produced** - The architecture and recommendations Claude gave.
4. **Key Decisions** - Why these specific choices for this specific context.
5. **What to Watch Out For** - Gotchas specific to this scenario.

## The Examples

| File | Scenario | Team Size | Key Constraint |
|---|---|---|---|
| [`01-startup-mvp.md`](./01-startup-mvp.md) | 6-person startup, GitHub + Notion, 90-day MVP | 6 engineers | Speed to value; minimal infra ops |
| [`02-enterprise-full.md`](./02-enterprise-full.md) | 200-person enterprise, Confluence + Jira + Slack, full platform | Large org | RBAC, SSO, audit logging, compliance |
| [`03-api-docs-only.md`](./03-api-docs-only.md) | API-first company, OpenAPI-driven, developer audience only | 15 engineers | Developers only; deep technical accuracy |
| [`04-healthcare-hipaa.md`](./04-healthcare-hipaa.md) | Healthcare SaaS, HIPAA constraints, potential on-prem | 30 engineers | No PHI in docs pipeline; data residency |

## Using Examples as Prompts

You can paste any of the "What They Asked Claude" prompts directly into Claude (with this skill installed) to see the skill in action. Modify the context details to match your own situation.
