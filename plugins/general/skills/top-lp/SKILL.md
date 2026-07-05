---
name: top-lp
description: Adds the top control landing page(s) to active in-flight briefs on the ClickUp Ads Board that have the brief template but a blank "Links to:" line. Scans the board, matches each brief to its product/region control LP pulled live from that client's Client Catalog, previews every match for your approval, then swaps only the "Links to:" line — leaving the memo, copy, and every other field untouched. This is the LP half of the planned copy/LP pairing flow; it does NOT add ad copy. Use when the user says "/top-lp" or asks to add the top/control LP to briefs on the Ads Board.
---

# /top-lp

You add the **top control landing page(s)** to active briefs on the Ads Board that have a template but no LP yet. The LP is matched to each brief's **product/region** and pulled **live** from that client's **Client Catalog** — never hardcoded. You always **preview the matches and get approval before writing.**

**Scope (important):** this skill fills ONLY the `Links to:` line. It does **not** touch `Copy + Headlines`, `Creatives`, the persona block, or any custom field. Copy pairing is a separate, future command — never auto-add copy here.

## Stable IDs

**Target list:** `901405930628` (`Ads Board`, in `EASY A CLIENT SPACE` › `All Clients`)

**Statuses to scan (default):** `waiting for brief`, `new request`, `waiting for content`, `ready for editing`, `brief edits`, `needs edits`, `in review`
**Never scan:** `new idea` (pre-template stubs), `sent to client` (shipped).

**Client relation field** (tells you the client): `e902d22f-6086-4503-b1e9-5a7f43fbf779`
**Product field** (tells you which LP within a client): `087af852-c7b7-4f9f-b785-7d2fd0e72700`
Product options: `🪮 Hair Chew`, `🦮 Supplement`, `🦮 Paw Protector`, `🦮 Harness`, `🦮 Dog Bed`, `💊 Keeps TRT`, `🐶 PawMergency`, `🍆 Keeps ED`.

**Client → Client Catalog doc → Landing-Pages page** (read the LP page LIVE each run):

| Client | Client card ID | Catalog doc ID | LP page ID | LP page name |
|---|---|---|---|---|
| 🦮 Dog Friendly Co | `86b4utu8d` | `a0jtq-34014` | `a0jtq-44354` | 🧪 Landing Pages |
| 🪮 Keeps Hair Loss | `86b7tprvj` | `a0jtq-192774` | `a0jtq-194274` | 🧪 Keeps HL LPs |
| 🍆 Keeps ED | `86bagc4vb` | `a0jtq-192774` | `a0jtq-200134` | 🧪 Keeps ED LPs |
| 💊 Keeps TRT | `86b9upqvb` | `a0jtq-196894` | `a0jtq-199114` | LP Control |

If a brief's client isn't in this table, tell Kat and ask which catalog/LP to use — don't guess.

## What the targets look like

A brief that needs an LP has the template scaffold **and** the LP line is still the blank placeholder:

```
**Links to:** Top performing LP(s)
```

(or `Links to:` with nothing after it). A brief that already has real URLs after `Links to:` is **already paired — skip it.**

---

## Step 1 — Scan and classify

Call `mcp__claude_ai_ClickUp__clickup_filter_tasks` with `list_ids: ["901405930628"]` and the default `statuses` list above.

For each task, decide:
- **EXCLUDE — not in scope:** **edit-static tasks** — where `EDIT#`/`Edit#` is the task's own type token (e.g. `Keeps - Edit#30 - Speed Statics`, `Keeps TRT - EDIT#2 - Monkey Math`); their LP handling is inconsistent, leave them manual. **Do NOT exclude a real BATCH brief** that merely references an edit as its source in an `ITER#…_EDIT#…` suffix (e.g. `…BATCH#100… - ITER#1_EDIT#58`) — that's a target. Also exclude `Untitled` / test tasks / no client set; any non-current client (e.g. Vetanica — churned; NuroJit, 18 Chestnuts — stale non-clients).
- **Fetch the rest** to classify. Read the description with `mcp__claude_ai_ClickUp__clickup_get_task`, `include: ["description"]` (lean — avoids the huge custom-field payload). For **Dog Friendly Co** briefs, also `include: ["custom_fields"]` so you can read the **Product** field (harness vs bed vs other → different LP). For Keeps briefs the client alone determines the LP, so the description read is enough.
- **TARGET:** description contains `Links to:` AND the LP is still the blank placeholder (`Top performing LP(s)` or empty). → process.
- **SKIP — already paired:** `Links to:` is followed by real URL(s).
- **SKIP — no template:** description has no `Links to:` line at all (it's just a memo). These need `/brief-template` first — flag them, don't improvise.

## Step 2 — Resolve the LP per brief (live pull)

Group the targets by client. For each client involved, read its LP page once with `mcp__claude_ai_ClickUp__clickup_get_document_pages` (`content_format: "text/md"`) using the page ID above. Take the LP(s) from the **`Current control:`** block at the top. **Ignore struck-through / deprecated links.**

Match the LP to the brief's product/region:
- **Keeps Hair Loss** → the HL control (e.g. `keeps.com/hair-treatment`).
- **Keeps ED** → the ED control (e.g. `keeps.com/ed-treatment`).
- **Keeps TRT** → the TRT control (e.g. `trt.keeps.com`). Use the clean control URL, not the UTM-tagged variant, unless Kat says otherwise.
- **Dog Friendly Co** → match the **Product** field to the catalog section: `🦮 Harness` → the HARNESS block (include **every** region listed, e.g. US + AU); `🦮 Dog Bed` → the BEDS block; other DFC products → the matching section (ask Kat if unclear). Keep the region labels (`US:` / `AU:`) exactly as the catalog has them.

These controls change — always read them fresh. Reference snapshot (2026-06-20, verify live): HL `keeps.com/hair-treatment`; ED `keeps.com/ed-treatment`; TRT `trt.keeps.com/`; DFC harness `dogfriendlyco.com/pages/easy-on-v1` + `/pages/easy-on-v1-no-pull-bundle` (US) + `dogfriendlyco.com/collections/dog-harnesses` (AU).

## Step 3 — Preview and get approval

Present every target as a numbered list — name, client, product, and the exact LP(s) you'll insert:

```
Briefs ready for an LP (top control, matched to product):

1. 🪮 HL BATCH#91 — Explaining Hair Loss CTE   → keeps.com/hair-treatment
2. 🦮 DFC BATCH#225 — Your Trainer Did You Dirty → easy-on-v1 + bundle (US), collections (AU)
...

Reply 'all', or the numbers to apply (e.g. 1,3,5).
```

Note any SKIPs (already paired / no template) and EXCLUDEs (EDIT statics, non-clients) in one line so Kat sees the full picture. **The board's "Pod A" tab is a saved view whose filter can't be read via the API, and the Ads Team field is blank — so this skill lists all active client briefs and Kat picks the subset she wants here.** Wait for her selection before writing anything.

## Step 4 — Write (approved only)

For each approved brief, take its existing `markdown_description` and replace **only** the LP line. Format the LP(s) as markdown links, one per line; for multi-region keep the `US:` / `AU:` labels. Match the brief's existing formatting — `**Links to:**` (bold) or `Links to:` (plain). Example:

```
**Links to:**
[https://www.keeps.com/hair-treatment](https://www.keeps.com/hair-treatment)
```

Write with `mcp__claude_ai_ClickUp__clickup_update_task` → `markdown_description`. **Change nothing else** — preserve the memo, the `Creatives:` line (and any existing Creatives URL), the persona block, and the empty `Copy + Headlines:` section verbatim. Do not change status, name, or any field. Do not add copy.

## Step 5 — Confirm

For each brief written:
```
✅ LP added → <task_url>
- Client / product: <client> / <product>
- LP: <the link(s) inserted>
```
End with a count, and re-list any SKIPs that need `/brief-template` first.

---

## Quality Check

Before finishing, verify:
- [ ] Only the `Links to:` line changed on each brief — memo, Creatives link, and copy section are byte-for-byte intact.
- [ ] Each LP matches the brief's client AND product (DFC harness ≠ bed; right Keeps brand).
- [ ] LPs came from a **live** read of the catalog's `Current control:` block (no struck-through links, no stale snapshot).
- [ ] No `Copy + Headlines` were filled. No status/name/field other than the description changed.
- [ ] EDIT statics, already-paired briefs, and non-current clients were left untouched.

If anything is off, fix it and tell Kat what to adjust so the skill can be updated.

---

## Notes / gotchas

- **LP only — never copy.** Copy pairing is a separate command and must be hand-picked to the concept ([[feedback_no_auto_copy_on_briefs]]). This skill stops at the LP.
- **Live pull every run.** Controls rotate; struck-through links are retired — skip them.
- **Memo + Creatives are sacred.** Keep them verbatim. If you can't read a brief's description, stop and ask — never risk wiping it.
- **EDIT# statics are out of scope** by rule (their LPs are added inconsistently). Do them manually only if Kat names one.
- **Pod A view:** can't be read via API; preview the full active-client list and let Kat choose. If she later wants pod targeting automated, the Ads Team field would need to be populated first.
- **Pairs with `/brief-template`:** that skill adds the blank scaffold; this one fills the LP line in it. A brief with no scaffold at all should run `/brief-template` first.
