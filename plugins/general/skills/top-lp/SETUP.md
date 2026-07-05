# Setup — /top-lp

Requires **ClickUp connected to your Claude account** (one-time OAuth) — the same connection `/new-ad-idea` and `/brief-template` use. If `/top-lp` doesn't appear or the ClickUp tools fail, the connection isn't active.

This skill reads from and writes to:
- The **Ads Board** (`901405930628`) — scans briefs, updates the `Links to:` line.
- Each client's **Client Catalog** doc — reads the `Current control:` LP block (read-only).

It changes only the `Links to:` line in a brief's description. It never edits copy, status, names, or other fields.

**When controls/catalogs change:** the LP page IDs and client→catalog mapping live in `SKILL.md` under *Stable IDs*. If a client gets a new catalog (e.g. Keeps ED splitting from the Hair Loss doc), update that table here in `SKILL.md` — one source of truth, do not copy this skill into the workspace folder.
