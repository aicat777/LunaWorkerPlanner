# Project-root file index

Use `<project-root>/.luna-worker-context.md` as a small, durable index shared by `fork_context=false` code workers. It contains only a file tree and per-file explanations.

## Required file format

````markdown
# Luna Worker File Index

## File tree

```text
<compact project tree>
```

## File explanations

- `path/to/file`: <purpose, important contents or behavior, and relevant relationships in one to three short sentences>
````

Do not add any other section. In particular, omit task history, commands, test results, project goals, progress, decisions, risks, architecture summaries, build instructions, repository constraints, timestamps, and worker commentary.

The tree should cover human-maintained source, test, configuration, script, and documentation paths. Exclude or collapse generated output, dependencies, caches, vendored code, binaries, and high-volume directories that do not help later workers locate project files.

File explanations are keyed by repository-relative path and must be deduplicated. Add an explanation only after directly inspecting the file; never infer it from the path or filename. Update an existing explanation when the file's role or behavior changes. Remove entries for deleted files and move entries for renamed files.

## Initial context worker

Before the first task-specific code worker, send one `luna_worker` task and wait for its final result:

```text
Create or refresh the Luna Worker file index.

Project root: <absolute project path>
Context file: <absolute project path>/.luna-worker-context.md
Current code-work objective: <brief objective used only to choose which files to inspect>

If the context file exists, read it first. Inspect the repository efficiently and create or update the context file using exactly the required two-section format: "File tree" and "File explanations".

Build a compact tree of human-maintained project files while excluding or collapsing generated output, dependencies, caches, vendored code, binaries, and unrelated high-volume directories. Use the current objective only to prioritize which files to inspect; do not store the objective in the context file. Add concise explanations only for files you directly inspect. Each explanation must be keyed by repository-relative path and describe the file's purpose, important contents or behavior, and relevant relationships in one to three short sentences.

This task may create or update only .luna-worker-context.md. Do not change other files, install dependencies, build, test, commit, push, deploy, or send external messages.

Return only whether the context file was created or refreshed, its absolute path, and any failure that prevents later workers from using it. Do not paste its contents into the report.
```

The parent retains only the absolute path and whether initialization succeeded.

## Later code workers

Every later code-worker message remains self-contained and includes:

```text
Task: <self-contained objective>
Project root: <absolute project path>
Context file: <absolute project path>/.luna-worker-context.md
Scope: <included and excluded work>
Permitted changes: <exact mutation boundary>
Verification: <targeted checks expected>

Before listing, searching, or opening any other project file, read the context file. Use its tree and file explanations to locate likely files, then inspect only what this task requires. Current file contents override stale notes.

Unless the user explicitly forbids workspace writes, maintain the context file after completing the task:
- update only affected branches of the file tree;
- add or revise one-to-three-sentence explanations for files directly inspected, created, or changed;
- remove deleted paths and move renamed paths;
- keep one explanation per path and preserve the exact two-section format.

If the context file is missing, create a compact version before continuing. Do not add task history, commands, test results, goals, progress, decisions, risks, architecture summaries, build instructions, repository constraints, or other general context.

Return the task result, supporting evidence, changed paths, verification outcomes, caveats, and whether the context file changed. Do not paste the context file contents.
```

Context maintenance permits changes only to `.luna-worker-context.md`; all other mutations remain governed by the task's explicit scope.
