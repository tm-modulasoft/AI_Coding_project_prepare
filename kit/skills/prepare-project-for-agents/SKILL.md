---
name: prepare-project-for-agents
description: Prepares a repository for AI agentic coding in two steps — (1) harness first (IDE/CLI defaults, Cursor plugins/MCPs, skills-lock + npx skills experimental_install from GitHub sources, always-on native rules), then (2) a lean AGENTS.md spine, on-demand helpers, and vendor shims. Defaults to globally accepted golden rules only where the host is silent; project standards win when they are more correct or specific. Use when the user wants to prepare a project for AI agents, add AGENTS.md, bootstrap agent context, set up the agent harness, follow START_HERE.html, fetch the kit from GitHub onto another project, or invokes /prepare-project-for-agents.
argument-hint: "[target-git-root] [--brownfield|--greenfield] [--harness-only|--instructions-only]"
disable-model-invocation: true
---

# Prepare project for AI agents

Write a portable AI instruction layer for the **host project**, and put the **harness** in place first so agents have plugins, skills, and always-on tool routing before they rely on `AGENTS.md`.

Do not invent a stack or a design system the code does not use. Fill silence with named golden defaults (`references/golden-rules.md`). Project standards win when they are more correct or specific. Do not commit unless asked.

## Port the kit (if missing)

The copyable unit is the **`kit/` folder**. Source: https://github.com/tm-modulasoft/AI_Coding_project_prepare (`kit/` on the default branch).

If this workspace has no `kit/skills/prepare-project-for-agents/SKILL.md` (or no `START_HERE.html` next to `skills/prepare-project-for-agents/`):

1. Shallow-clone the GitHub repo into a **temp** directory on the host:
   - Prefer `gh repo clone tm-modulasoft/AI_Coding_project_prepare .ai-coding-kit-src -- --depth 1`
   - Else `git clone --depth 1 https://github.com/tm-modulasoft/AI_Coding_project_prepare.git .ai-coding-kit-src`
2. Copy **only** `.ai-coding-kit-src/kit` → host `kit/`. Do not copy the clone's `README.md`, `AGENTS.md`, or `.git`.
3. Delete `.ai-coding-kit-src`. Remember that you fetched the kit in this run.

Then continue.

## Resolve paths

1. **Kit root** — directory that contains `START_HERE.html` and `skills/prepare-project-for-agents/` (walk up from this file).
2. **Host project** — git toplevel of the workspace being prepared.
   - Kit root is the git root → host is this repo.
   - Kit root is a subdirectory → host is the parent git root. Write agent files into the host, not into the kit.
3. If `AGENTS.md` / `CLAUDE.md` / `.cursor/rules/ai-coding-native-rules.mdc` already exist, copy to `*.bak` first, then merge (keep human steering that is still true; replace anything the code or kit defaults contradict).

## Mandatory reads

Before scanning the host, read `references/discovery.md`.
Before writing **harness** files, read `references/harness.md`.
Before writing **instruction** files, read `references/instruction-prompt.md`, `references/writing-rules.md`, and `references/golden-rules.md`.
When producing output, follow the matching file under `templates/` (and kit `harness-defaults/` for skills lock, native-rules body, IDE plugin settings, CLI permissions).

## Workflow

Two steps. Do not skip the harness unless the user passed `--instructions-only`. Stop after the harness if they passed `--harness-only`.

### 1. Harness first

IDE/CLI defaults, plugin enablement, `skills-lock.json`, `.agents/skills/` gitignore, always-on native rules.

**You** run `npx skills experimental_install --yes` from the host git root (needs network). That restores skills from **GitHub** sources listed in `skills-lock.json`. Do not leave it for the human.

Marketplace plugins have **no CLI**. After merging `.cursor/settings.json`, check whether the default plugins are already installed on this machine. If any are missing, put the `/add-plugin …` block at the top of your next message (human, in Cursor chat). Details: `references/harness.md`.

### 2. Project instruction layer

1. Classify **brownfield** (derive _what is_ from the codebase) vs **greenfield** (derive _what should be_ from an architecture spec; ask if none).
2. Discover stack, commands, seams, UI/theming, tests, git conventions, secrets/generated dirs. Cite paths. Surface conflicts; do not silently pick docs over code.
3. Merge with golden rules: for each applicable domain, keep the project standard if it is more correct or specific; gap-fill silence with `golden:`; keep safety/a11y floors out of Canonical-from-habit. Skip domains the stack cannot hit (no UI → no theming).
4. Draft the file tree. Skip helpers with no evidence **and** no applicable golden default.
5. Write on-demand helpers first (`docs/agents/` unless the host already uses `.agents/` or `.claude/references/`), then compress into root `AGENTS.md`, then thin shims.
6. Prune every line that would not cause a mistake if removed. Drop inapplicable golden rules.
7. Run the quality checklist in `references/writing-rules.md`. Smoke-check that listed commands exist in scripts/CI.
8. Report using `templates/report.md`. Do not commit.

### After both steps — copied kit folder

The kit is an **installer**, not runtime. Durable host files: `.cursor/`, `skills-lock.json`, `.gitignore` skills block, `AGENTS.md`, helpers, shims.

If you **fetched** `kit/` from GitHub in this run:

- Ask before deleting that folder. **Recommend yes.**
- Do not delete unless the user agrees.

Do **not** offer to delete `kit/` if this git remote is `tm-modulasoft/AI_Coding_project_prepare` (this product repo).

## Gotchas

- Root `AGENTS.md` is always-on context. Aim <250 lines, hard cap ~400.
- Filename is exactly `AGENTS.md` (uppercase, plural). Plain Markdown. No required headings.
- Closest nested `AGENTS.md` wins; do not copy the root stack into every package.
- Cursor `.mdc` rules are globbed pointers, not a second copy of `AGENTS.md` — **except** `.cursor/rules/ai-coding-native-rules.mdc` (`alwaysApply: true`) from step 1.
- `CLAUDE.md` is `@AGENTS.md`, not a fork.
- Working principles cannot be derived from code — ask once, or mark `elicited: default`.
- Agents must Read skill files from disk. Opening `START_HERE.html` via `file://` cannot fetch sibling markdown.
- Do not copy `golden-rules.md` into the host. Do not fight Prettier/gofmt/token files with a golden taste rule.
- Never commit plugin API keys. `"key": true` in settings means “connect in the UI.”

## Resources

- `references/harness.md` — step 1 runbook (mandatory before harness writes)
- `references/instruction-prompt.md` — step 2 runbook (mandatory before instruction writes)
- `references/discovery.md` — what to inspect in the host
- `references/writing-rules.md` — lean vs on-demand split and quality bar
- `references/golden-rules.md` — globally accepted defaults and precedence (mandatory before writing)
- `templates/AGENTS.md` — suggested spine
- `templates/CLAUDE.md` — Claude Code shim
- `templates/copilot-instructions.md` — Copilot shim
- `templates/best-practices.md` — Excluded / Canonical / Language shape
- `templates/conventions.md` — naming, imports, errors, seams
- `templates/ui-ux.md` — kit, states, WCAG floor
- `templates/theming.md` — tokens, layers, no one-off values
- `templates/cursor-rule.mdc` — globbed Cursor rule stub
- `templates/report.md` — end-of-run report
- Kit `harness-defaults/` — skills lock, `ai-coding-native-rules.md`, `cursor-settings.json`, `cli.json`
