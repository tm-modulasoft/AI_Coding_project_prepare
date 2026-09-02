# AGENTS.md

> README is for humans. This file is for coding agents.

## Project

<!-- One paragraph: what it is, who it serves. One line stack with versions. -->

**Stack:** <!-- e.g. React 18, TypeScript 5, Vite, Tailwind CSS 4 -->

## Layout

<!-- Tree of dirs that matter. One line each: what it is + why it lives there. Include seams. -->

```
src/
  <!-- … -->
```

**Seams** (where new work plugs in):

- Features →
- Routes →
- UI kit →
- Tests →

## Commands

<!-- Exact copy-paste CLIs. Prefer what CI runs. -->

- Install: ``
- Dev: ``
- Test: ``
- Typecheck: ``
- Lint: ``
- Build: ``

## Conventions

<!-- Only project-specific choices. Pointers to helpers for depth. One canonical example or path. -->

## Boundaries

- **Always:**
- **Ask first:** schema changes, new dependencies, CI, public API, secrets-adjacent config
- **Never:** commit secrets, edit vendor/generated output, skip hooks, delete failing tests to go green

## Testing

<!-- Where tests live, what to write, the command that must pass. -->

## Git / PRs

<!-- Only if the repo already has a convention. -->

## Pointers

Load the relevant helper before editing that area:

- Stack — `docs/agents/stack.md`
- Best practices (Excluded / Canonical / Language) — `docs/agents/best-practices.md`
- Conventions — `docs/agents/conventions.md`
- UI/UX — `docs/agents/ui-ux.md`
- Theming — `docs/agents/theming.md`
- Testing — `docs/agents/testing.md`

## Gotchas

<!-- Footguns tied to a path. -->
