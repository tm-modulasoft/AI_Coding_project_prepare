# AI Coding kit

Portable field manual for preparing a repository for LLM / agent-harness coding.

Open **[START_HERE.html](START_HERE.html)** in a browser (double-click the file). That page is the human runbook and the agent entry point.

## What this is

A copyable kit:

| Piece | Role |
|---|---|
| `START_HERE.html` | Open in a browser to read what to do. Click **Open skill (raw)** to view `SKILL.md`. Copy the bootstrap prompt and paste it into a coding agent. |
| `prompts/bootstrap.md` | The same bootstrap prompt, as plain Markdown. |
| `skills/prepare-project-for-agents/` | The skill the agent must Read and execute. |

It writes a lean [`AGENTS.md`](https://agents.md) spine, on-demand helpers (stack, conventions, Excluded / Canonical / Language, UI/UX, theming), and thin vendor shims.

Written rules **default to globally accepted golden standards** (WCAG 2.2 AA, DTCG tokens, official language guides, OWASP floors, AGENTS.md spec) only where the host is silent. The project's own code, linters, and tokens win when they are more correct or specific. Safety and accessibility floors are not encoded as "the project does it wrong, so copy that."

## Use on another project

Copy these three together (relative links must stay intact):

```
START_HERE.html
prompts/
skills/
```

Place them at the target repo root, or in a subfolder (`agent-prep/`, `_AI_Coding/`, …). Then open `START_HERE.html` from that copy.

## Maintainers

- Skill spine: `skills/prepare-project-for-agents/SKILL.md`
- Golden defaults + precedence: `skills/prepare-project-for-agents/references/golden-rules.md`
- Keep the prompt block in `START_HERE.html` identical to `prompts/bootstrap.md`
- This repo has no build step. The HTML must work as `file://`
