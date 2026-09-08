# Pipeline — Room planner redesign

Updated: 2026-09-08

| Stage | Status | Deliverable |
|---|---|---|
| Research | done | `research/panel-research.md` |
| Gate 1, Frame | done, logged in decisions.md | recommendation (c) accepted |
| Concept, ui-designer | running (opus) | restructured `index.html` + `brand.md` |
| Gate 2, Direction | not started | clickable prototype |
| QA, design-qa | not started | `qa/qa-report.md` |
| Fix cycle | not started | — |
| Gate 3, Ship | not started | merged to master, published |

**Next action:** when the designer reports, verify the deliverables exist on disk and the app still works, then run design-qa. Gates 1 and 2 are being presented together as one pack.

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
