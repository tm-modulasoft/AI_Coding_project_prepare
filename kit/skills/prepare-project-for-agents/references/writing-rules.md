# Writing rules

Always-on context is expensive. Split ruthlessly.

## Precedence (host files)

When a golden default and a project standard disagree, **do not average**. See `references/golden-rules.md`.

| Winner                     | When                                                                                                                        |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| User chat                  | Always                                                                                                                      |
| Host project (`repo:`)     | Taste, architecture, stack, naming, formatting, tokens — including when **stricter or more specific** than a golden default |
| Golden default (`golden:`) | Host is silent **and** the rule applies to this stack                                                                       |
| Safety / a11y floor        | Never Canonical-ize a dangerous or inaccessible habit; list it under Gotchas                                                |

Golden rules are a **default floor**, not a ceiling and not a second style guide. Do not copy `golden-rules.md` into the host.

## Destinations

| Destination        | What belongs                                                                                                               | Test                                                                                            |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Root `AGENTS.md`   | Map, stack one-liner, **Precedence**, exact commands, **Tools**, seams, three-tier boundaries, git/PR one-liners, pointers | Would removing this line cause a mistake on _most_ tasks?                                       |
| Helper file        | Recurring but task-type-specific: language, UI/UX, theming, testing, security, Excluded/Canonical                          | Recurs when that area is touched, not every task                                                |
| Nested `AGENTS.md` | Package-specific commands, boundaries, seams                                                                               | Root file would mislead work inside that package                                                |
| Vendor shim        | One-line import / glob pointer                                                                                             | Tool cannot see `AGENTS.md` otherwise                                                           |
| Native rule        | House workflow + default tools from kit `harness-defaults/ai-coding-native-rules.md`                                       | `.cursor/rules/ai-coding-native-rules.mdc` with `alwaysApply: true` — not a copy of `AGENTS.md` |
| Delete             | Slogans, restated linter rules, aspirational "we should", duplicated README, inapplicable golden rules                     | Would not change agent behavior                                                                 |

## Size

- Root `AGENTS.md`: aim <250 lines, hard cap ~400
- Each helper: aim <120 lines, one concern
- Cursor `.mdc`: <50 lines, one concern, globbed (`alwaysApply: false`) — **except** harness `ai-coding-native-rules.mdc` (`alwaysApply: true`). Do not also dump that body into `AGENTS.md`.

## Voice

- State the **choice**, not the virtue: `derive types with z.infer<>` not "type safety is critical"
- **Examples beat prose.** One real snippet from _this_ repo > three paragraphs
- Commands include **exact CLIs and flags**, near the top of `AGENTS.md`
- Boundaries: **Always / Ask first / Never**
- Brownfield **project** rules: every `repo:` bullet points at a proving path. If it cannot, omit it
- Brownfield **golden** gap-fills: point at the named standard (`golden: WCAG 2.2 AA`). Omit if the stack cannot hit that domain
- Do not narrate what ESLint/Prettier already forbids unless agents still violate it
- Token-driven UI: extend tokens; never one-off hex/font/space in components if tokens exist
- Mark sources: `repo:` / `golden:` / `overridden:`

## Official AGENTS.md facts

- Filename `AGENTS.md` (uppercase, plural)
- Plain Markdown; no required headings. Optional YAML `description` / `tags` for indexing only
- Closest `AGENTS.md` to the edited file wins; user chat overrides files
- README stays human-facing. Agent ops live in `AGENTS.md` + helpers
- Spec: https://agents.md (AAIF / Linux Foundation)

## Quality checklist

- [ ] `AGENTS.md` at host root, uppercase plural
- [ ] Precedence section present (chat > repo > golden > floors)
- [ ] Commands are the real ones (script/CI; smoke-run test/lint if cheap)
- [ ] Every Always/Ask/Never item is specific and checkable
- [ ] Helpers linked from Pointers; no orphans
- [ ] Helper bullets tagged `repo:` / `golden:` / `overridden:`
- [ ] Golden fills do not contradict formatter, compiler, or token file
- [ ] Safety/a11y floors not encoded as Canonical-from-a-bad-habit
- [ ] No secrets copied into agent files
- [ ] UI/theming helpers exist **iff** a UI exists
- [ ] Cursor rules are globbed, not always-on copies of `AGENTS.md` (ai-coding-native-rules.mdc is the allowed always-on exception)
- [ ] `AGENTS.md` has a short Tools stanza (Context7, Sonatype, `gh`) even if native-rules exist
- [ ] Host `.gitignore` ignores `.agents/skills/` and `skills-lock.json` is committed if present
- [ ] `CLAUDE.md` is an import, not a second bible
- [ ] Existing human rules were merged, not clobbered
