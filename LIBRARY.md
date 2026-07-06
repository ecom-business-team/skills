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
├── creative-strategy       (strategists) — Kat Prefontaine
│   ├── /brief-iteration
│   ├── /strategic-breakdown
│   └── /hemingway-check
│
├── clickup-operations      (shared ClickUp workspace) — Spencer
│   ├── /new-ad-idea
│   ├── /brief-template
│   └── /top-lp
│
└── media-buying            (buyers) — Sami
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

**Owner:** Kat Prefontaine

| Command | What it does |
|---|---|
| `/brief-iteration` | Adapt a top-performing ad script to a new format/trend. Outputs hook options, hold, and CTA. Use when iterating a winning batch into a new creative format. |
| `/strategic-breakdown` | Full psychological breakdown of a brief or script using the Elite Video Ad framework — Schwartz awareness, belief ladder, identity, curiosity gaps. Use for weekly QA and reverse-engineering winners. |
| `/hemingway-check` | Hemingway-style readability check on scripts or copy — flags hard sentences, passive voice, adverbs, and weak qualifiers; targets Grade 6. Use before finalizing any script or hook. |

---

## `clickup-operations` — for the shared ClickUp workspace

**Owner:** Spencer
**Scope note:** these skills carry baked-in shared-workspace IDs (ClickUp list IDs, Client Catalog doc IDs). The whole team uses the same workspace, so this is intentional. Each skill ships a `SETUP.md` documenting the client → catalog mapping.

| Command | What it does |
|---|---|
| `/new-ad-idea` | Interview-driven creation of a new ad idea task on the ClickUp Ads Board — walks Client → Product → Concept → Type → Desire → Sub Avatar → Angle → Awareness → Format → breakthrough memo, then builds the task with all fields set. |
| `/brief-template` | Adds a client's blank brief template (pulled live from their ClickUp Client Catalog) to Ads Board tasks that lack one. Scaffold only — does not pair copy or LP links. |
| `/top-lp` | Adds the top control landing page to active briefs that have a template but a blank `Links to:` line, matched per product/region from the Client Catalog. LP only — never copy. |

---

## `media-buying` — for media buyers

**Owner:** Sami
**Status:** Placeholder — awaiting first contribution.

---

## Getting updates

Run `/team-skills-update` inside Claude Code. It refreshes every skill you installed from the marketplace to its latest version. Personal skills you built yourself are never touched.
