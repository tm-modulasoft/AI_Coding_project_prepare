# Discovery

Inspect the **host project** (git toplevel). Cite paths. Do not guess scripts or versions.

## Existing AI layer (backup before replace)

Look for and read:

- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `AGENT.md`
- `.cursorrules`, `.cursor/rules/**`
- `.github/copilot-instructions.md`, `.github/instructions/**`
- `.claude/`, `.agents/`, `.windsurfrules`

If replacing, copy to `*.bak` in the same directory.

## Stack (from manifests, with versions)

`package.json`, `pnpm-workspace.yaml`, `Cargo.toml`, `pyproject.toml`, `go.mod`, Gradle / `.csproj`, lockfiles, Docker, IaC.

## Architecture

Top-level dirs, app entrypoints, seams where new routes/modules/components/tests plug in.

## Commands

Extract the real CLIs from package scripts, Makefile, justfile, Taskfile, tox, and CI workflows. Prefer the command CI runs. Include flags.

## Conventions

Read 2–3 canonical source files plus adjacent tests: imports, naming, errors, component shape, styling.

## UI / theming (only if a UI exists)

Design tokens, CSS variables, theme provider, component library, icon set, motion, a11y, dark mode.

## Tests, lint, git

Test layout, lint/format toolchain, CONTRIBUTING, PR template, commitlint, husky.

## Never-touch

Secrets, generated dirs, vendor, codegen, large fixtures.

## Monorepo

List packages that need their own nested `AGENTS.md` (different commands, seams, or boundaries).

## Conflicts

If docs and code disagree, encode **code** as current truth and list the conflict under Gotchas. Do not silently prefer the README.
