# Job Hunt System — guide for Claude

This folder is a small, customizable job-search dashboard. Four plain HTML files (no build step, no dependencies) plus this guide. Your job as the agent: **set it up for the user, keep it current, and score roles against their scorecard.** Everything below applies across all the pages.

## The pieces
| File | What it is | Where the user's data lives |
|------|------------|-----------------------------|
| `index.html` | Home page linking the three tools | static — rarely edited |
| `scoreboard.html` | Daily task tracker (calendar + checklist + streak) | the **CONFIG** block at the top of its `<script>` |
| `aligned-roles.html` | Job board scored against the user's criteria | the `.job-card` entries in the body |
| `the-map.html` | One-page vision (outcome → talent) | the `<text>` nodes inside the SVG |
| `your-scorecard.md` | The criteria roles are graded against | the whole file |

## When the user says "set up my job hunt system"
First, **act like a thoughtful career coach** — understand the person and their bigger picture *before* you fill anything in. Ask conversationally, a couple at a time, reflect back what you hear, and confirm before writing. The user is likely non-technical; never make them touch code. Keep it warm and brief — a 5-minute conversation, not an interrogation.

**Understand the bigger picture first:**
- *Why are you looking right now — what's pushing or pulling you toward a change?*
- *What stage of the hunt are you at — just starting, actively applying, interviewing, or weighing offers?*
- *Zoom out: what's the bigger-picture vision this next role is in service of?*

**Then get specific enough to fill the dashboard:**
- *What role(s) or title(s) are you going after, and what does the ideal version look like — people to lead, execs to convince, hands-on building?*
- *What are your hard non-negotiables? (pay floor, remote/location, a mission you believe in, time boundaries)*
- *Given all that — what would make this hunt a success? Shape it into the **one or two Wildly Important Goals** for this sprint (from-X-to-Y, with a date). These become the spine of both the map and the scoreboard.*
- *And what does a normal "do the work" day look like — the 2–4 tasks that would earn a green check?*

Then fill the pages in this order (read each file first, then edit only the data):
1. **Scorecard first** (`your-scorecard.md`) — their target role, non-negotiables, comp floor, company profile. Everything else leans on this.
2. **The map** (`the-map.html`) — their ultimate outcome, long-game vehicle, current moves, identity, core talent, and their **two** Wildly Important Goals. Edit the `<text>` nodes; keep the tree structure.
3. **The scoreboard** (`scoreboard.html`) — in the CONFIG block (look for `EDIT THIS: your plan`): set `START`/`END` (their sprint window), and the `ITEMS` daily tasks for their two goals (tag tasks `WIG 1 · ...` and `WIG 2 · ...` so they group correctly). `ANCHORS` = daily habits. `REST_WINDOWS` / `RECOVERY_WINDOWS` = stretches they want lighter. `DAY_OVERRIDE` = optional specific-date plans.
4. **Aligned roles** — run the grading job below and replace the two example cards.

## The "grade me" job (the core loop)
When asked to find or score roles:
1. Read `your-scorecard.md` — that is the rubric. Do **not** substitute generic advice.
2. For each role: apply the scoring rules in the scorecard's section 5 (fail-fast on non-negotiables, else score 1–10, one-line "Why you", honest caveat).
3. Write each as a `.job-card` in `aligned-roles.html` — copy the example card's markup exactly (rank, title + `@ company`, chips, bullets, `.fit`, apply link). Group strongest first.
4. Update `Last updated:` in the freshness line.

## Data shapes (when editing the scoreboard CONFIG)
- **A task** is an object: `{ id:"unique-id", tag:"WIG 1 · job", title:"6 outreach messages", note:"short why/how" }`. The `tag` must start with `WIG 1` or `WIG 2` so the task groups under the right goal; `note` is optional context. Only `title` shows on the card.
- **`ITEMS`** holds the default daily tasks, split by day type (`full` = high-power, `recovery`). Each has `required` (earn the green check) and `bonus` (extra).
- **`ANCHORS`** are everyday habits. **`DAY_OVERRIDE`** maps a date string to a custom task list for that one day. **Window arrays** (`REST_WINDOWS`, `RECOVERY_WINDOWS`) take `[["YYYY-MM-DD","YYYY-MM-DD"], ...]` date ranges.

## Day-type model (scoreboard)
Days are **High** (⚡), **Recovery** (🌿), or **Rest** (🌙). The user sets them: `defaultType()` returns rest on Sundays + `REST_WINDOWS`, recovery in `RECOVERY_WINDOWS`, else High. They can also tap the chips on any day to override. Rest days count as a win (the streak keeps growing).

## Conventions — keep these
- **Don't redesign.** Keep the forest-green/lime theme, the fonts, and the working JS (calendar, streak, progress ring, backup/restore). You're editing *data*, not the engine.
- **Progress is local.** Each page saves to `localStorage`; there's no backend. Don't add one.
- **No secrets, no personal data in commits.** If the user forks this for their own real use, their filled-in personal version should stay private (or gitignored) — this public copy stays a clean template.
- Each page links to the others via the nav pills (home · scoreboard · aligned roles · the map). Keep those links working if you rename files.
