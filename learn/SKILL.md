---
name: learn
description: Run an end-of-session learning pass that turns lessons from the current work into durable repo improvements. Use when the user explicitly invokes `$learn` or asks for an end-of-session retrospective that should feed rules, tasks, and canonical data fixes rather than staying as prose.
---

# Learn

## Overview

Use this skill at the end of a working session to turn session lessons into durable improvements across rules, tasks, and data.

## When To Use

Use this skill when the user:
- invokes `$learn`
- asks for an end-of-session learning pass
- wants the session’s lessons encoded into the system rather than left as chat-only observations

Typical triggers include sessions that:
- imported or updated case studies
- created or updated `companies/*.json`, `people/*.json`, `projects/*.json`, or `cache/companies/*.json`
- introduced new heuristics or standing rules
- surfaced ambiguity around naming, attribution, provenance, duplication, or time-sensitive claims

## Inputs

Use:
- the chat transcript and session context
- files touched in the session
- any source snapshots or exports materially used in the session

## Goal

End the session with concrete system improvements instead of prose-only lessons.

This skill should:
- summarize what changed
- extract lessons into explicit candidate rules
- improve the most relevant task templates
- apply safe data-normalization fixes when needed
- propose follow-up tasks for anything that still needs research or verification
- reflect on Python or heavy-tool usage and suggest machinery improvements when useful

## Workflow

### 0) Prioritize what to learn

- Identify the most recent 1-3 invoked task templates in the session and treat them as primary tasks.
- Identify the top 1-3 most discussed topics, prioritizing repeated back-and-forth, corrections, or failures.
- Focus first on the primary tasks, then on the top topics.

### 1) Summarize what changed

- List the created or edited files.
- State the intended outcome of the session.
- Follow `AGENTS.md` multi-agent safety rules when interpreting untracked or unknown files.

### 2) Extract lessons into explicit rules and task improvements

- Identify what was ambiguous, wrong, or repeatedly corrected.
- Identify which heuristic or decision resolved it.
- Convert each lesson into one or more candidate `$rule ...` lines when it belongs at the repo-wide level.
- Improve the primary tasks and any directly implicated task templates by default with minimal durable edits.
- Do not broaden into unrelated templates.
- Do not store new lessons back into `$learn` unless the learning procedure itself changed.

### 3) Normalize data when safe

- Apply only minimal correctness-focused fixes.
- Follow `AGENTS.md` and keep `README.md` coherent when touched.
- Do not guess when source support or provenance is missing.

### 4) Handle time-sensitive claims

- Ensure claims such as TVL or similar metrics are date-stamped or routed through the repo’s time-sensitive workflow.

### 5) Create follow-up tasks if needed

- If something still requires browsing, verification, or deferred work, create a scoped follow-up task instead of guessing.

## Guardrails

- Treat lessons as inputs to improving rules, tasks, and data, not as prose to archive.
- Do not store session lessons in scratch beyond minimal execution state.
- Prefer encoding repo-wide lessons via `$rule`.
- Only edit `$learn` when the learning procedure itself needs to change.
- Keep the patch set small and directly motivated by the session.

## Output

Provide a chat summary with:
- primary tasks
- top topics
- what changed
- what was learned
- rules to encode
- open questions
- follow-ups

## Definition Of Done

- The learning summary is provided in chat.
- New repo-wide lessons are converted into candidate rules and encoded when appropriate.
- The most relevant task templates are improved when needed.
- Safe data-normalization fixes are applied or explicitly deferred.
- Any remaining uncertainty is captured as a concrete follow-up instead of a vague note.
