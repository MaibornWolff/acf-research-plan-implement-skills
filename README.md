# Research-Plan-Implement Skills

A set of agent skills that provide a structured workflow for codebase research, implementation planning, and plan execution.

## Skills

| Skill | Description |
|---|---|
| **rpi-research** | Conduct deep codebase research and produce a written report. |
| **rpi-plan** | Create detailed, phased implementation plans through interactive research and iteration. |
| **rpi-implement** | Execute approved implementation plans phase by phase with automated and manual verification. |

The skill files are located under `skills/` in this repository, each in its own directory (`skills/rpi-research/`, `skills/rpi-plan/`, `skills/rpi-implement/`).

## Installation

Install the skills using the `skills` CLI with the repository's SSH URL:

```bash
npx skills add git@gitlab.maibornwolff.de:agentic-coding-school/research-plan-implement-skills.git
```

To install globally (available across all projects):

```bash
npx skills add git@gitlab.maibornwolff.de:agentic-coding-school/research-plan-implement-skills.git -g
```

To install specific skills only:

```bash
npx skills add git@gitlab.maibornwolff.de:agentic-coding-school/research-plan-implement-skills.git --skill rpi-research
```

## Usage

Invoke skills directly with slash commands:

```
/rpi-research How does authentication work in this codebase?
/rpi-plan Add a caching layer to the API
/rpi-implement
```

The skills also activate automatically based on trigger phrases like "deeply investigate", "create a plan", or "implement the plan".

### Output

- Research reports are saved to `docs/agents/research/` by default
- Plans are saved to `docs/agents/plans/` by default
- These output locations can be overridden via instructions in your `AGENTS.md` or `CLAUDE.md`
- `/rpi-implement` looks for recent plans in `docs/agents/plans/` or you can provide a specific path
