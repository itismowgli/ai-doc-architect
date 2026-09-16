# Publishing and Portability

**Read when:** the user asks where docs will be published, when wiring a distribution MCP, or when a published page looks wrong though the Markdown is fine.
**Skip if:** output stays in the repo as Markdown. Nothing here applies to GitHub.
**Cost:** ~3.2k tokens.

---

How one Markdown source reaches Docmost, Notion, Confluence, Mintlify, Docusaurus,
GitBook, and GitHub without being rewritten for each one.

## Table of Contents

1. [The short answer](#1-the-short-answer)
2. [What is identical everywhere](#2-what-is-identical-everywhere)
3. [What breaks on publish](#3-what-breaks-on-publish)
4. [Compatibility matrix](#4-compatibility-matrix)
5. [Authoring constraints that keep output portable](#5-authoring-constraints-that-keep-output-portable)
6. [The publish transform pass](#6-the-publish-transform-pass)
7. [Per-destination recipes](#7-per-destination-recipes)
8. [Idempotency, ids, and deletion](#8-idempotency-ids-and-deletion)
9. [What NOT to do](#9-what-not-to-do)

---

## 1. The short answer

**Yes, the syntax is the same. You author once.** The generated folder of Markdown is the
source of truth and never changes shape per destination.

**No, you cannot publish it unchanged.** Four things are resolved at publish time, because
they are relative to the filesystem and a hosted page tree has no filesystem:

1. Cross-links between files
2. Image paths
3. Callout blocks
4. Raw HTML

Everything else travels. The transform is mechanical, it runs at publish time, and it
never edits the source files.

The split that matters: **static site generators consume the folder** (the folder *is* the
site, relative paths work, you configure navigation). **Block-based apps import the
folder** (each file becomes a page with an id, relative paths are meaningless, you map and
rewrite). Docmost, Notion, and Confluence are the second kind. Mintlify, Docusaurus,
GitBook, and GitHub are the first.

---

## 2. What is identical everywhere

Write these once and stop thinking about them:

| Element | Notes |
|---|---|
| Headings H1-H3 | Section 4 already caps at H3. Notion supports exactly H1-H3, so the existing rule keeps output portable by accident - do not relax it |
| Paragraphs, bold, italic, inline code | Universal |
| Ordered and unordered lists, nested | Universal |
| Fenced code blocks with a language tag | Universal. Confluence and Docmost convert to a code macro/node, the language survives |
| Tables (simple: no merged cells, no block content in cells) | Universal |
| Blockquotes | Universal, though they may render as a plain quote rather than a styled callout |
| Links to external URLs | Universal |
| Horizontal rules | Universal |

This is the entire vocabulary the Documentation Standards need. Nothing in Sections 20 to
24 requires anything outside this list, which is deliberate.

---

## 3. What breaks on publish

### 3.1 Cross-links between files

The biggest one, because Rule 14 puts links inline at the step that needs them, so they
are everywhere.

```markdown
See [Invite members](../03-daily-workflows/invite-members.md) for the full steps.
```

On a static site generator this resolves correctly. On Docmost, Notion, or Confluence
there is no `../03-daily-workflows/` - there is a page with an id. The link must be
rewritten to that page's URL during publish.

This is why the path-to-page-id map in Section 8 is not an optimisation. Without it,
every cross-link in the manual is dead on arrival.

### 3.2 Image paths

```markdown
![The Members panel...](./img/invite-members-01-members-panel.png)
```

A static site generator serves the file. A block app has no `./img/`. Each capture is
uploaded as an attachment first, and the path is rewritten to the returned URL.

Upload before rewriting, and rewrite in memory. The source file keeps its relative path.

### 3.3 Callout blocks

```markdown
> **Important:** Removing a member is immediate and cannot be undone.
```

Travels everywhere as a blockquote, which is acceptable. Docmost, Notion, and Confluence
all have native callout blocks that look better, so convert when the connector supports
it. Map on the leading label:

| Source label | Native type |
|---|---|
| `**Note:**` | info |
| `**Important:**` | warning |
| `**Data sensitivity:**` | warning, or the platform's strongest available |

Degrading to a blockquote is a cosmetic loss, never a content loss. Never skip the
callout because the target lacks a native type.

### 3.4 Raw HTML

`<details>`, `<summary>`, inline `<br>`, and anchor tags do not survive Notion or
Confluence, and behave inconsistently in Docmost.

The Documentation Standards never require HTML, so generated manuals should contain none.
If a user asks for a collapsible section, tell them it is portable only to static site
generators.

---

## 4. Compatibility matrix

| | Docmost | Notion | Confluence | Mintlify | Docusaurus | GitBook | GitHub |
|---|---|---|---|---|---|---|---|
| **Kind** | Block app | Block app | Block app | SSG | SSG | SSG | Repo |
| Folder is the source | Imported | Imported | Imported | Yes | Yes | Yes | Yes |
| Relative `.md` links | Rewrite | Rewrite | Rewrite | Works | Works | Works | Works |
| Relative image paths | Upload + rewrite | Upload + rewrite | Upload + rewrite | Works | Works | Works | Works |
| H1-H3 | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| H4+ | Avoid | Not supported | Yes | Yes | Yes | Yes | Yes |
| Tables | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Code blocks with language | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Native callouts | Yes | Yes | Yes | Yes | Yes (admonitions) | Yes | No |
| Raw HTML / `<details>` | Partial | No | No | Yes | Yes | Partial | Yes |
| Navigation source | Page tree | Page tree | Page tree | `docs.json` | Sidebar config | `SUMMARY.md` or sync | Folder |
| Needs nav config | No | No | No | **Yes** | Light | Light | No |

Two things to read off this table.

**The block apps need no navigation config but need every path rewritten.** The page tree
*is* the folder tree, so ordering comes free, but nothing relative survives.

**The static site generators need no rewriting but do need navigation config.** Relative
paths work exactly as written, but "zero configuration" is only true for GitHub. Mintlify
in particular needs its navigation declared.

---

## 5. Authoring constraints that keep output portable

These are already Documentation Standards rules. This is why they exist, stated in terms
of publishing:

| Rule | Publishing reason |
|---|---|
| H4 and below avoided (Section 4) | Notion supports H1-H3 only |
| No merged cells, no block content inside table cells | Block apps flatten complex cells unpredictably |
| Relative paths only, never absolute URLs (Section 23.4) | An absolute URL cannot be rewritten to a page id, so it breaks the moment docs move |
| Numeric folder prefixes `NN-kebab-case` (Section 23.2) | Deterministic ordering in Git; Docusaurus reads it directly; the block apps use it to order the page tree |
| Callouts capped at two per section (Section 21.10) | Callout density is the first thing that looks wrong after conversion |
| No em dashes (Section 20.9) | Encoding differences across connectors |
| Straight quotes (Section 24, rule 23) | Copy-paste from a rendered page into a terminal |
| One `index.md` per folder (Section 23.3) | Becomes the section landing page everywhere; without it a block app creates a parent with no body |

Generated Markdown should require no per-destination authoring. If a destination needs
something the standards do not produce, that is a publish-time transform, not a change to
how the manual is written.

---

## 6. The publish transform pass

Runs after the plain-language pass, after the completeness checklist, and after
screenshots. It never edits files on disk.

```
1. Resolve the destination tree
   Create or find the target space/parent. Load the existing path-to-id map.

2. Create or update pages, parents before children
   Walk the folder depth-first. A child cannot be created before its parent exists.
   Record every returned id against its file path.

3. Upload attachments
   Every image under img/. Record the returned URL against its relative path.

4. Rewrite, in memory only
   a. ./img/... paths      → attachment URLs from step 3
   b. ../section/file.md   → page URLs from the map in step 2
   c. > **Note:** blocks   → native callouts, where supported
   d. Strip raw HTML       → where unsupported

5. Push content
   Second pass over every page, now that every id exists.

6. Persist the map
   Write the path-to-id map back. Without this, the next run creates a duplicate tree.

7. Report
   Pages created, pages updated, attachments uploaded, links rewritten,
   anything skipped and why.
```

**Two passes are required, not optional.** Page A links to page B, and page B may not
exist when A is written. Create everything first, then push content with resolved links.
A single-pass publish leaves dead links wherever a link points at a page created later in
the walk.

---

## 7. Per-destination recipes

### Docmost

Folder becomes a space, subfolder becomes a parent page, file becomes a child page. Full
tool list and the licensing and permission constraints are in `references/mcps.md`.

Publish the User Guide space publicly with subpages included; keep the Admin Guide space
private. This is the practical payoff of Rule 2.

### Notion

Folder becomes a page hierarchy under one parent page. Markdown converts to blocks.
Keep to H1-H3 and avoid raw HTML, which the standards already do. Store the returned
`page_id` per file path.

### Confluence

Space per guide, page tree mirrors the folder. Storage format is XHTML, so the connector
converts: code blocks become code macros, callouts become info and note macros. Store the
page id per file path.

### Mintlify

Copy the folder in as-is; relative links and images resolve. Navigation must be declared
in `docs.json` - this is the one SSG that does not infer it. Generate the navigation array
from the folder tree rather than asking the user to hand-write it, and do not overwrite a
`docs.json` the user already maintains without asking.

### Docusaurus

Copy the folder in as-is. Numeric prefixes drive ordering. Use an autogenerated sidebar so
new files appear without config edits, and add a `_category_.json` per folder when you
want a friendlier group label than the folder name.

### GitBook

Copy the folder in as-is, or use Git sync. Structure comes from `SUMMARY.md` or
auto-discovery depending on how the space is configured. Confirm which before generating a
`SUMMARY.md` that the space ignores.

### GitHub

Nothing to do. Relative links and images work in the web UI, and every file has its own
URL. This is the zero-effort destination and a reasonable default before a portal is
chosen.

---

## 8. Idempotency, ids, and deletion

**The map is the whole game.** Store it beside the docs, commit it:

```
docs/[product]/.publish/[destination].json

{
  "destination": "docmost",
  "space": "study-manager-user-guide",
  "commit": "a4f9c21",
  "pages": {
    "01-start-here/index.md": "pg_8f2a...",
    "01-start-here/quick-start.md": "pg_91bc..."
  },
  "attachments": {
    "02-participants/img/finding-a-participant-01.png": "att_44de..."
  }
}
```

Without it, the second publish creates a parallel tree and the team now has two manuals
with no way to tell which one is current.

**Deletion is never automatic.** A file removed from the repo leaves its published page
alone; report it and let a human decide. Someone may have linked to that page, and an
auto-delete on a shared space is not recoverable from the docs pipeline.

**Moves are updates, not create-and-delete.** If a file's path changes but its id is in the
map, move the page and update the key. Recreating it breaks every inbound link and any
comments on the original.

---

## 9. What NOT to do

- **Don't fork the Markdown per destination.** One source, transformed at publish. Two
  sources diverge within a sprint and nobody notices which one is stale.
- **Don't author in the destination and sync back.** Git is the source of truth; the
  portal is a render target. Two-way sync on a tree this size produces conflicts nobody
  will resolve, and it breaks the validation trail - a page edited in Notion has no commit
  SHA behind it.
- **Don't write absolute URLs to your own docs.** They cannot be rewritten to page ids and
  they break the moment the docs move.
- **Don't add platform-specific syntax to the source.** No MDX components, no Confluence
  macros, no Notion callout syntax. The moment one appears, the Markdown is no longer
  portable and every other destination degrades.
- **Don't publish drafts.** Only content that passed validation, the checklist, and the
  plain-language pass reaches a shared space.
- **Don't let a failed publish leave a half-tree.** If step 5 fails partway, report which
  pages have content and which are empty shells. An empty page in a shared space looks
  like a deleted section.
