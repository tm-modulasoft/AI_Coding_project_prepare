# AI Coding kit

Portable field manual for preparing a repository for LLM / agent-harness coding.

## Use on another project

1. Open the **local project** you want prepared in your coding agent (Cursor, etc.).
2. Paste this prompt as the first message:

```
Prepare this project for AI agentic coding.

1. If this workspace has no `kit/skills/prepare-project-for-agents/SKILL.md`, fetch `kit/` from https://github.com/tm-modulasoft/AI_Coding_project_prepare into this repo as `kit/`. Shallow-clone into a temp dir (`gh repo clone tm-modulasoft/AI_Coding_project_prepare .ai-coding-kit-src -- --depth 1` or `git clone --depth 1 https://github.com/tm-modulasoft/AI_Coding_project_prepare.git .ai-coding-kit-src`), copy only `.ai-coding-kit-src/kit` → `kit/`, delete the temp dir. Do not copy the kit repo's README, AGENTS.md, or .git.
2. Read `kit/START_HERE.html`, then execute `kit/skills/prepare-project-for-agents/SKILL.md`. Two steps, in order — do not skip the harness.
3. Host = this repo's git root. Write harness files and `AGENTS.md` into the host, not into `kit/`.
4. Harness: copy `kit/harness-defaults/skills-lock.json` to the host if missing. You MUST restore skills from that lock (GitHub sources) with `npx skills experimental_install --yes` at the host root. Marketplace plugins have no CLI — after writing `.cursor/settings.json`, tell me to paste `/add-plugin …` for any plugin not already installed on this machine, then reload.
5. Then write AGENTS.md (with Pointers to helpers) / helpers / shims only for tools that cannot read AGENTS.md, and gap-fill the host README (create if missing; do not fight a more correct existing structure), from `instruction-prompt.md` + `golden-rules.md`. Do not write globbed project Cursor `.mdc` rules. Do not invent conventions. Project standards win when more correct or specific. Do not commit unless asked.
6. After both steps: if you fetched `kit/` in this run, ask before deleting it (recommended: yes — installer). Keep harness files + AGENTS.md + host README. Do not delete `kit/` if this git remote is `tm-modulasoft/AI_Coding_project_prepare`.

Do not ask me to paste the skill. Load it from disk and run it.
```

The agent pulls [`kit/`](https://github.com/tm-modulasoft/AI_Coding_project_prepare/tree/main/kit) from this GitHub repo, restores project skills from GitHub via [`skills-lock.json`](https://github.com/tm-modulasoft/AI_Coding_project_prepare/blob/main/kit/harness-defaults/skills-lock.json), then writes the harness, `AGENTS.md`, and a gap-fill of the host `README.md`.

You may still need to paste `/add-plugin …` in Cursor chat if those marketplace plugins are not already on the machine (no install CLI).

Same prompt: [`kit/prompts/bootstrap.md`](kit/prompts/bootstrap.md).

## After setup, delete the copy?

**Yes**, if the agent fetched `kit/` into another project only to run prepare. It is an installer. Keep `.cursor/` (harness native-rules + settings), `skills-lock.json`, `AI_CODING_README.md`, `AI_CODING_LEARN.md`, `AGENTS.md`, the host `README.md`, helpers, and shims for tools that cannot read `AGENTS.md`.

**No**, do not delete `kit/` inside _this_ repository.

## Two steps (what the agent runs)

1. **Harness** — IDE/CLI defaults, Cursor plugins, skills lock, always-on native rules. Skills restore is `npx skills experimental_install --yes` (GitHub sources in the lockfile). Details: [kit/pages/harness.html](kit/pages/harness.html).
2. **Project rules** — lean `AGENTS.md` with Pointers, helpers, shims only for tools that cannot read `AGENTS.md`, and a gap-fill of the host `README.md` (create if missing; keep existing structure when it already covers the jobs). Details: [kit/pages/prepare-project.html](kit/pages/prepare-project.html).

Working in _this_ clone: open [kit/START_HERE.html](kit/START_HERE.html).

## What this is

| Piece                                    | Role                                                                                                                                                                  |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `kit/`                                   | Payload the agent fetches onto a host                                                                                                                                 |
| `kit/START_HERE.html`                    | Local hub after fetch. **For humans** plus a compact agent hint.                                                                                                      |
| `kit/pages/`                             | Step detail (harness, project rules) plus `kit.css`.                                                                                                                  |
| `kit/prompts/bootstrap.md`               | Same prompt as this README. Keep them identical.                                                                                                                      |
| `kit/skills/prepare-project-for-agents/` | The skill the agent must Read and execute.                                                                                                                            |
| `kit/harness-defaults/`                  | Host payloads: skills lock (GitHub skill sources), `AI_CODING_README.md`, `AI_CODING_LEARN.md`, `ai-coding-native-rules.md`, Cursor plugin settings, CLI permissions. |

Step 2 writes a lean [`AGENTS.md`](https://agents.md) spine (when-to-load Pointers, not globbed Cursor project rules), on-demand helpers, thin shims only for tools that cannot read `AGENTS.md`, and a gap-fill of the host [`README.md`](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes) (human jobs: what / why / how to start / help / license — not a forced heading rename).

Written rules **default to globally accepted golden standards** (WCAG 2.2 AA, DTCG tokens, official language guides, OWASP floors, AGENTS.md spec) only where the host is silent. The project's own code, linters, and tokens win when they are more correct or specific. Safety and accessibility floors are not encoded as "the project does it wrong, so copy that."

## Maintainers

- Skill spine: `kit/skills/prepare-project-for-agents/SKILL.md`
- Harness runbook: `kit/skills/prepare-project-for-agents/references/harness.md`
- Golden defaults + precedence: `kit/skills/prepare-project-for-agents/references/golden-rules.md`
- Keep the prompt in this README identical to `kit/prompts/bootstrap.md` and the prompt block in `kit/START_HERE.html`
- This repo has no build step. The HTML must work as `file://`
