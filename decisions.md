# Decisions

## 2026-09-08 — Studio project opened

Brief: redesign the interface for clarity, full-page experience, better right
panel, clearer sections. Deviation from the Excursion web brand is permitted
where it buys clarity and is documented.

Pipeline: ux-researcher (patterns + heuristic review) → Gate 1 → ui-designer
concept built as a working branch → Gate 2 → design-qa → fix cycle → Gate 3.

Rule 6a satisfied: `perkins-brand` exists and is the starting point. Any
deviation gets written into `brand.md` in this folder as a delta, not a
replacement.

Work happens on branch `redesign`, so `master` and the live Pages site keep
working throughout.

## 2026-09-08 — Gate 1, Frame. Research accepted with one correction

**Research delivered:** `research/panel-research.md`. Heuristic review of the
live app, six comparable tools (two used hands-on, four marketing pages only
because their editors are account-gated, labelled as such), a task-frequency
ranking and a recommendation.

**Recommendation accepted:** option (c). The furniture list and the selected
item's fields belong together at the top, because moving and turning furniture
is nearly the whole session while room setup happens once. Room setup and
Share collapse by default, on GOV.UK's own accordion rule rather than fashion.

**Two claimed product gaps, verified directly rather than taken on trust:**

| Claim | Verdict |
|---|---|
| Furniture stranded by a corner cut isn't flagged | **Wrong.** It is flagged "outside the room", and the canvas shows it. Re-tested on the live site. The researcher most likely hit the CDN cache serving an older build, the same trap this session hit earlier. |
| Two overlapping doors on the same wall aren't flagged | **Confirmed.** Two doors at 40cm and 60cm, both 80 wide, overlap silently. Real gap, and inconsistent with furniture overlap, which is flagged. |

Only the confirmed one goes into the build.

**Added to the brief by me, beyond the research:**

1. **Go full width.** The app sits in Excursion's 1120px document column with
   large empty margins. A canvas tool should use the screen. This is the
   biggest single "this is an app, not a page" move and the main sanctioned
   brand deviation.
2. **No page scroll on desktop.** Header fixed, plan fills the height it has,
   panel scrolls inside its own column. Today the whole page scrolls and the
   plan is pinned with a sticky hack.
3. **An empty state.** A family member opening the link sees an empty room and
   no instruction. It should say what to do first.

**Gates 1 and 2 are being merged** into one review pack. David is not at the
keyboard, the work is on a branch, master and the live site are untouched, and
a page he can click is worth more than a written proposal he'd have to imagine.

## 2026-09-08 — Gate 2 and 3 approved by David: "run all proposed improvements and updates, merge and publish"

Shipping is authorised, including publishing to the live URL family may hold.

**The designer's open question, resolved.** They asked whether Doors and
windows should collapse once a room has openings, since its add form is six
controls sitting permanently under a two-row list. The research says keep the
list visible, and it is right: you check the door position constantly while
placing furniture near it. But the research's argument was about the *list*,
not the *form*. So: keep the list open, collapse the add form behind its own
`<details>`. That satisfies both, and matches how the furniture block already
works, where the list is open and Common sizes is collapsed.

**Proposed improvements, and their status going into the fix cycle:**

| Proposal | Source | Status |
|---|---|---|
| Selected item's fields inside its list row | research §5 | built |
| Progressive disclosure for once-a-visit sections | research §5, GOV.UK | built |
| Flag overlapping openings on a wall | research §5, verified by me | built |
| Protect the canvas status line | research §5 | kept |
| Full width, app shell, no page scroll | my Gate 1 additions | built |
| Empty state for a first-time visitor | my Gate 1 addition | built |
| Collapse the openings *add form*, keep the list | this decision | to build |
| Everything design-qa raises | QA gate | pending |
| Separate planner per job; bottom library drawer | research §5 | rejected, with reasons in the research |

## 2026-09-08 — QA gate and fix cycle

design-qa returned 1 major and 1 minor, no blockers. Both reproduced, both
fixed. Its report is `qa/qa-report.md`.

**Major, focus left on a hidden field.** QA corrected my own framing usefully:
an ordinary mouse click does not trigger it, because the pointer moves focus
to the button before the handler runs. It does trigger on any activation that
skips that, which is how switch access and voice control invoke a control. So
the people it hurts are exactly the ones least able to recover from it.

The first fix, a visibility guard in `restoreRowFocus`, was not enough. Focus
was being restored while the editor was still on screen, and only hidden a
moment later by `renderSelectedForm`, so it still ended on the body. The real
fix hands focus on *before* hiding, to the first furniture row. Verified for
delete, deselect and remove-while-editing.

**Minor, the room switcher clipped its own labels below 560px.** Fixed by
dropping the dimensions from the option text rather than fighting it with CSS,
which native `<select>` does not support reliably. The size is printed in the
plan caption immediately below, so the long label was repeating itself.

**Also built:** the doors and windows add form now collapses, keeping the list
visible, per the decision above.

**Regression run before merge:** both rooms load, the L-shape draws, rotate,
nudge, duplicate, delete and undo all work, the whole-house Markdown
round-trips including the cut, the PNG exports, overlapping openings are
flagged in words, no console errors, and nothing overflows at 375px.

Merged to master and published, as authorised.

## 2026-09-08 — Renamed to Chalk, and the old address kept alive

The repo and the folder are now `chalk`, and the tool is served at
<https://dave-perkins-studio.github.io/chalk/>.

GitHub redirects a renamed repository but **not** its Pages URL, so
`/room-planner/` began returning 404 the moment the rename went through. A
second public repo, `dave-perkins-studio/room-planner`, now holds nothing but a
forwarding page so any link sent out before the rename still works.
`index.html` and `404.html` there are the same file, so deep links forward too,
and the script carries any query or fragment across.

That repo is a signpost with no working copy under `~/Code`: it has one page,
nothing to maintain, and keeping a folder named after the old name next to the
new one would only confuse the workspace. Clone it if it ever needs editing,
or delete it once the old link stops mattering.
