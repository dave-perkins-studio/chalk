# Pipeline — Room planner redesign

Updated: 2026-09-08

| Stage | Status | Deliverable |
|---|---|---|
| Research | done | `research/panel-research.md` |
| Gate 1, Frame | done, logged in decisions.md | recommendation (c) accepted |
| Concept, ui-designer | done, commit dab31dc | restructured `index.html` + `brand.md` |
| Gate 2, Direction | not started | clickable prototype |
| QA, design-qa | running | `qa/qa-report.md` |
| Fix cycle | not started | — |
| Gate 3, Ship | not started | merged to master, published |

**Next action:** when QA reports, reconcile its findings with the one already
confirmed below, run one fix cycle, then present Gates 1 to 3 as a single pack.
Do not merge to master or publish without David's word: the live URL is shared
with family, so shipping is his call, not mine.

## Verified against disk, not against the summary

- `index.html` on `redesign` is 1,972 lines against master's 1,812. `brand.md`
  and both journals exist.
- The Markdown functions are byte-identical to master: `serialiseRoom`,
  `serialise`, `parsePlan`, `parseCut`, `normalise`. The format is safe.
- The designer's claim of stray JavaScript inside the `<style>` block on master
  is true, and it was my own bug: a string replace matched a comment in the CSS
  as well as the one in the script, duplicating a block of room-switcher code as
  junk CSS. Master has 14 such lines; the redesign branch has none, so the merge
  carries the fix.
- Ran it myself: no page scroll, plan 964px wide at 1440, selecting an item
  expands its fields inside its own row, typing in X keeps focus and the caret
  and the canvas follows.

## Confirmed finding, seeded into QA

Deleting the selected item while focus is in one of its row fields leaves focus
on `#sel-x`, which is then invisible. Worse than landing on the body, because
the browser still believes something is focused.

## Measured before-state (8 Sep, live site, 1280x900)

| Thing | Value |
|---|---|
| Right panel height | 2,183px |
| Screens of scrolling for the panel | 2.4 |
| Form controls visible at once | 25 |
| Buttons in the markup | 18 |
| Panel sections, all expanded | 6 |

## Fixed in passing, on the branch

`[hidden]` was losing to `.field-row{display:flex}`, so the two conditional
field rows never hid: a window opening still offered swing and hinge, and a
rectangular room still showed the cut sizes with a previous L-shape's numbers
in them. Commit 68efe64.

**Notes for whoever picks this up:** the app is one file, `index.html`, about
1,500 lines with CSS and JS inline. `master` is live on GitHub Pages and must
keep working. Never edit `excursion-web.css` here; it is a copy.
