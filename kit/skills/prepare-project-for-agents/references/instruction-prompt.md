# Instruction prompt (runbook)

This is **step 2** (project instruction layer). Complete **step 1** first: `references/harness.md`.

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
- Do not dump the PRD, changelog, full architecture doc, or `golden-rules.md` into `AGENTS.md`.
- Do not add generic slogans ("write clean code", "follow SOLID", "be helpful").
- Do not add dependencies, CI, or refactors "to help agents" unless a file cannot be accurate without it.
- Do not overwrite existing agent files without backing them up first.
- Do not commit unless the user asks.
- Do not invent a stack, a design system, or dark mode the host does not have.

## Source of truth (strict)

| Lane                | When                                | Truth is                                                                                                                    |
| ------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Brownfield**      | Existing code                       | _What is_ for project choices — every `repo:` rule points at a proving path. If you cannot point, omit that _project_ rule. |
| **Greenfield**      | Empty/scaffold                      | _What should be_ — only from an architecture spec the user provides, or ask. Never guess a stack.                           |
| **Golden gap-fill** | Host silent on an applicable domain | Checkable default from `references/golden-rules.md`, tagged `golden:`. Skip if the stack cannot hit that domain.            |

If both an existing `AGENTS.md`/`CLAUDE.md` and the codebase exist, **merge**: keep human-written steering that is still true; replace anything the code contradicts.

**Precedence** (do not average): user chat > host project standard when it is more correct/specific (including stricter) > golden default for silence > safety/a11y floors that are never Canonical-ized from a bad habit. Full test: `references/golden-rules.md`.

Working principles (plan-first, ask-don't-guess, scope discipline) **cannot** be derived from code. Ask the user once, briefly, if they are not already stated. Do not stall the rest of the work on that answer — use a conservative default and mark it `elicited: default`.

## Files to produce

Create only what the host needs. Skip sections that have no evidence **and** no applicable golden default.

### 1. `AGENTS.md` (root, always-on spine)

Follow `templates/AGENTS.md`. Cover the six areas that empirically matter: **commands, testing, structure, code style, git workflow, boundaries**. Include the **Precedence** stanza from the template.

Suggested sections (drop empties): Project, Precedence, Layout (including **seams**), Commands (near the top), Tools, Conventions, Boundaries, Testing, Git / PRs, Pointers, Gotchas.

Do not copy `AI_CODING_README.md` or `AI_CODING_LEARN.md` into this file. Those are step 1 (human). Point at them from Layout if useful.

**Tools** (always include a short stanza, even when `.cursor/rules/ai-coding-native-rules.mdc` exists): prefer **Context7 MCP** over client web search / training memory for library and framework docs; prefer **Sonatype MCP** for package version selection and security; GitHub via `gh` CLI only. Do not paste the full native-rules workflow here — that file is step 1.

### 2. Helper files (on-demand)

Prefer a **tool-agnostic** folder: `docs/agents/`. If the host already uses `.agents/` or `.claude/references/`, match that.

Create as needed:

| File                            | Contents                                                                                                                          | Template                      |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `docs/agents/stack.md`          | Languages, runtimes, frameworks, DB, ORM, styling, test runners, deploy — **with versions**.                                      | —                             |
| `docs/agents/best-practices.md` | **Excluded / Canonical / Language** tables. Source tags required.                                                                 | `templates/best-practices.md` |
| `docs/agents/conventions.md`    | Naming, file layout, imports, errors, logging, API shape, where new code goes.                                                    | `templates/conventions.md`    |
| `docs/agents/ui-ux.md`          | Component anatomy, composition, states, a11y floor, breakpoints, motion — from _this_ UI kit, plus WCAG 2.2 AA if not overridden. | `templates/ui-ux.md`          |
| `docs/agents/theming.md`        | Token source of truth, how to add a token, never hardcode color/type/space, light/dark only if present.                           | `templates/theming.md`        |
| `docs/agents/testing.md`        | Unit vs integration vs e2e, file naming, fixtures vs mocks, what not to test.                                                     | —                             |
| `docs/agents/security.md`       | Authz, secrets, PII, stack-specific footguns. OWASP as floor, not a lecture.                                                      | —                             |
| `docs/agents/git-workflow.md`   | Only if non-obvious.                                                                                                              | —                             |

**`best-practices.md` required shape** — for each topic that actually exists:

```markdown
## <Topic>

### Canonical

- `repo:` The project's chosen way, with a path to an example file.
- One short snippet copied from the repo (not invented).
- `golden:` Only if the host is silent; name the standard.

### Excluded

- Patterns this repo rejects (even if popular).
- `overridden:` A golden rule this repo deliberately does not follow, with why.

### Language

- Language/framework-specific rules — **only if true here**. Tag `repo:` or `golden:`.
```

Topics to consider (include only if evidenced): architecture boundaries, data fetching, state, errors, forms, styling, tokens, testing, concurrency, observability, i18n, package management.

### 3. Compatibility shims (thin)

| File                               | Content                                                                                                           |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `CLAUDE.md`                        | First line: `@AGENTS.md`. Add Claude-only hooks/skills **only if they already exist**. See `templates/CLAUDE.md`. |
| `.github/copilot-instructions.md`  | 5–15 lines pointing at `AGENTS.md` and `docs/agents/*`. See `templates/copilot-instructions.md`.                  |
| `.cursor/rules/*.mdc`              | Path-scoped, `alwaysApply: false`. Local must-dos + pointer to the helper. No always-on duplicate of `AGENTS.md`. |
| Nested `packages/<name>/AGENTS.md` | Package commands, extra Never/Ask, local seams. Do not repeat root stack.                                         |

Optional: `GEMINI.md` / Aider config **only** if those tools are already in the host.

Do **not** revive `.cursorrules` if `.cursor/rules/` or `AGENTS.md` exists.

Do **not** rewrite step 1’s `.cursor/rules/ai-coding-native-rules.mdc` here. Globbed `.mdc` shims stay `alwaysApply: false`.

### 4. README touch (minimal)

If README has no pointer, add one or two lines under Contributing/Development:

`Coding agents: read AGENTS.md.`

`Humans: AI_CODING_README.md (cmds). Intros: AI_CODING_LEARN.md.`

Do not otherwise rewrite README.

If step 1 was skipped (`--instructions-only`) and the host is missing `AI_CODING_README.md` or `AI_CODING_LEARN.md`, copy the missing files from `harness-defaults/`.

## Defaults if working principles were not elicited

- Plan before non-trivial work; execute immediately for obvious one-file fixes.
- Ask when requirements or conventions conflict; do not silently pick.
- Smallest change that solves the asked problem.
- Match existing style even if another style is preferred.
- Never commit secrets; never skip hooks; never delete tests to go green.

## After files are written

If the host has `skills-lock.json`, include this command in `AGENTS.md` **Commands** (so later clones restore skills):

- Restore agent skills: `npx skills experimental_install --yes`

Report with `templates/report.md`. Do not commit.

If the kit was fetched from GitHub into a host subfolder for this run, ask about deleting it (see `SKILL.md` → After both steps). Do not delete without agreement.
