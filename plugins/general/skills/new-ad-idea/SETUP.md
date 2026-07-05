# Setup: /new-ad-idea

This skill lets you log a new ad idea directly into the ClickUp Ads Board from a guided interview in Claude Code. One command, all fields filled, task created.

**Time to set up: ~20 minutes (mostly the ClickUp connection)**

---

## What you need

1. Claude Code installed on your Mac
2. ClickUp connected to your Claude account
3. This skill folder copied into the right place

---

## Step 1 — Install Claude Code (skip if you already have it)

1. Go to claude.ai/code and download the Mac app
2. Open it and sign in with your Anthropic account

That's it. Claude Code is the app you'll run all skills from.

---

## Step 2 — Connect ClickUp to Claude

This is the one-time step that lets Claude create ClickUp tasks on your behalf.

1. Open Claude Code
2. In any conversation, type: connect ClickUp
3. Claude will walk you through the OAuth flow — click Authorize in the browser window that opens
4. Once authorized, Claude can read and write to ClickUp as you

Important: when you create a task with this skill, it gets attributed to YOUR ClickUp account as the Ideator. Connect with the same account you use for the Ads Board.

---

## Step 3 — Install the skill

1. Download this entire folder (new-ad-idea/) from Google Drive to your computer
2. Move it into: ~/.claude/skills/

   On Mac: open Finder, press Cmd + Shift + G, paste ~/.claude/skills/ and hit Enter.
   If the skills folder doesn't exist yet, create it.

3. Your folder should look like this when done:
   ~/.claude/skills/
   └── new-ad-idea/
       ├── SKILL.md
       └── SETUP.md

4. Restart Claude Code (quit and reopen)

---

## Step 4 — Test it

1. Open Claude Code (any project, or no project)
2. Type: /new-ad-idea
3. Claude kicks off the interview — Client, Product, Concept, and so on
4. At the end, it creates the task in ClickUp and gives you a direct link

---

## Troubleshooting

/new-ad-idea doesn't show up
Make sure the folder is named exactly new-ad-idea (no spaces, no caps) and is directly inside ~/.claude/skills/. Restart Claude Code after moving it.

ClickUp says permission denied
Your connection may have expired. Type reconnect ClickUp in Claude Code and go through the OAuth flow again.

Task was created but some fields are blank
Desire, Sub Avatar, and Product can only be set if they already exist as options in ClickUp. The skill will tell you exactly which ones to add manually at the end — just follow the dual-write instructions it shows.

---

## What gets filled automatically

| Field | How it's set |
|-------|-------------|
| Task name | Built from client + product emoji + your concept |
| Status | new idea |
| Client | Linked to the client card |
| Product | Dropdown selection |
| Type | Ideation / Iteration / Imitation / Mashup |
| Desire / Core Avatar | Dropdown with winner tiers highlighted |
| Sub Avatar | Dropdown with winner tiers highlighted |
| Angle | Your one-sentence angle |
| Awareness Level | Unaware / Problem / Solution / Product Aware |
| Ad Format | Video / Image / Promo |
| Breakthrough Memo | Task description (WHAT / WHY / HOW) |
| Ideator | Auto-set to you via ClickUp auth |
| Votes | Starts at 0 |
