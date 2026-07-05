# Ecom Business Team — Skills Marketplace

Shared skills for the team. Organized by functional domain. Install once, update anytime.

## What this is

A collection of **skills** — natural-language commands you can type in Claude Code (e.g. `/memo`, `/new-workflow`). Skills are grouped into **plugins** by role. Everyone installs `general` and `team-build-kit`. You install your domain plugin (`copywriting`, `creative-strategy`, `media-buying`, etc.) on top.

When someone on the team improves or adds a skill, you get it by running `/team-skills-update`.

**Works everywhere:** VSCode extension, terminal Claude Code, any Claude Code surface. Skills install directly into `~/.claude/skills/` — no plugin manager required.

## First-time setup (one paste, one prompt)

You do NOT need to open a terminal. Just open Claude Code (the VSCode extension is fine) and paste the following prompt into the chat:

> **Please install the ecom-business-team team-skills toolkit. Clone `https://github.com/ecom-business-team/team-skills.git` to `~/.cache/team-skills`, then copy every skill folder inside `plugins/general/skills/` (there should be five: `new-workflow`, `onboard`, `team-skills-add`, `team-skills-browse`, `team-skills-update`) into `~/.claude/skills/`. Write `general` into a `.team-skills-source` file inside each copied skill folder. When you're done, tell me it's ready and that I can run `/team-skills-browse` next.**

Claude will do the setup for you. Once it confirms, **open a new Claude Code session** (close the current window, open a fresh chat) — new skills only load on session start.

In the new session, run:

```
/team-skills-browse
```

This shows every plugin available. Pick `team-build-kit` (everyone should have it) plus your role plugin (media-buying, copywriting, etc.), and it installs them.

That's the whole onboarding. No terminal. No git commands. No `/plugins` command surface.

## Everyday commands

| Command | When |
|---|---|
| `/team-skills-browse` | Discover and install new skills |
| `/team-skills-update` | Refresh every team-skill you have to the latest version |
| `/team-skills-add` | Share a skill you built with the team (opens a PR for you) |

## Plugins

| Plugin | Who | What's inside |
|---|---|---|
| `general` | Everyone | `new-workflow`, `onboard`, `team-skills-add`, `team-skills-browse`, `team-skills-update` |
| `team-build-kit` | Everyone | `memo` -> `prd` -> `build` -> `ship` lifecycle for building tools you can trust |
| `copywriting` | Copywriters | *(placeholder — awaiting first contribution)* |
| `creative-strategy` | Strategists | *(placeholder — awaiting first contribution)* |
| `media-buying` | Buyers | *(placeholder — awaiting first contribution)* |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the standard every skill must meet. Short version: skills must be **complete, standalone, and shipped-quality** — the same bar you'd use for something you rely on in production. Use `/team-skills-add` to submit — it runs the checklist and opens the PR for you.

## For maintainers

The full living index is in [CONTEXT.md](CONTEXT.md). The interactive flow diagram is [flow.html](flow.html). The library of skills lives at [LIBRARY.md](LIBRARY.md) and is auto-updated by `/team-skills-add` on every merged submission.
