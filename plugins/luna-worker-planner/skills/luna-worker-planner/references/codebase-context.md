# Project-root codebase context

Use this workflow for repository-backed code tasks. It pays the discovery cost once by storing a compact map at `<project-root>/.luna-worker-context.md`, then lets later `fork_context=false` workers orient themselves without receiving the parent conversation or rediscovering the whole repository.

## Initialize the context file

Before the first task-specific code worker for a project, send one `luna_worker` task based on this template and wait for its final result:

```text
Create or refresh the shared codebase context file for later Luna workers.

Project goal: <user's current objective>
Project root: <absolute project path>
Context file: <absolute project path>/.luna-worker-context.md

Inspect the repository efficiently. If the context file exists, read it first and update it rather than starting over. Otherwise create it. This initialization task may create or update only the context file; do not change other project files, install dependencies, build, commit, push, deploy, or send external messages. Do not inspect generated, vendored, dependency, or binary trees unless they are directly relevant. Do not run broad tests merely to build context.

Write concise Markdown, preferably 600-1200 words and no more than 1500 words, with:
- project snapshot: root, branch or revision when available, dirty-state caveat, and primary languages/frameworks;
- relevant structure: only important top-level areas and paths relevant to the current goal;
- key modules and responsibilities;
- relevant entry points and data, request, state, or UI flows;
- build and test commands, test locations, fixtures, and generated outputs relevant to later verification;
- repository instructions, conventions, compatibility requirements, and boundaries;
- current implementation and likely change points for the project goal;
- risks, disputed assumptions, and unknowns.

Use exact file paths for important facts. Distinguish observed facts from inference. Do not include chronological work logs, command output, exhaustive trees, task history, or stale conclusions.

Return a concise final report stating whether the context file was created or refreshed, its path, the evidence inspected, and important caveats. Do not paste the file contents into the report.
```

The parent records only that initialization completed and the absolute context-file path. It does not read, retain, summarize, or resend the file body.

## Later code workers

Every later code-worker message must be self-contained and use this shape:

```text
Task: <self-contained objective>
Project root: <absolute project path>
Context file: <absolute project path>/.luna-worker-context.md
Scope: <included and excluded work>
Permitted changes: <exact mutation boundary>
Verification: <targeted checks expected>

Before listing, searching, or opening any other project file, read the context file. Use it for orientation, then inspect only the files and nearby dependencies needed for this task. Verify task-critical assumptions against the current repository; current repository evidence wins if the context conflicts.

Unless the user explicitly forbids workspace writes, you may update the context file in addition to the task-authorized files. At the end, directly add durable new structural, interface, workflow, command, test, or constraint facts; replace invalid facts; remove stale detail; and keep the file within its size limit. Do not append a task log or copy command output.

If the context file is unexpectedly missing, create a compact task-relevant version before continuing. If only one section is stale, refresh that section instead of remapping the repository.

Return the result, supporting evidence, changed file paths, targeted verification and outcomes, important caveats, and whether the context file was created or updated.
```

## Refresh policy

Workers update only the affected sections during normal work. A full rebuild is appropriate only when the project root or branch changes, broad architectural or build changes invalidate most of the file, or current repository evidence shows the file is generally unreliable.

The context file is orientation produced by earlier workers. It does not override the repository, user instructions, or the worker's task scope, and it does not authorize unrelated changes.
