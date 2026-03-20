# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

This is a **Claude Code plugin** called `data-platform-toolkit`. It is not a traditional application — there is no build step, no tests, and no runtime. It consists of Markdown-based skill and agent definitions that Claude Code loads at runtime.

## Architecture

The plugin manifest lives at `.claude-plugin/plugin.json` and wires together two types of components:

- **Skills** (`skills/*/SKILL.md`): Reusable rule sets with YAML frontmatter (`name`, `description`) and a Markdown body containing domain-specific rules. Skills are invoked by name (e.g., `/trino-optimizer`) and can be composed into agents.
- **Agents** (`agents/*.md`): Persona definitions with YAML frontmatter (`name`, `skills[]`) and a system prompt body. Agents combine multiple skills and define how/when to apply them. Invoked via `@agent-name`.

### Skill → Agent Mapping

| Agent | Skills |
|---|---|
| `data-engineer` | `trino-optimizer`, `k8s-deployer` |
| `java-engineer` | `java-testing`, `k8s-deployer` |

`k8s-deployer` is shared across both agents — changes to it affect both personas.

## Editing Guidelines

- Skill rules should be strict and enforceable (e.g., "MUST", "never", "reject if"). Vague guidance gets ignored.
- Agent system prompts should specify *when* to apply each skill, not just list them.
- The `plugin.json` manifest uses relative paths from `.claude-plugin/` — all paths start with `../`.
- Adding a new skill requires: creating `skills/<name>/SKILL.md` with frontmatter, and adding the path to `plugin.json`'s `skills` array.
- Adding a new agent requires: creating `agents/<name>.md` with frontmatter listing its skills, and adding the path to `plugin.json`'s `agents` array.
