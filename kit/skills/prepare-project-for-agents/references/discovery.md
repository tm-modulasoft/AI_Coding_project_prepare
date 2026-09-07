# Discovery

Inspect the **host project** (git toplevel). Cite paths. Do not guess scripts or versions.

After this pass, merge with `references/golden-rules.md`: project standard if more correct/specific; golden default if silent; safety floors never Canonical-ized from a bad habit.

## Existing AI layer (backup before replace)

Look for and read:

- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `AGENT.md`, `AI_CODING_README.md`, `AI_CODING_LEARN.md`
- `.cursorrules`, `.cursor/rules/**` (fold globbed _project_ rules into helpers + Pointers; do not recreate them. Leave step 1 `ai-coding-native-rules.mdc` to harness merge)
- `.github/copilot-instructions.md`, `.github/instructions/**`
- `.claude/`, `.claude/rules/`, `.agents/`, `.windsurfrules`

If replacing, copy to `*.bak` in the same directory.

## Human README

Find the GitHub-visible README (first match wins for edits): `.github/README.md`, then root `README.md`, then `docs/README.md`.

Also read if present: `LICENSE` / `LICENSE.md` / `COPYING`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `SUPPORT.md`, `.env.example`, package/`pyproject` `description`.

Inventory which golden README **jobs** already have a heading (see `golden-rules.md` → README). Do not plan a heading rename. If docs and code disagree on commands, code/scripts/CI win — the README gap-fill must match them.

## Existing harness (merge, do not wipe)

Look for and read:

- `.cursor/settings.json` (especially `plugins`)
- `.cursor/cli.json`
- `skills-lock.json`
- `AI_CODING_README.md`
- `AI_CODING_LEARN.md`
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

If docs and code disagree, encode **code** as current truth in `AGENTS.md` and list the conflict under Gotchas. Do not silently prefer the README as agent truth. When gap-filling the README, correct commands that contradict scripts/CI so the two files match.

If code disagrees with a **safety / a11y floor**, keep the floor in Boundaries; list the code habit under Gotchas. Do not Canonical-ize the defect.
