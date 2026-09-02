# Writing rules

Always-on context is expensive. Split ruthlessly.

## Destinations

| Destination | What belongs | Test |
|---|---|---|
| Root `AGENTS.md` | Map, stack one-liner, exact commands, seams, three-tier boundaries, git/PR one-liners, pointers | Would removing this line cause a mistake on *most* tasks? |
| Helper file | Recurring but task-type-specific: language, UI/UX, theming, testing, security, Excluded/Canonical | Recurs when that area is touched, not every task |
| Nested `AGENTS.md` | Package-specific commands, boundaries, seams | Root file would mislead work inside that package |
| Vendor shim | One-line import / glob pointer | Tool cannot see `AGENTS.md` otherwise |
| Delete | Slogans, restated linter rules, aspirational "we should", duplicated README | Would not change agent behavior |

## Size

- Root `AGENTS.md`: aim <250 lines, hard cap ~400
- Each helper: aim <120 lines, one concern
- Cursor `.mdc`: <50 lines, one concern, globbed (`alwaysApply: false`)

## Voice

- State the **choice**, not the virtue: `derive types with z.infer<>` not "type safety is critical"
- **Examples beat prose.** One real snippet from *this* repo > three paragraphs
- Commands include **exact CLIs and flags**, near the top of `AGENTS.md`
- Boundaries: **Always / Ask first / Never**
- Brownfield: every rule points at a proving path. If it cannot, omit it
- Do not narrate what ESLint/Prettier already forbids unless agents still violate it
- Token-driven UI: extend tokens; never one-off hex/font/space in components if tokens exist

## Official AGENTS.md facts

- Filename `AGENTS.md` (uppercase, plural)
- Plain Markdown; no required headings. Optional YAML `description` / `tags` for indexing only
- Closest `AGENTS.md` to the edited file wins; user chat overrides files
- README stays human-facing. Agent ops live in `AGENTS.md` + helpers
- Spec: https://agents.md (AAIF / Linux Foundation)

## Quality checklist

- [ ] `AGENTS.md` at host root, uppercase plural
- [ ] Commands are the real ones (script/CI; smoke-run test/lint if cheap)
- [ ] Every Always/Ask/Never item is specific and checkable
- [ ] Helpers linked from Pointers; no orphans
- [ ] No secrets copied into agent files
- [ ] UI/theming helpers exist **iff** a UI exists
- [ ] Cursor rules are globbed, not always-on copies of `AGENTS.md`
- [ ] `CLAUDE.md` is an import, not a second bible
- [ ] Existing human rules were merged, not clobbered
