---
name: brief-template
description: Adds the client brief template scaffold to Ads Board tasks that don't have one yet. Scans the board across five statuses (waiting for brief, new request, waiting for content, ready for editing, brief edits), finds tasks whose description lacks the Creatives / Links to / Copy + Headlines scaffold, and inserts that client's template — pulled live from their ClickUp Client Catalog — ABOVE the existing memo (or on its own if the task has no memo yet), all fields left blank. Does NOT pair copy or LP links (that is a separate, future command). Use when the user says "/brief-template" or asks to add the brief template to tasks.
---

# Brief Template

You add the **blank** brief template scaffold to Ads Board tasks that don't have a template yet. The template is pulled live from that client's **Client Catalog** doc — nothing hardcoded.

**Scope (important):** this skill ONLY adds the empty template scaffold. It does **not** fill in copy or landing-page links — pairing copy into blank templates is a separate, future command. Leave every template field blank for Kat to fill.

**Target list:** `901405930628` (`Ads Board`, in `EASY A CLIENT SPACE` › `All Clients`)
**Statuses to scan** (lowercase, exact — all five): `waiting for brief`, `new request`, `waiting for content`, `ready for editing`, `brief edits`. (Kat's shorthand "ready to be edited" = the board's `ready for editing` status.)
**Placement rule:** the template goes **ABOVE** the existing breakthrough memo. Never overwrite or reword the memo — read it first and keep it verbatim below the template. If the task has no memo yet (empty description), write the scaffold on its own.

## Stable IDs

**Client relation field** (on each task, tells you the client): `e902d22f-6086-4503-b1e9-5a7f43fbf779`

**Client card → Client Catalog doc:**
| Client | Client card ID | Catalog doc ID |
|---|---|---|
| 🦮 Dog Friendly Co | `86b4utu8d` | `a0jtq-34014` |
| 🪮 Keeps Hair Loss | `86b7tprvj` | `a0jtq-192774` |
| 🍆 Keeps ED | `86bagc4vb` | `a0jtq-200534` (own catalog — has its own 📌 TASK FORMAT TEMPLATE page) |
| 💊 Keeps TRT | `86b9upqvb` | `a0jtq-196894` |

If a task's client isn't in this table, tell Kat and ask which catalog to use — don't guess.

## Step 1 — Scan and classify

Call `mcp__claude_ai_ClickUp__clickup_filter_tasks` with `list_ids: ["901405930628"]`, `statuses: ["waiting for brief", "new request", "waiting for content", "ready for editing", "brief edits"]`. For each task fetch its description + client (`mcp__claude_ai_ClickUp__clickup_get_task` with `include: ["description", "custom_fields"]`).

Classify each:
- **Needs a template (the target):** the description does NOT contain the scaffold — i.e. it has neither `Links to:` nor `Copy + Headlines:`. This includes tasks that are just the memo (e.g. `WHAT` / `WHY` / `HOW`) AND tasks with an **empty description but a Client set** (common for UGC tasks — they get the scaffold on its own). → **process.**
- **Already has a template — SKIP:** the description already contains `Copy + Headlines:` (whether the copy fields are blank OR filled). A blank template still counts as "has a template" — this skill does not touch it. Filling those is the separate copy command's job.
- **Not a brief — SKIP:** `Untitled` tasks with **no Client set**, and **edit-static tasks** where `Edit#`/`EDIT#` is the task's own type token (e.g. `Keeps - Edit#30 - Speed Statics`). Do NOT skip a real `BATCH#…` brief that only references an edit as its source in an `ITER#…_EDIT#…` suffix (e.g. `…BATCH#100… - ITER#1_EDIT#58`) — that's a target. (An empty description WITH a client is still a target — see above.)

Present the tasks that need a template as a numbered list (name + client + status). Ask which to do — accept a single number, several, or `all`. If none need one, say everything across those statuses already has a template and stop.

## Step 2 — Resolve client + catalog (per task)

Read the task's **Client relation** field → map the client card ID to the catalog doc ID via the table above.

## Step 3 — Pull the template (live)

Call `mcp__claude_ai_ClickUp__clickup_list_document_pages` on the catalog doc (`max_page_depth: -1`). Find the page whose name contains **`TASK FORMAT TEMPLATE`**. Get it with `mcp__claude_ai_ClickUp__clickup_get_document_pages` (`content_format: "text/md"`).

Use that page's body as the scaffold, **verbatim**, with one change: strip any instruction line like `COPY + PASTE THE TEMPLATE BELOW`. Keep everything else exactly as the catalog has it — including the `Links to: Top performing LP(s)` placeholder and (for DFC) the Persona/Desire/Motivator/Angle block. Each catalog carries the right shape for its client (DFC long w/ persona block; Keeps short). Do NOT fill any field.

DFC template for reference (always read it live):
```
**Creatives:**

**Links to:** Top performing LP(s)

**Copy + Headlines:**

---
Persona/identity (id):
Desire (de):
Core emotional motivator (cm):
Angle description (an):
---
```

## Step 4 — Assemble and write

Take the task's existing description (the memo) verbatim. Build:
1. The blank template scaffold from Step 3.
2. A `---` divider.
3. The original memo, unchanged, below it.

**If the task has no memo (empty description):** write just the blank template scaffold — no divider, no memo.

Write with `mcp__claude_ai_ClickUp__clickup_update_task` → `markdown_description`. **Do not change status, name, or any other field. Do not alter the memo's words.**

## Step 5 — Confirm

For each task processed:
```
✅ Template added → <task_url>
- Client: <client>
- Template: pulled blank from <catalog name>
- Memo: preserved below
```
Several at once → one block each + a final count.

## Rules / gotchas

- **Scaffold only — no copy.** Never fill `Copy + Headlines`, `Links to`, `Creatives`, or the persona block. They stay blank. Copy pairing is a separate command.
- **Memo is sacred.** Keep the existing memo verbatim, below the template. If you can't read the existing description, stop and ask — never risk wiping it.
- **A blank template = already done for this skill.** Only tasks with NO scaffold at all are targets. Don't add a second template to a task that already has one (even an empty one).
- **Live pull, every time.** Always read the catalog's own template page fresh; never paste a remembered version.

## When to refuse / clarify

- Task's client isn't in the mapping table → ask which catalog, don't guess.
- Catalog has no `TASK FORMAT TEMPLATE` page → tell Kat, don't improvise a template.
- Kat picks a task that already has a template → say so, don't re-add.
