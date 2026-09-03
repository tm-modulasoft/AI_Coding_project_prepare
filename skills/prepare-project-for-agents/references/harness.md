# Harness (step 1)

Do this **before** writing `AGENTS.md`. The harness is the runtime: IDE + CLI defaults, plugins/MCPs, project skills, and always-on native rules. Step 2 (`instruction-prompt.md`) is the project instruction layer.

Kit root contains `START_HERE.html` and `one_offs/`. Write into the **host** git toplevel, not into the kit folder when the kit is nested.

Do not commit. Do not write API keys, tokens, or plugin secrets.

## Goal

A teammate who clones the host can:

1. Install the default Cursor plugins (`/add-plugin …`).
2. Restore skills with `npx skills experimental_install`.
3. Get house workflow + tool defaults injected every chat (native rule).
4. Then run step 2 (or find `AGENTS.md` already written in the same prepare run).

## Non-goals

- Do not overwrite a richer host `skills-lock.json` or a host `.cursor/settings.json` plugin list the user already tuned — merge, keep extras.
- Do not write global `~/.cursor/cli-config.json` or Cursor User Rules unless the user asked. Those are machine-personal.
- Do not duplicate `one_offs/native-rules.md` into `AGENTS.md`. Step 2 writes a **short** portable Tools stanza so non-Cursor agents still prefer Context7 and Sonatype.

## Source files (kit)

| Kit path | Host destination |
|---|---|
| `one_offs/skills-lock.json` | `skills-lock.json` (git root) |
| `one_offs/native-rules.md` | `.cursor/rules/native-rules.mdc` (`alwaysApply: true`) |
| `templates/cursor-settings.json` | merge into `.cursor/settings.json` |
| `templates/cli.json` | `.cursor/cli.json` if missing or empty of `permissions` |

## Human-only vs agent-run

| Action | Who |
|---|---|
| Write/merge the files below | Agent |
| `/add-plugin cursor-team-kit` (and the other three) | Human, in Cursor chat — first install |
| Connect plugin keys (Context7, Sonatype) in Customize | Human — never commit keys |
| `npx skills experimental_install` at host root | Agent should **run** it (needs network). If it fails, put the command in the report for the human |
| Paste native-rules into Cursor **User Rules** (optional, all projects) | Human |

## Files to produce

### 1. `.cursor/settings.json` (IDE client defaults)

Merge `plugins` from `templates/cursor-settings.json`. Preserve every other key already in the file.

`"key": true` means the plugin needs a user-supplied credential in the Cursor UI. Do **not** put the credential in git.

After the file exists, the human still has to **install** marketplace plugins once:

```
/add-plugin cursor-team-kit
/add-plugin context7-plugin
/add-plugin sonatype-cursor-plugin
/add-plugin modern-web-guidance
```

Same names as Customize → Marketplace. Reload the window after install. Settings `enabled: true` only applies once the plugin is installed.

### 2. `.cursor/cli.json` (CLI client, project layer)

Cursor CLI project overrides are **permissions only** (see Cursor CLI configuration). Global prefs stay in `~/.cursor/cli-config.json`.

If the host has no `.cursor/cli.json`, copy `templates/cli.json`. If it already has `permissions`, leave them unless they are empty and the template is stricter in a useful way — do not wipe a hand-tuned allow/deny list.

### 3. `skills-lock.json` + first skill install

If the host has no `skills-lock.json`, copy `one_offs/skills-lock.json` verbatim.

If the host already has one, keep it. Report that the kit default was skipped.

Then from the **host git root**:

```bash
npx skills experimental_install
```

That restores skills into `.agents/skills/` (canonical project install). Other agent folders may symlink or copy from there.

`one_offs/native-rules.md` may name extra **global** workflow skills (for example coleam00 / PIV). Those are not in the project lock. Do not add them to `skills-lock.json` unless the user asks.

### 4. `.gitignore`

Ensure this block exists (create the file if needed; otherwise append if missing):

```
# Installed agent skills (restore with: npx skills experimental_install)
.agents/skills/
```

Do not gitignore `skills-lock.json`.

### 5. Native rules (pre-prompt / always-on)

Cursor injects `.cursor/rules/*.mdc` with `alwaysApply: true` into every chat — that is the project-level equivalent of a pre-prompt. User Rules in Customize are global and cannot be set from the repo.

1. Read kit `one_offs/native-rules.md`.
2. Write host `.cursor/rules/native-rules.mdc`:

```markdown
---
description: House workflow and default tools (always on)
alwaysApply: true
---
```

Then the native-rules body **verbatim** (no extra commentary).

If that path already exists, backup to `native-rules.mdc.bak` and merge: keep human steering that is still true; replace kit-owned sections that drifted.

This file is **not** a second `AGENTS.md`. It is house process + tool routing. Project map, commands, and seams stay in step 2.

## Other clients

Skills CLI’s project install is the shared layer for Cursor IDE, Cursor CLI, Claude Code, Codex, and others that read `.agents/skills/` (or a symlink). Do not invent a second plugin marketplace for those tools in this step.

Optional human: paste `one_offs/native-rules.md` into Cursor Customize → Rules → User Rules so the same house rules apply in every repo.

## After harness files

List in the report: created/merged paths, whether `experimental_install` succeeded, and the `/add-plugin` lines the human must run. Then continue to step 2 unless the user asked for harness only.
