---
name: new-ad-idea
description: Interview-driven creation of a new ad idea task on the ClickUp Ads Board. Walks the user through Client → Product → Concept → Type → Desire → Sub Avatar → Angle → Awareness → Format → Breakthrough Memo, then builds the task with all fields set. Handles new Desire / Sub Avatar / Product values via dual-write reminders at the end. Use when the user says "/new-ad-idea" or asks to create a new ad concept in ClickUp.
---

# New Ad Idea

You are running the ad-idea creation interview for the production Ads Board.

**Target list:** `901405930628` (`Ads Board` in the `All Clients` folder)
**Status to set:** `new idea` (lowercase, exact)
**Naming template:** `{product_emoji} {Client String} - BATCH# - {Concept}`
**Always write `BATCH` in all caps** — every letter capitalized, in every task title. Never `Batch#` or `batch#`.

## Step 0 — Refresh dropdown options

Before asking any questions, call `mcp__claude_ai_ClickUp__clickup_get_custom_fields` with `list_id: "901405930628"` and cache the response. You'll need it for:
- `📦 Product` field id `087af852-c7b7-4f9f-b785-7d2fd0e72700` (current product labels)
- `❤️‍🔥 Desire/Core Avatar` field id `6c11fee4-edd5-4e9d-be7d-2bf9d38e2497` (current desires)
- `👤 Sub Avatar` field id `bfef9b0a-56f0-4d88-9fec-7de19dfc2530` (current sub avatars)
- `Type` field id `bfaf1496-db58-423c-aee8-16f42ad795cc`
- `👀 Awareness Level` field id `dcea12a3-7b8f-44cf-98b1-2db1ebbce537`
- `Ad Format` field id `02d18256-7020-4ddf-ae0f-4a5a554203fe`

Other field IDs (stable):
- Client (tasks-relation): `e902d22f-6086-4503-b1e9-5a7f43fbf779`
- Votes (number): `26771bde-53a4-4595-a7d7-921851a700d1`
- Angle (text): `d346b356-acdd-45f3-9c1d-a5f76941b894`
- Ideator (users): `ae6a9e84-ba32-4ce6-b639-4e39419481ca` (auto-set by MCP auth, don't touch)

## Step 1 — Client

Use `AskUserQuestion` with these 3 options:
- 🦮 Dog Friendly Co
- 🪮 Keeps (Hair Loss or ED)
- 💊 Keeps TRT

**Client task IDs (for the tasks-relation field):**
| Client | Task ID |
|---|---|
| 🦮 Dog Friendly Co | `86b4utu8d` |
| 🪮 Keeps Hair Loss | `86b7tprvj` |
| 🍆 Keeps ED | `86bagc4vb` |
| 💊 Keeps TRT | `86b9upqvb` |

**Keeps splits by product, not by the Step 1 pick.** When the user picks `🪮 Keeps (Hair Loss or ED)`, the client card is decided in Step 2 by the product:
- Product = 🪮 Hair Chew → client card `86b7tprvj` (Keeps Hair Loss)
- Product = 🍆 Keeps ED → client card `86bagc4vb` (Keeps ED)

ED and Hair Loss are **separate clients** now — never relate an ED idea to the Hair Loss card.

## Step 2 — Product

**Filter the `📦 Product` labels by client emoji.** Products per client:
- DFC: 🦮 Harness, 🦮 Dog Bed, 🦮 Paw Protector, 🦮 Supplement
- Keeps: 🪮 Hair Chew, 🍆 Keeps ED
- Keeps TRT: 💊 Keeps TRT

**Logic:**
- If the client has exactly 1 product → auto-assign, announce ("Keeps TRT has one product, auto-filling **Product = 💊 Keeps TRT**."), skip to Step 3.
- If >1 → use `AskUserQuestion` with one option per product, **plus a final option labeled `+ New Product`**.
- If user picks `+ New Product`: ask for the product name. They type something like "Calming Chew". You prepend the client emoji → `🦮 Calming Chew`. Store as `new_product_string`. Field stays blank on the task. Surface in the dual-write block at the end.

## Step 3 — Concept name

Plain text prompt: "What's the concept name?"

Build the task name immediately and confirm with the user:
- `{product_emoji} {Client String} - BATCH# - {Concept}`
- Client String mapping (the brand string in the task title, NOT the client card name):
  - 🦮 → `Dog Friendly Co`
  - 🪮 → `Keeps Hair Loss`
  - 🍆 → `Keeps ED`
  - 💊 → `Keeps TRT`
- The emoji comes from the **product**, not the client. So `🍆 Keeps ED - BATCH# - X` for ED, `🪮 Keeps Hair Loss - BATCH# - X` for Hair Chew.

## Step 4 — Ad type

`AskUserQuestion` with 4 options:
- 💡 Ideation — brand new idea from scratch
- 🔁 Iteration — variation/improvement on an existing ad you've run
- 🎭 Imitation — inspired by something you saw working elsewhere
- 🥔 Mashup — combining two or more existing winning angles/formats

## Step 5 — Desire / Core Avatar

**Filter desires by the product emoji** (NOT client emoji — Keeps Hair Loss desires use 🪮, Keeps ED uses 🍆).

Present as a numbered text list:
```
Current 🍆 Keeps ED desires:
1. I'm tired of empty promises - I want to find something that actually works
2. I want you to reignite the spark in the bedroom
3. I want to choose the most effective treatment for me
...

Reply with the number, or type `new: <your desire>` to add a new one.
```

**Match logic:** Accept either the number or the exact label string (case-insensitive). If user types `new: <string>`, prepend product emoji → store as `new_desire_string`. Field stays blank on task. Surface at end.

## Step 6 — Sub Avatar

Same flow as Step 5, filtered by product emoji. Same number-or-string match. Same `new:` handling → `new_sub_avatar_string`.

## Step 7 — Angle

Plain text prompt:
```
Next: the angle.

> **Quick reminder:** the angle is *the main reason you're giving someone to buy.* One sentence.

What's your angle?
```

Save user's reply as-is to the Angle text field.

## Step 8 — Awareness level

`AskUserQuestion` with 4 options (skip "Most Aware" since it's rarely a starting state):
- Unaware — Doesn't know they have the problem
- Problem Aware — Knows the problem, not the solution
- Solution Aware — Knows solutions exist, not your product
- Product Aware — Knows your product, not yet convinced

(All 5 options exist in the dropdown — if the user types Most Aware via the AskUserQuestion "Other" path, accept it.)

## Step 9 — Ad format

`AskUserQuestion`:
- 🎥 Video
- 🖼️ Image
- 🏷️ Promo

## Step 10 — Breakthrough memo

Prompt:
```
Last step: the breakthrough memo. This becomes the task description.

Cover all three:
- **WHAT** — what is the ad you're building?
- **WHY** — why are you doing it?
- **HOW** — how do you plan on doing it? (inspo links, format details, hooks, etc.)

Paste your memo in one reply.
```

**Format the memo lightly** when writing it to the description: bold the WHAT/WHY/HOW headers, preserve user's lists and links inline, otherwise don't rewrite their content. The "advanced version" that auto-improves memos is future work — for now, keep it minimal.

## Step 11 — Build the task

Call `mcp__claude_ai_ClickUp__clickup_create_task` with:

```
name: <built task name>
list_id: 901405930628
status: "new idea"
markdown_description: <formatted memo>
tags: ["keeps-ed"]  // ONLY when product = 🍆 Keeps ED. Omit for every other product. This is what routes the idea onto the Keeps ED sub-board view.
custom_fields:
  - Votes = 0
  - Client (tasks-relation): {"add": ["<client_task_id>"]}
  - Type (labels): [<type_option_id>]
  - Ad Format (labels): [<format_option_id>]
  - Awareness (drop_down): "<awareness_option_id>"
  - Angle (text): "<user's angle>"
  - Product (labels): [<product_option_id>]  // OMIT if new_product_string set
  - Desire (drop_down): "<desire_option_id>"  // OMIT if new_desire_string set
  - Sub Avatar (drop_down): "<sub_avatar_option_id>"  // OMIT if new_sub_avatar_string set
```

**Ideator:** do NOT pass `assignees` or set the Ideator field. The ClickUp MCP authenticates as the user running the skill, and the workspace automation handles auto-adding them as Ideator. Spencer's automation auto-removes him specifically (user id 12883832) so he gets clean tasks; other team members stay attached.

## Step 12 — Confirmation + dual-write reminders

Always end with a confirmation block in this format:

```
Task created → <task_url>

✅ **Built with:**
- Name: <task name>
- Status: new idea
- Client: <client>
- Product: <product or "(pending — see below)">
- Type: <type>
- Desire: <desire or "(pending — see below)">
- Sub Avatar: <sub avatar or "(pending — see below)">
- Angle: <angle>
- Awareness: <awareness>
- Format: <format>
- Votes: 0
```

**Then, IF any new strings were captured, append the dual-write block:**

```
🚨 **One UI step to finish — copy/paste these and you're done:**

[For each new_desire_string:]
1. Paste this exact string into the `❤️‍🔥 Desire/Core Avatar` dropdown:
   ```
   <new_desire_string>
   ```

[For each new_sub_avatar_string:]
2. Paste this exact string into the `👤 Sub Avatar` dropdown:
   ```
   <new_sub_avatar_string>
   ```

[For each new_product_string:]
3. Paste this exact string into the `📦 Product` label field on the Ads Board:
   ```
   <new_product_string>
   ```
   AND create a matching task in the `📦 Products` master list (list id `901416458668`) with that exact name and the matching 🙍‍♂️ Client assigned.

Once you've added the dropdown/label options, open your new task and select them.
```

If NO new strings → just the confirmation block, no dual-write needed.

## Rules / gotchas

- **Status is `new idea`** — lowercase, exact match required by the API.
- **Numbered desire/sub avatar matching:** accept either the number (`2`) OR the exact string the user copies back. Case-insensitive. If ambiguous, ask.
- **Concept name should NOT include "BATCH#" or the emoji prefix** — the user types just the concept, you build the full title.
- **Product filtering by emoji:** when client = Keeps, show both 🪮 Hair Chew and 🍆 Keeps ED. When client = anything else, products are obvious from the client.
- **Keeps ED tag:** whenever product = 🍆 Keeps ED, the task MUST get the `keeps-ed` tag. ED ideas live in the shared Ads Board but surface on their own "Keeps ED" sub-board view, which filters on this tag. No tag → the idea won't show on the ED board. Apply only to ED; never tag Hair Chew or other products with it.
- **Don't set Ideator manually** — the MCP auto-attaches the authenticated user. Trust the automation.
- **Don't set assignees** on the task. Ideator handles attribution.
- **Memo formatting:** light only. Bold the WHAT/WHY/HOW headers. Preserve user's links, lists, and prose. Do not rewrite or "improve" what they wrote (v1).

## When to refuse / clarify

- If user can't identify which client → ask, don't guess.
- If user picks a product that doesn't match the client (e.g. DFC + Hair Chew) → flag and re-ask.
- If memo is empty or one line → ask them to flesh out at least WHY and HOW before creating the task.
