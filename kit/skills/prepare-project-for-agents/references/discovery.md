# Discovery

Inspect the **host project** (git toplevel). Cite paths. Do not guess scripts or versions.

After this pass, merge with `references/golden-rules.md`: project standard if more correct/specific; golden default if silent; safety floors never Canonical-ized from a bad habit.

## Existing AI layer (backup before replace)

Look for and read:

- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `AGENT.md`
- `.cursorrules`, `.cursor/rules/**`
- `.github/copilot-instructions.md`, `.github/instructions/**`
- `.claude/`, `.agents/`, `.windsurfrules`

If replacing, copy to `*.bak` in the same directory.

## Existing harness (merge, do not wipe)

Look for and read:

- `.cursor/settings.json` (especially `plugins`)
- `.cursor/cli.json`
- `skills-lock.json`
- `.gitignore` (whether `.agents/skills/` is ignored)
- `.cursor/rules/ai-coding-native-rules.mdc`

If the host already has a richer plugin list or lockfile, keep extras. Step 1 is `references/harness.md`.

## Stack (from manifests, with versions)

`package.json`, `pnpm-workspace.yaml`, `Cargo.toml`, `pyproject.toml`, `go.mod`, Gradle / `.csproj`, lockfiles, Docker, IaC.

Record languages and frameworks so Language golden guides can be selected (and inapplicable ones skipped).

## Architecture

Top-level dirs, app entrypoints, seams where new routes/modules/components/tests plug in.

## Commands

Extract the real CLIs from package scripts, Makefile, justfile, Taskfile, tox, and CI workflows. Prefer the command CI runs. Include flags.

## Conventions

Read 2–3 canonical source files plus adjacent tests: imports, naming, errors, component shape, styling.

Read the formatter/linter as the style spec: Prettier, ESLint, Ruff, Black, gofmt, rustfmt, EditorConfig, Checkstyle.

## UI / theming (only if a UI exists)

Design tokens, CSS variables, theme provider, component library, icon set, motion, a11y, dark mode, `prefers-reduced-motion`, `prefers-color-scheme`.

If tokens exist, they are the theming standard (rank 2). If not, note "no token file" — do not invent a system.

## Tests, lint, git

Test layout, lint/format toolchain, CONTRIBUTING, PR template, commitlint, husky.

## Never-touch

Secrets, generated dirs, vendor, codegen, large fixtures.

## Monorepo

List packages that need their own nested `AGENTS.md` (different commands, seams, or boundaries).

## Conflicts

If docs and code disagree, encode **code** as current truth and list the conflict under Gotchas. Do not silently prefer the README.

If code disagrees with a **safety / a11y floor**, keep the floor in Boundaries; list the code habit under Gotchas. Do not Canonical-ize the defect.
