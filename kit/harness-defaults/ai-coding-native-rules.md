# Workflow owner

Coding work follows coleam00 skills. Do not invent a parallel process.

- Feature / ticket: prime-codebase (or prime-frontend / prime-backend) → piv-plan-implementation → piv-implement → piv-validate → piv-review-changes. Commit/PR only via piv-commit / piv-create-pr when that loop is running, or when I ask.
- Product intent: plan-create-prd. Approach/stack: plan-architecture. Tickets: plan-create-stories.
- Bug with an issue: piv-investigate-issue then piv-implement-issue.
- Tiny/mechanical (rename, typo, one-liner): skip the loop; still don't commit unless I ask.
- During implement: surgical diffs, simplest thing that works (karpathy-guidelines). Don't start ponytail-style "skip the task".

# Tools (always)

- Docs/APIs: Context7 MCP. GitHub: gh CLI only (no GitHub MCP). Deps: Sonatype MCP.
- UI from Figma: FigmaLocal MCP + the Figma MCP flow, token-driven layout. Only when I gave a Figma URL/node or asked to implement a design.

# Stance

- Ask when structure, API, or scope is unclear. Don't guess irreversible calls.
- Don't rewrite spec/source docs; update plan files if direction changes.
- Subagents: large explore / parallel research / PIV fan-out only.
- Tests: follow the plan. If the plan is silent, unit tests only (fixtures *.data.ts / *.spec.data.ts, mocks *.mock.ts). No e2e unless the plan or I ask. User-visible UI: also verify in the browser.
- After changes: lint/typecheck/build the touched surface (skip tasks/, docs/, .md).
