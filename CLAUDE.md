# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

This repository backs the **iThome 2026 鐵人賽** 系列文, a 30-day series building a Laravel + Filament pet store admin called **PetHub**, day by day.

- Root of the repo: `day##-*.png` / `day##-*.svg` pairs, exported diagrams for the corresponding day's article. Two families exist side by side:
  - PetHub feature diagrams, e.g. `day12-order-flow-four-stages`, `day20-inventory-category-chart-widget`, `day26-report-resource-skeleton-flow`, illustrating the feature built in `PetHub/` that day.
  - Kotlin coroutine concept diagrams, e.g. `day05-coroutine-scope-lifecycle`, `day11-thread-per-request-vs-event-loop`, `day13-parent-scope-cancellation-propagation`, used as teaching analogies contrasted against PHP's thread-per-request model. These do not correspond to code in this repo.
- `PetHub/` is the actual Laravel application source code. All development commands below run from inside this directory.

## Development commands

Run from `PetHub/`:

```bash
composer run dev      # starts the app (php artisan dev)
composer run test     # config:clear + lint:check + types:check + php artisan test
composer run lint     # pint --parallel (fix formatting)
composer run lint:check   # pint --parallel --test (check only)
composer run types:check  # phpstan / larastan
```

Single test:
```bash
php artisan test --filter=testName
php artisan test path/to/FileTest.php
vendor/bin/pest --filter=testName
```

After editing any PHP file, run `vendor/bin/pint --dirty --format agent` before finishing, per the project's own Boost guidelines.

## Architecture

PetHub is a Laravel 13 + Filament 5 + Livewire 4 app, built on the Livewire starter kit with Fortify auth and Flux UI, using SQLite by default.

- **Multi-tenancy**: routes are scoped under a `{current_team}` prefix guarded by `EnsureTeamMembership` middleware (`app/Http/Middleware/EnsureTeamMembership.php`), which checks team membership, enforces minimum role, and switches the user's current team. Core models: `Team`, `TeamInvitation`, `Membership`, `User`.
- **Filament resources** live under `app/Filament/Resources/<Name>/`, split into `Pages/` (Create/Edit/List/View), `Schemas/` (form and infolist definitions), and `Tables/` (table definitions) — see `Products/` as the reference layout for adding new resources.
- **`PetHub/CLAUDE.md` is auto-generated and maintained by Laravel Boost** (`composer run` triggers `artisan boost:update` on `post-update-cmd`). It already documents detailed per-package conventions, Artisan/Tinker/Pint/Pest usage, and skill activation rules — read it for Laravel-specific guidance instead of duplicating it here, and don't hand-edit it directly.
