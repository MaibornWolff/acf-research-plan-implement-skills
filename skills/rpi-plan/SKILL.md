---
name: rpi-plan
description: Create detailed, phased implementation plans through interactive research and iteration. Use when the user explicitly asks to "create a plan", "plan the implementation", or "design an approach" for a feature, refactor, or bug fix. Do not use for quick questions or simple tasks.
---

# Implementation Plan

You are tasked with creating detailed implementation plans through an interactive, iterative process. You should be skeptical, thorough, and work collaboratively with the user to produce high-quality technical specifications.

## Initial Setup

If the user already provided a task description, file path, or topic alongside this command, proceed directly to step 1 below. Only if no context was given, respond with:
```
I'll help you create a detailed implementation plan. Let me start by understanding what we're building.

Please provide:
1. A description of what you want to build or change
2. Any relevant context, constraints, or specific requirements
3. Pointers to related files or previous research

I'll analyze this information and work with you to create a comprehensive plan.
```
Then wait for the user's input.

## Process Steps

### Step 1: Context Gathering & Initial Analysis

1. **Read all mentioned files immediately and FULLY**:
   - Any files the user referenced (docs, research, code)
   - **IMPORTANT**: Use the Read tool WITHOUT limit/offset parameters to read entire files
   - **CRITICAL**: DO NOT spawn sub-tasks before reading these files yourself in the main context
   - **NEVER** read files partially - if a file is mentioned, read it completely

2. **Determine if research already exists**:
   - If the user provided a research document (e.g. from `docs/agents/research/`), **trust it as the source of truth**. Do NOT re-research topics that the document already covers. Use its findings, file references, and architecture analysis directly as the basis for planning.
   - **NEVER repeat or re-do research that has already been provided.** The plan phase is about turning existing research into actionable implementation steps, not about gathering information that's already available.
   - If NO research document was provided, proceed with targeted research as described below.

3. **Read the most relevant files directly into your main context**:
   - Based on the research document and/or user input, identify the most relevant source files
   - **Read these files yourself using the Read tool** — do NOT delegate this to sub-agents. You need these files in your own context to write an accurate plan.
   - Focus on files that will be modified or that define interfaces/patterns you need to follow

4. **Only spawn sub-agents for genuinely missing information**:
   - Do NOT spawn sub-agents to re-discover what the research document already covers
   - Only use sub-agents if there are specific gaps: e.g. the research doesn't cover test conventions, a specific API surface, or a file that was added after the research was written
   - Each sub-agent should have a narrow, specific question to answer — not broad exploration

5. **Analyze and verify understanding**:
   - Cross-reference the requirements with actual code (and research document if provided)
   - Identify any discrepancies or misunderstandings
   - Note assumptions that need verification
   - Determine true scope based on codebase reality

6. **Present informed understanding and focused questions**:
   ```
   Based on the task and my research of the codebase, I understand we need to [accurate summary].

   I've found that:
   - [Current implementation detail with file:line reference]
   - [Relevant pattern or constraint discovered]
   - [Potential complexity or edge case identified]

   Questions that my research couldn't answer:
   - [Specific technical question that requires human judgment]
   - [Business logic clarification]
   - [Design preference that affects implementation]
   ```

   Only ask questions that you genuinely cannot answer through code investigation.

### Step 2: Targeted Research & Discovery

After getting initial clarifications:

1. **If the user corrects any misunderstanding**:
   - DO NOT just accept the correction
   - Read the specific files/directories they mention directly into your context
   - Only proceed once you've verified the facts yourself

2. If you have a todo list, use it to track exploration progress

3. **Fill in gaps — do NOT redo existing research**:
   - If a research document was provided, identify only the specific gaps that need filling
   - Read additional files directly when possible — only spawn sub-agents for searches where you don't know the file paths
   - **Ask yourself before any research action: "Is this already covered by the provided research?"** If yes, skip it and use what's there.

4. **Present findings and design options**:
   ```
   Based on my research, here's what I found:

   **Current State:**
   - [Key discovery about existing code]
   - [Pattern or convention to follow]

   **Design Options:**
   1. [Option A] - [pros/cons]
   2. [Option B] - [pros/cons]

   **Open Questions:**
   - [Technical uncertainty]
   - [Design decision needed]

   Which approach aligns best with your vision?
   ```

### Step 3: Plan Structure Development

Once aligned on approach:

1. **Create initial plan outline**:
   ```
   Here's my proposed plan structure:

   ## Overview
   [1-2 sentence summary]

   ## Implementation Phases:
   1. [Phase name] - [what it accomplishes]
   2. [Phase name] - [what it accomplishes]
   3. [Phase name] - [what it accomplishes]

   Does this phasing make sense? Should I adjust the order or granularity?
   ```

2. **Get feedback on structure** before writing details

### Step 4: Detailed Plan Writing

After structure approval:

1. **Gather metadata**:
   - Run `python <skill_directory>/scripts/metadata.py` to get date, commit, branch, and repository info
   - Determine the output filename: `docs/agents/plans/YYYY-MM-DD-description.md`
     - YYYY-MM-DD is today's date
     - description is a brief kebab-case description
     - Example: `2025-01-08-improve-error-handling.md`
     - The output folder (`docs/agents/plans/`) can be overridden by instructions in the project's `AGENTS.md` or `CLAUDE.md`

2. **Write the plan** to `docs/agents/plans/YYYY-MM-DD-description.md`
   - Ensure the `docs/agents/plans/` directory exists (create if needed)
   - **Every actionable item must have a checkbox** (`- [ ]`) so progress can be tracked during implementation. This includes each change in "Changes Required" and each verification step in "Success Criteria".
   - Use the template structure below:

````markdown
---
date: [ISO date/time from metadata]
git_commit: [Current commit hash from metadata]
branch: [Current branch name from metadata]
topic: "[Feature/Task Name]"
tags: [plan, relevant-component-names]
status: draft
---

# [Feature/Task Name] Implementation Plan

## Overview

[Brief description of what we're implementing and why]

## Current State Analysis

[What exists now, what's missing, key constraints discovered]

## Desired End State

[A specification of the desired end state after this plan is complete, and how to verify it]

### UI Mockups (if applicable)
[If the changes involve user-facing interfaces (CLI output, web UI, terminal UI, etc.), include ASCII mockups
that visually illustrate the intended result. This helps the reader quickly grasp the change.]

### Key Discoveries:
- [Important finding with file:line reference]
- [Pattern to follow]
- [Constraint to work within]

## What We're NOT Doing

[Explicitly list out-of-scope items to prevent scope creep]

## Implementation Approach

[High-level strategy and reasoning]

## Phase 1: [Descriptive Name]

### Overview
[What this phase accomplishes]

### Changes Required:

#### [ ] 1. [Component/File Group]
**File**: `path/to/file.ext`
**Changes**: [Summary of changes]

```[language]
// High level code to add/modify
// Focus on signatures, types, and structure
```

### Success Criteria:

#### Automated Verification:
- [ ] Tests pass: `[test command]`
- [ ] Type checking passes: `[typecheck command]`
- [ ] Linting passes: `[lint command]`

#### Manual Verification:
- [ ] Feature works as expected when tested
- [ ] Edge case handling verified
- [ ] No regressions in related features

**Implementation Note**: After completing this phase and all automated verification passes, pause here for manual confirmation from the human before proceeding to the next phase.

---

## Phase 2: [Descriptive Name]

[Similar structure with both automated and manual success criteria...]

---

## Testing Strategy

### Unit Tests:
- [What to test. List concrete test cases that cover all the requirements.]
- [Key edge cases]

### Integration Tests:
- [End-to-end test scenarios]

### Manual Testing Steps:
*Only include steps the user can test by interacting with the app naturally.*
1. [Specific step to verify feature]
2. [Another verification step]

## Performance Considerations

[Any performance implications or optimizations needed]

## Migration Notes

[If applicable, how to handle existing data/systems]

## References

- [Related research or documentation]
- [Similar implementation: file:line]
````

### Step 5: Review & Iterate

1. **Present the draft plan location**:
   ```
   I've created the initial implementation plan at:
   `docs/agents/plans/YYYY-MM-DD-description.md`

   Please review it and let me know:
   - Are the phases properly scoped?
   - Are the success criteria specific enough?
   - Any technical details that need adjustment?
   - Missing edge cases or considerations?
   ```

2. **Iterate based on feedback** - be ready to:
   - Add missing phases
   - Adjust technical approach
   - Clarify success criteria (both automated and manual)
   - Add/remove scope items

3. **Continue refining** until the user is satisfied

## Important Guidelines

1. **Be Skeptical**:
   - Question vague requirements
   - Identify potential issues early
   - Ask "why" and "what about"
   - Don't assume - verify with code

2. **Be Interactive**:
   - Don't write the full plan in one shot
   - Get buy-in at each major step
   - Allow course corrections
   - Work collaboratively

3. **Be Thorough But Not Redundant**:
   - Read all context files COMPLETELY before planning
   - Use provided research as-is — do not re-investigate what's already documented
   - Read key source files directly into your context rather than delegating to sub-agents
   - Only spawn sub-agents for narrow, specific questions that aren't answered by existing research
   - Include specific file paths and line numbers
   - Write measurable success criteria with clear automated vs manual distinction

4. **Be Visual**:
   - If the change involves any user-facing interface (web UI, CLI output, terminal UI, forms, dashboards, etc.), include ASCII mockups in the plan
   - Mockups make the intended result immediately understandable and help catch misunderstandings early
   - Study the current UI before creating mockups
   - Show both the current state and the proposed state when the change modifies an existing UI
   - Keep mockups simple but accurate enough to convey layout, key elements, and interactions

5. **Be Practical**:
   - Focus on incremental, testable changes
   - Consider migration and rollback
   - Think about edge cases
   - Include "what we're NOT doing"

6. **No Open Questions in Final Plan**:
   - If you encounter open questions during planning, STOP
   - Research or ask for clarification immediately
   - Do NOT write the plan with unresolved questions
   - The implementation plan must be complete and actionable
   - Every decision must be made before finalizing the plan

## Success Criteria Guidelines

**Always separate success criteria into two categories:**

1. **Automated Verification** (can be run by agents):
   - Commands that can be run: test suites, linters, type checkers
   - Specific files that should exist
   - Code compilation/type checking

2. **Manual Verification** (requires human testing):
   - UI/UX functionality
   - Performance under real conditions
   - Edge cases that are hard to automate
   - User acceptance criteria

## Common Patterns

### For Database Changes:
- Start with schema/migration
- Add store methods
- Update business logic
- Expose via API
- Update clients

### For New Features:
- Research existing patterns first
- Start with data model
- Build backend logic
- Add API endpoints
- Implement UI last

### For Refactoring:
- Document current behavior
- Plan incremental changes
- Maintain backwards compatibility
- Include migration strategy
