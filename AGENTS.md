<laravel-boost-guidelines>
=== foundation rules ===

# Laravel Boost Guidelines

The Laravel Boost guidelines are specifically curated by Laravel maintainers for this application. These guidelines should be followed closely to ensure the best experience when building Laravel applications.

## Foundational Context

This application is a Laravel application running on PHP 8.4. You are an expert with the Laravel ecosystem. Always use the APIs that match the installed major version of each package — do not assume a version.

Before relying on a package's API, confirm its installed version:

- PHP packages: run `composer show --direct` to list direct dependencies with versions, or `composer show <vendor/package>` for a single package.
- JS packages: check `package.json` for the installed versions.

## Skills Activation

This project has domain-specific skills available in `**/skills/**`. You MUST activate the relevant skill whenever you work in that domain — don't wait until you're stuck.

## Conventions

- You must follow all existing code conventions used in this application. When creating or editing a file, check sibling files for the correct structure, approach, and naming.
- Use descriptive names for variables and methods. For example, `isRegisteredForDiscounts`, not `discount()`.
- Check for existing components to reuse before writing a new one.

## Application Structure & Architecture

- Stick to existing directory structure; don't create new base folders without approval.
- Do not change the application's dependencies without approval.

## Frontend Bundling

- If the user doesn't see a frontend change reflected in the UI, it could mean they need to run `npm run build`, `npm run dev`, or `composer run dev`. Ask them.

## Documentation Files

- You must only create documentation files if explicitly requested by the user.

## Replies

- Be concise in your explanations — focus on what's important rather than explaining obvious details.

=== git workflow rules ===

# Git Branching & Workflow

- Never commit directly to `main`/`master`/`develop`. Every new feature, fix, chore, or refactor gets its own branch, created before any file is touched.
- Branch naming convention (all lowercase, hyphen-separated):
    - `feature/<short-description>` — new functionality (e.g. `feature/student-document-upload`)
    - `fix/<short-description>` — bug fixes (e.g. `fix/admission-status-validation`)
    - `refactor/<short-description>` — internal restructuring with no behavior change
    - `chore/<short-description>` — tooling, deps, config, CI
    - `hotfix/<short-description>` — urgent production fixes, branched from `main`
- Before starting work: confirm the current branch with `git status` / `git branch --show-current`. If on `main`/`master`/`develop` and the task involves any file change, create and switch to a new branch first: `git checkout -b feature/<short-description>`.
- If the current branch already matches the requested task and is clearly dedicated to it, reuse it instead of creating another branch. Never branch off an already-appropriate `feature/`/`fix/`/`refactor/` branch just to start a new sub-task that belongs to the same piece of work (e.g. don't turn `feature/auth` into `feature/auth-fix` for a follow-up fix on the same feature).
- One branch = one logical unit of work. Do not mix unrelated features or fixes on the same branch.
- Commit messages: imperative mood, concise summary line (≤72 chars), optional body explaining _why_ not _what_. Prefer Conventional Commits style when the project already uses it: `feat:`, `fix:`, `refactor:`, `test:`, `chore:`.
- Commit logically — group related changes into a single commit rather than one commit per file, but don't bundle unrelated changes together.
- Never force-push to shared branches (`main`, `develop`, or any branch others may be working on) without explicit approval.
- Before opening a pull request / handing work back: rebase or merge the latest target branch to catch conflicts early, and re-run the verification steps below.
- Do not merge your own branch into `main` unless explicitly instructed — surface the branch/PR and let the user decide.

=== agent workflow rules ===

# Agent Workflow

For every non-trivial task, follow these phases in order. Do not skip a phase unless the task is trivial (e.g. a one-line copy fix).

### 1. Explore

- Inspect the relevant codebase before proposing anything.
- Read applicable `.ai/rules`.
- Inspect related tests to understand expected behavior.
- Identify existing, reusable implementations before writing new code.
- Confirm package versions when the task depends on version-specific APIs.
- Do not modify files during this phase.

### 2. Plan

- State the proposed implementation and which files will be created or modified.
- Identify risks, edge cases, and anything uncertain.
- Do not modify files during this phase.

### 3. Implement

- Implement the approved plan, on the correct branch (see Git Branching & Workflow).
- In interactive mode, wait for user approval after the Plan phase before implementing.
- In autonomous mode, the task request itself counts as approval unless explicitly stated otherwise — don't stall waiting for a confirmation that won't come.
- Keep changes focused on the task; avoid drive-by refactors.
- Follow existing architecture and conventions.
- Add or update tests alongside the code.

### 4. Verify

- Run relevant tests, static analysis, and formatters/linters.
- Verify frontend/build changes when applicable.
- Review the git diff.
- Fix discovered issues before reporting the task done.

## Simplicity

- Prefer the simplest solution that fits the existing architecture.
- Do not introduce abstractions (interfaces, repositories, services, traits, base classes) without a concrete, current need — not for hypothetical future requirements.
- Avoid refactoring unrelated code while implementing a task; if you spot something worth improving, flag it separately instead of bundling it in.

## Preserve User Changes

- Never discard, reset, stash, overwrite, or revert user changes without explicit approval.
- Before modifying files, inspect `git status` and `git diff` to see what's already in flight.
- If unrelated uncommitted changes exist, preserve them — don't clean them up as a side effect of your task.
- Never use destructive Git commands (`reset --hard`, `checkout --` on modified files, `clean -fd`, force-push) unless explicitly approved.

## Safe Editing

- Prefer targeted edits over broad search-and-replace operations.
- Before applying a large automated change, inspect all affected files individually rather than trusting the pattern blindly.
- Do not rewrite entire files when a focused change is sufficient — changing one method should not turn into regenerating the whole file.
- After large edits, inspect the complete git diff to confirm the scope matches what was intended.

## Uncertainty

If required information cannot be determined from the repository:

- Do not guess.
- Do not invent APIs, database fields, business rules, or architecture.
- State plainly what is unknown.
- Ask for clarification when the missing information affects correctness; proceed with a clearly-stated assumption only when it doesn't.

=== verification rules ===

# Verification

Verification is not optional and is not just "run the tests." Before considering any change finished, work through this checklist for anything touched:

## Code Quality

- This project defines a `composer quality` script (`composer.json`) that chains: `composer validate --strict`, `composer audit`, `pint --test`, `phpstan analyse`, and the test suite. Run `composer quality` as the final gate before considering a change done, in addition to — not instead of — the fix-in-place steps below.
- Run `vendor/bin/pint --dirty --format agent` on any modified PHP files to fix style issues _before_ running `composer quality`, since `quality:format` runs Pint with `--test` (check-only, fails without fixing). Never rely on `--test` to fix anything.
- Static analysis (`quality:analyse`) currently runs `phpstan analyse app/Models/User.php --level=5` — scoped to a single file. This means PHPStan is not checking the rest of `app/` yet. Treat this as a known gap: don't assume PHPStan coverage extends beyond `User.php` until the script is widened, and flag it to the user rather than silently expanding scope yourself.
- Run `npm run lint` / `npm run format` (or the project's equivalent) for any modified JS/TS/Vue/React files, if configured — this isn't covered by `composer quality`.

## Tests

- Do not create ad-hoc verification scripts or tinker snippets when a real test can prove the same thing. Unit and feature tests are the source of truth.
- Every new feature or fix needs a corresponding test (feature test by default, unit test for isolated logic). Use `php artisan make:test [options] {name}`.
- Run the specific test(s) related to the change first: `php artisan test --compact --filter=testName`.
- If those pass, run the full suite before considering the task complete (`php artisan test --compact`), unless the full suite is prohibitively slow/expensive. If the full suite is skipped, explicitly report that it was skipped and why — never report "done" with an unrun suite left unmentioned.
- Never delete, skip, or comment out an existing test to make the suite pass. If a test seems wrong, flag it and ask — don't silently remove it.
- Tests must cover: the happy path, expected failure/validation paths, and meaningful edge cases (not just the one scenario that was requested).

## Build & Runtime

- If the change touches frontend assets, verify the build actually reflects the change: `npm run build` or ask the user to run `npm run dev` / `composer run dev`.
- Check `browser-logs` (via Boost) for new JS errors or console warnings introduced by the change, when the change touches the frontend.
- For anything routing-related, confirm with `php artisan route:list` that the expected route exists and resolves as intended.

## Database

- Before writing a migration, inspect current structure with `database-schema` (Boost) rather than guessing column names/types.
- After running new migrations, verify them against a test database, not production data. Never run destructive migration commands (`migrate:fresh`, `migrate:reset`) against a database with data the user cares about without explicit confirmation.
- Any read-only data check during development should use `database-query` (Boost) instead of raw SQL in tinker.

## Security & Config

- Never commit secrets, API keys, or `.env` files. Double-check `git status` / `git diff` before committing for accidentally staged credentials.
- Confirm authorization (policies/gates) is applied to any new controller action that touches user-owned or sensitive data — don't rely on route existence alone as access control.
- Validate all user input via Form Requests or explicit `validate()` calls; don't trust raw request data in controllers.

## Before Handing Off / Opening a PR

- Diff review: read `git diff` yourself before presenting the change — catch leftover debug statements (`dd()`, `dump()`, `console.log`), commented-out code, and unrelated changes.
- Confirm the branch follows the naming convention above and contains only the intended commits.
- Summarize what was verified (tests run, build checked, migrations tested) when reporting back — don't just say "done."

=== boost rules ===

# Laravel Boost

## Tools

- Laravel Boost is an MCP server with tools designed specifically for this application. Prefer Boost tools over manual alternatives like shell commands or file reads.
- Use `database-query` to run read-only queries against the database instead of writing raw SQL in tinker.
- Use `database-schema` to inspect table structure before writing migrations or models.
- Use `get-absolute-url` to resolve the correct scheme, domain, and port for project URLs. Always use this before sharing a URL with the user.
- Use `browser-logs` to read browser logs, errors, and exceptions. Only recent logs are useful, ignore old entries.

## Searching Documentation (IMPORTANT)

- Use `search-docs` before changes that depend on Laravel ecosystem APIs, behavior, configuration, or version-specific syntax. Skip it for copy-only edits and other changes where package documentation is irrelevant. Reuse sufficient results already in context instead of searching again.
- Pass a `packages` array to scope results when you know which packages are relevant.
- Use multiple broad, topic-based queries: `['rate limiting', 'routing rate limiting', 'routing']`. Expect the most relevant results first.
- Do not add package names to queries because package info is already shared. Use `test resource table`, not `filament 4 test resource table`.

### Search Syntax

1. Use words for auto-stemmed AND logic: `rate limit` matches both "rate" AND "limit".
2. Use `"quoted phrases"` for exact position matching: `"infinite scroll"` requires adjacent words in order.
3. Combine words and phrases for mixed queries: `middleware "rate limit"`.
4. Use multiple queries for OR logic: `queries=["authentication", "middleware"]`.

## Project Rules

- This project contains committed, area-grouped rules in `.ai/rules` when that directory exists (settled decisions, non-obvious traps, standing constraints). Framework and package guidelines that only apply to specific paths (testing, frontend, components) also live there, under `.ai/rules/boost` — this is not just recorded decisions, it is load-bearing guidance you have not seen inline. Before you enter plan mode or create/edit any file, you MUST first: open @.ai/rules/index.md (it maps file globs to rule files), read every rule file whose globs cover the path(s) in scope, and run `grep -rin 'keyword' .ai/rules` to catch what a path match alone misses. Do not write code until you have read and are following every matching rule. If `.ai/rules` does not exist, continue without it.
- Record durable rules with `record-rule` so the next agent or teammate inherits them instead of working them out again. Pass a `glob` (e.g. `app/Http/Controllers/**`), a short `title`, and a few-line `note`. Always use `record-rule`, never your native memory or notes tool — native memory is personal and session-scoped; only `.ai/rules` is shared with the team and persists in the repo.

## Artisan

- Run Artisan commands directly via the command line (e.g., `php artisan route:list`). Use `php artisan list` to discover available commands and `php artisan [command] --help` to check parameters.
- Inspect routes with `php artisan route:list`. Filter with: `--method=GET`, `--name=users`, `--path=api`, `--except-vendor`, `--only-vendor`.
- Read configuration values using dot notation: `php artisan config:show app.name`, `php artisan config:show database.default`. Or read config files directly from the `config/` directory.

## Tinker

- Execute PHP in app context for debugging and testing code. Do not create models without user approval, prefer tests with factories instead. Prefer existing Artisan commands over custom tinker code.
- Always use single quotes to prevent shell expansion: `php artisan tinker --execute 'Your::code();'`
    - Double quotes for PHP strings inside: `php artisan tinker --execute 'User::where("active", true)->count();'`

=== php rules ===

# PHP

- Always use curly braces for control structures, even for single-line bodies.
- Use PHP 8 constructor property promotion: `public function __construct(public GitHub $github) { }`. Do not leave empty zero-parameter `__construct()` methods unless the constructor is private.
- Use explicit return type declarations and type hints for all method parameters: `function isAccessible(User $user, ?string $path = null): bool`
- Use TitleCase for Enum keys: `FavoritePerson`, `BestLake`, `Monthly`.
- Prefer PHPDoc blocks over inline comments. Only add inline comments for exceptionally complex logic.
- Use array shape type definitions in PHPDoc blocks.
- Never suppress errors with `@`; handle exceptions explicitly with try/catch and meaningful logging.
- Prefer strict comparisons (`===`, `!==`) over loose ones unless loose comparison is intentional and commented.

=== deployments rules ===

# Deployment

- Laravel can be deployed using [Laravel Cloud](https://cloud.laravel.com/), which is the fastest way to deploy and scale production Laravel applications.
- Never deploy directly from a feature branch; deploy from `main`/`master` (or the project's designated release branch) only after verification passes.
- Confirm `.env` values for the target environment (especially `APP_ENV`, `APP_DEBUG`, and queue/cache drivers) before deploying — `APP_DEBUG` must be `false` in production.

=== laravel/core rules ===

# Do Things the Laravel Way

- Use `php artisan make:` commands to create new files (i.e. migrations, controllers, models, etc.). You can list available Artisan commands using `php artisan list` and check their parameters with `php artisan [command] --help`.
- If you're creating a generic PHP class, use `php artisan make:class`.
- Pass `--no-interaction` to all Artisan commands to ensure they work without user input. You should also pass the correct `--options` to ensure correct behavior.

### Model Creation

- When creating new models, create useful factories and seeders for them too. Ask the user if they need any other things, using `php artisan make:model --help` to check the available options.

## APIs & Eloquent Resources

- For APIs, default to using Eloquent API Resources and API versioning unless existing API routes do not, then you should follow existing application convention.

## URL Generation

- When generating links to other pages, prefer named routes and the `route()` function.

## Testing

- When creating models for tests, use the factories for the models. Check if the factory has custom states that can be used before manually setting up the model.
- Faker: Use methods such as `$this->faker->word()` or `fake()->randomDigit()`. Follow existing conventions whether to use `$this->faker` or `fake()`.
- When creating tests, make use of `php artisan make:test [options] {name}` to create a feature test, and pass `--unit` to create a unit test. Most tests should be feature tests.

## Vite Error

- If you receive an "Illuminate\Foundation\ViteException: Unable to locate file in Vite manifest" error, you can run `npm run build` or ask the user to run `npm run dev` or `composer run dev`.

=== pint/core rules ===

# Laravel Pint Code Formatter

- If you have modified any PHP files, you must run `vendor/bin/pint --dirty --format agent` before finalizing changes to ensure your code matches the project's expected style.
- Do not run `vendor/bin/pint --test --format agent`, simply run `vendor/bin/pint --format agent` to fix any formatting issues.

=== phpunit/core rules ===

# PHPUnit

- This application uses PHPUnit for testing. All tests must be written as PHPUnit classes. Use `php artisan make:test --phpunit {name}` to create a new test.
- If you see a test using "Pest", convert it to PHPUnit.
- Every time a test has been updated, run that singular test.
- When the tests relating to your feature are passing, run the entire test suite before considering the task complete, unless it's prohibitively slow — see Verification for the full policy.
- Tests should cover all happy paths, failure paths, and edge cases.
- You must not remove any tests or test files from the tests directory without approval. These are not temporary or helper files; these are core to the application.

## Running Tests

- Run the minimal number of tests, using an appropriate filter, before finalizing.
- To run all tests: `php artisan test --compact`.
- To run all tests in a file: `php artisan test --compact tests/Feature/ExampleTest.php`.
- To filter on a particular test name: `php artisan test --compact --filter=testName` (recommended after making a change to a related file).

</laravel-boost-guidelines>
