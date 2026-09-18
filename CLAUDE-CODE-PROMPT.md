# DGGT Manager Hub — handoff for Claude Code

## What this is

`index.html` is the complete, self-contained Dropping Gloves Getting Tugs Manager Hub page.
One file. No build step. No framework. No dependencies except a Google Fonts stylesheet link.

Deploy target: GitHub Pages (Ryan already has access configured).

---

## Task 1 — Ship it to GitHub Pages

Put `index.html` at the repo root, enable Pages on the default branch, done.
Do not add a bundler, a static site generator, or a package.json. This page does not need them.

---

## Task 2 — Updating the draft board (the commit-per-pick workflow)

Near the bottom of `index.html` there is a single block that holds all draft state:

```html
<script type="application/json" id="draft-picks">
{}
</script>
```

The keys are manager names exactly as they appear in the `ORDER` array in the script below it.
The values are draft position numbers, 1 through 10.

When Ryan says something like "Jack took 7", edit that block to:

```html
<script type="application/json" id="draft-picks">
{"Jack": 7}
</script>
```

Then commit and push. That's the whole update. The page reads the block on load and:

- marks position 7 as taken and struck through in the Pick a Position sampler
- shows 7 next to Jack in the Who Chooses When board
- shows Jack next to position 7 in the Draft Position board

Rules:
- Never assign the same position to two managers. Validate before writing.
- Manager names must match `ORDER` exactly, including "New manager 1" and "New manager 2".
  When the two new managers are named, update BOTH the `ORDER` array and the Keeper Tracker
  table rows in the Keepers section so the names match everywhere.
- Commit message convention: `draft: Jack takes position 7`

---

## Task 3 — Add NHL images

The page currently has two example stat blocks in the League Settings > Categories section:

- Skaters — Roope Hintz, DAL, 2025-26
- Goalies — Jake Oettinger, DAL, 2025-26

Each is a `<div class="catbox">` containing an `<h4>`, a `<div class="who">` line, and a stat table.

Ryan wants a team logo and a player headshot on these. On GitHub Pages external images load
fine, but committing the image files to the repo is more reliable than hotlinking NHL's CDN,
which can move or rate-limit.

Suggested markup, added inside each `.catbox` above the `<h4>`:

```html
<div class="playerhead">
  <img class="headshot" src="assets/hintz.png" alt="Roope Hintz" width="72" height="72">
  <img class="teamlogo" src="assets/dal.svg" alt="Dallas Stars" width="40" height="40">
  <div>
    <h4>Skaters</h4>
    <div class="who">Roope Hintz, DAL, 2025-26</div>
  </div>
</div>
```

Suggested CSS, added near the `/* category example tables */` comment:

```css
.playerhead{display:flex;align-items:center;gap:var(--s3)}
.playerhead .headshot{border-radius:50%;background:var(--surface-2);flex:none}
.playerhead .teamlogo{flex:none}
@media (max-width:520px){ .playerhead .teamlogo{display:none} }
```

Keep the existing `<h4>` and `<div class="who">` content unchanged, just nest them.

Image sources Ryan can supply or you can fetch on his instruction:
- Headshots: `https://assets.nhle.com/mugs/nhl/20252026/DAL/8478449.png` (Hintz)
  and `.../8479979.png` (Oettinger)
- Team logo: `https://assets.nhle.com/logos/nhl/svg/DAL_light.svg`

Save them under `assets/` in the repo and reference them by relative path.
Add `assets/DAL_dark.svg` and swap via a `prefers-color-scheme` rule if the light logo
disappears on the dark theme.

---

## Things to know before editing

**Design system.** Everything is tokenized at the top of the `<style>` block. There are eight
type sizes (`--t-xs` through `--t-3xl`), a six-step spacing scale (`--s1`–`--s7`), two radii,
and a full color palette defined three times: once on bare `:root` for light, once under
`prefers-color-scheme: dark` guarded by `:root:not([data-theme="light"])`, and once under
`:root[data-theme="dark"]`. If you add a color or a size, add a token. Do not hardcode values.

**One typeface.** Archivo, weights 400–800. Hierarchy comes from weight, size and color only.
No second font. No all caps, no letter-spaced labels.

**No em dashes or en dashes anywhere in the copy.** This is deliberate. Use commas, periods
or restructure.

**The rink** in the Roster subsection is hand-authored inline SVG on an 860×810 viewBox.
Position chips are `<g><rect class="chip-bg">` plus `<text class="chip-t">`. It is themed
through CSS classes, not fill attributes, so it works in both light and dark.

**The draft numbering.** Rounds 1, 2 and 3 are the keepers (FW, D, G), so they take picks
1 through 30 and nobody drafts in them. Drafting starts at round 4, pick 31, with draft
position 1 going first, and snakes from there. Position 1 gets 31 and 50, position 10 gets
40 and 41. The script holds this as `KEEP_ROUNDS`, `DRAFT_ROUNDS` and `OFFSET`; the pick pool
is 180. The `.scale` labels under the tick strip and the shaded keeper band on it both follow
from those constants, but the scale labels are literal text in the markup, so change them by
hand if the pool size ever changes.

**The playoffs table** colors the Draft Position Decision column with `--v1` (green, best)
through `--v10` (red, worst) to show value dispersion. Both themes have their own ramp.

**Bullet lists** use `ul.plain` with an absolutely positioned dot, specifically so inline
`<strong>` and `<em>` do not break the layout. Do not convert them back to grid.

**Anchor scrolling.** `section[id]` carries a `scroll-margin-top` equal to the sticky nav
height. `#top` overrides it to 0. If you change the nav height, change that value.

---

## Copy rules

Ryan writes in his own voice and is sensitive to AI-sounding prose. If you are asked to write
or edit copy on this page:

- Plain and direct. Say the thing, do not set it up.
- No aphorisms, no "X is not Y, it's Z" constructions, no stacked short fragments for drama.
- Use the league's own vocabulary: "draft position" (never "column" or "slot"),
  "losers bracket winner" (never "consolation winner"), "adds and drops" (never "the wire"),
  "pre-draft", "keeper lock", "the board".
- The explainer inside the changes card is Ryan's own writing. Do not rewrite it.

---

## Dates

Nothing is TBD any more. The draft position selection row was removed.

Every row that has a clock time is a `<time datetime="...">` holding the moment in UTC, with
the ET wording as its text. The "Show times in my timezone" button rewrites those rows into
the viewer's own zone using `Intl.DateTimeFormat().resolvedOptions().timeZone`. That needs no
permission prompt, so do not swap it for the Geolocation API. If you change a time, change
BOTH the `datetime` attribute and the visible ET text, or the two will disagree.

Set by Ryan, all ET, all 2026:
- Dues due, September 24, 11pm
- Keepers lock, September 24, 11pm
- Pre-draft for unkept D and G, September 26, 11am
- Draft day, September 27, 8pm

Confirmed and sourced from the NHL API:
- Scoring begins September 29, 2026 (NHL opening night)
- Trade deadline March 3, 2027
- Playoff rounds run March 22 through April 10, 2027 (the last day of the NHL regular season)
