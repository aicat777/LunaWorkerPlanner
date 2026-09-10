# Reusable codebase context

Use this workflow for repository-backed code tasks. Its purpose is to pay the discovery cost once, then give later `fork_context=false` workers enough orientation to work narrowly.

## Initial context worker

When no current Project Context Packet exists, send one read-only `luna_worker` task based on this template:

```text
Create a compact, reusable Project Context Packet for later code workers.

Project goal: <user's current objective>
Workspace: <absolute project path>

This task is read-only. Do not edit files, install dependencies, build, commit, deploy, or send external messages. Inspect the repository efficiently with targeted listing and search. Do not read generated, vendored, dependency, or binary trees unless they are directly relevant. Do not run broad tests merely to build context.

Return a concise report with:
1. Snapshot: repository root, current branch or revision when available, dirty-state caveat, and major languages/frameworks.
2. Relevant structure: a compact tree containing only important top-level areas and the directories relevant to the current goal.
3. Module map: key files/modules and their responsibilities.
4. Entry points and flows: how the relevant request, data, state, or UI path moves through the system.
5. Build and test map: package/build tools, targeted commands, test locations, fixtures, and generated outputs relevant to later verification.
6. Project constraints: repository instructions, conventions, compatibility requirements, and boundaries later workers must preserve.
7. Current implementation: what already exists for the stated goal, likely change points, known gaps, and important risks.
8. Unknowns: facts not established by inspection.

Distinguish observed facts from inference and cite concrete file paths for important claims. Keep the packet compact enough to reuse; prefer a focused map over an exhaustive inventory.
```

The planner reviews the final report, removes irrelevant detail, preserves source/revision and uncertainty labels, and retains it in conversation as the current Project Context Packet. This is reasoning over the report, not a new parent-side inspection.

## Later code workers

Include the relevant packet subset in every later worker message:

```text
Project Context Packet (orientation; verify only task-critical assumptions):
<relevant compact context>

Task: <self-contained objective>
Workspace: <absolute project path>
Scope: <included and excluded work>
Permitted changes: <read-only or exact mutation boundary>
Verification: <targeted checks expected>

Use the packet to avoid repeating broad repository discovery. Inspect only the files and nearby dependencies needed to complete this task safely. If the packet conflicts with the repository, follow the observed repository state and report the discrepancy.

Return the result, supporting evidence, changed file paths, targeted verification and outcomes, caveats, and a Context Delta. The Context Delta must list facts that later workers should add, replace, or invalidate, including structural, interface, workflow, command, or test changes; write `none` when nothing reusable changed.
```

## Incremental refresh

Request a focused read-only refresh instead of rebuilding the packet when only one area is stale. Give the worker the current packet subset, the suspected stale facts, and the exact module or flow to recheck. Merge only supported changes after planner review.

Build a new full packet only when there is no usable packet, the project or branch changed, or broad architectural changes invalidated most of the existing map.
