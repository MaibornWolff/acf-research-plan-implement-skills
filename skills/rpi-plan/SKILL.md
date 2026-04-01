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
   - If NO research document was provided, spawn a subagent to find relevant research documents:
     - The subagent first should quickly scan the research folder by filename & yaml frontmatter to find relevant documents
     - Then check if the research is relevant for our plan
     - IF an document is relevant, verify that the research document is up-to-date by checking all changes since it's creation
     - IF the document is outdated, the subagent should make sure to fully understand the latest information and update the document

3. **Spawn sub-agents for missing information**:
   - Do NOT spawn sub-agents to re-discover what the research document already covers
   - Spawn explorer subagents to find additional, relevant files for task
   - The job of the subagent it to explore and return paths to relevant files line numbers, NOT to provide long answers
   - Each sub-agent should have a narrow, specific question to answer — not broad exploration
   - You MUST fully understand third party interfaces you are going to use:
     - IF you have access to the source code (e.g. node_modules), start a subagent to explore the relevant parts
     - OTHERWISE start a websearch, either as a subagent or directly if your websearch tool already behaves like a subagent
   - You MUST find and identify all related documentation to that feature and plan how to udpate it

4. **Read the most relevant files directly into your main context**:
   - Based on the research, user input and/or subagent results, identify the most relevant source files for the given tasks
   - **Read these files yourself using the Read tool** — do NOT delegate this to sub-agents. You need these files in your own context to write an accurate plan.
   - Focus on files that will be modified or that define interfaces/patterns you need to follow

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
   1. [Specific technical question that requires human judgment]
      _Recommended_: [assumed/recommended answer]
   2. [Business logic clarification]
      _Recommended_: [assumed/recommended answer]
   3. [Design preference that affects implementation]
      _Recommended_: [assumed/recommended answer]
   ... [Above are just examples. Ask as many questions as needed]
   ```

   Only ask questions that you genuinely cannot answer through code investigation.

   Work with ASCII mockups for UI/Design related questions to give the user a visual understanding.

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
   1. [Technical uncertainty]
      _Recommended_: [assumed/recommended answer]
   2. [Design decision needed]
      _Recommended_: [assumed/recommended answer]

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
   - Run `python3 <skill_directory>/scripts/metadata.py` to get date, commit, branch, and repository info
   - Determine the output filename: `docs/agents/plans/YYYY-MM-DD-description.md`
     - YYYY-MM-DD is today's date
     - description is a brief kebab-case description
     - Example: `2025-01-08-improve-error-handling.md`
     - The output folder (`docs/agents/plans/`) can be overridden by instructions in the project's `AGENTS.md` or `CLAUDE.md`

2. **Write the plan** to `docs/agents/plans/YYYY-MM-DD-description.md`
   - Ensure the `docs/agents/plans/` directory exists (create if needed)
   - **Every actionable item must have a checkbox** (`- [ ]`) so progress can be tracked during implementation. This includes each change in "Changes Required" and each verification step in "Success Criteria".
   - Split the plan into phases:
     - Keep phases very coarse. It is okay to only have a single phase for small features.
     - Split phases if they are not or only loosely related or can be implemented in parallel
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

[A short specification of the desired end state after this plan is complete]

## What We're NOT Doing

[Explicitly list out-of-scope items to prevent scope creep]

## UI Mockups (if applicable)
[If the changes involve user-facing interfaces (CLI output, web UI, terminal UI, etc.), include ASCII mockups
that visually illustrate the intended result. This helps the reader quickly grasp the change.]

## Architecture and Code Reuse

[Explicitly list the code & utils we can reuse or should extract. Refactorings are fine if they are related to the task and improve the code regarding DRY and reuse. Also make sure to research & mention all relevant third party libs and APIs you plan to use. Use ascii diagrams to visualize architecture decisions if appropriate.]

[High level file tree showing the affected files and signatures]
- `folder`
  - `file1` - [consise change summary]
    - `AffectedClass` - [few word summar]
    - [...]
  - `file2` - [...]

## Performance Considerations

[Any performance implications or optimizations needed]

## Migration Notes

[If applicable, how to handle existing data/systems]

## Phase 1: [Descriptive Name]

[Dependencies: **Phase x**, **Phase y** (Only show this section if applicable)]

[Short summary of this phase accomplishes]

**Tasks**:
- [ ] [Short desciption of first Task. One task is a single actionable action in one `file` or `symbol` (`class`, `function`, etc)]
   [High level code snippet, if applicable. AVOID verbosity or unncecssary details]
- [ ] [Next task, same format as above]
- [ ] [etc.]

**Automated Verification** (Test cases MUST cover ALL acceptance criteria):
- [ ] [`new testcase` (Unit|Integration|E2E) passes]
- [ ] [`another new testcase` (Unit|Integration|E2E) passes]
- [ ] `general check/lint/test commands` passes

**Manual Verification** (OMIT if not feasible, MUST BE user facing behaviour. Focus on acceptance criteria that couldn't be covered by automated tests):
- [ ] [Description of manual test case]
  1. [Step 1]
  2. [Step 2]
- [ ] [...]

---

## Phase 2: [Descriptive Name]

[Similar structure as above...]

---

## References

- [Related research or documentation]
- [Similar implementation: file:line]
````

### Step 5: Review & Iterate

1. **Start an subagent to review the plan first. Focus on consistency and other common mistakes.**

2. **Fix relevant findings of the review subagents directly**

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
   - Use provided research as-is — do not re-investigate what's already documented
   - Use subagents to find key files quickly
   - Read key source files directly into your context rather than delegating to sub-agents
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
   - Ensure that all relevant documentation updates are fully covered by the plan
   - Think about edge cases

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
   - You MUST only add manual verification when the user can interact with a working feature (e.g. open a UI, run a command, trigger a workflow).
   - You MUST NOT use "review the code" or "check the implementation" as a verification step.
   - You MUST NOT add manual verification to internal phases (refactoring, utilities, types, backend without entry point). Use automated verification instead.
   - You SHOULD place manual verification at milestones where a user-facing feature is complete.
   - Examples: UI/UX functionality, performance under real conditions, edge cases that are hard to automate, user acceptance criteria.

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
