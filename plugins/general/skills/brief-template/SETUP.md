# Setup: /brief-template

This skill adds the **blank** brief template to Ads Board tasks that have moved into **waiting for brief** but don't have a template yet — pulling the template live from each client's ClickUp Client Catalog and placing it above the existing breakthrough memo.

It does **not** fill in copy or landing-page links — that's a separate command (to be built later). This one just gets the scaffold in place.

**Time to set up: ~2 minutes (if ClickUp is already connected)**

---

## What you need

1. Claude Code installed
2. ClickUp connected to your Claude account (same one-time OAuth as `/new-ad-idea`)
3. This skill folder at `~/.claude/skills/brief-template/`

---

## Step 1 — ClickUp connection

Already done if `/new-ad-idea` works. If not: open Claude Code, type `connect ClickUp`, authorize in the browser. Use the account you use for the Ads Board.

## Step 2 — Install

Put the folder at `~/.claude/skills/brief-template/` (Finder → Cmd+Shift+G → `~/.claude/skills/`). It should contain `SKILL.md` and `SETUP.md`. Restart Claude Code.

## Step 3 — Use it

1. In ClickUp, drag the ad task into **waiting for brief**.
2. Type `/brief-template`.
3. It scans the board, lists every waiting-for-brief brief that has **no template yet**, and asks which to do (one, several, or `all`).
4. It inserts that client's blank template above the memo and gives you the link.

A task that already has a template (even a blank one) is left alone — this skill only fills the gap where a template is missing.

---

## How it decides

| Piece | Where it comes from |
|-------|--------------------|
| Which tasks | Ads Board, status `waiting for brief`, where the description has no `Copy + Headlines:` scaffold |
| Template | That client's catalog → `📌 TASK FORMAT TEMPLATE` page (live, verbatim, all fields blank) |
| Memo | Left untouched, kept below the template |

Client → catalog mapping (baked into `SKILL.md`):

| Client | Catalog doc |
|--------|------------|
| 🦮 Dog Friendly Co | `a0jtq-34014` |
| 🪮 Keeps Hair Loss | `a0jtq-192774` |
| 🍆 Keeps ED | `a0jtq-192774` (shares Hair Loss for now) |
| 💊 Keeps TRT | `a0jtq-196894` |

**When Keeps ED gets its own catalog,** update the ED row in the `SKILL.md` mapping table — only edit needed.

---

## Troubleshooting

**Doesn't show up** — folder must be exactly `brief-template` inside `~/.claude/skills/`. Restart Claude Code.
**Says everything already has a template** — correct if every waiting-for-brief task already carries the scaffold; this skill only adds where missing.
**Client not recognized** — if a task's client isn't in the mapping table, the skill asks which catalog instead of guessing. Add it to the table to make it automatic.
**Permission denied** — type `reconnect ClickUp` and re-authorize.
