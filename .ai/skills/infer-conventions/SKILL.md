---
name: infer-conventions
description: "Use this skill to analyze how a Laravel application is actually written and record its conventions as shared rules. Trigger when the user wants to detect, infer, document, or standardize project conventions or coding style, set up or grow `.ai/rules`, resolve mixed or conflicting patterns, or onboard agents and teammates to how we do things here. Do not use for one-off code review, enforcing formatting a linter already handles, or editing `.ai/rules` files by hand."
license: MIT
metadata:
    author: laravel
---

# Infer Conventions

Learn how this application writes Laravel, then record what you learn as durable, path-scoped rules other agents will read. You are documenting reality, not improving it.

## Ground Rules

- Consistency first. The codebase's majority style is the convention. Never judge it or propose a better pattern.
- Inspect Pint, PHPStan, and other active tooling first; do not record forms those tools already own.
- Record decisions, architecture choices, and deliberate absences, not framework defaults.
- Read `.ai/rules/index.md` and matching area files before the sweep. Never duplicate existing `.ai/rules`.
- Require at least three consistent examples and no meaningful rival before recording a convention.
- Record only the bare convention in one or two imperative lines, without evidence or file lists.

## Process

1. **Orient:** Read `composer.json`, active tooling configuration, `.ai/rules/index.md`, and map the `app/` tree. Identify applicable checklist groups and non-default architecture directories.
2. **Sweep:** Open [`references/checklist.md`](references/checklist.md) and give every applicable dimension one verdict: pattern, conflict, default, no-signal, tooling-owned, or already-recorded.
3. **Open-ended pass:** Confirm each non-default architecture directory and identify up to five additional high-signal house conventions.
4. **Confirm:** Present candidates with evidence, proposed globs, titles, and notes. Record only approved candidates unless the invocation explicitly requests yolo mode. Keep conflicts deferred unless an existing path boundary explains them.
5. **Record:** Make one `record-rule` call per approved glob. If unavailable, report the exact rule text so it can be recorded manually.
6. **Summarize:** List recorded rules, deferred conflicts, notable no-signals, and remind the user to commit `.ai/rules`.

## Evidence Standard

Framework defaults are not conventions. A candidate must be a deliberate choice that a competent agent could plausibly implement differently, with consistent evidence and no meaningful rival. Scope rules to the narrowest path that contains the evidence.

## Glob Mapping

Use `app/Models/**` for models, `app/Http/**` for controllers and HTTP concerns, `database/migrations/**` for migrations, `tests/**` for tests, and the actual application subtree for Actions, Services, Data, Queries, or domain modules. Use `app/**` only for truly app-wide rules.
