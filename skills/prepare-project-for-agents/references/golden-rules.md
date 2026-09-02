# Golden rules

Catalog the **preparing agent** consults when writing host `AGENTS.md` and helpers. Do **not** copy this file into the host. Write only the rules that apply to this stack and survive the precedence test.

A golden rule is a **checkable default** from a named standard (W3C, OWASP, language/framework official guide, AGENTS.md spec). It is not a slogan (`SOLID`, `KISS`, "write clean code").

## Precedence

Golden rules are the **default floor** when the host is silent. They are **not a ceiling** and **not a second style guide**.

| Rank | Wins when | Examples |
|---|---|---|
| 1. User chat | Always | "use tabs", "skip dark mode" |
| 2. Host project standard | Taste, architecture, stack, naming, formatting, tokens, and any choice already made in code, linters, architecture spec, or existing `AGENTS.md` — **if it is more correct for this codebase** (matches what the code does, is internally consistent, or is *stricter* than the golden floor) | Prettier config, `tsconfig`, token file, existing button anatomy, AAA contrast if they already require it |
| 3. Golden default | Host is silent **and** the rule applies to this stack | WCAG 2.2 AA on a UI; PEP 8 on Python with no formatter; Conventional Commits when no commitlint |
| 4. Model habit | Never write this | Purple gradients, invented hex, generic "best practices" |

**More correct (keep the project):** stricter a11y than AA; a real token pipeline; a documented rejection of a popular pattern; formatter/compiler as the style spec.

**Not more correct (keep the golden floor, do not Canonical-ize the habit):** committing secrets, string-built SQL, `outline: none` with no replacement, swallowing errors, deleting tests to go green, skipping hooks. Put the habit under **Gotchas**, the floor under **Always / Never**.

**Conflict test (do not average):**

1. Project *explicitly* rejects a golden rule (linter, repeated pattern, documented Excluded) and it is a taste/architecture choice → Canonical = project; Excluded = the golden rule + why.
2. Project is silent and the rule applies → Canonical = golden, mark `golden:`.
3. Following the golden rule would fight the formatter, compiler, or token file → drop the golden rule.
4. Project practice is a safety/a11y defect → golden floor stays; Gotchas names the defect; do not encode it as Canonical.

Skip a domain with no evidence (no UI → no UI/UX or theming helper, no WCAG dump).

## Source tags (required in helpers)

Every Canonical / Excluded / Language bullet starts with one of:

- `repo:` `path` — observed in this host
- `golden:` short standard name — gap fill from this catalog
- `overridden:` standard name — project rejected it on purpose

## Agent instruction files

Source: [AGENTS.md](https://agents.md) (AAIF / Linux Foundation), [Claude Code memory](https://code.claude.com/docs/en/memory).

- Filename `AGENTS.md` (uppercase, plural). Plain Markdown; no required headings.
- Cover what empirically matters: **commands, testing, structure, code style, git, boundaries**.
- Commands are copy-paste runnable (or marked placeholder). Prefer what CI runs.
- Closest nested `AGENTS.md` wins; user chat overrides files. Nested files are **deltas**.
- README = humans. Agent ops = `AGENTS.md` + helpers. If both list a command, they must match.
- Keep the spine lean; link out. Do not dump PRDs or this catalog.

## Language

When the host is silent, consult the **official** guide for the detected language/framework (verify current docs; do not invent a second dialect). Overlay the project's formatter/linter — that overlay is rank 2.

| Detected | Official / de facto (gap fill) |
|---|---|
| TypeScript / JavaScript | TypeScript Handbook + this repo's `tsconfig` / ESLint; MDN for web APIs |
| Python | PEP 8, PEP 484; Ruff/Black config if present **is** the standard |
| Go | Effective Go, Go Code Review Comments; `gofmt` is non-negotiable |
| Rust | rustfmt + Rust API Guidelines |
| C# | Microsoft C# coding conventions |
| Java | This repo's Checkstyle/Spotless, else Google Java Format as a last resort |
| HTML / CSS | HTML Living Standard, CSS spec; semantic HTML before ARIA |
| SQL | Parameterized queries (OWASP); never concatenate untrusted input |
| React / Vue / Angular / etc. | That framework's **current official** style guide and docs only |

Do not write Language bullets for languages that are not in the stack.

## Conventions

Gap-fill only where the host has no proving path.

- Match existing names, file layout, import style, error shape, and API shape (`repo:`).
- Imports at module top. No inline imports unless the file already documents a circular-dependency exception.
- One error strategy, used everywhere (the one in canonical files — HTTP problem+JSON, Result type, exceptions — do not mix).
- Fail at the boundary: validate untrusted input once; do not re-validate the same rule in three layers unless the repo already does.
- Public surfaces stay small (Hyrum's Law): do not leak internals "for convenience".
- Logs are event streams (12-factor XI) if the host is a service; never log secrets or raw PII.
- Config/secrets from the environment or a secret manager (12-factor III), not committed files.

## Best practices (Excluded / Canonical / Language)

For each **evidenced** topic, write three subsections. Canonical is the way agents must copy. Excluded is rejected here even if popular. Language is only rules true in this stack.

Fill Canonical from `repo:` first. Use `golden:` only for silence. Use Excluded for (a) patterns the repo rejects and (b) popular patterns that would fight this codebase.

Topics to consider (include only if evidenced): architecture boundaries, data fetching, state, errors, forms, styling, tokens, testing, concurrency, observability, i18n, package management.

## UI / UX

Apply **only if a UI exists**. Default target: **WCAG 2.2 Level AA** ([W3C](https://www.w3.org/TR/WCAG22/)) unless the host already requires stricter (keep stricter).

Translate Nielsen's [10 heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) into states and behavior — do not paste the essay.

| Floor | Checkable default |
|---|---|
| Semantics | Native HTML for controls (`button`, `a`, `input`, `label`, `dialog`). First rule of ARIA: no ARIA if a native element works ([APG](https://www.w3.org/WAI/ARIA/apg/)). |
| Keyboard | All function available from the keyboard (2.1.1). Tab order = reading order. No positive `tabindex`. |
| Focus | Visible indicator (2.4.7). Do not `outline: none` without an equal replacement. Focus not entirely hidden by sticky chrome (2.4.11). Move focus into dialogs; restore on close. |
| Name | Every control has an accessible name; every input a programmatic label (placeholder is not a label). Informative images have `alt`; decorative `alt=""`. |
| Contrast | Text 4.5:1 (3:1 large). UI/focus 3:1 (1.4.3, 1.4.11). Meaning is never color alone. |
| Target | Pointer targets ≥ 24×24 CSS px (2.5.8) unless an existing kit documents otherwise. |
| Motion | Honor `prefers-reduced-motion`. Do not animate high-frequency / keyboard-driven actions. |
| States | Loading, empty, error, disabled — each has UI (heuristic 1, 9). Destructive actions have a way out (heuristic 3). |
| Consistency | Use the host UI kit. Do not invent a second button, radius, or type scale (heuristic 4). |
| Content | Real copy, not lorem. User language, not internal jargon (heuristic 2). |
| Layout | Follow the host breakpoints. If silent: mobile-first, don't skip heading levels. |
| Anti-slop | No generic "AI look": purple/indigo-by-default, oversized rounding, gradient soup, stock card grids, shadow stacks — unless the host design actually uses them (`repo:`). |

If the host UI kit contradicts a taste heuristic (e.g. dense admin tables, terminal aesthetic), the kit wins. WCAG floors still apply.

## Theming

Apply **only if a UI exists**. Source of truth: [DTCG](https://www.designtokens.org/) — primitive/base → alias/semantic → component.

- Point at the host token file (CSS variables, JS tokens, DTCG JSON, Tailwind theme). That file is rank 2.
- Components consume **semantic** tokens (`color-text`, `space-4`), not raw hex / `px` / font names.
- Add a token in the token file first; never one-off color/type/space in a component when tokens exist.
- Light/dark (and other themes) are token-set swaps, not scattered component overrides. If the host has only one theme, do not invent dark mode.
- Contrast is verified on the semantic tokens, not ad hoc per component.
- If the host has no tokens yet, do **not** invent a design system. Gap-fill: CSS custom properties on `:root` for the colors/spacing already used, and say so as `golden:` with Gotchas "no token file yet".

## Testing

- File names and locations match the host (`repo:`).
- Test observable behavior, not private implementation.
- Do not delete failing tests to go green.
- If silent on unit vs e2e: write unit tests at seams the host already tests; do not add a new e2e framework.
- Commands in `AGENTS.md` are the ones CI runs.

## Security

Source: current [OWASP Top 10](https://owasp.org/www-project-top-ten/) as a floor, not a lecture.

Always (if the stack can hit the risk): validate at the trust boundary; parameterize queries; encode output (XSS); hash passwords with a modern KDF; HTTPS; no secrets in git; no secrets in logs.

Never Canonical a vulnerability "because the code does it". Gotchas + Never instead.

## Git / services

- Git workflow: only document what the host already uses (CONTRIBUTING, commitlint, PR template). If **fully silent**, Conventional Commits is the golden default — one line, not a tutorial.
- Do not impose Gitflow or trunk-based on a repo that already chose the other.
- 12-factor applies to **services** (config in env, explicit deps, logs as streams). Do not dump all twelve into a static site or CLI.
