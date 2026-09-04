# AI coding notes

Short notes for people working in this repo after it was prepared for agentic coding. **Agents read `AGENTS.md`.** This file is for you.

**Start here (mandatory):** intros and tutorials are in `AI_CODING_LEARN.md`. Watch/read that before you treat the agent as a process you already know.

> A skill you haven't read is just a longer prompt you don't control. Read them anyway.

House workflow skills: [github.com/coleam00/skills](https://github.com/coleam00/skills). Installed copies: `.agents/skills/<name>/SKILL.md`.

The workflow section below is a working default. Tighten it later if the team wants a different process.

## First clone (or empty `.agents/skills/`)

From the repo root, restore skills (they are gitignored; `skills-lock.json` is committed):

```bash
npx skills experimental_install --yes
```

Needs network. Then open the project in Cursor, install any missing marketplace plugins, reload, and connect Context7 / Sonatype keys in Customize. Never commit keys.

## Workflow

**prime → plan → implement → validate → review → commit → PR**

Around that loop sit the pieces that feed it (PRD, architecture, epic slicing), the pieces that run it in parallel (worktrees), and the meta-skills that let you build more of your own AI Layer (rules, hooks, skills, opportunity scans).

**Simple** — typo, rename, obvious one-file fix:

chat spec → implement → verify (test / lint / typecheck; browser if UI)

**Full** — feature, ticket, non-trivial change:

spec → tasks → implement → verify

That maps to coleam00 skills (do not invent a parallel process). Names only — open each `SKILL.md` ([coleam00/skills](https://github.com/coleam00/skills) or `.agents/skills/<name>/SKILL.md`) before you rely on it:

| Kind              | Skills                                                                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Product intent    | `plan-create-prd`                                                                                                                              |
| Approach / stack  | `plan-architecture`                                                                                                                            |
| Tickets           | `plan-create-stories`                                                                                                                          |
| Feature / ticket  | `prime-codebase` (or `prime-frontend` / `prime-backend`) → `piv-plan-implementation` → `piv-implement` → `piv-validate` → `piv-review-changes` |
| Bug with an issue | `piv-investigate-issue` → `piv-implement-issue`                                                                                                |
| Commit / PR       | `piv-commit` / `piv-create-pr` only inside that loop, or when you ask                                                                          |

Skip the PIV loop for tiny/mechanical work. During implement: surgical diffs (`karpathy-guidelines`). Project commands (dev, test, lint, build) live in `AGENTS.md`.

## Skills

Run from the **repo root**. Installs land in `.agents/skills/` (gitignored). Commit `skills-lock.json` after add/update.

| What                                                         | Command                                 |
| ------------------------------------------------------------ | --------------------------------------- |
| Restore from the lockfile (clone, new machine, wiped folder) | `npx skills experimental_install --yes` |
| See whether installed skills have updates                    | `npx skills check`                      |
| Apply updates                                                | `npx skills update --yes`               |
| List what is installed                                       | `npx skills list`                       |

**This kit uses only three Matt Pocock skills** (not the whole catalog):

```bash
npx skills add mattpocock/skills --skill improve-codebase-architecture research codebase-design --yes
```

**New skills from a whole collection** (not day-to-day restore — use when those repos added skills you do not have yet):

```bash
npx skills add emilkowalski/skills --yes
npx skills add coleam00/skills --yes
```

Then commit the updated `skills-lock.json`.

Browse more: [skills.sh](https://skills.sh/). Search: `npx skills find [query]`.

## Cursor plugins (once per machine)

No install CLI. If a plugin is missing, paste in Cursor chat and reload:

```
/add-plugin cursor-team-kit
/add-plugin context7-plugin
/add-plugin sonatype-cursor-plugin
/add-plugin modern-web-guidance
```

Connect Context7 and Sonatype in Customize. `"key": true` in settings means “connect in the UI,” not “put the secret in git.”

## Tools the agent should use

- Library / API docs: Context7 MCP (not training memory or generic web search)
- New or upgraded packages: Sonatype MCP before pinning
- GitHub: `gh` CLI only (no GitHub MCP)

## Do not commit

- Plugin API keys, `.env`, credentials
- `.agents/skills/` — restore with `npx skills experimental_install --yes`

## Map

| Path                                       | Role                                            |
| ------------------------------------------ | ----------------------------------------------- |
| `AGENTS.md`                                | Always-on spine for agents                      |
| `docs/agents/`                             | On-demand depth (stack, conventions, UI, tests) |
| `skills-lock.json`                         | Pinned skill sources — commit this              |
| `.cursor/rules/ai-coding-native-rules.mdc` | Cursor house workflow + tools, every chat       |
| `AI_CODING_LEARN.md`                       | Mandatory intros and tutorials                  |
| This file                                  | Human cheat sheet                               |
