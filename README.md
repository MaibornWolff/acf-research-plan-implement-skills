# Research-Plan-Implement Skills

A set of agent skills that provide a structured workflow for codebase research, implementation planning, and plan execution.

## Workflow

The three skills form a sequential pipeline. Each step produces an artifact that feeds into the next:

```
                  research report                   approved plan
                  (docs/agents/                     (docs/agents/
                   research/)                         plans/)
                       │                                │
  User Request         │                                │
       │               │                                │
       ▼               ▼                                ▼
┌──────────────┐  ┌──────────────┐              ┌──────────────┐
│ rpi-research │─▶│   rpi-plan   │─────────────▶│rpi-implement │─▶ Working Code
└──────────────┘  └──────────────┘              └──────────────┘
   (optional)        Creates a phased              Executes the plan
  Investigates        implementation plan,          phase by phase with
  the codebase,       iterates with                 automated and manual
  produces a          the user                      verification
  written report
```

You can run all three steps end-to-end for a large feature, or start at any stage. Research can be skipped for simple codebases or straightforward features where the agent can plan directly without prior investigation.

## Skills

### rpi-research

Conduct deep codebase research and produce a written report. Reports are saved to `docs/agents/research/` by default.

```
/rpi-research How does authentication work in this codebase?
```

### rpi-plan

Create detailed, phased implementation plans through interactive research and iteration. Plans are saved to `docs/agents/plans/` by default.

```
/rpi-plan Add a caching layer to the API
```

### rpi-implement

Execute approved implementation plans phase by phase with automated and manual verification. Looks for recent plans in `docs/agents/plans/` or you can provide a specific path.

```
/rpi-implement
```

---

The skills also activate automatically based on trigger phrases like "deeply investigate", "create a plan", or "implement the plan".

Output locations can be overridden via instructions in your `AGENTS.md` or `CLAUDE.md`.

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
