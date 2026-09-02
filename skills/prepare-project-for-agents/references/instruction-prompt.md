# Instruction prompt (runbook)

Execute this runbook against the **host project** after `SKILL.md` path resolution. This is the detailed instruction set; do not duplicate it into `AGENTS.md`.

## Goal

Produce a portable, lean, living agent context pack:

1. Root **`AGENTS.md`** as the single source of truth (open format: https://agents.md).
2. **Helper files** for detail that should not sit always-on.
3. **Thin compatibility shims** so vendor tools load the same truth without duplicating it.
4. Nested `AGENTS.md` only if this is a real monorepo with meaningfully different packages.

Success = an agent that has never seen the setup conversation can clone the host, read `AGENTS.md`, follow pointers, run the real commands, place new code in the right seams, match UI/theming, and know what is forbidden.

## Non-goals

- Do not rewrite the product README into an agent bible.
- Do not dump the PRD, changelog, or full architecture doc into `AGENTS.md`.
- Do not add generic slogans ("write clean code", "follow SOLID", "be helpful").
- Do not add dependencies, CI, or refactors "to help agents" unless a file cannot be accurate without it.
- Do not overwrite existing agent files without backing them up first.
- Do not commit unless the user asks.

## Source of truth (strict)

| Lane | When | Truth is |
|---|---|---|
| **Brownfield** | Existing code | *What is* — every rule must point at a file/path that proves it. If you cannot point, omit it. |
| **Greenfield** | Empty/scaffold | *What should be* — only from an architecture spec the user provides, or ask. Never guess a stack. |

If both an existing `AGENTS.md`/`CLAUDE.md` and the codebase exist, **merge**: keep human-written steering that is still true; replace anything the code contradicts.

Working principles (plan-first, ask-don't-guess, scope discipline) **cannot** be derived from code. Ask the user once, briefly, if they are not already stated. Do not stall the rest of the work on that answer — use a conservative default and mark it `elicited: default`.

## Files to produce

Create only what the host needs. Skip sections that have no evidence.

### 1. `AGENTS.md` (root, always-on spine)

Follow `templates/AGENTS.md`. Cover the six areas that empirically matter: **commands, testing, structure, code style, git workflow, boundaries**.

Suggested sections (drop empties): Project, Layout (including **seams**), Commands (near the top), Conventions, Boundaries, Testing, Git / PRs, Pointers, Gotchas.

### 2. Helper files (on-demand)

Prefer a **tool-agnostic** folder: `docs/agents/`. If the host already uses `.agents/` or `.claude/references/`, match that.

Create as needed:

| File | Contents |
|---|---|
| `docs/agents/stack.md` | Languages, runtimes, frameworks, DB, ORM, styling, test runners, deploy — **with versions**. |
| `docs/agents/best-practices.md` | **Excluded / Canonical / Language** tables. Follow `templates/best-practices.md`. |
| `docs/agents/conventions.md` | Naming, file layout, imports, errors, logging, API shape, where new code goes. |
| `docs/agents/ui-ux.md` | Component anatomy, composition, states (loading/empty/error/disabled), a11y, breakpoints, motion — from *this* UI kit. |
| `docs/agents/theming.md` | Token source of truth, how to add a token, never hardcode color/type/space, light/dark. Point at the token file. |
| `docs/agents/testing.md` | Unit vs integration vs e2e, file naming, fixtures vs mocks, what not to test. |
| `docs/agents/security.md` | Authz, secrets, PII, stack-specific footguns. |
| `docs/agents/git-workflow.md` | Only if non-obvious. |

**`best-practices.md` required shape** — for each topic that actually exists:

```markdown
## <Topic>

### Canonical
- The project's chosen way, with a path to an example file.
- One short snippet copied from the repo (not invented).

### Excluded
- Patterns this repo rejects (even if popular).
- Why in half a sentence, or "not used here".

### Language
- Language/framework-specific rules — **only if true here**.
```

Topics to consider (include only if evidenced): architecture boundaries, data fetching, state, errors, forms, styling, tokens, testing, concurrency, observability, i18n, package management.

### 3. Compatibility shims (thin)

| File | Content |
|---|---|
| `CLAUDE.md` | First line: `@AGENTS.md`. Add Claude-only hooks/skills **only if they already exist**. See `templates/CLAUDE.md`. |
| `.github/copilot-instructions.md` | 5–15 lines pointing at `AGENTS.md` and `docs/agents/*`. See `templates/copilot-instructions.md`. |
| `.cursor/rules/*.mdc` | Path-scoped, `alwaysApply: false`. Local must-dos + pointer to the helper. No always-on duplicate of `AGENTS.md`. |
| Nested `packages/<name>/AGENTS.md` | Package commands, extra Never/Ask, local seams. Do not repeat root stack. |

Optional: `GEMINI.md` / Aider config **only** if those tools are already in the host.

Do **not** revive `.cursorrules` if `.cursor/rules/` or `AGENTS.md` exists.

### 4. README touch (minimal)

If README has no pointer, add one line under Contributing/Development:

`Coding agents: read AGENTS.md.`

Do not otherwise rewrite README.

## Defaults if working principles were not elicited

- Plan before non-trivial work; execute immediately for obvious one-file fixes.
- Ask when requirements or conventions conflict; do not silently pick.
- Smallest change that solves the asked problem.
- Match existing style even if another style is preferred.
- Never commit secrets; never skip hooks; never delete tests to go green.

## After files are written

Report with `templates/report.md`. Do not commit.
