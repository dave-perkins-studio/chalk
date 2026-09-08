# Handoff — Room planner redesign, app shell and three-place panel

- Status: complete
- Role: ui-designer · Last updated: 2026-09-08 16:40
- Project: /Users/davidperkins/Code/personal/room-planner (branch `redesign`)
- Read first: brief.md · decisions.md · research/panel-research.md ·
  brand.md · perkins-brand skill `reference/excursion-web.md` §7 and §9

## Task

A build, not a mockup. Restructure
`/Users/davidperkins/Code/personal/room-planner/index.html` (one self-contained
file, no build step) into a full-page app shell, reorganise the right panel
from six flat sections into three places, add an empty state, and fix the
overlapping-openings bug. Write `brand.md` recording every deviation from
Excursion. Commit on `redesign`; do not push, do not merge.

## Done

**Layout.** The `.hold` 1120px column is gone. The page is now a shell: header
bar, bolted rule, then a two-column body. On screens 900px and wider, `html` and
`body` are `overflow:hidden`, the shell is exactly the viewport height, and the
panel scrolls inside its own column. The `position:sticky` canvas hack is
deleted. Below 900px it stacks and the page scrolls, as before. The SVG sits in
a `.canvas-stage` and is absolutely positioned inside it on desktop, so the plan
takes the height the flex row hands it.

**Panel, six sections to three places.**
1. Furniture, always open: the list, with the selected item's fields expanded
   inside its own row, then Add a piece and Common sizes in the same block.
2. Doors and windows, always open.
3. Room setup and Share, native `<details>`, collapsed, each with its `<h2>`
   inside the `<summary>`.
The old page footer (ticket + Clear saved plan) is now the foot of the panel's
own scroll.

**The row editor.** `#selected-section` keeps all its ids and its nodes; the
render moves it into the selected `<li>`. The list is only rebuilt when its
shape changes (which items exist, in what order, which is selected) — typing in
a field takes a cheap in-place text refresh instead, so nothing under the cursor
is destroyed. `focusedRow`/`restoreRowFocus` now also remember a focused editor
field and its caret position.

**Empty state.** `#empty-note` under the plan caption when the room has no
furniture. The panel list says "Nothing on the list yet."

**Bug fixed.** `openingClashes()` finds openings sharing a stretch of the same
wall. Flagged in words in the row ("overlaps the door on the same wall") and
drawn with a new `.is-clashing` class, a sibling of `.is-off-wall` in the same
berry grammar, and added to the PNG export's colour map.

**Also.** `reveal()` opens a collapsed `<details>` before a message is printed
into it or a field inside it takes focus (New room, Delete room, corner cut,
paste errors). Removed 60 lines of JavaScript that had been pasted into the
`<style>` block by mistake; it was on `master` too, parsed as junk CSS.

## Tests run, all on http://127.0.0.1:8732/index.html in Chrome

1. Sample plan loads, both rooms switch, L-shaped Living room draws correctly
   (`0,0 370,0 370,120 520,120 520,400 0,400`).
2. Add, drag, R, arrows, Delete, Cmd+Z all work; status line live throughout.
3. Editing X in the panel: canvas follows, `document.activeElement` stays
   `sel-x`, flag text updates in place.
4. Markdown round-trips byte-identically, including `samples/example-plan.md`
   re-serialised against the file.
5. PNG export returns a real 3200 × 2600 `image/png`.
6. Tab from the skip link to the end: order is header, toolbar, SVG items,
   furniture rows, add block, openings, Room setup, Share, share buttons, foot.
   Never trapped; GOV.UK focus block confirmed on the summaries.
7. 1280, 900 and 375 wide: no horizontal overflow. Dark mode correct, no
   hard-coded colour.

## Not done

- `file://` not exercised in the browser tool (it refuses to script local
  files). Nothing in the change depends on the origin — no fetch, no modules,
  no new external requests — so the behaviour should be identical to `master`.
  Worth one manual open before merge.

## Next action

Gate 2 review of the branch. Serve the folder and open it, or run design-qa
against `/Users/davidperkins/Code/personal/room-planner/index.html` on branch
`redesign`. Nothing else is pending in this file.

## Then

Merge to `master` after Gate 2, which republishes the GitHub Pages site.
