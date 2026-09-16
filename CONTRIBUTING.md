# Contributing to ai-doc-architect

Thank you for helping improve this skill. Contributions of all kinds are welcome - new examples, additional anti-patterns, MCP notes, eval test cases, and fixes to existing content.

---

## What Needs Contributions Most

In rough priority order:

1. **New scenario examples** - Real-world scenarios in `examples/` that show how the architecture adapts to specific constraints (OSS projects, mobile-first teams, regulated industries, non-English documentation, etc.).
2. **Anti-patterns from the field** - Mistakes you've actually seen or made when building documentation pipelines. The more specific, the better.
3. **MCP integration notes** - Coverage for platforms not yet in `references/mcps.md` (Asana, ClickUp, Monday.com, Zendesk, Intercom, GitLab-specific workflows, etc.).
4. **Eval test cases** - Additional prompts in `evals/evals.json` that test edge cases or phrasing variants the skill should handle.
5. **Decision guide expansions** - New decision trees for choices not yet covered.

---

## How to Contribute

### 1. Fork and clone

```bash
git clone https://github.com/itismowgli/ai-doc-architect.git
cd ai-doc-architect
```

### 2. Create a branch

Use a descriptive branch name:

```bash
git checkout -b examples/open-source-project
git checkout -b fix/mcp-notion-scope-correction
git checkout -b feat/anti-patterns-translation-workflow
```

### 3. Make your changes

Follow the style guidelines below.

### 4. Open a pull request

Use the PR template. Describe what you changed and why. Link to any relevant issues.

---

## Style Guidelines

### Markdown style

- Use ATX headings (`##`, `###`) - not underline style.
- Use fenced code blocks with language tags (` ```bash `, ` ```sql `, ` ```json `).
- Use tables for comparisons and option matrices - they render cleanly on GitHub and in Claude's context.
- Keep line length under 120 characters for readability in editors.
- No trailing whitespace.

### Writing tone

- **Direct and specific.** Say "Use pgvector when you need vector search and already run PostgreSQL" rather than "pgvector could be a good option in certain cases."
- **Explain the why.** Every recommendation should have a reason. Claude uses the reasoning to make judgment calls, not just follow rules.
- **Avoid jargon without definition.** If you use an acronym (RAG, AST, RBAC, SemVer), define it on first use within the file.
- **Use second person.** "You should..." not "The user should..." or "One should..."

### Reference files

- Keep each reference file under 600 lines. If you're approaching that limit, split into two files and add a pointer.
- Include a `## Table of Contents` with anchor links at the top of any file over 200 lines.
- File names are lowercase, hyphenated: `decision-guide.md`, not `DecisionGuide.md`.

### Examples

Each example file in `examples/` should follow this structure:

```markdown
# Example N: [Scenario Title]

## Context
[Who is this team? What are their constraints? What problem are they solving?]

## What They Asked Claude
[The actual prompt(s) the user would type]

## What the Skill Produces
[The architecture / recommendations / output Claude gives]

## Key Decisions Made
[Why these specific choices for this specific context]

## What to Watch Out For
[Gotchas specific to this scenario]
```

### Eval test cases

Each entry in `evals/evals.json` must include:

```json
{
  "id": <integer>,
  "prompt": "<realistic user prompt - specific, not generic>",
  "should_trigger": <true|false>,
  "expected_output": "<description of what a good response includes>",
  "tags": ["<category>"]
}
```

Good eval prompts are specific and realistic - they include context, constraints, and detail. Bad eval prompts are one-liners like "document my code." See `evals/README.md` for examples of both.

---

## What Not to Contribute

- **Vendor promotion.** The skill recommends specific tools based on technical merit, not affiliation. Don't add recommendations that are primarily promotional.
- **Opinions without reasoning.** "X is better than Y" with no explanation doesn't belong in a reference document.
- **Overly narrow examples.** Examples should be useful to teams beyond the contributor's specific situation. Generalize wherever possible.
- **Duplicate content.** Check existing files before adding. If you want to expand something already covered, edit the existing file rather than adding a new one.

---

## Reporting Issues

Use the issue templates in `.github/ISSUE_TEMPLATE/`:

- **Bug report** - The skill triggers when it shouldn't, doesn't trigger when it should, produces incorrect recommendations, or a reference file contains an error.
- **Feature request** - A scenario, platform, anti-pattern, or decision tree not yet covered.

---

## Releasing

Versioning follows [Semantic Versioning](https://semver.org/). For a skill, that means:

| Bump | When |
|---|---|
| **Major** | A rule changes meaning, or generated output changes shape in a way that breaks existing docs |
| **Minor** | New rules, reference files, commands, or integrations. The common case |
| **Patch** | Corrections and clarifications that do not change what the skill produces |

Steps:

1. Add the release section to `CHANGELOG.md` under `## [X.Y.Z] - YYYY-MM-DD`, moving
   anything relevant out of `## [Unreleased]`.
2. Update the version badge in `README.md` and the entry in its Version history block.
3. Commit the work.
4. Tag it — **annotated, `v`-prefixed**, message `Release vX.Y.Z`:

   ```bash
   git tag -a v2.5.0 -m "Release v2.5.0"
   ```

   The `v` prefix is the convention here. Changelog headings stay unprefixed
   (`## [2.5.0]`), matching Keep a Changelog; the tag is `v2.5.0`.

5. Add the compare link at the bottom of `CHANGELOG.md`:
   `[X.Y.Z]: .../compare/<previous tag>...vX.Y.Z`
6. Push the commit and the tag to every remote:

   ```bash
   git push origin main && git push origin v2.5.0
   git push gitlab main && git push gitlab v2.5.0
   ```

7. **Publish the release on both hosts.** A tag is not a release. Pushing a tag creates a
   tag and nothing else — GitHub and GitLab both layer a separate Release object on top,
   and neither creates one for you. Skip this step and the repo sidebar reads
   "N tags / Create a new release" with an empty releases page.

   ```bash
   # extract this version's section from CHANGELOG.md into notes.md first
   gh   release create v2.5.0 --title "v2.5.0 — <short summary>" \
        --notes-file notes.md --verify-tag --latest

   glab release create v2.5.0 --name "v2.5.0 — <short summary>" \
        --notes-file notes.md
   ```

   `--verify-tag` matters: without it `gh` will silently create a tag that does not exist
   rather than failing, which is how a release ends up pointing at a tag nobody meant to
   make.

   Release notes come from the version's `CHANGELOG.md` section verbatim, so the releases
   page and the changelog cannot drift apart.

Tags are never moved or force-pushed once they exist. A tag is a claim about what a
version contained, and moving it makes every reference to it a lie. Correct a bad release
with a new patch version instead.

A version documented in `CHANGELOG.md` without a tag was a development increment, not a
release. The Release tags table at the bottom of the changelog records which is which.

---

## Questions

Open a Discussion on GitHub or file an issue with the `question` label.
