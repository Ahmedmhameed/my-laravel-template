# Detection Checklist

Use this checklist to inspect genuine Laravel convention forks. Read matching files rather than recording rules from raw counts. Do not record framework defaults, active-tool targets, or dimensions already covered by `.ai/rules`.

## Validation and HTTP input

- Inline `$request->validate()` vs Form Requests vs `Validator::make()`.
- Custom rule objects vs inline closures vs provider extensions.
- Typed request getters vs raw input access.
- Shared localization files vs Form Request messages and attributes.

## Controllers, routing, and authorization

- Invokable, resource, or multi-method controllers.
- Fat controllers vs Actions, Services, or Jobs.
- Route closures vs controller classes.
- Route middleware vs controller middleware or attributes.
- Implicit binding vs explicit binding or manual lookup.
- Named rate limiters vs inline throttles.
- Gates vs policies and authorization call sites.

## Eloquent and architecture

- Fillable vs guarded mass assignment.
- Attribute class vs legacy accessors and mutators.
- Auto-incrementing keys vs UUIDs or ULIDs.
- Dedicated casts vs inline or built-in casts.
- Direct Eloquent vs repositories or query objects.
- Local scopes vs dedicated builders.
- Observers vs model boot hooks or events.
- Explicit eager loading vs model-level defaults.
- Action or Service structure and invocation method.
- DTO representation and dependency acquisition.
- Events and listeners vs direct calls.
- Helper vs facade idioms, namespace layout, and enum conventions.

## Frontend, database, and tests

- Blade, Livewire, Inertia, or separate SPA stack.
- Blade component and partial composition.
- Localization key style.
- Foreign key syntax, reversible migrations, and enum storage.
- Transaction style and idempotent writes.
- PHPUnit vs Pest, database reset traits, fixtures, collaborator doubles, and JSON assertions.

## Responses, collections, and dates

- API Resources vs JSON responses or direct models.
- Conditional relationship fields and pagination contracts.
- Named routes vs URL helpers or actions.
- Collection pipelines vs arrays and loops.
- Fluent, static, or native string APIs.
- Date construction and mutability policy.

Every applicable dimension receives exactly one verdict: pattern, conflict, default, no-signal, tooling-owned, or already-recorded.
