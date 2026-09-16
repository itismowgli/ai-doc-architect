# Documentation Standards

**Read when:** generating or reviewing any manual. This is the mandatory structure.
**Skip if:** the question is about architecture, tech stack, MCPs, or writing craft.
**Pairs with:** `plain-language.md` (the final edit), `screenshots.md`, `publishing.md`.
**Cost:** ~10k tokens. Read one section if you know which one you need.

| Need | Section |
|---|---|
| What sections a manual must contain | 20.1 |
| How to structure one feature section | 20.2 |
| Role-specific reading guide | 20.3 |
| Troubleshooting section format | 20.4 |
| Glossary requirements | 20.5 |
| Scenario writing | 20.6 |
| Limits and constraints reference | 20.7 |
| PHI section (healthcare) | 20.8, then `healthcare.md` |
| **Completeness checklist - run before shipping** | **20.9** |
| The 10 critique-proven rules (no "Chapter", audience split, terminology lock, quick start, frequency order, no field names, UX brevity, no implementation detail, tone, visual hierarchy) | 21.1 - 21.10 |
| Journey mapping, entry triggers, decision forks, waiting states | 22.1 - 22.8 |
| Folder structure, file naming, index.md, cross-linking, portals | 23.1 - 23.7 |

Section numbers are stable. They were 20-23 of the old combined writing guide and kept
their numbers when it was split, so any existing reference still resolves.

---

## Section 20: Documentation Standards - How Every Manual Must Be Structured

A user manual is a book. Not a reference dump, not a feature list, not a collection of
release notes. A book has sections that build on each other. Every section is
self-contained. A reader can open any section and get everything they need without
hunting through other sections.

This section defines the mandatory structure every ADUMAS-generated manual must follow.
Apply it regardless of the product, platform, or audience.

---

### 20.1 The Mandatory Section List

Every complete user manual must include these sections, in this order.
Use descriptive names - never "Chapter N". See Section 21.1 for the naming guide.

**Introduction**
- Who this manual is for (all personas listed)
- How to use this manual (role-specific reading guide)
- Where to get help if the manual does not answer your question

**Quick Start** (always second, always present)
- 10 steps or fewer per persona to accomplish the first meaningful task
- Steps only - no explanations, no edge cases

**Platform Overview**
- One-paragraph plain-language description of what the product does
- The problem it solves and for whom
- How it fits into the broader platform or workflow
- A "how it all fits together" overview - a narrative of the full workflow from start to finish
- Key concepts: every term a user will encounter, defined before they encounter it

**Roles and Access Control**
- Every role defined in plain language
- A permissions matrix: rows = actions, columns = roles, cells = Yes/No/Conditional
- How roles are assigned (by whom, when, under what conditions)
- What a user should do if their role seems wrong

**Getting Started**
- Prerequisites (access, setup, any configuration required)
- First login or first-time access
- A complete end-to-end walkthrough of the most common first task for the primary persona
- Not a feature tour - a real task, from opening the app to a completed outcome

**[One section per major feature or workflow]**
See Section 20.2 for the mandatory structure of each section.

**Troubleshooting**
See Section 20.4 for mandatory content.

**Reference and Appendix**
- Glossary of all terms used in the manual
- Limits and constraints table (file sizes, counts, timeouts, etc.)
- System messages and toast notifications index
- Email notifications reference table
- Keyboard shortcuts (if applicable)
- Support and escalation contacts

---

### 20.2 Mandatory Structure of Every Feature Section

Every section covering a feature or workflow must have all of these subsections.
A section is not complete if any subsection is missing.

```
# [Descriptive Workflow Name - e.g. "Case Creation Workflow"]

> **Who this is for:** [Persona list]
> **What you will be able to do:** [1-2 sentence outcome statement]
> **Before you start:** [Prerequisites - access, prior steps, required data]

## Overview
One paragraph. What this feature is, why it exists, what problem it solves.
Not a list of sub-features. A narrative that gives context.

## [Core task 1 - imperative heading, e.g. "Create a Case Review"]
### When to use this
### Before you begin
### Steps
(numbered, second person, active voice, each step = one action)
### What happens next
(the outcome - what the user sees, what the system does, what email fires)
### If something goes wrong
(the most common errors and how to resolve them)

## [Core task 2...]
(same structure)

## Best Practices
3-5 specific, actionable recommendations. Not generic advice.
E.g. "Mark a case urgent only when the clinical decision must be made within 24 hours.
Overusing urgency flags reduces their impact on panel prioritisation."

## Limits and Constraints
A table of any limits that apply to this feature specifically.
| Limit | Value | Notes |
|---|---|---|
| Maximum file attachment size | 25 MB | Per file. No limit on number of attachments. |
| Maximum collaborators per case | 10 | Contact your panel owner to request an exception. |

## Reference: System Messages and Toasts
A table of every system-generated message a user will see in this feature.
| Trigger | Message shown | Where shown |
|---|---|---|
| Case assigned | "Case reassigned successfully." | Toast |
| Collaborator accepts | "[Name] accepted the collaborator invitation." | System chat message |

## Related Topics
Links to other sections this feature connects to.
```

---

### 20.3 Role-Specific Reading Guide

Every manual must include a reading guide in the Introduction section.
Users should never have to read sections that do not apply to them.

Format:

```
## How to use this manual

Find your role below and start with the recommended sections.

| Your role | Start here | Then read |
|---|---|---|
| [Primary persona, e.g. End User] | Getting Started, [Core daily workflow] | [Secondary workflows relevant to this role] |
| [Secondary persona, e.g. Admin] | Roles and Access Control, [Config section] | [All admin sections] |
| [Power user / manager] | Getting Started, [Analytics section] | [Reporting, Admin Guide] |
```

Replace the role names and section links with the actual roles and section names
for the product you are documenting. Use the terminology lock table (Section 21.4)
to ensure role names match what the product calls them.

---

### 20.4 Mandatory Troubleshooting Section

The troubleshooting section is not an FAQ. It is structured by symptom, not by question.
Every troubleshooting entry has: symptom, likely cause, resolution steps, escalation path.

Required format for each entry:

```
### [Symptom - what the user observes, not what they think caused it]

**You see:** [Exact description of what the user sees]
**Likely cause:** [Plain language explanation]
**To resolve:**
1. [Step]
2. [Step]
**If this does not resolve it:** [Who to contact, what information to provide]
```

Minimum 8 troubleshooting entries per manual. Derive them from:
- Support tickets or known issues (if available)
- Pest feature tests that cover error paths
- Edge cases documented in the PRD or functional requirements
- The "if something goes wrong" subsections in each feature section (consolidate here)

---

### 20.5 Glossary Requirements

A glossary must appear as the first section of the Reference and Appendix section.

Rules:
- Define every product-specific term on its first use in the body, AND collect all definitions in the glossary.
- Write definitions for a reader who has never used the product before.
- Do not define a term by using the term itself.
- Sort alphabetically.
- Cross-link related terms with "See also: [Term]".

Example entry:

```
**Panel**
A group of specialists organised around a clinical specialty (e.g. Cardiology, Radiology).
Cases are submitted to panels for expert review. Each panel has its own members, settings,
and access controls. A panel must have at least one Panel Owner. See also: Panel Member,
Panel Leader, Panel Owner.
```

---

### 20.6 Scenario-Based Writing

Every feature section must include at least one scenario that grounds the instructions
in a realistic clinical or operational context.

A scenario is not an example input. It is a brief narrative that establishes:
- Who the user is (role + context)
- What situation they are in
- What they need to accomplish
- Why this particular feature is the right tool

Format:

```
> **Scenario:** Dr. Patel is a cardiologist at Site 01. She has reviewed a patient
> with an unusual ECG pattern and wants a second opinion from the Cardiac Imaging panel
> before deciding on the next steps. She needs to create a case review that includes
> the patient's recent ECG form and a clear clinical question.
>
> Follow the steps below to create a case review.
```

The scenario appears at the top of the relevant task section, before the steps.
It is set off in a blockquote so it is visually distinct from the instructions.

---

### 20.7 Limits, Constraints, and System Boundaries Reference

Every manual must include a consolidated limits table in the Reference and Appendix section.
This captures all system constraints so users never discover them by hitting an error.

Derive limits from:
- FormRequest validation rules (max, min, size, mimes validators in Laravel)
- Database column constraints (character limits, nullable vs required)
- Config files (timeout values, SLA thresholds, file size limits)
- Feature tests that assert boundary conditions
- PRD implementation decisions section

Table format:

```
| Feature | Constraint | Value | Notes |
|---|---|---|---|
| File attachments | Maximum file size | 25 MB per file | All formats accepted unless listed |
| File attachments | Blocked file types | .exe, .bat, .sh | Security policy |
| Case collaborators | Maximum per case | 10 | Contact panel owner for exceptions |
| AI summary | Timeout | 30 seconds | Regenerate if it does not appear |
| Panel invitation | Expiry | 7 days | Panel owner can resend expired invitations |
| Case inactivity | Warning threshold | 14 days | Email sent to case creator |
| Case inactivity | Auto-close threshold | 30 days | Cannot be reopened; new case required |
```

---

### 20.8 PHI and Data Privacy Section (Healthcare Platforms)

Any manual for a healthcare or clinical platform must include a dedicated data privacy
section in the Platform Overview or Roles and Access Control section. It is not optional.

Required content:
- What data the product can display and to whom
- What "restricted data" means and who controls access to it
- How data access requests work (who requests, who approves, what is logged)
- Audit trail: what is logged, who can view logs, how long logs are retained
- What users should NOT do (screenshots of patient data, sharing access credentials, etc.)
- Who to contact if a data access issue occurs

Format this as a dedicated section with a prominent callout:

```
> **Data sensitivity notice.** [Product name] displays protected health information (PHI).
> All actions are logged and audited. Do not share screenshots of patient data outside
> of authorised channels. If you believe data has been accessed inappropriately,
> contact your site administrator immediately.
```

---

### 20.9 Completeness Checklist

Before a manual is considered complete, verify every item below. A manual with any
unchecked item is a draft, not a deliverable.

**Structure**
- [ ] Introduction section with role-specific reading guide
- [ ] Quick Start section (second section, always present)
- [ ] Platform Overview with full workflow narrative and key concepts
- [ ] Roles and Access Control with permission matrix
- [ ] Getting Started with end-to-end first-task walkthrough
- [ ] One section per major feature or workflow
- [ ] Troubleshooting (minimum 8 entries, symptom-structured)
- [ ] Reference and Appendix with glossary, limits table, notifications table, system messages index

**Every feature section**
- [ ] Who-for and what-you-will-learn preamble
- [ ] Overview paragraph (not a bullet list)
- [ ] At least one scenario (blockquote format)
- [ ] All core tasks documented (step-by-step, second person active voice)
- [ ] "What happens next" outcome for each task
- [ ] "If something goes wrong" for each task
- [ ] Best practices section (3-5 specific items)
- [ ] Limits and constraints table
- [ ] System messages and toasts reference table
- [ ] Related topics links

**Evidence**
- [ ] Every factual claim in the file has a verdict in `validation-report.md`
- [ ] No claim marked Unshipped or Unverified appears anywhere in the prose
- [ ] Every limit, timeout, expiry, and role restriction cites a real file, not an estimate
- [ ] Contradictions resolved toward the code, with the conflict logged in the gap report
- [ ] Terminology overrides (spec term vs UI label) recorded in the inference report
- [ ] The commit SHA validated against is recorded

**Content**
- [ ] Every product-specific term defined in glossary
- [ ] All role types documented with plain-language definitions
- [ ] Permission matrix covers all actions in the product
- [ ] All email notification triggers documented in reference table
- [ ] All file/data limits documented
- [ ] All SLA/timeout thresholds documented
- [ ] PHI/privacy section present (healthcare products only)
- [ ] No em dashes (hyphens only)
- [ ] No passive voice in step instructions (every step starts with a verb)
- [ ] No "click here" link text (all links use destination title)
- [ ] All screenshots use synthetic data, not real patient or user data

---

## Section 21: Critique-Proven Rules - What Separates Good from Enterprise-Grade

The rules in this section come directly from real critique feedback on AI-generated
manuals. Each rule was identified as a specific failure mode in production documentation.
Apply every rule to every manual, with no exceptions.

---

### 21.1 Never Use "Chapter" - Use Descriptive Workflow Names

"Chapter" sounds like a printed textbook. SaaS and enterprise documentation uses
descriptive, action-oriented or role-oriented section names.

**Never:**
- Chapter 1: What Is [Product]?
- Chapter 5: Creating a Case Review
- Chapter 12: Support Requests

**Always:** Use names that tell the reader what they will be able to do or understand.

| Academic (never use) | Enterprise (always use) |
|---|---|
| Chapter 0: About This Manual | Introduction |
| Chapter 1: What Is [Product]? | Platform Overview |
| Chapter 2: Roles and Permissions | Roles and Access Control |
| Chapter 3: Getting Started | Getting Started |
| Chapter N: [Feature] | [Feature] Workflow / [Feature] Management / [Feature] Guide |
| Chapter N: Troubleshooting | Troubleshooting |
| Chapter N: Reference | Reference and Appendix |

Use a PART structure to group sections by frequency of use (see Section 21.5).

---

### 21.2 Always Generate Separate Documents for Separate Audiences

A single document that combines user instructions, admin configuration, and system
internals is not a user manual. It is a specification document dressed as one.

**Always produce at minimum two separate documents:**

**Document 1: User Guide**
- For end users performing daily tasks
- Covers: what buttons do, what happens next, common workflows, errors they will see
- Does NOT cover: configuration, admin tools, timeout internals, permission logic,
  assignment algorithms, audit systems, dashboard analytics

**Document 2: Admin and Configuration Guide**
- For administrators, panel owners, system operators
- Covers: configuration options, permission management, automation rules,
  timeout settings, audit logs, dashboard analytics, assignment logic
- References the User Guide for end-user workflows rather than repeating them

If the product has a developer audience: a third document covering APIs, webhooks,
and integration configuration.

**The test:** Would a clinician on their first day need to read this section? If no,
it belongs in the Admin Guide, not the User Guide.

---

### 21.3 Never Let Internal Field Names Appear in User Documentation

Internal field names from the database, code, or configuration are never appropriate
in user-facing documentation. They create cognitive load, expose implementation details,
and signal that the document was written from code rather than from the user's perspective.

**Never write:**
- "If `prevent_non_matched_specialty` is enabled..."
- "The `published_at` timestamp is set when..."
- "The `timeout_urgent_hrs` value controls..."
- "The `consultant_id` field stores..."
- "Set `allow_any_specialty_case` to true..."

**Always translate to plain language:**
- "If the panel is configured to restrict cases to matching specialties..."
- "When the panel is published..."
- "The timeout period for urgent cases is configured in Panel Settings..."
- "The assigned specialist is..."
- "To allow cases from any specialty..."

**The rule:** If a user would need to open a database client or read code to understand
the term, rewrite it. The user guide has zero technical identifiers. The admin guide
may have field names only in configuration reference tables, clearly labelled as such.

---

### 21.4 Lock Terminology Before Writing - One Noun Per Concept

Inconsistent terminology is one of the most damaging quality problems in documentation.
When the same thing is called by different names in different sections - "workspace",
"organization", "account", "team" - users lose confidence and search fails.

**Before writing any section:**

1. Identify every key concept in the product.
2. Choose ONE term for each concept. Use the term the user sees in the UI.
3. Document the chosen terms in the glossary first.
4. Use only those terms throughout. No synonyms, no variation.

**Terminology lock table (example):**

| Concept | Chosen term | Never use |
|---|---|---|
| A shared space for team work | Workspace | Organization, Account, Team, Project |
| The person who owns the account | Owner | Admin, Creator, Super admin |
| A request submitted for action | Ticket | Issue, Request, Case, Item |
| The person assigned to act on it | Assignee | Owner, Handler, Responsible party |

This table is an example. Build your own from the actual UI labels in the product
you are documenting. Use the term the user sees on screen - never a synonym from
the codebase or internal documents.

Write the terminology lock table before the first section. If you find yourself reaching
for a synonym, the terminology is not locked properly.

---

### 21.5 Structure by Frequency of Use - Daily Tasks Before Edge Cases

Information architecture by frequency of use determines whether a user can find what
they need in under 30 seconds. Always structure the document in this order:

**PART 1 - Introduction**
Everything a new user needs to get oriented: what the product is, quick start,
roles and access. Maximum 3 sections. Minimum words.

**PART 2 - Daily Workflows**
The tasks a user performs every day or every week. These sections are the most read,
must be the clearest, and must be first after the introduction. No admin content here.

**PART 3 - Operational Features**
Features used regularly but not daily: configuration tasks the admin performs weekly,
panel management, notifications settings.

**PART 4 - Analytics and Administration**
Dashboards, reports, audit logs, and system configuration. This is where admin-only
content lives. Users who do not have admin access skip this section entirely.

**PART 5 - Troubleshooting and Reference**
Troubleshooting (symptom-structured), glossary, limits table, system messages index,
notifications reference. This is looked up, not read sequentially.

**The test:** Could a new user on their first day skip PART 4 entirely and still use
the product effectively? If not, something in PART 4 belongs in PART 2.

---

### 21.6 Always Include a Quick Start Near the Top

A Quick Start section is not optional. It is the single most impactful addition to
any user manual. Users who cannot accomplish their first task in under five minutes
stop reading the manual and start asking colleagues.

The Quick Start appears immediately after the Introduction, before any detailed sections.
It is numbered steps only - no explanations, no prerequisites, no error states.
Those belong in the detailed sections that follow.

**Format:**

```markdown
## Quick Start

Get to your first result in under 5 minutes. Detailed instructions for each step
are in the sections below.

### For [Primary Persona - e.g. Clinicians]

1. Click [Product Name] in the main navigation.
2. Click **[Primary Action]**.
3. Search for and select the patient.
4. Enter your clinical question.
5. Click **Submit**.

Your case is now open and the panel has been notified.

### For [Secondary Persona - e.g. Specialists]

1. Click [Product Name] in the main navigation.
2. Open **[Inbox View]**.
3. Click a case to open it.
4. Click **Accept** to take responsibility for the review.
5. Reply in the chat with your assessment.
```

Keep the Quick Start to 10 steps or fewer per persona. If a workflow cannot be
summarised in 10 steps, it is not a Quick Start - it is a workflow guide. Write both.

---

### 21.7 Keep Standard UX Patterns Brief

Not every feature requires a full task block. Standard UX interactions that users
encounter in every SaaS product should be documented briefly, not exhaustively.

**Over-document these:** (they have meaningful product-specific behavior)
- Permission-gated actions
- Multi-step workflows with branching paths
- Actions with irreversible consequences
- Actions that trigger notifications or emails

**Document briefly:** (one or two sentences each)
- Typing indicators
- Message like / reaction buttons
- Draft auto-save
- Pagination and "load more"
- Standard date pickers, dropdowns, search bars
- Toast notifications for common success states

**The test:** Does this feature work differently in this product than it does in Gmail,
Slack, or Notion? If no, one sentence is enough. If yes, write the full task block.

---

### 21.8 Keep Implementation Details Out of User Documentation

The following content belongs in developer documentation or admin guides, never in
user-facing manuals:

**Never include in a User Guide:**
- Email subject line templates with placeholders (`Reminder - Case [ID] inactive for 14 days`)
- API endpoints or HTTP methods
- Database field names or column references
- Cache durations or S3 URL expiry times
- Template engine mentions (MJML, Blade, Handlebars)
- Internal flag names or feature toggle identifiers
- Cron job schedules or background job names
- Deployment environment specifics

**The rule:** If the content requires knowing how the system is built rather than how
to use it, it does not belong in the User Guide. Move it to the Admin Guide under a
"How it works" or "Configuration reference" section, clearly scoped to administrators.

---

### 21.9 Tone: Direct, Not Formal

Enterprise documentation is clear and direct. It is not formal, legal, or academic.
Formal tone creates distance between the user and the task. Clear tone gets them there faster.

**Replace these patterns immediately:**

| Formal / technical (never use) | Direct / clear (always use) |
|---|---|
| "The collaborator invitation expires at the same timeout horizon." | "Collaborator invitations expire after the case timeout period." |
| "The system will process the request in an asynchronous manner." | "The request may take a few seconds to process." |
| "Users with insufficient privileges will receive an error." | "If you see an access error, contact your administrator." |
| "The aforementioned fields are required prior to submission." | "Fill in all required fields before submitting." |
| "In the event that no reviewer has been assigned..." | "If no reviewer has been assigned..." |
| "It should be noted that..." | (Delete this phrase. Say the thing.) |
| "Please be advised that..." | (Delete this phrase. Say the thing.) |

**The tone test:** Read the sentence out loud. If you would not say it in a conversation
with a colleague, rewrite it.

---

### 21.10 Visual Hierarchy in Markdown

Text-heavy documentation loses readers. Use visual hierarchy to break up instruction
blocks and draw attention to important information.

**Callout blocks** (use sparingly - maximum 2 per section):

```markdown
> **Note:** You can change this setting later from Panel Settings.

> **Important:** Removing a member is immediate and cannot be undone.

> **Data sensitivity:** This section contains protected health information (PHI).
> All actions are logged and audited.
```

**When to use bold:** Only for UI element names (button labels, field names, tab names)
and the first mention of a defined term. Never for emphasis. Never in step instructions.

**Tables over lists:** When comparing options, roles, or states, use a table.
When listing steps, use a numbered list. When listing items with no order, use bullets
sparingly - if there are more than 5 bullet points, consider whether a table is clearer.

**Status and workflow diagrams:** For any feature with more than 3 states, include a
simple state diagram in Markdown:

```
Open -> Accepted -> In Progress -> Closed
  |                     |
  v                     v
Rejected            Cancelled
```

This takes three lines and eliminates two paragraphs of prose explanation.


---

## Section 22: Process-Oriented Documentation - Journeys, Not Features

The single most important difference between good documentation and great documentation
is the unit of organization. Good documentation is organized around features - what
the product has. Great documentation is organized around user intentions - what the
user is trying to accomplish.

A user never opens a manual thinking "I want to read about the Case Creation feature."
They open it thinking "I need to submit a question to the cardiology panel" or "my case
has been open for 3 days and I do not know what to do next." The documentation should
be organized around those intentions, not around the feature that happens to serve them.

This is what the best SaaS documentation does - Linear, Notion, Stripe, Intercom. Every
page flows into the next. Every completed action points forward. Every section knows why
the user arrived there.

---

### 22.1 Journey Mapping - Do This Before Writing Anything

Before generating any documentation section, map every primary persona's journey through
the product from first login to their most common completed outcome. This journey map
becomes the backbone of the documentation structure.

**Journey mapping format:**

```
Persona: Clinician (Case Creator)
Primary goal: Get a specialist's opinion on a complex patient case

Journey:
1. TRIGGER: Patient presents with complex symptoms. Clinician needs expert input.
2. Open Care Insight for the first time.
3. Understand the Review Center layout.
4. Create a case: select patient, choose panel, enter clinical question.
5. Review and confirm the AI summary.
6. Submit the case and see it appear in "Requested by Me."
7. Wait: track status, optionally add urgency or a collaborator.
8. Specialist responds in chat. Clinician reads and replies.
9. Clinician approves or denies restricted data access (if requested).
10. Review concludes. Clinician closes the case.
11. Impact survey appears. Clinician records the outcome.

Connected journeys (where this journey leads):
- If specialist does not respond -> Support Request workflow
- If case needs reassignment -> Reassignment workflow (admin journey)
- If case auto-closes due to inactivity -> understand auto-close, create new case
```

**Every persona needs a journey map.** Write the journey maps first. Each numbered step
in the journey map becomes a page or section in the documentation. Steps that connect to
other steps get explicit "what to do next" links.

---

### 22.2 Contextual "What to Do Next" Connectors

Every completed action is a decision point. The user has just done something - now what?
Feature-organized docs leave the user stranded. Process-oriented docs carry them forward.

After every task completion, include a "What to do next" block that gives 2-4 forward
paths based on what the user most commonly needs at that moment.

**Format:**

```markdown
## What to do next

Your case is now open and the panel has been notified.

- **Check who it was assigned to:** Open the case - the assignee's name appears
  in the case header. If it shows "Unassigned," see [Unassigned Cases].
- **Add urgency:** If the clinical decision is time-sensitive, [mark the case urgent].
- **Add a collaborator:** If a colleague should also see the case,
  [invite them as a collaborator].
- **Track the status:** The case moves through Open -> Accepted -> In Progress ->
  Closed. The current status is always visible on your "Requested by Me" list.
```

**Rules for "What to do next" blocks:**

- Always 2-4 options. Never more - choice paralysis kills momentum.
- Link to the exact section that covers each option, not to a general page.
- Order by frequency: the most common next action first.
- Include the "something went wrong" path: "If nobody has responded in 24 hours, [use Support Request]."
- Never use "What to do next" as a title if it sounds robotic for the context. Alternatives:
  "From here you can...", "While you wait...", "Next steps", "Your case is live."

---

### 22.3 Entry Point Triggers - Every Section Knows Why You Are Here

Every documentation section should open with a one-line trigger: the user's situation
that brings them to this page. This orients users who arrive via search or a link from
another page without reading the manual from the beginning.

**Format:**

```markdown
> You are here because: a case was assigned to you and you need to decide
> whether to accept it and begin the review.
```

Or embedded in the overview:

```markdown
## Specialist Review Workflow

When a case is assigned to you, you receive an email notification and the case
appears in your **Assigned to Me** list. This section covers everything from
receiving that notification to completing the review.
```

**The entry point test:** Cover the page title and read only the first paragraph.
Can a user who arrived from a search result immediately understand whether this is
the right page for their situation? If not, the entry point is missing.

---

### 22.4 Progressive Disclosure - Simple First, Complexity Later

Every section should start with the simple, common case and introduce complexity only
after the user has completed the basic flow at least once. Do not put edge cases,
exceptions, admin configuration, and advanced options on the same page as the basic task.

**The three-layer model:**

**Layer 1 - The first-time path:** The minimum a user needs to know to complete the
task the first time. No edge cases. No options. No "if you have configured X..." Just
the direct path from start to done.

**Layer 2 - Common variations:** The 2-3 most common variations from the basic path.
"What if I need to change the specialty?" "What if the patient isn't showing up in search?"

**Layer 3 - Edge cases and admin:** Configuration, exceptions, unusual states. Link to
the Admin Guide or a separate "Advanced" section. Do not put these in Layer 1.

**Example for Case Creation:**

```
Layer 1: Select patient → choose panel → enter question → confirm AI summary → submit.
         (8 steps. Simple. Every word needed.)

Layer 2: "What if I don't see my panel?" / "What if I want to skip the AI summary?"
         (Short answers. Link to Panel Settings if configuration is needed.)

Layer 3: Auto-assignment logic, timeout configuration, specialty filtering rules.
         (Admin Guide only. Not in the User Guide.)
```

---

### 22.5 Decision Forks - "If You See X, Go to Y"

At every point where users with different roles, states, or configurations see different
screens, the documentation must fork explicitly. A user who sees something different from
what the doc describes immediately loses trust and closes the manual.

**Format for decision forks:**

```markdown
After clicking **Submit**, one of two things happens:

- **If the panel has auto-assignment enabled:** The case is assigned immediately to
  an available specialist. You see the assignee's name in the case header.
  [Track the case status ->]

- **If the panel uses manual assignment:** The case status is Open and the Reviewer
  field shows "Unassigned." The panel leader assigns it manually, usually within
  24 hours. [What to do if your case stays unassigned ->]
```

**When to use decision forks:**
- When the user's role determines what they see (common in permission-heavy apps)
- When a configuration setting changes the flow (auto-assign vs manual assign)
- When the result of a previous step changes what happens next
- When there are two valid ways to accomplish the same thing

**Never:** Write a single set of steps that silently fail for half the audience. If
the steps work differently for different users, fork the steps.

---

### 22.6 Contextual Inline Links - Woven Into Steps, Not Appended at the End

Links to related content belong inside the flow of the instructions, not only in a
"Related topics" footer. A user following a 10-step process should not have to jump
to the end of the page to find the link they need at step 4.

**Wrong - all links at the end:**
```markdown
1. Click **Create case review**.
2. Select the patient.
3. Select the panel.
4. Enter your clinical question.
5. Attach supporting forms.

Related topics: Panel Management, Patient Records, AI Summary
```

**Right - links woven into the steps at the moment they are needed:**
```markdown
1. Click **Create case review**.
2. Search for and select the patient from the dropdown.
3. Select the panel. [Don't see your panel? You may not be a member. ->]
4. Enter your clinical question. Be specific about what you need from the specialist.
   [Tips for writing a good clinical question ->]
5. Optionally attach supporting forms from the patient record.
   [Which forms are available? ->]
```

The link appears at exactly the moment the user needs it, not after they've already
moved past the relevant step.

---

### 22.7 The "Waiting State" - Document What Users Do Between Actions

Most documentation covers the active steps: create, submit, complete. It ignores the
waiting states: what the user does while waiting for a specialist to respond, waiting
for an invitation to be accepted, waiting for a report to generate.

Waiting states are where users get anxious and create support tickets. Document them.

**Format:**

```markdown
## While you wait

Your case is now open. The specialist typically responds within [SLA period].
Here is what you can do in the meantime:

**Check status:** Your case appears in the **Requested by Me** tab with its current
status. Open -> Accepted means the specialist has taken responsibility for the review.

**Add urgency:** If the clinical decision becomes time-sensitive, open the case and
[mark it urgent ->]. The specialist and panel leader are notified.

**Add a collaborator:** If a second colleague should also see the case and the chat,
[invite them as a collaborator ->].

**If nothing has happened after 24 hours:** The case may not have been assigned yet.
Use the [Support Request ->] to ask the panel leader to assign it, or contact them directly.
```

---

### 22.8 The Minimum Journey Standard

Every generated User Guide must pass this test before delivery:

Pick the primary persona (the most common user type). Read only the documentation
for their first-time use case - not the reference sections, not the admin guide.

Ask these questions:
- [ ] Can the user complete their primary workflow from start to finish using only this documentation?
- [ ] At every step, does the documentation tell them what to do next?
- [ ] At every point where something might look different, does the documentation fork?
- [ ] Is there documentation for what to do while waiting?
- [ ] Is there documentation for the most common error they will encounter?
- [ ] Does every page know why the user arrived there?

If any answer is no, the documentation is feature-organized, not process-oriented.
Fix it before delivering.


---

## Section 23: Folder-Based Documentation Output

Every generated user manual is a folder of Markdown files, not a single file. This
section defines the structure, naming conventions, cross-linking rules, and how the
output maps to documentation portals.

---

### 23.1 The Standard Folder Structure

```
docs/
  [product-name]/
    index.md                          <- Master TOC. Entry point for the entire manual.

    01-introduction/
      index.md                        <- Section overview + links to files in this folder
      who-this-is-for.md             <- Role-specific reading guide
      quick-start.md                 <- 10 steps or fewer per persona

    02-[section-name]/
      index.md
      [workflow-1].md
      [workflow-2].md

    03-[section-name]/
      index.md
      [workflow].md

    [n]-troubleshooting-and-reference/
      index.md
      troubleshooting.md
      glossary.md
      limits.md
      system-messages.md
      notifications.md
```

The folder numbers control sort order. Files within each folder are listed
alphabetically unless the workflow has a natural sequence, in which case prefix
them: `01-create.md`, `02-invite.md`, `03-remove.md`.

---

### 23.2 File Naming Rules

**Folders:** `NN-descriptive-name` where NN is a two-digit zero-padded number.

```
01-introduction
02-roles-and-access
03-getting-started
04-daily-workflows
05-operational-features
06-analytics-and-admin
07-troubleshooting-and-reference
```

**Files:** kebab-case, task-oriented where the content is a workflow. Use nouns
for reference content and verb phrases for workflow content.

```
Workflow files:       create-a-team.md, invite-members.md, reset-password.md
Reference files:      glossary.md, limits.md, system-messages.md
Overview files:       index.md (always this name)
```

Never use spaces, uppercase, or underscores in file or folder names.

---

### 23.3 The index.md Files

Every folder has an `index.md`. There are two types:

**Root index.md (master table of contents):**

```markdown
# [Product Name] Documentation

Brief one-paragraph description of the product.

## How to use this guide

[Role-specific reading guide - link to each section by role]

## Contents

### Introduction
- [Who This Guide Is For](./01-introduction/who-this-is-for.md)
- [Quick Start](./01-introduction/quick-start.md)

### [Section Name]
- [Workflow 1](./02-section-name/workflow-1.md)
- [Workflow 2](./02-section-name/workflow-2.md)

### Troubleshooting and Reference
- [Troubleshooting](./07-troubleshooting-and-reference/troubleshooting.md)
- [Glossary](./07-troubleshooting-and-reference/glossary.md)
```

**Section index.md (section overview):**

```markdown
# [Section Name]

One paragraph: what this section covers, who it is for, what you will be
able to do after reading it.

## In this section

- [Workflow 1](./workflow-1.md) - when to use this workflow
- [Workflow 2](./workflow-2.md) - when to use this workflow
```

---

### 23.4 Cross-Linking Between Files

Always use relative paths. Never use absolute URLs or anchor-only links between files.

```markdown
# Correct
See [Invite Members](../04-daily-workflows/invite-members.md) for the full steps.
Return to [Daily Workflows](../04-daily-workflows/index.md).

# Wrong
See [Invite Members](#invite-members)               <- broken if user opens a different file
See [Invite Members](https://docs.example.com/...)  <- breaks when docs move
```

Every section file ends with a "Related topics" or "What to do next" block using
relative links:

```markdown
## What to do next

- [Manage member roles](./manage-roles.md)
- [Remove a member](./remove-member.md)
- [Return to Team Management overview](./index.md)
```

---

### 23.5 How This Maps to Documentation Portals

The folder structure maps to sidebar navigation in every major docs portal. The Markdown
is identical everywhere - you author once. What differs is what has to happen at publish
time, and destinations split into two kinds.

**Static site generators consume the folder.** The folder *is* the site, so relative links
and image paths resolve exactly as written. Navigation is configured, not inferred.

| Portal | How it reads the folder | Config needed |
|---|---|---|
| GitHub | Each file is browsable at its own URL | None |
| Docusaurus | Reads `docs/`, numeric prefixes drive ordering | Autogenerated sidebar; `_category_.json` for friendly folder labels |
| Mintlify | Reads the folder, `index.md` as section pages | Navigation declared in `docs.json` |
| GitBook | Folder structure via `SUMMARY.md` or Git sync auto-discovery | Depends on how the space is configured |

**Block-based apps import the folder.** Each file becomes a page with an id. There is no
filesystem, so every relative path must be rewritten at publish time.

| Portal | How it reads the folder | Config needed |
|---|---|---|
| Docmost | Space per guide, folder becomes parent page, file becomes child page | None, but every link and image path is rewritten |
| Notion | Page tree mirrors folder tree | None, but every link and image path is rewritten |
| Confluence | Space per guide, page tree mirrors folder | None, but every link and image path is rewritten |

Four things are resolved at publish and never in the source: cross-links between files,
image paths, callout blocks, and raw HTML. The transform is mechanical and never edits the
source files. Full matrix, the two-pass publish sequence, the path-to-page-id map, and
per-destination recipes are in `references/publishing.md`.

The folder names become sidebar group headers. The file names become sidebar items.
The `index.md` files become the section landing pages.

---

### 23.6 Separation: User Guide and Admin Guide

Always generate as two parallel folder trees:

```
docs/
  [product-name]/
    user-guide/
      index.md
      01-introduction/
      02-getting-started/
      03-daily-workflows/
      04-troubleshooting-and-reference/

    admin-guide/
      index.md
      01-introduction/
      02-configuration/
      03-user-management/
      04-analytics-and-reports/
      05-troubleshooting-and-reference/
```

The User Guide contains zero admin content. The Admin Guide may reference the User
Guide for end-user workflows rather than repeating them.

---

### 23.7 When a Single File Is Acceptable

Use a single `.md` file only when:
- The user explicitly requests it
- The feature has fewer than 3 tasks, one audience, and no admin content
- The output is a gap report, release note, or changelog (inherently single-document)

In all other cases, use the folder structure.
