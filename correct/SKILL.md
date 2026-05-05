---
name: correct
description: Explain questionable agent behavior and propose the smallest durable prevention change. Use when the user explicitly invokes `$correct` to analyze what the agent did, attribute the primary cause, and recommend a persistent fix so the same mistake does not repeat.
---

# Correct

## Overview

Use this skill to analyze an observable agent deviation, identify its primary cause, and propose the lowest-scope durable correction.

## Task Class

Treat this as a lightweight stateless workflow:
- do not create or update `scratch/correct.md`
- do not rely on `$continue` or `$task-todo`
- return the explanation and proposed prevention changes in chat

## When To Use

Use this skill when the user:
- invokes `$correct`
- asks why the agent took a questionable action
- wants a concrete prevention change so the same behavior does not recur

Do not use this skill for generic retrospectives, repo-wide rule authoring, or direct task-template edits without a correction analysis.

## Input

Treat the exact remainder of the user's request after `$correct` as the case text, verbatim.

If the case text is empty, stop and ask the user to provide:
- the action or actions they are questioning
- the immediate context of what they asked right before it happened

## Goal

For each questioned agent action:
- determine exactly one primary cause
- recommend the lowest-scope durable correction
- include at least one persistent prevention change proposal

Primary cause must be attributed to exactly one of:
- global rule
- task instruction
- user input

If a higher-priority system, developer, or tool constraint was decisive, call it out explicitly as a platform constraint in addition to the closest primary cause category.

Guardrail:
- do not use edits to this skill itself as the prevention mechanism
- prefer fixing the implicated skill/task or adding or refining a repo-wide rule via `$rule`

## Workflow

### 0) Resolve the correction subject

- Scan backwards through the chat transcript for the latest user-invoked task-template command that resolves to an existing `tasks/**/*.md`.
- Skip `$correct` and `$improve`.
- If the case text explicitly identifies a different task or file, prefer that explicit subject.
- If no suitable prior task invocation exists, continue with `no task subject resolved`.

Guardrail:
- never pick `$correct` or `$improve` as the subject

### 1) Decompose the behavior

- Break the confusing behavior into atomic questioned actions.
- If the actions are still ambiguous, ask the user to restate them as 1-3 bullets before proceeding.

### 2) Gather evidence

For each action, gather the smallest useful instruction set:
- the immediately preceding user input
- the resolved subject task file, if any
- any other explicitly implicated task file
- the relevant `AGENTS.md` rules
- any material platform constraints

Do not guess the cause when you can verify it with exact instruction text.

### 3) Attribute the primary cause

Use this decision order and stop at the first explicit match:
1. task instruction
2. global rule
3. user input

If the action came from ambiguity or missing information:
- attribute it to user input when the prompt was under-specified
- otherwise attribute it to the task instruction or global rule that mandated the default behavior

### 4) Recommend the minimal correction

- If user input caused it:
  - propose better phrasing for next time
  - still propose the smallest persistent repo-side guard if the ambiguity is likely to recur

- If a task instruction caused it:
  - propose a minimal patch to the implicated task template or skill
  - do not broaden it into a global rule unless the issue is clearly cross-task

- If a global rule caused it:
  - propose one or more candidate `$rule` lines that refine or clarify the rule

If the best correction path is genuinely unclear, ask one targeted question that forces a choice.

### 5) Stay in proposal mode unless explicitly asked to apply changes

- Phase 1: analyze the cause and present proposals only.
- Phase 2: edit files only if the user explicitly asks to apply the proposed change in a later message.
- If explicit apply approval is missing, stop after the proposal reply and do not edit files.

## Output

Keep the reply short and scannable. Provide:
0. Subject resolved
1. Actions in question
2. Attribution for each action:
   - cause category
   - exact triggering instruction text
3. What to correct for each action:
   - fix target
   - why it is the minimal durable fix
4. Proposed changes:
   - candidate `$rule ...` lines and or
   - minimal task or skill patch proposals

If no explicit apply approval exists yet, end with a one-line gate such as:
- `If you want, I can apply this patch.`

## Definition Of Done

- Every questioned action has exactly one primary cause category.
- The exact triggering instruction text is identified.
- A minimal correction recommendation is provided.
- At least one persistent prevention change is proposed.
- The reply explicitly states the resolved subject or `no task subject resolved`.
- No files are edited unless the user explicitly asked to apply the proposed change.
