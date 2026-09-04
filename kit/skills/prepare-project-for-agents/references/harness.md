# Harness (step 1)

Do this **before** writing `AGENTS.md`. The harness is the runtime: IDE + CLI defaults, plugins/MCPs, project skills, and always-on native rules. Step 2 (`instruction-prompt.md`) is the project instruction layer.

Kit root contains `START_HERE.html` and `harness-defaults/`. Write into the **host** git toplevel, not into the kit folder when the kit is nested.

Do not commit. Do not write API keys, tokens, or plugin secrets.

## Goal

A teammate who clones the host can:

1. Restore skills from GitHub via `skills-lock.json` (`npx skills experimental_install --yes` — agent already ran this once during prepare).
2. Install the default Cursor plugins (`/add-plugin …`) if this machine does not already have them.
3. Get house workflow + tool defaults injected every chat (native rule).
4. Then run step 2 (or find `AGENTS.md` already written in the same prepare run).

## Non-goals

- Do not overwrite a richer host `skills-lock.json` or a host `.cursor/settings.json` plugin list the user already tuned — merge, keep extras.
- Do not write global `~/.cursor/cli-config.json` or Cursor User Rules unless the user asked. Those are machine-personal.
- Do not duplicate `harness-defaults/ai-coding-native-rules.md` into `AGENTS.md`. Step 2 writes a **short** portable Tools stanza so non-Cursor agents still prefer Context7 and Sonatype.

## Source files (kit)

| Kit path                                     | Host destination                                                 |
| -------------------------------------------- | ---------------------------------------------------------------- |
| `harness-defaults/skills-lock.json`          | `skills-lock.json` (git root)                                    |
| `harness-defaults/ai-coding-native-rules.md` | `.cursor/rules/ai-coding-native-rules.mdc` (`alwaysApply: true`) |
| `harness-defaults/cursor-settings.json`      | merge into `.cursor/settings.json`                               |
| `harness-defaults/cli.json`                  | `.cursor/cli.json` if missing or empty of `permissions`          |

## Who does what

| Action                                                                 | Who                                                                                                                                                                                    |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Write/merge the files below                                            | Agent                                                                                                                                                                                  |
| `npx skills experimental_install --yes` at host root                   | **Agent must run** (needs network). Restores **GitHub** skill sources from the lockfile. If it fails after one retry, put the command in chat and in the report — do not silently skip |
| `/add-plugin cursor-team-kit` (and the other three)                    | Human, in Cursor chat — **no marketplace CLI exists**. Agent must surface this immediately if plugins are missing                                                                      |
| Connect plugin keys (Context7, Sonatype) in Customize                  | Human — never commit keys                                                                                                                                                              |
| Paste native-rules into Cursor **User Rules** (optional, all projects) | Human                                                                                                                                                                                  |

There is no supported shell/CLI to install Cursor Marketplace plugins. Do not invent `cursor --install-plugin` or similar. `/add-plugin` is a chat slash command. Settings `enabled: true` only applies once the plugin is installed.

## Files to produce

### 1. `.cursor/settings.json` (IDE client defaults)

Merge `plugins` from `harness-defaults/cursor-settings.json`. Preserve every other key already in the file.

`"key": true` means the plugin needs a user-supplied credential in the Cursor UI. Do **not** put the credential in git.

After the file exists, detect install state, then either skip the nag or put this block **at the top of your next message** (not only in the final report):

```
/add-plugin cursor-team-kit
/add-plugin context7-plugin
/add-plugin sonatype-cursor-plugin
/add-plugin modern-web-guidance
```

Same names as Customize → Marketplace. Reload the window after install.

**Detect before nagging.** Look under the user's Cursor plugins directory, for example `~/.cursor/plugins/` (Windows: `%USERPROFILE%\.cursor\plugins\`, including `cache/`). If all four plugins already appear installed on this machine, say so and ask for a reload if they are not active in this project. Do not tell the human to re-run `/add-plugin` for plugins that are already there.

If any of the four are missing, list only the missing `/add-plugin` lines. Continue to step 2 unless `--harness-only` — writing `AGENTS.md` does not need the plugins, but the repo is not "ready" until the human has installed the missing ones and reloaded.

### 2. `.cursor/cli.json` (CLI client, project layer)

Cursor CLI project overrides are **permissions only** (see Cursor CLI configuration). Global prefs stay in `~/.cursor/cli-config.json`.

If the host has no `.cursor/cli.json`, copy `harness-defaults/cli.json`. If it already has `permissions`, leave them unless they are empty and the template is stricter in a useful way — do not wipe a hand-tuned allow/deny list.

### 3. `skills-lock.json` + first skill install

If the host has no `skills-lock.json`, copy `harness-defaults/skills-lock.json` verbatim.

If the host already has one, keep it. Report that the kit default was skipped.

Then from the **host git root** (not the kit folder), **run**:

```bash
npx skills experimental_install --yes
```

`--yes` skips the npm prompt. Needs network. The lockfile lists **GitHub** skill sources; this command restores them into `.agents/skills/` (canonical project install). Other agent folders may symlink or copy from there. Do not ask the human to clone skill repos by hand.

If the command fails, retry once. If it still fails, do not mark this harness item complete — print the exact command for the human.

`harness-defaults/ai-coding-native-rules.md` may name extra **global** workflow skills (for example coleam00 / PIV). Those are not in the project lock. Do not add them to `skills-lock.json` unless the user asks.

### 4. `.gitignore`

Ensure this block exists (create the file if needed; otherwise append if missing):

```
# Installed agent skills (restore with: npx skills experimental_install --yes)
.agents/skills/
```

Do not gitignore `skills-lock.json`.

### 5. Native rules (pre-prompt / always-on)

Cursor injects `.cursor/rules/*.mdc` with `alwaysApply: true` into every chat — that is the project-level equivalent of a pre-prompt. User Rules in Customize are global and cannot be set from the repo.

1. Read kit `harness-defaults/ai-coding-native-rules.md`.
2. Write host `.cursor/rules/ai-coding-native-rules.mdc`:

```markdown
---
description: House workflow and default tools (always on)
alwaysApply: true
---
```

Then the native-rules body **verbatim** (no extra commentary).

If that path already exists, backup to `ai-coding-native-rules.mdc.bak` and merge: keep human steering that is still true; replace kit-owned sections that drifted.

If the host still has `.cursor/rules/native-rules.mdc` from an older prepare, treat that as the same rule: merge into `ai-coding-native-rules.mdc`, then remove the old file so Cursor does not inject two always-on copies.

This file is **not** a second `AGENTS.md`. It is house process + tool routing. Project map, commands, and seams stay in step 2.

## Other clients

Skills CLI’s project install is the shared layer for Cursor IDE, Cursor CLI, Claude Code, Codex, and others that read `.agents/skills/` (or a symlink). Do not invent a second plugin marketplace for those tools in this step.

Optional human: paste `harness-defaults/ai-coding-native-rules.md` into Cursor Customize → Rules → User Rules so the same house rules apply in every repo.

## After harness files

List in the report: created/merged paths, whether `experimental_install` succeeded, missing `/add-plugin` lines (or “already installed”), and plugin keys to connect. Then continue to step 2 unless the user asked for harness only.
