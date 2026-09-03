# AGENTS.md

> This repository is the **portable kit**, not a product app.

## Project

Field manual + skill for preparing any host repo for AI agentic coding: harness first (plugins, skills, native rules), then `AGENTS.md` + helpers + shims.

**Stack:** static HTML, Markdown skill files. No build, no runtime dependencies.

## Layout

```
START_HERE.html                         Human-first hub + compact agent hint (open in a browser or Read from disk)
pages/                                  Step detail (harness, project rules) + kit.css
prompts/bootstrap.md                    Copy-paste kickoff prompt (keep in sync with the HTML)
one_offs/skills-lock.json               Default skills lock copied to the host
one_offs/native-rules.md                Body for always-on native-rules.mdc
skills/prepare-project-for-agents/      The skill. SKILL.md is the spine; references/ and templates/ load on demand
```

**Seams** (where new work plugs in):

- Skill runbooks → `skills/prepare-project-for-agents/`
- Default harness payloads → `one_offs/`
- Entry + step pages → `START_HERE.html`, `pages/` (keep `prompts/bootstrap.md` identical to the HTML prompt block)

## Commands

None. Open `START_HERE.html` in a browser, or point a coding agent at it.

## Tools

- Docs/APIs: Context7 MCP — prefer over client web search and training memory for library and framework docs.
- Dependencies: Sonatype MCP — prefer for version selection and security before adding or upgrading packages.
- GitHub: `gh` CLI only (no GitHub MCP).

## Conventions

- One source of truth for the runbook: `skills/prepare-project-for-agents/`. Do not fork it into `.cursor/skills/` in this repo.
- HTML links are relative so the kit stays portable when copied.
- Do not invent a product stack here.
- Host-project output: project standards (`repo:`) win when more correct or specific; named golden defaults fill silence; do not copy `references/golden-rules.md` into a host.
- Two-step prepare: harness (`references/harness.md`) then instruction layer (`references/instruction-prompt.md`).

## Boundaries

- **Always:** keep `START_HERE.html` and `pages/*.html` working as `file://` (no fetch of sibling markdown; no CDN required for core reading).
- **Ask first:** adding a build step or a framework.
- **Never:** commit secrets or plugin API keys; duplicate the skill in two folders; turn `AGENTS.md` into a second copy of `SKILL.md`.

## Pointers

- Skill spine — `skills/prepare-project-for-agents/SKILL.md`
- Harness (step 1) — `skills/prepare-project-for-agents/references/harness.md`
- Full instruction runbook (step 2) — `skills/prepare-project-for-agents/references/instruction-prompt.md`
- Golden defaults — `skills/prepare-project-for-agents/references/golden-rules.md`
