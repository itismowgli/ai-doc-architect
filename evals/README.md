# Evals

This directory contains test cases used to validate that the `ai-doc-architect` skill triggers correctly and produces high-quality output.

## How Evals Work

Each entry in `evals.json` defines a test prompt and what a good response should include. The skill-creator's eval runner (if available) can run these prompts and compare outputs. In Cowork without the eval runner, use these prompts manually to spot-check the skill after any changes.

## What Makes a Good Eval

A good eval prompt is:

- **Specific** - it includes real constraints, tools, and context (not "design a documentation system").
- **Realistic** - it reads like something a real user would actually type.
- **Discriminating** - it tests something the skill might plausibly get wrong or handle inconsistently.

Each eval has a `should_trigger` field. Prompts with `should_trigger: false` test that the skill does NOT activate when it shouldn't - these are the "near-miss" cases that prevent false positives.

## File Format

See `evals.json` for the full dataset. Schema:

```json
{
  "skill_name": "ai-doc-architect",
  "evals": [
    {
      "id": 1,
      "prompt": "...",
      "should_trigger": true,
      "expected_output": "...",
      "tags": ["..."]
    }
  ]
}
```

## Tags Reference

| Tag | Description |
|---|---|
| `mvp` | Tests MVP scope / 90-day plan recommendations |
| `enterprise` | Tests enterprise architecture recommendations |
| `mcp` | Tests MCP selection and integration guidance |
| `agent-design` | Tests agent roster recommendations |
| `anti-pattern` | Tests anti-pattern detection and avoidance |
| `tech-stack` | Tests technology choice recommendations |
| `should-not-trigger` | Near-miss prompts that should NOT trigger the skill |
| `compliance` | Tests compliance-aware recommendations (HIPAA, SOC 2) |
| `api-docs` | Tests API documentation-specific recommendations |
