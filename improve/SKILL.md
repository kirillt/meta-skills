---
name: improve
description: Improve the latest used task template with minimal durable edits. Use when the user explicitly invokes `$improve` to refine task-template behavior without turning the request into a repo-wide rule.
---

# Improve

## Overview

Use this skill to convert a concrete improvement request into minimal durable edits to the latest relevant task template.

## Task Class

Treat this as a lightweight stateless workflow:
- do not create or update `scratch/improve.md`
- do not rely on `$continue` or `$task-todo`
- apply changes directly to the resolved `tasks/**/*.md` template and finish in one invocation

## When To Use

Use this skill when the user:
- invokes `$improve`
- wants a concrete task template to behave differently next time
- is asking for a task-local fix rather than a repo-wide rule

If the requested change is clearly repo-wide, stop and suggest using `$rule` instead.

## Input

Treat the exact remainder of the user's request after `$improve` as the improvement text, verbatim.

If the improvement text is empty, stop and ask the user to provide it.

## Scope

- Apply changes only to the latest used task template under `tasks/`, excluding `$correct` and `$improve`.
- Do not update `AGENTS.md` or `README.md` unless the user explicitly asks.
- Do not broaden into unrelated templates, schemas, or repo-wide policy work.

## Goal

Turn the improvement text into minimal durable edits to the latest used `tasks/**/*.md` template so future runs behave as requested.

## Workflow

### 0) Identify the latest used task template

- Scan backwards through the chat transcript for the latest user-invoked task-template command.
- Skip `$correct` and `$improve`.
- Resolve the command name to a task template under `tasks/`.
- If it resolves to a folder, a reserved non-task command, or a missing task file, keep scanning backwards.
- If no suitable prior task invocation exists, stop and ask the user which task to target.

Guardrail:
- never pick `$correct` or `$improve` as the target

### 1) Apply the improvement

1. Read the target task template in full.
2. Translate the improvement text into 1-3 concrete testable changes inside that file.
3. Prefer changes such as:
   - tightening ambiguous language into must or must not
   - adding explicit ordering or stop conditions
   - adding a brief example when it prevents misinterpretation
4. Keep edits minimal and aligned with the existing style and section structure.

If the requested improvement really requires other tasks, global rules, or schema changes:
- do not do that here
- propose the smallest alternative that stays within scope, or escalate to `$rule`

### 2) Validate

- Re-scan the edited task template to ensure it still complies with `AGENTS.md`.
- Check that it does not introduce conflicting instructions.
- Check that it still reads as a runnable standalone procedure template.

## Output

Provide:
- target task resolved
- the exact improvements applied
- any follow-ups only if blocked or if the request should become `$rule`

## Definition Of Done

- The latest relevant task template is resolved without guessing.
- The template receives minimal durable edits aligned with the user's request.
- The edited template still complies with `AGENTS.md`.
- The reply names the target task and summarizes the applied changes.
