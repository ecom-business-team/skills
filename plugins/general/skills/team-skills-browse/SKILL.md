---
name: team-skills-browse
description: Browse and install skills from the ecom-business-team team-skills marketplace. Works in every Claude Code surface (VSCode extension, terminal, etc.) — it copies skills directly into ~/.claude/skills/ instead of using the /plugins command surface. Use when the user asks "what skills are available", "what can I install", or "show me what the team has built".
---

# /team-skills-browse

**Purpose:** Give the user a friendly, interactive way to discover and install team-skills. Bypass the `/plugins` command surface entirely — that surface only works in the terminal CLI, and most teammates use the VSCode extension. Instead, this skill clones the marketplace repo and copies chosen skill folders straight into `~/.claude/skills/`, where they're picked up by every Claude Code surface.

**Marketplace repo:** `https://github.com/ecom-business-team/team-skills`

## Prerequisites

- `git` installed (macOS ships it; if a teammate is on a fresh Mac they may need to accept the Xcode Command Line Tools prompt on first `git` use)
- Internet access to GitHub

## Flow

### Step 1 — Refresh the local cache

The marketplace repo is cached at `~/.cache/team-skills/`. Ensure it exists and is up to date:

```bash
CACHE_DIR="$HOME/.cache/team-skills"
if [ -d "$CACHE_DIR/.git" ]; then
  git -C "$CACHE_DIR" pull --ff-only --quiet
else
  mkdir -p "$HOME/.cache"
  git clone --quiet https://github.com/ecom-business-team/team-skills.git "$CACHE_DIR"
fi
```

If the clone/pull fails, tell the user: *"Couldn't reach the team-skills marketplace. Check your internet, then try `/team-skills-browse` again."*

### Step 2 — Read the catalog

Parse `$CACHE_DIR/.claude-plugin/marketplace.json` — the `plugins` array. For each plugin, note: `name`, `description`, `category`.

### Step 3 — Present the catalog

Show plugins grouped by category (`general` first, then `domain`). Mark installed plugins with `✓` (detect by checking whether *any* skill from the plugin exists in `~/.claude/skills/` — see marker file below).

```
GENERAL — recommended for everyone
  ✓ general           new-workflow, onboard, team-skills-add, team-skills-browse, team-skills-update
  ✓ team-build-kit    memo, prd, build, ship, quick-fix, new-workspace

DOMAIN — pick your role
    copywriting       (empty — awaiting first contribution)
    creative-strategy (empty — awaiting first contribution)
    media-buying      (empty — awaiting first contribution)
```

Ask: *"Which plugin(s) do you want to install? (You can pick multiple. Type 'all' to install everything.)"*

### Step 4 — Drill in (optional)

If the user wants to see individual skills inside a plugin before installing, list the folders in `$CACHE_DIR/plugins/<plugin>/skills/` and read the `name:` + `description:` from each `SKILL.md` frontmatter.

### Step 5 — Install what they picked

For each selected plugin, copy every skill folder inside it into `~/.claude/skills/`. Overwrite if a skill with the same name is already there (this is the update path — see also `/team-skills-update`).

```bash
mkdir -p "$HOME/.claude/skills"
for skill_dir in "$CACHE_DIR/plugins/<plugin>/skills"/*/; do
  skill_name=$(basename "$skill_dir")
  target="$HOME/.claude/skills/$skill_name"
  rm -rf "$target"
  cp -R "$skill_dir" "$target"
  # Write a marker so /team-skills-update knows where this came from
  echo "<plugin>" > "$target/.team-skills-source"
done
```

The `.team-skills-source` marker records which plugin the skill came from, so `/team-skills-update` can refresh only skills it owns and avoid touching skills the user built themselves.

### Step 6 — Report

Tell the user:
- *"Installed from `<plugin-list>`: `<skill-list>`. These are available in every Claude Code session from now on — including VSCode and terminal."*
- *"To browse again, run `/team-skills-browse`."*
- *"To get the latest versions later, run `/team-skills-update`."*
- *"You may need to open a new Claude Code session for the skills to become available."*

## Design notes for future maintainers

- **Do not use `/plugins` commands.** The whole point of this skill is to work in the VSCode extension, where `/plugins` is unavailable. Copy files into `~/.claude/skills/` directly.
- **Do not hardcode the skill list.** Always read from the cached clone. When someone contributes a new skill, it shows up here on the next pull.
- **Cache is disposable.** If something looks wrong, deleting `~/.cache/team-skills/` and re-running is always safe — the next `/team-skills-browse` will re-clone.
- **Fail gracefully offline.** If `git clone`/`pull` fails and no cache exists, tell the user the marketplace is unreachable. Don't try to guess a stale answer.
