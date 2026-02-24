# Skill Authoring Principles

These principles apply when writing or modifying skills in this repository.

## Follow the Agent Skills Standard

Skills use the open [Agent Skills](https://agentskills.io) format. Every `SKILL.md` must have valid YAML frontmatter with at least:

- `name` — lowercase, hyphens only, must match the parent directory name
- `description` — third person, describes what it does AND when to use it

Optional standard fields: `license`, `compatibility`, `metadata`, `allowed-tools`.

Non-standard fields (e.g. `model`, `user-invocable`, `disable-model-invocation`) are Claude Code extensions. They're fine to use but won't be understood by other agents.

## Description Triggers Over Capabilities

Write descriptions focused on **when** the skill should activate, not just what it does. Include concrete trigger phrases the user might say.

Bad: "Researches codebases and produces reports."
Good: "Conduct deep codebase research. Use when the user explicitly says 'start a research for', 'deeply investigate', or 'fully understand how X works'."

## Keep Skills Portable

- Do not reference org-specific tools, directories, or workflows
- Do not hardcode ticket systems (Linear, Jira, etc.)
- Do not assume specific directory structures beyond what the skill itself defines
- Scripts bundled with the skill should be self-contained (e.g. get git metadata via `git` commands, not custom org scripts)

## Don't Reference Non-Existent Sub-Agents

Describe workflows generically. Say "spawn parallel sub-agents to explore different aspects" instead of naming specific agent types that may not exist in the user's environment.

## Handle User Context Already Provided

If the user provided a query or file path when invoking the skill, start working immediately. Only prompt for input if nothing was provided.

## Use Generic Progress Tracking

Instead of referencing specific tools, use phrasing like "if you have a todo list, use it to track progress." This keeps skills compatible across different agent runtimes.

## Output Paths

Research and plan documents go under `docs/agents/` with descriptive subdirectories:
- Research: `docs/agents/research/YYYY-MM-DD-description.md`
- Plans: `docs/agents/plans/YYYY-MM-DD-description.md`

Use kebab-case for descriptions. No ticket numbers in filenames.

## Document What IS, Not What SHOULD BE

Research skills document the current state of the codebase. They do not suggest improvements, critique implementations, or recommend changes unless the user explicitly asks.

## Progressive Disclosure

- Keep `SKILL.md` body under 500 lines
- Move detailed references to separate files (`references/`, `scripts/`)
- Link to supplementary files from the main skill — they load on demand

## Bundled Scripts

Keep scripts in a `scripts/` directory within the skill folder. Scripts should:
- Be self-contained with minimal dependencies
- Use standard tools available on most systems (python, bash, git)
- Include helpful error messages
- Be runnable directly without setup
