# The Plain-Language Rules

**Read when:** running the final edit on any generated page, or when asked to remove AI
tells from existing prose.
**Skip if:** you are still deciding what a page contains. This is the last pass, not the
first.
**Cost:** ~3k tokens. Read it whole; the rules are ordered by strength and the ordering
is the point.

28 numbered tells in five groups. Rules 1-5 justify an edit on one sighting. Rules marked
*weak alone* need company.

---

## Section 24: The Plain-Language Rules - Removing AI Tells from Generated Docs

Every rule so far says what a manual must contain. This section says how the sentences
must read. It is the last pass before a generated page is published, and it is the one
that decides whether a reader trusts the manual.

### Why generated documentation sounds wrong

A language model writes whatever is most likely to come next, so by default it makes the
choice that fits the widest range of readers and subjects. A technical writer chooses for
one reader doing one task. Every tell below is a form of that default choice: a sentence
that signals importance instead of adding a fact, rhythm or formatting applied by rule,
an ordinary behavior dressed as a capability, or text left over from the chat.

In documentation the cost is concrete. A reader mid-task is scanning for the next action.
Every sentence that does not carry one is a sentence they have to skip, and after enough
of them they stop reading and open a support ticket instead.

Two rules follow. **Every sentence you keep must tell the reader something they cannot
already see on their screen.** And **a tell counts in proportion to how rarely a careful
writer would make it on purpose.**

The rules are numbered by strength. Rules 1 to 5 justify an edit on a single sighting.
A rule marked *weak alone* counts only when several tells share a passage, because a
careful writer may use any one of them deliberately.

> Adapted from Wikipedia's [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
> and applied to technical documentation. Where the two disagree, this section wins,
> because documentation has constraints general prose does not: second person is
> mandatory (Section 10), bold on UI labels is required (Section 21.10), and the voice
> stays neutral rather than personal.

---

### A. Staging instead of stating

The strongest and most frequent tells. Act on one sighting.

| # | Rule | Before | After |
|---|---|---|---|
| 1 | **Not X but Y** | "This isn't just a setting, it's a permission boundary." | "This setting controls who can see the project." |
| 2 | **Closers that repeat the section** | "And that's how you invite a member." at the end of every workflow | Cut it. End on the last concrete fact or the "What to do next" links |
| 3 | **Sayings that sound deep** | "At its core, permissions are about trust." | "Permissions control which actions each role can perform." |
| 4 | **Run-up before the point** | "Let's dive into how notifications work. It's actually pretty simple!" | "Notifications are sent by email and in the app." |
| 5 | **Arguing with no one** | "You might be tempted to invite everyone as an admin, but..." | "Invite people as Members unless they need to manage billing." |

Rule 2 has one exception that matters in docs: an **outcome statement** is not a closer.
"The invitation appears in the Members list marked **Pending**" tells the reader what they
should now see on screen, which is required by Section 5. "And that's how you invite a
member" tells them nothing. The test is whether the sentence names something the reader
can verify.

---

### B. Rhythm by rule

| # | Rule | Before | After |
|---|---|---|---|
| 6 | **Forced triads** | "fast, flexible, and powerful"; three bullets where two facts exist | Use the number of items the meaning needs |
| 7 | **Repeated sentence openings** | "You can export... You can filter... You can schedule..." | Merge into one sentence or a table |
| 8 | **Dashes as the universal connector** (*weak alone*) | "The invite expires—after seven days—unless resent" | Use periods, commas, colons, or parentheses |
| 9 | **Stacked qualifiers** (*weak alone*) | "This may potentially take up to approximately a few minutes" | "This takes up to two minutes." Give the real number or cut the sentence |
| 10 | **Parallel steps padded to match length** | Step 2 padded with filler so it matches step 1 | Let short steps be short |
| 11 | **Passive voice with no actor** (*weak alone*) | "The request will be processed." | "Your workspace processes the request in about ten seconds." |

Rule 9 is the most damaging of these in documentation. A hedge is a fact the writer did
not look up. If the timeout is seven days, say seven days. If you do not know it, flag it
in the inference report rather than writing "a period of time."

---

### C. Inflation and borrowed authority

| # | Rule | Before | After |
|---|---|---|---|
| 12 | **Stock AI words** | "seamlessly", "robust", "leverage", "powerful", "streamline", "delve", "landscape", "unlock", "empower", "elevate" | Plain words. See the watch list below |
| 13 | **Inflated significance** | "Inviting members is a crucial part of your workspace journey." | Delete the sentence. Start with the first step |
| 14 | **Vague relationship** | "This setting is associated with your billing configuration." | "Changing this setting changes your monthly bill." |
| 15 | **Shallow -ing riders** | "...sending an invite, ensuring your team stays connected and enabling collaboration." | "...sending an invite." |
| 16 | **Sales language** | "Our powerful notification engine keeps you effortlessly in the loop." | "Notifications tell you when someone assigns you a task." |
| 17 | **Borrowed authority** | "Best practices recommend...", "Industry standard is..." | Name the source, or state the rule as your product's rule |
| 18 | **Avoiding is, are, and has** | "The dialog features a role dropdown and serves as the entry point for..." | "The dialog has a role dropdown." |

**Watch list.** Replace on sight in generated documentation:

| Stock word | Use instead |
|---|---|
| seamlessly, effortlessly, smoothly | Delete. If it needs saying, say what the user does not have to do |
| robust, powerful, comprehensive, rich | Delete, or name the specific capability |
| leverage, utilize | use |
| streamline, optimize, enhance, elevate, empower, unlock | Name the actual change |
| ensure | make sure, or name what happens |
| simply, just, easily, quickly | Delete. If the step is easy the reader will notice; if it is not, you have lied |
| navigate to | go to, open |
| in order to | to |
| a variety of, a range of, numerous | The number, or delete |
| it is important to note that | Delete. Say the thing |
| delve, landscape, realm, testament, tapestry | Delete the sentence and rewrite the claim |

"Simply" and "just" deserve their own note. They are the most common words in bad
documentation and the most expensive. A reader who is stuck reads "simply click Save"
as an accusation.

---

### D. Formatting by rule

| # | Rule | Before | After |
|---|---|---|---|
| 19 | **Bold as decoration** | "**Note:** The **invitation** expires after **seven days**." | Bold only UI labels and a term's first definition (Section 21.10) |
| 20 | **Decorative headings** | "🚀 Getting Started With Your Invitation Journey" | "Invite a member" |
| 21 | **Title Case headings** | "How To Configure Your Notification Preferences" | "Configure notification preferences" |
| 22 | **Callouts on ordinary facts** | A `> **Note:**` block on every paragraph | Maximum two per section, for gated or irreversible actions |
| 23 | **Curly quotation marks** (*weak alone*) | `press "Enter"` with typographic quotes | Straight quotes, so copy-paste into a terminal works |

Rule 21 has one exception. Document titles and sidebar navigation labels keep the
product's own capitalisation, so "Workspace User Guide" stays as it is, and so do the
fixed PART names from Rule 21.1 (Introduction, Daily Workflows, Operational Features,
Analytics and Admin, Reference). Proper nouns and UI element labels keep their capitals
everywhere. Everything else - section headings, task headings, table headers - is
sentence case.

Rules 19 and 22 are where generated documentation most often fails a human review. Bold
and callouts are the model's way of signaling importance without adding information. When
everything is marked important, the genuinely irreversible action stops standing out, and
that is the one the reader most needed marked.

---

### E. Leftovers from the chat and the draft

| # | Rule | Before | After |
|---|---|---|---|
| 24 | **Chatbot residue** | "Great question! Here's how to invite a member. I hope this helps!" | Remove the wrapper, keep the content |
| 25 | **Knowledge-limit disclaimers** | "While the exact timeout is unclear from the codebase, it appears to be..." | State what the code shows, or put it in the inference report |
| 26 | **Heading repeated in the first sentence** | "## Invite a member" + "This section explains how to invite a member." | Let the heading do the work. Start with the entry-point trigger or step 1 |
| 27 | **Writing about the previous version** | "This replaces the old Team Settings page." | Describe what exists now. Version history belongs in the changelog |
| 28 | **Describing the document instead of the product** | "In this guide, we will explore the various aspects of..." | Delete. The contents list already did this |

Rule 25 is a hard error in this system, not a style preference. When the source does not
say, the answer is never a guess dressed as a fact. Write what you can verify, and record
what you could not in the inference report where a human can fill it in. This is the same
rule as "never document a feature that did not ship," applied at the sentence level.

---

### The final pass

Run this after the completeness checklist in Section 20.9, not instead of it. A page can
be complete and still unreadable.

1. **Mark the tells, strongest first.** Read the whole page once. Look at paragraph shape
   as well as sentences: a contrast split across two sentences, three parallel bullets
   where two facts exist, or the same closer after every workflow is the same tell at a
   larger scale.
2. **Rewrite without treating the structure as fixed.** Keep every supported claim. You
   may merge paragraphs, cut sentences, and turn a labeled list into a table. Do not add a
   fact, number, name, or limit that is not in the source.
3. **Check the draft against the source.** Did the rewrite drop a limit, a role, an error
   case, or a step? Shape edits under rules 6, 10, and 19 drop those most often. Then
   search for the five tells that survive rewrites: a not-X-but-Y contrast, a repeated
   closer, a dash, a triad, a bold label that is not a UI element.
4. **Read it as the reader.** They are mid-task, mildly annoyed, scanning for the next
   action. Every sentence that does not give them one is a sentence to cut.

**What must survive the pass, always:**

- Second person (Section 10)
- Bold on UI element names (Section 21.10)
- The entry-point trigger opening every section (Rule 10)
- Outcome statements after every task (Section 5)
- "What to do next" connectors (Section 22.2)
- Every limit, error case, role restriction, and system message

Removing tells is half the job. A page that reads like a person wrote it but has lost the
timeout value is worse than the one it replaced.
