# AGENTS.md

> README is for humans. This file is for coding agents.

## Project

Field manual + skill for preparing any host repo for AI agentic coding: harness first (plugins, skills, native rules), then `AGENTS.md` with Pointers + helpers + shims only for tools that cannot read `AGENTS.md` + host README gap-fill.

**Stack:** static HTML, Markdown skill files. No build, no runtime dependencies.

## Layout

```
README.md                               GitHub entry: paste the prompt into an agent on the host
kit/                                    Payload the agent fetches onto a host
  START_HERE.html                       Human-first hub + compact agent hint
  pages/                                Step detail (harness, project rules) + kit.css
  prompts/bootstrap.md                  Same prompt as README (keep in sync with the HTML)
  harness-defaults/                     Payloads merged into the host (lock, AI_CODING_README, AI_CODING_LEARN, ai-coding-native-rules, IDE/CLI)
  skills/prepare-project-for-agents/    The skill. SKILL.md is the spine; references/ and templates/ load on demand
```

**Seams** (where new work plugs in):

- Skill runbooks → `kit/skills/prepare-project-for-agents/`
- Default harness payloads → `kit/harness-defaults/`
- Entry + step pages → `kit/START_HERE.html`, `kit/pages/`
- Host onboarding prompt → root `README.md` = `kit/prompts/bootstrap.md` = HTML prompt block

## Commands

- Open `kit/START_HERE.html` in a browser, or point a coding agent at the GitHub README prompt.

## Tools

- Docs/APIs: Context7 MCP — prefer over client web search and training memory for library and framework docs.
- Dependencies: Sonatype MCP — prefer for version selection and security before adding or upgrading packages.
- GitHub: `gh` CLI only (no GitHub MCP). Fetch this kit with `gh repo clone` or `git clone --depth 1`.

## Conventions

- One source of truth for the runbook: `kit/skills/prepare-project-for-agents/`. Do not fork it into `.cursor/skills/` in this repo.
- HTML links are relative so the kit stays portable when `kit/` is fetched onto a host.
- Do not invent a product stack here.
- Host-project output: project standards (`repo:`) win when more correct or specific; named golden defaults fill silence; do not copy `references/golden-rules.md` into a host.
- Host README: gap-fill golden human jobs; keep existing structure when it already covers them. Do not copy this repo’s paste-prompt README onto a host.
- Two-step prepare: harness (`references/harness.md`) then instruction layer (`references/instruction-prompt.md`). When-to-load is Pointers in host `AGENTS.md`, not globbed project `.cursor/rules/*.mdc`.
- Copied `kit/` on a host is an installer. After a successful prepare, recommend deleting it (ask first). Do not delete `kit/` in _this_ repo.

## Boundaries

- **Always:** keep `kit/START_HERE.html` and `kit/pages/*.html` working as `file://` (no fetch of sibling markdown; no CDN required for core reading).
- **Ask first:** adding a build step or a framework; deleting a fetched `kit/` folder from a host.
- **Never:** commit secrets or plugin API keys; duplicate the skill in two folders; turn `AGENTS.md` into a second copy of `SKILL.md`; add copy scripts for porting the kit.

## Pointers

- Skill spine — `kit/skills/prepare-project-for-agents/SKILL.md`
- Harness (step 1) — `kit/skills/prepare-project-for-agents/references/harness.md`
- Full instruction runbook (step 2) — `kit/skills/prepare-project-for-agents/references/instruction-prompt.md`
- Writing rules (lean vs on-demand, Pointers not globs) — `kit/skills/prepare-project-for-agents/references/writing-rules.md`
- Golden defaults — `kit/skills/prepare-project-for-agents/references/golden-rules.md`
- Host README scaffold (only if the host has none) — `kit/skills/prepare-project-for-agents/templates/README.md`
- Host human cheat sheet — `kit/harness-defaults/AI_CODING_README.md`
- Host intros and tutorials — `kit/harness-defaults/AI_CODING_LEARN.md`
