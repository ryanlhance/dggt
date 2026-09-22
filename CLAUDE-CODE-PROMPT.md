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

## Task 2 — Updating the draft board (parked)

The Who Chooses When and Draft Position boards are commented out for the 26-27
reset, so this workflow is dormant. The `#draft-picks` JSON block and the script that
reads it are both still in place and still work; the boards just are not on the page.
See PARKED.md before switching it back on. The `ORDER` array still holds the ten names
from the old league and needs rewriting to the new eight managers first.

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

**The draft numbering.** Eight teams, eighteen rounds, no keeper rounds, so the sampler
runs R1 through R18 over picks 1 to 144. The script holds this as three constants:

    var TEAMS=8, KEEP_ROUNDS=0, DRAFT_ROUNDS=18, TOTAL=TEAMS*DRAFT_ROUNDS;

Everything else follows from them. The sampler grid gets its column count from `--cols`
and `--cols-sm`, the keeper bar only renders when `KEEP_ROUNDS` is above zero, and the
keeper pills come from `KEEP_SLOTS.slice(0, KEEP_ROUNDS)`. So changing the number of teams
or bringing keepers back is a constant change, not a markup change.

There is deliberately no number scale under the tick strip; the pills below carry every
pick number already.

**The playoffs table** (parked, see PARKED.md) colors the Draft Position Decision column with `--v1` (green, best)
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

Every row that has a clock time is a `<time datetime="...">` holding the moment in UTC, with
the ET wording as its text. The "Show times in my timezone" button rewrites those rows into
the viewer's own zone using `Intl.DateTimeFormat().resolvedOptions().timeZone`. That needs no
permission prompt, so do not swap it for the Geolocation API. If you change a time, change
BOTH the `datetime` attribute and the visible ET text, or the two will disagree.

Set by Ryan:
- Dues due, 11pm ET September 25, 2026
- Draft day, TBD

Keepers lock and the pre-draft came off the calendar in the 26-27 reset. See PARKED.md.

Confirmed and sourced from the NHL API:
- Scoring begins September 29, 2026 (NHL opening night)
- Trade deadline March 3, 2027
- Playoff rounds run March 22 through April 10, 2027 (the last day of the NHL regular season)
