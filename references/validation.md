# Source Precedence and Validation

**Read when:** starting any generation run, or when deciding whether a claim can be
written. Every command depends on this file.
**Skip if:** the question is purely about architecture, tech stack, or writing style.
**Cost:** ~1.5k tokens. Short by design - it runs on every job.

**Nothing is written until it has been validated against code. No exceptions, no run type.**

Two different questions, two different sources:

| Question | Authoritative source |
|---|---|
| **What should exist, what is it called, who is it for?** Scope, terminology, personas, acceptance criteria | The URD, PRD, or feature brief |
| **What actually happens?** Steps, fields, limits, roles, error messages, states, defaults | The codebase, always |

### The four passes

Every generation run does all four, in order. Skipping straight from a spec to a written page is the single most common way an AI-generated manual becomes confidently wrong.

**Pass 1: Discover.** Always look for a URD, PRD, and feature brief, even when the user did not mention one and even in `/autodoc`. Check the connected MCPs (Linear documents, not just issues; Docmost spaces; Confluence; Jira; Notion) and the repo (`docs/specs/`, `docs/prd/`, files named URD, PRD, spec, requirements, brief). If you find specs the user did not mention, say so before generating anything. Never generate from code alone while a spec sits unread.

**Pass 2: Read the specs.** Fetch each one **including its child pages** - a URD is normally a parent page with sections underneath, and the parent alone is a table of contents. Extract scope, personas, terminology, acceptance criteria, and non-goals. Discard implementation notes; they do not belong in a user manual (Rule 8).

**Pass 3: Read the code.** Routes, controllers, policies, validation rules, models, migrations, config, and feature tests. Feature tests are the most precise statement of what shipped. Build the permission matrix from the policy layer before writing a single section.

**Pass 4: Validate claim by claim, then write.** Not feature by feature. A feature can ship while three of its five documented behaviours are wrong. Every factual claim that will appear in the manual gets a verdict first:

| Verdict | Meaning | What happens |
|---|---|---|
| **Verified** | The spec claim is confirmed in code, or the behaviour was read directly from code | Write it |
| **Contradicted** | Spec and code disagree | Write what the **code** does. Log the conflict in the gap report |
| **Unshipped** | The spec describes it; no code implements it | Do **not** write it. Gap report only |
| **Unverified** | Neither source settles it (a limit with no constant, a state with no test) | Do **not** write a number or a rule. Flag in the inference report |

A "factual claim" is anything a reader could act on and be wrong about: a limit, a timeout, an expiry, a role restriction, a required field, an error message, a state name, a default value, an ordering, a permission. Prose that describes intent is not a claim; "invitations expire after 7 days" is.

### The evidence ledger

Produce it during pass 4, save it beside the manual as `validation-report.md`, and keep it as the audit trail for how the manual was built.

```
| Claim | Source | Code evidence | Verdict |
|---|---|---|---|
| Invitations expire after 7 days | PRD §4.2 | config/invitation.php:12 | Verified |
| Limit of 20 pending invites | (none) | InvitationPolicy.php:41 | Verified |
| Owners can bulk-invite by CSV | URD §3.1 | no route, no controller | Unshipped |
| Invite emails retry 3 times | PRD §4.5 | SendInvite.php:28 retries twice | Contradicted |
| Link invites expire | (none) | no expiry found | Unverified |
```

**The gate: a section may not be written until every claim in it has a verdict.** Verified claims become prose. Contradicted claims become prose describing the code, plus a gap report row. Unshipped and Unverified claims never become prose.

### Precedence rules

1. **Code always wins on behaviour.** A spec states intent; code states reality. This does not bend when the spec is recent, signed off, or the feature is "nearly done."
2. **Within the spec tier**, when several specs exist: feature brief beats PRD beats URD for a specific feature, because it is closest to what was built. URD beats both for personas and terminology, because it is written from the user's side. When two specs conflict on user-facing behaviour, the code settles it and the conflict goes in the gap report.
3. **UI labels beat spec terminology.** Lock terms from the spec (Rule 3), then override with the actual on-screen label wherever they differ. Users read the screen, not the PRD. Record every override in the inference report.
4. **Never guess to fill a hole.** An Unverified claim is flagged, never estimated. "Approximately a week" is the same failure as inventing a citation (Section 24, rules 9 and 25).
5. **No spec found is a finding, not a default.** If pass 1 turns up nothing, say so explicitly, and note in the inference report that terminology was chosen rather than given.

### Re-validation

The manual is validated against a commit, not against a moment. Record the commit SHA in the validation report. On any later run, re-validate every claim rather than trusting the previous verdict - a claim verified six months ago is an assumption today. This is what `/gap-report` automates.
