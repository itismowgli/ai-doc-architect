# ai-doc-architect

> **Design, architect, and implement AI-driven documentation and user manual automation systems.**
> Reads PRDs from Confluence, FRs from Linear or Jira, compares against your codebase, and
> generates Markdown user manuals automatically.

[![Skill Version](https://img.shields.io/badge/skill-v2.0.0-blue)](./SKILL.md)
[![skills.sh](https://skills.sh/b/itismowgli/ai-doc-architect)](https://skills.sh/itismowgli/ai-doc-architect)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)
[![Works with Claude Code](https://img.shields.io/badge/Claude%20Code-supported-blueviolet)](https://code.claude.com/docs/en/skills)
[![Works with Cursor](https://img.shields.io/badge/Cursor-supported-blue)](https://cursor.com/docs/context/skills)
[![Works with 50+ agents](https://img.shields.io/badge/agents-50%2B-orange)](#supported-agents)

---

## Install a Skill

```bash
npx skills add itismowgli/ai-doc-architect
```

### Source Formats

```bash
# GitHub shorthand (owner/repo)
npx skills add itismowgli/ai-doc-architect

# Full GitHub URL
npx skills add https://github.com/itismowgli/ai-doc-architect

# Direct path to the skill in a monorepo
npx skills add https://github.com/itismowgli/ai-doc-architect/tree/main/skills/ai-doc-architect

# GitLab URL
npx skills add https://gitlab.com/itismowgli/ai-doc-architect

# Any git URL
npx skills add git@github.com:itismowgli/ai-doc-architect.git

# Local path (for development and testing)
npx skills add ./ai-doc-architect
```

### Options

| Option | Description |
|---|---|
| `-g, --global` | Install to user directory instead of project |
| `-a, --agent <agents...>` | Target specific agents (e.g., `claude-code`, `codex`). See [Supported Agents](#supported-agents) |
| `-s, --skill <skills...>` | Install specific skills by name (use `'*'` for all skills) |
| `-l, --list` | List available skills without installing |
| `--copy` | Copy files instead of symlinking to agent directories |
| `-y, --yes` | Skip all confirmation prompts |
| `--all` | Install all skills to all agents without prompts |

### Examples

```bash
# List skills in this repository without installing
npx skills add itismowgli/ai-doc-architect --list

# Install to Claude Code only (project scope)
npx skills add itismowgli/ai-doc-architect -a claude-code

# Install to Claude Code and Cursor
npx skills add itismowgli/ai-doc-architect -a claude-code -a cursor

# Install globally (available in all projects)
npx skills add itismowgli/ai-doc-architect -g

# Install globally to Claude Code only
npx skills add itismowgli/ai-doc-architect -g -a claude-code

# Non-interactive install for CI/CD pipelines
npx skills add itismowgli/ai-doc-architect -a claude-code -g -y

# Install all skills to all detected agents
npx skills add itismowgli/ai-doc-architect --all

# Install to all agents
npx skills add itismowgli/ai-doc-architect --agent '*'
```

### Installation Scope

| Scope | Flag | Location | Use Case |
|---|---|---|---|
| **Project** | (default) | `./<agent>/skills/` | Committed with your project; shared with the whole team |
| **Global** | `-g` | `~/<agent>/skills/` | Available across all your projects |

### Installation Methods

When installing interactively, you can choose:

| Method | Description |
|---|---|
| **Symlink** (Recommended) | Creates symlinks from each agent to a canonical copy. Single source of truth; easy to update. |
| **Copy** | Creates independent copies per agent. Use when symlinks are not supported (Windows, some CI environments). |

---

## Other Commands

| Command | Description |
|---|---|
| `npx skills list` | List installed skills (alias: `ls`) |
| `npx skills find [query]` | Search for skills interactively or by keyword |
| `npx skills remove [skills]` | Remove installed skills from agents |
| `npx skills update [skills]` | Update installed skills to latest versions |
| `npx skills init [name]` | Create a new SKILL.md template |

### `skills list`

List all installed skills, similar to `npm ls`.

```bash
# List all installed skills (project and global)
npx skills list

# List only global skills
npx skills ls -g

# Filter by specific agents
npx skills ls -a claude-code -a cursor
```

### `skills find`

Search for skills interactively or by keyword.

```bash
# Interactive search (fzf-style)
npx skills find

# Search by keyword
npx skills find documentation
```

### `skills update`

```bash
# Update all skills (interactive scope prompt)
npx skills update

# Update this specific skill
npx skills update ai-doc-architect

# Update only global skills
npx skills update -g

# Update only project skills
npx skills update -p

# Non-interactive (auto-detects scope)
npx skills update -y
```

| Option | Description |
|---|---|
| `-g, --global` | Only update global skills |
| `-p, --project` | Only update project skills |
| `-y, --yes` | Skip scope prompt (auto-detect: project if in a project dir, else global) |
| `[skills...]` | Update specific skills by name instead of all |

### `skills remove`

```bash
# Remove interactively (select from installed skills)
npx skills remove

# Remove this skill by name
npx skills remove ai-doc-architect

# Remove from global scope
npx skills remove --global ai-doc-architect

# Remove from specific agents only
npx skills remove --agent claude-code cursor ai-doc-architect

# Remove all installed skills
npx skills remove --all

# Use 'rm' alias
npx skills rm ai-doc-architect
```

| Option | Description |
|---|---|
| `-g, --global` | Remove from global scope instead of project |
| `-a, --agent` | Remove from specific agents (use `'*'` for all) |
| `-s, --skill` | Specify skills to remove (use `'*'` for all) |
| `-y, --yes` | Skip confirmation prompts |
| `--all` | Shorthand for `--skill '*' --agent '*' -y` |

### `skills init`

```bash
# Create SKILL.md in current directory
npx skills init

# Create a new skill in a named subdirectory
npx skills init ai-doc-architect
```

---

## Manual Installation

If you prefer to install without `npx`:

**Claude Code (project):**

```bash
cp -r ai-doc-architect/ .claude/skills/
```

**Claude Code (global):**

```bash
cp -r ai-doc-architect/ ~/.claude/skills/
```

**Cursor (global):**

```bash
cp -r ai-doc-architect/ ~/.cursor/skills/
```

**Claude Cowork:**

1. Download [`ai-doc-architect.skill`](./ai-doc-architect.skill)
2. Open Claude Cowork -> Settings -> Skills -> **Install from file**
3. Select the `.skill` file

---

## What This Skill Does

`ai-doc-architect` turns your AI agent into a specialized documentation systems architect and writer.
Once installed, you can:

- **Design** a complete AI-driven documentation pipeline: agents, databases, workflows, and integrations
  tailored to your team size and tech stack.
- **Plan** which MCPs and external tools to connect, and exactly how they fit into the architecture.
- **Generate a user manual from requirements**: read PRDs from Confluence, functional requirements from
  Linear or Jira, compare against your shipped code on GitHub, and output a Markdown user manual plus
  a gap report. Default output is always Markdown.
- **Write better documentation**: the writing guide covers plain language, second person style,
  task-oriented structure, testing with real users, and how to prompt AI agents for quality output.
- **Scope an MVP** (what to build in 90 days) vs. the full enterprise platform (Year 1+).
- **Avoid the common mistakes**: the anti-patterns guide documents 20 failure modes with root causes
  and fixes.

The core principle: **write code once, let AI maintain accurate documentation automatically.**

---

## Example Prompts

Once installed, try any of these:

```
Design an AI documentation system for our GitHub monorepo.
We use Notion for internal docs and want docs to auto-update on every PR merge.
```

```
What agents do I need for a documentation automation pipeline?
We're a 6-person startup using GitHub and Slack. No Confluence.
```

```
Give me the MVP scope for an AI documentation system.
What do I build in the first 90 days?
```

```
Build the full architecture as a Word document.
Include tech stack, database schema, agent design, and a phased roadmap.
```

```
Read our Confluence PRD and Linear issues, compare against the GitHub PR,
and generate a user manual in Markdown.
Confluence page: https://your-org.atlassian.net/wiki/spaces/PROD/pages/12345678
Linear epic: ENG-142
GitHub PR: #891
```

```
What are the biggest mistakes teams make when automating documentation?
```

```
How do I write task-oriented documentation that users will actually follow?
What are the plain language principles I should apply?
```

```
We already have Confluence and Jira connected.
How should the docs sync work? Which MCPs do I actually need?
```

See [`examples/`](./examples/) for complete, detailed scenario walkthroughs.

---

## What's Inside

### Skill Structure

```
ai-doc-architect/
├── SKILL.md                          # Core skill - triggers, principles, usage guide
│
├── references/
│   ├── architecture.md               # Five-tier architecture, 7 agents, workflows,
│   │                                 #   DB schemas, automation pipelines
│   ├── tech-stack.md                 # Tech choices, integrations, scalability,
│   │                                 #   security, enterprise deployment, roadmap
│   ├── mcps.md                       # MCP integration guide: which to connect,
│   │                                 #   when, how, PRD-to-manual wiring
│   ├── decision-guide.md             # 10 decision trees: MVP scope, LLM, database,
│   │                                 #   destination, deployment, agent count
│   ├── anti-patterns.md              # 20 anti-patterns across planning, build,
│   │                                 #   and operations phases
│   └── writing-guide.md              # How to write docs that work: 7-step process,
│                                     #   plain language, second person, task-oriented
│                                     #   writing, testing with users, analytics
│
├── examples/
│   ├── README.md                     # How to read the examples
│   ├── 01-startup-mvp.md            # 6-person startup, GitHub + Notion, 90-day MVP
│   ├── 02-enterprise-full.md        # Enterprise, multi-repo, Confluence + Jira + Slack
│   ├── 03-api-docs-only.md          # API-first team, OpenAPI auto-documentation
│   ├── 04-healthcare-hipaa.md       # Healthcare SaaS, HIPAA, on-prem option
│   └── 05-prd-to-manual.md         # PRD + Linear FRs + GitHub code = Markdown manual
│
├── evals/
│   ├── README.md                     # How the evals work
│   └── evals.json                    # 15 test cases for skill validation
│
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
│
├── CHANGELOG.md                      # Version history
├── CONTRIBUTING.md                   # How to contribute
└── LICENSE                           # MIT
```

### Progressive Disclosure

Claude loads only what it needs for each query:

| What Claude loads | When |
|---|---|
| `SKILL.md` frontmatter (name + description) | Always - used to decide whether to trigger the skill |
| `SKILL.md` body | When the skill is triggered |
| `references/architecture.md` | When designing agents, workflows, or databases |
| `references/tech-stack.md` | When recommending tech stack, integrations, or deployment model |
| `references/mcps.md` | When the user asks about MCP connections, platform integrations, or the PRD-to-manual workflow |
| `references/decision-guide.md` | When the user asks "should I use X or Y?" |
| `references/anti-patterns.md` | When the user asks what to avoid or is troubleshooting |
| `references/writing-guide.md` | When the user asks about writing quality, plain language, second person, or task-oriented documentation |

This keeps response time fast. Claude never loads reference files it does not need.

### The Architecture

The skill covers a **15-component system** across 5 architectural tiers:

| Tier | Components |
|---|---|
| **Ingestion** | Git webhooks, API schema parsers, UI scrapers, log ingestors |
| **Intelligence** | 7 AI agents + Knowledge Graph (Neo4j) + RAG pipeline (pgvector) |
| **Generation** | Multi-persona doc generator, 12 composable skill modules |
| **Management** | Versioning, approval workflows, quality scoring, translation |
| **Distribution** | 10+ platform adapters (Notion, Confluence, Slack, GitHub Pages...) |

### The 7 Agents

| Agent | MVP? | Role |
|---|---|---|
| Codebase Analyst | Optional | Deep static analysis of source code and APIs |
| **Commit Intelligence** | Core | Understands what changed and what docs are impacted |
| **Documentation Generator** | Core | Writes task-oriented documentation from AI analysis |
| Quality Auditor | Phase 2 | Scores, validates, and flags documentation issues |
| Audience Adapter | Phase 2 | Tailors docs per user role, language, and permissions |
| **Distribution and Sync** | Core | Publishes to Notion, Confluence, Slack, and more |
| Feedback Intelligence | Phase 3 | Learns from usage analytics to improve quality over time |

### MCP Integrations

| MCP | Phase | Primary Use |
|---|---|---|
| GitHub MCP | Core | Commit intelligence, PR diffs, release triggers |
| Atlassian MCP | Core (if using Confluence) | PRD reading, Confluence publishing, Jira gap tickets |
| Notion MCP | Core (if using Notion) | Documentation destination for Notion-first teams |
| Linear MCP | Recommended | FR reading, gap issue creation, status sync |
| Slack MCP | Recommended | Release notifications, staleness alerts, approval flow |
| Figma MCP | Phase 2 | UI component names and screen annotations |
| Intercom MCP | Phase 3 | Sync help articles to in-product support widget |

---

## Supported Agents

Skills can be installed to any of these agents:

| Agent | `--agent` | Project Path | Global Path |
|---|---|---|---|
| AiderDesk | `aider-desk` | `.aider-desk/skills/` | `~/.aider-desk/skills/` |
| Amp, Kimi Code CLI, Replit, Universal | `amp`, `kimi-cli`, `replit`, `universal` | `.agents/skills/` | `~/.config/agents/skills/` |
| Antigravity | `antigravity` | `.agents/skills/` | `~/.gemini/antigravity/skills/` |
| Augment | `augment` | `.augment/skills/` | `~/.augment/skills/` |
| IBM Bob | `bob` | `.bob/skills/` | `~/.bob/skills/` |
| Claude Code | `claude-code` | `.claude/skills/` | `~/.claude/skills/` |
| OpenClaw | `openclaw` | `skills/` | `~/.openclaw/skills/` |
| Cline, Dexto, Warp | `cline`, `dexto`, `warp` | `.agents/skills/` | `~/.agents/skills/` |
| CodeArts Agent | `codearts-agent` | `.codeartsdoer/skills/` | `~/.codeartsdoer/skills/` |
| CodeBuddy | `codebuddy` | `.codebuddy/skills/` | `~/.codebuddy/skills/` |
| Codemaker | `codemaker` | `.codemaker/skills/` | `~/.codemaker/skills/` |
| Code Studio | `codestudio` | `.codestudio/skills/` | `~/.codestudio/skills/` |
| Codex | `codex` | `.agents/skills/` | `~/.codex/skills/` |
| Command Code | `command-code` | `.commandcode/skills/` | `~/.commandcode/skills/` |
| Continue | `continue` | `.continue/skills/` | `~/.continue/skills/` |
| Cortex Code | `cortex` | `.cortex/skills/` | `~/.snowflake/cortex/skills/` |
| Crush | `crush` | `.crush/skills/` | `~/.config/crush/skills/` |
| Cursor | `cursor` | `.agents/skills/` | `~/.cursor/skills/` |
| Deep Agents | `deepagents` | `.agents/skills/` | `~/.deepagents/agent/skills/` |
| Devin for Terminal | `devin` | `.devin/skills/` | `~/.config/devin/skills/` |
| Droid | `droid` | `.factory/skills/` | `~/.factory/skills/` |
| Firebender | `firebender` | `.agents/skills/` | `~/.firebender/skills/` |
| ForgeCode | `forgecode` | `.forge/skills/` | `~/.forge/skills/` |
| Gemini CLI | `gemini-cli` | `.agents/skills/` | `~/.gemini/skills/` |
| GitHub Copilot | `github-copilot` | `.agents/skills/` | `~/.copilot/skills/` |
| Goose | `goose` | `.goose/skills/` | `~/.config/goose/skills/` |
| Hermes Agent | `hermes-agent` | `.hermes/skills/` | `~/.hermes/skills/` |
| Junie | `junie` | `.junie/skills/` | `~/.junie/skills/` |
| iFlow CLI | `iflow-cli` | `.iflow/skills/` | `~/.iflow/skills/` |
| Kilo Code | `kilo` | `.kilocode/skills/` | `~/.kilocode/skills/` |
| Kiro CLI | `kiro-cli` | `.kiro/skills/` | `~/.kiro/skills/` |
| Kode | `kode` | `.kode/skills/` | `~/.kode/skills/` |
| MCPJam | `mcpjam` | `.mcpjam/skills/` | `~/.mcpjam/skills/` |
| Mistral Vibe | `mistral-vibe` | `.vibe/skills/` | `~/.vibe/skills/` |
| Mux | `mux` | `.mux/skills/` | `~/.mux/skills/` |
| OpenCode | `opencode` | `.agents/skills/` | `~/.config/opencode/skills/` |
| OpenHands | `openhands` | `.openhands/skills/` | `~/.openhands/skills/` |
| Pi | `pi` | `.pi/skills/` | `~/.pi/agent/skills/` |
| Qoder | `qoder` | `.qoder/skills/` | `~/.qoder/skills/` |
| Qwen Code | `qwen-code` | `.qwen/skills/` | `~/.qwen/skills/` |
| Rovo Dev | `rovodev` | `.rovodev/skills/` | `~/.rovodev/skills/` |
| Roo Code | `roo` | `.roo/skills/` | `~/.roo/skills/` |
| Tabnine CLI | `tabnine-cli` | `.tabnine/agent/skills/` | `~/.tabnine/agent/skills/` |
| Trae | `trae` | `.trae/skills/` | `~/.trae/skills/` |
| Trae CN | `trae-cn` | `.trae/skills/` | `~/.trae-cn/skills/` |
| Windsurf | `windsurf` | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |
| Zencoder | `zencoder` | `.zencoder/skills/` | `~/.zencoder/skills/` |
| Neovate | `neovate` | `.neovate/skills/` | `~/.neovate/skills/` |
| Pochi | `pochi` | `.pochi/skills/` | `~/.pochi/skills/` |
| AdaL | `adal` | `.adal/skills/` | `~/.adal/skills/` |

> **Kiro CLI users:** The default agent automatically loads skills from `.kiro/skills/` and
> `~/.kiro/skills/` with no configuration needed. For a custom agent, add skills to its
> `resources` in `.kiro/agents/<agent>.json`:
> ```json
> { "resources": ["skill://.kiro/skills/**/SKILL.md"] }
> ```

The `npx skills` CLI automatically detects which coding agents you have installed.
If none are detected, you will be prompted to select which agents to install to.

---

## Compatibility

| Feature | Claude Code | Cursor | Codex | Cline | OpenCode | Roo Code | Windsurf | Gemini CLI |
|---|---|---|---|---|---|---|---|---|
| Basic skills | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| `allowed-tools` | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| `context: fork` | Yes | No | No | No | No | No | No | No |
| Hooks | Yes | No | No | Yes | No | No | No | No |

---

## Environment Variables

| Variable | Description |
|---|---|
| `INSTALL_INTERNAL_SKILLS` | Set to `1` or `true` to show and install skills marked as `internal: true` |
| `DISABLE_TELEMETRY` | Set to disable anonymous usage telemetry from the `skills` CLI |
| `DO_NOT_TRACK` | Alternative way to disable telemetry |

```bash
# Example: install including internal/preview skills
INSTALL_INTERNAL_SKILLS=1 npx skills add itismowgli/ai-doc-architect --list
```

---

## Troubleshooting

**"No skills found"**

Verify that `SKILL.md` exists and contains both `name` and `description` in the frontmatter.

**Skill not loading in agent**

- Check the skill was installed to the correct path for your agent (see [Supported Agents](#supported-agents))
- Restart the agent after installing
- Ensure the `SKILL.md` frontmatter is valid YAML

**Permission errors during install**

Ensure you have write access to the target directory. For global installs, you may need
to run with `sudo` on Linux/macOS if the home directory is restricted.

**Skill triggers when it should not (or does not trigger when it should)**

File a bug report using the [bug report template](.github/ISSUE_TEMPLATE/bug_report.md).
Include the exact prompt used and which output was produced.

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on adding scenarios, anti-patterns,
MCP notes, decision trees, and eval cases.

Issue templates:
- [Bug report](.github/ISSUE_TEMPLATE/bug_report.md)
- [Feature request](.github/ISSUE_TEMPLATE/feature_request.md)

---

## Related Links

- [Agent Skills Specification](https://agentskills.io)
- [Skills Directory](https://skills.sh)
- [vercel-labs/skills CLI](https://github.com/vercel-labs/skills)
- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)
- [Cursor Skills Documentation](https://cursor.com/docs/context/skills)
- [OpenCode Skills Documentation](https://opencode.ai/docs/skills)
- [Roo Code Skills Documentation](https://docs.roocode.com/features/skills)

---

## License

MIT - see [LICENSE](./LICENSE)
