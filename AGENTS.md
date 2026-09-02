# AGENTS.md

> This repository is the **portable kit**, not a product app.

## Project

Field manual + skill for preparing any host repo for AI agentic coding (`AGENTS.md` + helpers + shims).

**Stack:** static HTML, Markdown skill files. No build, no runtime dependencies.

## Layout

```
START_HERE.html                         Human + agent entry (open in a browser or Read from disk)
prompts/bootstrap.md                    Copy-paste kickoff prompt (keep in sync with the HTML)
skills/prepare-project-for-agents/      The skill. SKILL.md is the spine; references/ and templates/ load on demand
```

**Seams:** edit the skill under `skills/prepare-project-for-agents/`. Edit the entry page `START_HERE.html` and keep `prompts/bootstrap.md` identical to the HTML prompt block.

## Commands

None. Open `START_HERE.html` in a browser, or point a coding agent at it.

## Conventions

- One source of truth for the runbook: `skills/prepare-project-for-agents/`. Do not fork it into `.cursor/skills/` in this repo.
- HTML links are relative so the kit stays portable when copied.
- Do not invent a product stack here.

## Boundaries

- **Always:** keep `START_HERE.html` working as `file://` (no fetch of sibling markdown; no CDN required for core reading).
- **Ask first:** adding a build step or a framework.
- **Never:** commit secrets; duplicate the skill in two folders; turn `AGENTS.md` into a second copy of `SKILL.md`.

## Pointers

- Skill spine — `skills/prepare-project-for-agents/SKILL.md`
- Full runbook — `skills/prepare-project-for-agents/references/instruction-prompt.md`
