# Skills Marketplace — System Context

**What it is:** The `ecom-business-team/team-skills` GitHub repo. A shared library of Claude Code skills the whole team can install and improve. Distributed by direct file copy into `~/.claude/skills/` — no `/plugins` command surface required, so it works in the VSCode extension and every other Claude Code surface. Every skill is production-quality (see [CONTRIBUTING.md](CONTRIBUTING.md)).

**Repo:** https://github.com/ecom-business-team/team-skills

**Local clone:** this folder (`/Users/zacharyblake/Desktop/ClaudeCode/team-skills/`) is the working copy of the repo. Edits here + `git push` = published to the team.

## Purpose

Solve the "how does the team share work across Claude Code" problem without inventing infrastructure. Uses standard GitHub PR flow plus three custom skills that hide git and file management from the user. **Bypasses Claude Code's `/plugins` command surface** because it's terminal-only today and the team uses the VSCode extension; skills install straight into `~/.claude/skills/` where every surface picks them up.

## What lives here

| File / Folder | What it is | Living or disposable |
|---|---|---|
| `README.md` | Team-facing onboarding — the one-paste bootstrap prompt + everyday commands | Living |
| `CONTRIBUTING.md` | The shipped-quality checklist every skill must pass | Living |
| `CONTEXT.md` | This file — the local index (Zak's personal map) | Living |
| `LIBRARY.md` | Enumerated list of every plugin and skill with descriptions | Living — regenerate when skills are added/removed |
| `flow.html` | Interactive diagram of the publish + install flow (team-visible) | Living — update when the mechanism changes |
| `.claude-plugin/marketplace.json` | The manifest Claude Code reads when adding this marketplace | Living |
| `plugins/general/` | Skills recommended for everyone (`new-workflow`, `onboard`, `team-skills-add`, `team-skills-browse`, `team-skills-update`) | Living |
| `plugins/team-build-kit/` | The `/memo -> /prd -> /build -> /ship` lifecycle | Living |
| `plugins/copywriting/` | Copywriter skills — placeholder pending first contribution | Living |
| `plugins/creative-strategy/` | Creative strategist skills — placeholder pending first contribution | Living |
| `plugins/media-buying/` | Media buyer skills — placeholder pending first contribution | Living |

## How to find X

| Question | Look here |
|---|---|
| "What plugins exist and what's in them?" | [LIBRARY.md](LIBRARY.md) |
| "How does a teammate install this?" | [README.md](README.md) — one-paste bootstrap |
| "How does a teammate submit a skill?" | [CONTRIBUTING.md](CONTRIBUTING.md) + `/team-skills-add` |
| "How does the whole system work end-to-end?" | [flow.html](flow.html) — visual diagram |
| "Who owns each plugin?" | Each plugin's own `README.md` |
| "What's the source of truth for what's live?" | `.claude-plugin/marketplace.json` on `main` |

## Plugin owners

| Plugin | Owner | Status |
|---|---|---|
| `general` | Zachary Blake | Live |
| `team-build-kit` | Zachary Blake | Live |
| `copywriting` | TBD | Placeholder |
| `creative-strategy` | TBD | Placeholder |
| `media-buying` | TBD | Placeholder |

## Governance

- **PR-only** — direct commits to `main` are blocked (except for Zak as org admin, for seeding)
- **1 required approval** per PR
- Plugin owners are the reviewers for PRs to their plugin
- New plugins are proposed via PR (open against `general`, note the request in the description)

## The three custom skills (why they exist)

The whole point of this system is that **teammates never touch git or the `/plugins` surface.** Three skills carry that promise, all in `general`:

- **`/team-skills-browse`** — teammate says "what's available?" Skill clones/pulls the marketplace to `~/.cache/team-skills/`, presents the catalog with descriptions, copies chosen skill folders into `~/.claude/skills/`. Writes a `.team-skills-source` marker in each installed skill so updates know what it manages.
- **`/team-skills-update`** — teammate says "give me the latest." Skill pulls the marketplace cache and refreshes every marker-tagged skill in `~/.claude/skills/`. Personal skills without the marker are never touched.
- **`/team-skills-add`** — teammate says "I want to share this skill." Skill runs the CONTRIBUTING checklist, forks the repo, branches, commits, opens the PR. Teammate sees a PR URL.

**Install mechanism (why we bypass `/plugins`):** `/plugin marketplace add` and `/plugin install` only work in the terminal CLI today, not the VSCode extension where most of the team lives. Copying files into `~/.claude/skills/` works on every surface. When Anthropic ships plugin management in the extension, we can layer it on top — the file-based path continues to work.

## What this system does NOT include (yet)

- **Centralized context sharing** — deferred by Zak's call. Skills and repos only for now. Revisit when team is actively using this and a specific context-sharing pain surfaces.
- **Workflow (n8n) sharing** — deferred. Team isn't using shared n8n workflows yet.
- **A knowledge base** — Zak's flagged as future work, not priority now.

## Cross-workspace links

Points down (sub-areas):
- Each `plugins/<name>/` is its own logical sub-area with its own README and owner.

Points up (cross-workspace):
- Root [CLAUDE.md](../CLAUDE.md) routing table (this system is registered there)
- **`zjamesblake/team-build-kit` (public repo) is the source of truth for team-build-kit skill content.** It stays public because YouTube videos link to it. The workspace-root `team-build-kit/` folder is its working copy. `plugins/team-build-kit/skills/` in this marketplace is a **downstream mirror** for team distribution — after editing skills in the public repo, propagate here with:

  ```bash
  rsync -a --delete ~/Desktop/ClaudeCode/team-build-kit/.skills/{build,memo,prd,ship,quick-fix,new-workspace}/ \
    ~/Desktop/ClaudeCode/team-skills/plugins/team-build-kit/skills/
  ```

  Then commit + push the marketplace. Do **not** edit `plugins/team-build-kit/skills/` directly — changes there won't reach the public repo.

## Don't Load (for this system)

- `ad-bounty/`, `northstar/`, `ad-analysis/`, `personal-os/` — unrelated systems
- The workspace's own `~/.claude/skills/` — Zak's local skills. Only relevant if the current task is publishing one of them into the marketplace.
