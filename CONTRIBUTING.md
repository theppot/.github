# Contributing to PPOT Platform

Welcome to the PPOT development team. This document covers everything
you need to know to contribute effectively. Please read it fully before
opening your first pull request.

If you have questions not covered here, reach out to the IT Director
before assuming.

---

## Branching Strategy

We use a simplified Git Flow. All active development happens on `develop`.
The `main` branch is production-ready code only.

| Branch | Purpose |
|--------|---------|
| `main` | Production. Never commit directly. |
| `develop` | Integration branch. All features merge here first. |
| `feature/*` | New features and enhancements |
| `fix/*` | Bug fixes |
| `hotfix/*` | Urgent production fixes branched from `main` |
| `chore/*` | Maintenance, dependencies, tooling |

**Branch naming examples:**
```
feature/mentorship-pairing-ui
fix/consent-form-expiry
hotfix/login-case-sensitivity
chore/upgrade-laravel-11
```

All branches should be created from `develop` unless it is a `hotfix`,
which branches from `main`.

## Commit Messages

We use [Conventional Commits](https://www.conventionalcommits.org). Keep
messages clear and scannable.

**Format:**
```
type: short description in present tense
```

**Types:**

| Type | When to use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `chore` | Maintenance, dependencies, tooling |
| `refactor` | Code change that isn't a fix or feature |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `style` | Formatting, whitespace (no logic change) |

**Examples:**
```
feat: add mentor availability scheduling
fix: resolve case-sensitive email login bug
refactor: extract pairing logic into service class
chore: upgrade to Laravel 11
docs: update local setup instructions
```

Keep the description under 72 characters. If you need more context,
leave a blank line after the subject and add a body.

## Pull Requests

All code must go through a pull request. No direct commits to `main`
or `develop`.

**Before opening a PR:**
- [ ] Branch is up to date with `develop`
- [ ] Code is self-reviewed — read your own diff first
- [ ] Laravel Pint has been run (`./vendor/bin/pint`)
- [ ] No debug code, `dd()`, `dump()`, or `console.log()` left in
- [ ] Migrations run cleanly from scratch

**Opening a PR:**
- Use the provided PR template — fill it out fully
- Title should follow conventional commit format
- Keep PRs focused — one feature or fix per PR
- If it's a work in progress, open as a Draft PR

**Review:**
- At least one approval is required before merging
- Address all comments before requesting re-review
- Don't merge your own PR

**Merging:**
- Squash merge into `develop`
- Delete the branch after merging

## Local Setup

**Requirements:**
- PHP 8.2+
- Composer
- Node.js 20+
- MySQL 8.0+
- [Laravel Herd](https://herd.laravel.com) (recommended, free tier is sufficient)

**Database:**

The easiest local MySQL setup is [DBngin](https://dbngin.com) (free).
If you're on Herd Pro, the built-in database services work great too.

**Steps:**

1. Clone the repository
```
git clone git@github.com:theppot/portal.git
cd portal
```

2. Install dependencies
```
composer install
npm install
```

3. Set up your environment
```
cp .env.example .env
php artisan key:generate
```

4. Configure your `.env` — update these at minimum:
```
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=ppot_portal
DB_USERNAME=root
DB_PASSWORD=
```

5. Run migrations
```
php artisan migrate
```

6. Start the dev server
```
npm run dev
```

If using Herd, the site will be available at
`http://portal.test` automatically. Otherwise run
`php artisan serve`.

## Code Standards

We use [Laravel Pint](https://laravel.com/docs/pint) for code style.
It is included in the project — no separate install needed.

**Run before every PR:**
```
./vendor/bin/pint
```

**General guidelines:**

- Follow standard Laravel conventions — controllers, models, jobs, and
  services should live where Laravel expects them
- Keep controllers thin — business logic belongs in service classes or
  actions
- Use Livewire components for UI interactions — avoid writing vanilla JS
  unless absolutely necessary
- All new features should have at least basic test coverage
- Use `php artisan make:*` commands to generate files — don't create
  them manually
- Never commit secrets, credentials, or environment-specific values —
  use `.env` and document new variables in `.env.example`
- Migrations are permanent once merged to `develop` — never modify an
  existing migration, always create a new one

**Naming conventions:**

| Thing | Convention |
|-------|-----------|
| Controllers | `PascalCase`, singular — `MentorController` |
| Models | `PascalCase`, singular — `MentorPairing` |
| Migrations | Snake case, descriptive — `create_mentor_pairings_table` |
| Livewire components | `PascalCase` — `MentorAvailabilityForm` |
| Jobs | `PascalCase`, verb noun — `CheckIdlePairings` |
| Routes | `kebab-case` — `/mentor-pairings` |
