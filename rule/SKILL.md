---
name: rule
description: Turn freeform user policy text into a precise operational rule and apply it directly to the repo control surface. Use when the user explicitly invokes `$rule` or asks to codify a new standing instruction, guardrail, or workflow rule for this repository.
---

# Rule

## Overview

Convert the user's rule text into precise operational language, update the enforced repo instructions directly, and keep human-facing documentation aligned when useful.

## Task Class

Treat this as a lightweight stateless workflow:
- do not create or update `scratch/rule.md`
- do not rely on `$continue` or `$task-todo`
- apply changes directly and finish in one invocation

## When To Use

Use this skill when the user:
- invokes `$rule`
- provides a new standing repo rule, guardrail, or operational policy
- asks to formalize a workflow instruction so future agents follow it consistently

Do not use this skill for one-off advice, temporary scratch-task notes, or repo changes that are not intended to become standing instructions.

## Input

Treat the exact remainder of the user's request after `$rule` as the proposed rule text, verbatim.

If the rule text is empty, stop and ask the user to provide it.

## Workflow

### 1) Clarify

- Rewrite the rule as 1-2 canonical bullets that are short and testable.
- Define the scope clearly, including which files, folders, or workflows the rule applies to when that matters.
- Provide 2-3 examples and non-examples when they would reduce ambiguity.
- If the rule is ambiguous or conflicts with existing instructions, stop and ask 1-2 targeted questions that force a clear choice.

### 2) Encode

- Update `AGENTS.md` in the most relevant section.
- Keep the encoded rule compact; do not create a dated ledger of rule additions.
- Update `README.md` only when it improves human understanding of repo usage.

### 3) Enforce

- Search for impacted patterns, references, paths, folder usage, schema fields, or task/template language.
- Apply minimal correctness-focused patches to bring the repo into compliance.
- Do not delete or clean up data without explicit user permission.

### 4) Validate and summarize

- Re-run searches to confirm the old pattern is gone, or explicitly list the remaining exceptions.
- Summarize:
  - canonical rule wording
  - examples and non-examples
  - files changed
  - blockers and exact unblock conditions

## Writing Standard

- Prefer imperative wording.
- Keep the rule scoped to one behavior unless the user clearly asked for a bundled policy.
- Preserve the user's actual intent rather than over-generalizing.
- Add short examples only when they materially reduce ambiguity.
- Avoid duplicating existing rules when a small amendment to current wording is enough.

## Guardrails

- `AGENTS.md` is the enforced source of truth.
- Keep `README.md` aligned when it helps, but do not let it diverge from `AGENTS.md`.
- Do not create scratch files for this skill.
- Do not leave the rule as chat-only advice; if the request is clear, persist it.
- If the proposed rule conflicts with an existing enforced instruction, stop and surface the conflict instead of silently overriding it.

## Definition Of Done

- The rule is encoded in `AGENTS.md` and is coherent with existing instructions.
- `README.md` is updated if needed for human clarity.
- Repository references or templates are aligned, or any remaining conflicts are explicitly escalated.

## Output

When you use this skill, apply the rule directly and report:
- the final canonical wording
- any helpful examples or non-examples
- files changed
- blockers or ambiguities, if any
