# Research-Plan-Implement Skills

A set of agent skills that provide a structured workflow for codebase research, implementation planning, and plan execution.

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

## Workflow

The three skills form a sequential pipeline. Each step produces an artifact that feeds into the next:

```
┌──────────────┐  ┌──────────────┐              ┌──────────────┐
│ rpi-research │─▶│   rpi-plan   │─────────────▶│rpi-implement │─▶ Working Code
└──────────────┘  └──────────────┘              └──────────────┘
  (optional)       Creates a phased              Executes the plan
  Investigates     implementation plan,          phase by phase with
  the codebase,    iterates with                 automated and manual
  produces a       the user                      verification
  written report
```

You can run all three steps end-to-end for a large feature, or start at any stage. Research can be skipped for simple codebases or straightforward features where the agent can plan directly without prior investigation.

### Example: Adding rate limiting to an API

Here is how you would use the full pipeline to add rate limiting to an existing API.

**Step 1: Research the codebase**

Start by understanding how the API currently handles requests, what middleware exists, and where authentication is enforced. The research skill produces a report at `docs/agents/research/`.

```
/rpi-research How does request handling work in this API? I want to understand
the middleware chain, how routes are registered, and how authentication is enforced.
I'm planning to add rate limiting later.
```

This creates a report like `docs/agents/research/2026-04-02-api-request-handling.md` that documents the current middleware pipeline, route registration patterns, and relevant file paths with line numbers.

Key points for good research prompts:
- Focus on understanding what exists today, not on the future feature
- Be specific about which areas to investigate (middleware, routes, auth)
- Mention the broader goal so the agent knows what's relevant to cover

**Step 2: Plan the implementation**

Reference the research report so the planning skill can build on it directly instead of re-investigating the codebase.

```
/rpi-plan Add rate limiting to the API. Here is the research:
@docs/agents/research/2026-04-02-api-request-handling.md
```

The agent reads the research, identifies what else it needs to know, asks clarifying questions (storage backend, rate limits per endpoint, etc.), and produces a phased plan at `docs/agents/plans/2026-04-02-api-rate-limiting.md`. You iterate on the plan until it covers all cases.

**Step 3: Implement the plan**

Point the implementation skill at the approved plan. It reads the plan, checks off tasks as it goes, and pauses for manual verification at the right moments.

```
/rpi-implement @docs/agents/plans/2026-04-02-api-rate-limiting.md
```

The agent works through each phase, runs automated tests after every step, and asks you to verify user-facing behavior at milestones.

## Skills

### rpi-research

Conduct deep codebase research and produce a written report. Reports are saved to `docs/agents/research/` by default.

The research skill documents what exists today: where code lives, how it works, and how components connect. It does not suggest improvements or plan changes.

### rpi-plan

Create detailed, phased implementation plans through interactive research and iteration. Plans are saved to `docs/agents/plans/` by default.

When a research document is provided, the plan skill trusts it as the source of truth and builds on top of it. It asks clarifying questions, proposes design options, and produces a plan with checkboxes that can be tracked during implementation.

### rpi-implement

Execute approved implementation plans phase by phase with automated and manual verification. Looks for recent plans in `docs/agents/plans/` or you can provide a specific path.

The implementation skill uses the plan as both a guide and a progress tracker, marking tasks as in-progress or done as it works through each phase.

---

The skills also activate automatically based on trigger phrases like "deeply investigate", "create a plan", or "implement the plan".

Output locations can be overridden via instructions in your `AGENTS.md` or `CLAUDE.md`.
