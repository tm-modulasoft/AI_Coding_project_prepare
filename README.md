# AI Coding kit

Portable field manual for preparing a repository for LLM / agent-harness coding.

Open **[START_HERE.html](START_HERE.html)** in a browser (double-click the file). That page is the human entry. The HTML comment at the top is the compact agent hint.

## Two steps

1. **Harness** — IDE/CLI defaults, Cursor plugins, skills lock, always-on native rules. Details: [pages/harness.html](pages/harness.html).
2. **Project rules** — lean `AGENTS.md`, helpers, vendor shims. Details: [pages/prepare-project.html](pages/prepare-project.html).

## What this is

A copyable kit:

| Piece | Role |
|---|---|
| `START_HERE.html` | Open in a browser. **For humans** is the visual start. Copy the bootstrap prompt into a coding agent. |
| `pages/` | Step detail pages (harness, project rules) plus shared `kit.css`. |
| `prompts/bootstrap.md` | The same bootstrap prompt, as plain Markdown. |
| `skills/prepare-project-for-agents/` | The skill the agent must Read and execute. |
| `one_offs/skills-lock.json` | Default general + agentic-coding skills lock (copied to the host as `skills-lock.json`). |
| `one_offs/native-rules.md` | Body for Cursor always-on `native-rules.mdc`. |

Step 2 writes a lean [`AGENTS.md`](https://agents.md) spine, on-demand helpers, and thin vendor shims.

Written rules **default to globally accepted golden standards** (WCAG 2.2 AA, DTCG tokens, official language guides, OWASP floors, AGENTS.md spec) only where the host is silent. The project's own code, linters, and tokens win when they are more correct or specific. Safety and accessibility floors are not encoded as "the project does it wrong, so copy that."

## Use on another project

Copy these together (relative links must stay intact):

```
START_HERE.html
prompts/
skills/
one_offs/
pages/
```

Place them at the target repo root, or in a subfolder (`agent-prep/`, `_AI_Coding/`, …). Then open `START_HERE.html` from that copy.

## Maintainers

- Skill spine: `skills/prepare-project-for-agents/SKILL.md`
- Harness runbook: `skills/prepare-project-for-agents/references/harness.md`
- Golden defaults + precedence: `skills/prepare-project-for-agents/references/golden-rules.md`
- Keep the prompt block in `START_HERE.html` identical to `prompts/bootstrap.md`
- This repo has no build step. The HTML must work as `file://`
