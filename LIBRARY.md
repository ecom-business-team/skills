# Skills Library

Every plugin and skill available in the ecom-business-team marketplace. **Automatically updated by `/team-skills-add`** on every skill submission — you should never need to edit this file by hand.

> **How to install:** Run `/team-skills-browse` in Claude Code and pick what you want. Works in the VSCode extension, terminal, and any Claude Code surface — no `/plugins` command needed.
>
> First time? See [README.md](README.md) for the one-paste bootstrap.

---

## Map

```
ecom-business-team/team-skills
│
├── general                 (everyone) — Zachary Blake
│   ├── /new-workflow
│   ├── /onboard
│   ├── /team-skills-add
│   ├── /team-skills-browse
│   └── /team-skills-update
│
├── team-build-kit          (everyone) — Zachary Blake
│   ├── /new-workspace
│   ├── /memo
│   ├── /prd
│   ├── /build
│   ├── /ship
│   └── /quick-fix
│
├── copywriting             (copywriters) — owner TBD
│   └── (empty — awaiting first contribution)
│
├── creative-strategy       (strategists) — owner TBD
│   └── (empty — awaiting first contribution)
│
└── media-buying            (buyers) — owner TBD
    └── (empty — awaiting first contribution)
```

**Reading the map:** each plugin is a group of related skills. Skills are individual commands you type in Claude Code (`/memo`, `/new-workflow`, etc.). Everyone installs `general` + `team-build-kit`; the domain plugins are optional by role. Use `/team-skills-browse` to install; no `/plugins install` required.

---

## `general` — recommended for everyone

**Owner:** Zachary Blake

| Command | What it does |
|---|---|
| `/new-workflow` | Turn a repeat process into a reusable skill through a guided interview. The daily driver for building your own skill library. |
| `/onboard` | First-run skill for someone new to Claude Code. Interviews the user about their work, scaffolds a personalized workspace with CLAUDE.md, CONTEXT.md files, and visual folder maps. |
| `/team-skills-add` | Submit a skill from your local `~/.claude/skills/` to this marketplace. Runs the quality checklist, forks + branches + commits + opens a PR on your behalf. You never touch git. |
| `/team-skills-browse` | Browse and install skills from this marketplace. Copies chosen skills directly into `~/.claude/skills/` — works in VSCode, terminal, any surface. |
| `/team-skills-update` | Refresh every team-skill you have installed to the latest version. Only touches skills that came from the marketplace; leaves your personal skills alone. |

---

## `team-build-kit` — the three-gate build lifecycle

**Owner:** Zachary Blake
**Big idea:** *Complexity is the enemy. If you can't explain it simply, it's probably too complicated.*

| Command | When to use it | Gate |
|---|---|---|
| `/new-workspace` | Before building anything — sets up a tidy home for your work | (foundation) |
| `/memo` | You have an idea worth building | **Gate 1 — should this exist?** |
| `/prd` | The idea is worth it — now design it properly | **Gate 2 — is it designed right?** |
| `/build` | The design is solid — implement it | (execution) |
| `/ship` | It works — but others will rely on it, or it touches money or real data | **Gate 3 — safe to rely on?** |
| `/quick-fix` | Something small broke or a tiny tweak — no ceremony needed | (below the gates) |

---

## `copywriting` — for copywriters

**Owner:** TBD
**Status:** Placeholder — awaiting first contribution. Be the first to run `/team-skills-add` and submit something.

---

## `creative-strategy` — for creative strategists

**Owner:** TBD
**Status:** Placeholder — awaiting first contribution.

---

## `media-buying` — for media buyers

**Owner:** TBD
**Status:** Placeholder — awaiting first contribution.

---

## Getting updates

Run `/team-skills-update` inside Claude Code. It refreshes every skill you installed from the marketplace to its latest version. Personal skills you built yourself are never touched.
