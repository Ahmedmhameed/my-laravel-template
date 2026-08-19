---
name: laravel-best-practices
description: "Apply this skill whenever writing, reviewing, or refactoring Laravel PHP code. This includes creating or modifying controllers, models, migrations, form requests, policies, jobs, scheduled commands, service classes, and Eloquent queries. Triggers for N+1 and query performance issues, caching strategies, authorization and security patterns, validation, error handling, queue and job configuration, route definitions, and architectural decisions. Also use for Laravel code reviews and refactoring existing Laravel code to follow best practices. Covers any task involving Laravel backend PHP code patterns."
license: MIT
metadata:
    author: laravel
---

# Laravel Best Practices

Best practices for Laravel, organized by concern. Check `.ai/rules` for project-specific guidance before editing. For exact API syntax, verify with `search-docs`.

## Consistency First

Before applying any rule, check what the application already does. Laravel offers multiple valid approaches, and the best choice is the one the codebase already uses, even if another pattern would be theoretically better. Inconsistency is worse than a suboptimal pattern.

Check sibling files, related controllers, models, or tests for established patterns. If one exists, follow it. Don't introduce a second way. These rules are defaults for when no pattern exists yet, not overrides.

## How to Apply

1. Check the changed files, nearby code, project configuration, and relevant tests for established patterns. Deviate only for a correctness or security defect, and call the deviation out.
2. Map every affected concern to the rule index below. Read each mapped rule file before editing. Skip unrelated rule files.
3. Make the smallest coherent change. Keep the application's architecture and naming instead of introducing a second pattern for the same job.
4. Verify version-sensitive Laravel APIs for the installed version with `search-docs`, or inspect the installed framework when it is unavailable.
5. Run the narrowest relevant tests first, then the project's formatting and static-analysis checks when the change warrants them.
6. Re-read the diff against every mapped rule before finishing.

## Concern Index

For query performance, eager loading, and complex queries, inspect the relevant Eloquent query path and database indexes.
For models, relationships, scopes, casts, controllers, routing, authorization, validation, migrations, jobs, caching, HTTP clients, errors, events, mail, scheduling, collections, Blade views, configuration, testing, naming, and application structure, inspect the matching Laravel surface and existing neighboring code before editing.

## Decision Rules

- Prefer framework features and existing application abstractions over new helpers or dependencies.
- Avoid speculative abstractions. Extract code when it creates a clear domain boundary, removes meaningful duplication, or makes behavior independently testable.
- Keep database access out of Blade views and prevent hidden N+1 queries across controllers, resources, jobs, and serialization.
