---
name: team-skills-update
description: Refresh every skill you installed from the ecom-business-team team-skills marketplace to the latest version on main. Only touches skills that came from the marketplace (marked with .team-skills-source) — never touches your own personal skills. Use when the user wants the latest team skills, has heard "someone updated X", or asks "do I have the newest version".
---

# /team-skills-update

**Purpose:** Refresh every skill in `~/.claude/skills/` that came from the team-skills marketplace. Leave the user's personal skills alone.

**Marketplace repo:** `https://github.com/ecom-business-team/team-skills`

## Flow

### Step 1 — Refresh the local cache

```bash
CACHE_DIR="$HOME/.cache/team-skills"
if [ -d "$CACHE_DIR/.git" ]; then
  git -C "$CACHE_DIR" pull --ff-only --quiet
else
  mkdir -p "$HOME/.cache"
  git clone --quiet https://github.com/ecom-business-team/team-skills.git "$CACHE_DIR"
fi
```

If the pull fails, tell the user: *"Couldn't reach the team-skills marketplace. Check your internet, then try `/team-skills-update` again."*

### Step 2 — Find installed team skills

Scan `~/.claude/skills/*/` for any subdirectory containing a `.team-skills-source` file. That marker was written by `/team-skills-browse` at install time and records the plugin the skill came from.

```bash
for skill_dir in "$HOME/.claude/skills"/*/; do
  if [ -f "$skill_dir/.team-skills-source" ]; then
    plugin=$(cat "$skill_dir/.team-skills-source")
    skill_name=$(basename "$skill_dir")
    # This skill is a team-skills install from plugin=<plugin>
  fi
done
```

If no marker files are found, tell the user: *"You don't have any team-skills installed yet. Run `/team-skills-browse` to install some."* and stop.

### Step 3 — Update each one

For each marked skill, check whether it still exists in the marketplace at `$CACHE_DIR/plugins/<plugin>/skills/<skill-name>/`. Three cases:

- **Still there** → overwrite the local copy with the cached version, preserving the marker file:
  ```bash
  rm -rf "$skill_dir"
  cp -R "$CACHE_DIR/plugins/$plugin/skills/$skill_name" "$skill_dir"
  echo "$plugin" > "$skill_dir/.team-skills-source"
  ```
- **Removed from the marketplace** → tell the user this skill was removed upstream and ask whether to delete the local copy. Don't delete without confirmation.
- **Moved to a different plugin** → treat as still-there but update the marker to the new plugin name.

### Step 4 — Report

Tell the user:
- *"Refreshed `<count>` team-skills: `<list>`."*
- If any content actually changed vs. what they had, note that ("2 skills had updates, 4 were already current"). You can compare quickly with `diff -qr` before overwriting.
- If any skills were removed upstream, list them and prompt for delete confirmation.
- *"You may need to open a new Claude Code session for the changes to take effect."*

## Design notes for future maintainers

- **Only touch skills with the marker file.** The user's personal skills in `~/.claude/skills/` must never be modified by this command. That's the whole safety guarantee.
- **Preserve the marker file** after overwriting. If it's lost, the skill becomes invisible to future updates.
- **Do not delete without asking.** If a skill was removed upstream, prompt — someone may still want to use it locally.
