---
name: team-skills-add
description: Add a skill from your local ~/.claude/skills/ to the ecom-business-team skills marketplace. Runs the quality checklist, then forks + branches + commits + opens a pull request on your behalf, and updates LIBRARY.md with the new entry. You never touch git. Use this when you've built a skill that would be useful to the team.
---

# /team-skills-add

**Purpose:** Take a skill the user has built locally, verify it meets the marketplace's shipped-quality bar, and get it into a pull request against `ecom-business-team/team-skills`. The user never touches git.

**Marketplace repo:** `https://github.com/ecom-business-team/team-skills`

## Prerequisites

- `gh` CLI installed and authenticated (`gh auth status` should show a logged-in account)
- The skill exists locally at `~/.claude/skills/<skill-name>/SKILL.md`

If `gh` is not installed, stop and tell the user: *"You need the GitHub CLI to submit a skill. Install with `brew install gh`, then run `gh auth login`, then re-run `/team-skills-add`."*

## Flow

### Step 1 — Pick the skill

Ask the user which skill they want to submit. If they don't know the name, list what's in `~/.claude/skills/` (excluding `_archive` and `_shared`).

Confirm the path exists. Read the SKILL.md so you have its content.

### Step 2 — Run the quality checklist

Silently check each item. Report any failures back to the user before proceeding — they must fix these first.

- [ ] **Frontmatter present** — starts with `---`, has `name:` and `description:` fields
- [ ] **Description tells when to invoke** — should include phrases like "use when", "run when", "for [situation]". A description that only says what the skill does (not when) is a fail.
- [ ] **No hardcoded personal paths** — grep for `/Users/` in the skill directory. If found, list the offending lines and stop.
- [ ] **No secrets** — grep for common secret patterns: `sk-`, `xox`, `ghp_`, `gho_`, `AKIA`, `-----BEGIN`. If found, stop.
- [ ] **No hardcoded workspace IDs** — grep for likely ClickUp list IDs (long numeric strings 9+ digits), n8n workflow IDs (patterns like `[A-Za-z0-9]{16}`), Supabase keys. If found, ask the user whether these are intended to be shared or should be parameterized.
- [ ] **Has at least one usage example** — the skill body mentions an example prompt or scenario

If any check fails, print the specific issue and stop. The user fixes it locally, then re-runs `/team-skills-add`.

### Step 3 — Pick the plugin

Fetch the current marketplace manifest from GitHub:

```
curl -sL https://raw.githubusercontent.com/ecom-business-team/team-skills/main/.claude-plugin/marketplace.json
```

Present the list of plugins (`general`, `team-build-kit`, `copywriting`, `creative-strategy`, `media-buying`, ...) with their descriptions. Ask which one the skill belongs in.

If the user thinks it doesn't fit any existing plugin, tell them: *"That's a request for a new plugin. Open the PR against `general` for now and note in the PR description that you'd like a new `<name>` plugin — the marketplace owner will decide."*

### Step 4 — Fork, branch, commit, PR

Do this without asking the user for git commands. Show what you're doing but don't require input.

```bash
# Check if user has a fork; create if not
gh repo fork ecom-business-team/team-skills --clone=false --remote=false 2>&1 || true

# Work in a temp directory
WORKDIR=$(mktemp -d)
cd "$WORKDIR"
gh repo clone "$(gh api user --jq .login)/skills" -- --depth=1
cd skills
git remote add upstream https://github.com/ecom-business-team/team-skills.git
git fetch upstream main
git checkout -b add-<skill-name> upstream/main

# Copy the skill in
mkdir -p plugins/<plugin>/skills/<skill-name>
cp -R ~/.claude/skills/<skill-name>/* plugins/<plugin>/skills/<skill-name>/

# Update LIBRARY.md — add a row under the target plugin's table
# Read LIBRARY.md, find the "## `<plugin>`" section, insert a new
# `| /<skill-name> | <one-line description from frontmatter> |` row at
# the end of that plugin's command table. If the plugin doesn't have a
# table yet (placeholder), replace the "Status: Placeholder" line with a
# fresh command table containing this skill as the first entry.

# Commit and push (skill + LIBRARY.md update in one commit)
git add plugins/<plugin>/skills/<skill-name> LIBRARY.md
git commit -m "Add <skill-name> to <plugin>"
git push origin add-<skill-name>

# Open PR
gh pr create --repo ecom-business-team/team-skills \
  --title "Add <skill-name> to <plugin>" \
  --body "Adds \`<skill-name>\` to the \`<plugin>\` plugin.

**Description:** <one-line description from frontmatter>

**Author confirms:**
- Skill runs end-to-end on a fresh machine
- No secrets or personal paths
- Has been used for real work at least once

Contributed via \`/team-skills-add\`."
```

### Step 5 — Report

Print the PR URL to the user. Tell them:
- *"Your submission is now a pull request: `<url>`. The plugin owner will review. When they merge, everyone on the team gets it on their next `/team-skills-update`."*
- *"You can safely close this Claude Code session — the PR is on GitHub now."*

## Failure modes

- **User doesn't have a GitHub account signed into `gh`** — tell them to run `gh auth login` and try again
- **Skill directory doesn't exist** — check the spelling, list `~/.claude/skills/` for them
- **PR fails to open** — print the git error verbatim and stop; don't try to recover silently
