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
