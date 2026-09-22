# Parked

Things pulled off the page for the 26-27 reset that Ryan may want back.
Nothing here is deleted. The markup still sits in `index.html` inside
`<!-- PARKED: ... -->` comments, so bringing a piece back means deleting the
comment wrapper around it. This file is the index of what is parked and what
else has to change to switch it on again.

---

## Changes to the season card

`index.html`, inside `<section id="top">`. The whole `.alert` block, including
the "Explain that to me" details and Ryan's own explainer inside it.

To bring back: uncomment. Nothing else depends on it.

---

## Keepers section

`index.html`, the whole `<section id="keepers">`, including the Keeper Tracker
table with the ten manager rows.

To bring back: uncomment, and put the Keepers link back in the nav.

---

## Keeper rounds in the Pick a Position sampler

Not commented out, driven by a constant. In the script:

    var TEAMS=8, KEEP_ROUNDS=0, DRAFT_ROUNDS=18;

Set `KEEP_ROUNDS` to 3 and `DRAFT_ROUNDS` to 15 and the keeper rounds come
back on their own: the R1 FW, R2 D, R3 G pills reappear at the front of the
pill list and the shaded "Keepers" bar reappears at the front of the tick
strip. `KEEP_SLOTS` holds the position labels. Pick numbers stay 1 through
`TEAMS * DRAFT_ROUNDS` either way.

---

## The picks-between summary

`index.html`, the `<p class="summary" id="summary">` under the sampler. It
rendered as, for example, "18 picks between your first two picks #1 and #20".

To bring back: uncomment. The script already fills it when the element exists.

---

## Draft position selection copy

Two paragraphs that sat under the sampler:

> Your draft position is determined by you, based on how your post-season went.
> The last place player in the regular season picks their position first. Then
> the second to last regular season player. Then the winner of the playoffs
> losers bracket. Then 6th, 7th, 8th in playoffs. Then 4th, 3rd, 2nd, 1st. You
> can see this year's order below.

> Use the Pick a Position sampler above to see what available position would
> serve your strategy best. As positions are chosen, they will come off the
> board and be shown in the table on the right.

This is Ryan's own writing. Do not reword it if it goes back up.

---

## Who Chooses When and Draft Position boards

`index.html`, the `.boards` block with `#orderboard` and `#positionboard`.

To bring back: uncomment. The script already fills both when the elements
exist, and reads the `#draft-picks` JSON block to do it. The `ORDER` array
still holds the ten names from the old league, so it needs rewriting to the
new eight managers first.

---

## Playoffs table

`index.html`, the `.tablewrap` inside `<section id="playoffs">`. Payouts, the
draft position decision column with its value ramp, and the dues column.

To bring back: uncomment. Note it is built for ten finishing places.

---

## Dates that came off the calendar

- Keepers lock, was 11pm ET September 24, 2026
- Pre-draft for unkept D and G, was 11am ET September 26, 2026

Each row needs both the `datetime` attribute in UTC and the visible ET text.
