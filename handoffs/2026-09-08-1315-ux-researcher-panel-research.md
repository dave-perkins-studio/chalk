# Handoff — Panel research for room planner redesign

- Status: complete
- Role: ux-researcher · Last updated: 2026-09-08 13:45
- Project: /Users/davidperkins/Code/personal/room-planner
- Read first: brief.md · decisions.md · research/panel-research.md ·
  handoffs/research-journal.md

## Task

Delegated by the studio orchestrator (decisions.md, Gate 1 stage): desk
research on how to reorganise the right-hand panel of the room planner.
Deliverable: one file, `/Users/davidperkins/Code/personal/room-planner/research/panel-research.md`,
covering: heuristic review of the live app, 6–10 comparable-tool teardowns
with sourced screenshots, a synthesis of layout options for the right side,
a task-frequency ranking, stealable ideas, and a one-paragraph recommendation.
Constraints: British English, every product claim sourced with URL + date
seen, under ~2,000 words, no invented stats.

## Done so far

- Read brief.md, decisions.md, handoff-template.md.
- Used the live app hands-on: loaded the sample plan, added and dragged and
  rotated a "Bookshelf", cut a corner over furniture already there, added a
  deliberately overlapping second door, and opened "New room" for the empty
  state. Measured the panel's real height and per-section heights via the
  browser console rather than guessing.
- Found and documented two real product gaps (not just layout friction):
  furniture stranded outside the room after a corner cut isn't flagged, and
  overlapping doors on the same wall aren't flagged either, while
  furniture-on-furniture overlap already is.
- Researched comparable tools live in the browser: full hands-on access to
  Excalidraw and Floorplanner's lighter "Roomstyles" demo; marketing/landing
  pages only for RoomSketcher, SketchUp Free, IKEA (Kreativ/PAX etc.), and
  Canva Whiteboard, since their actual editors are account-gated and no
  accounts were created. Also read GOV.UK Design System's Accordion and
  Details component pages live, per the brief's sector-context instruction.
- Wrote `/Users/davidperkins/Code/personal/room-planner/research/panel-research.md`
  (2,152 words), covering all six requested sections with sourced, dated
  claims and explicit "labelled uncertain" tags where a sign-up wall blocked
  direct observation.
- Wrote `/Users/davidperkins/Code/personal/room-planner/handoffs/research-journal.md`
  summarising the work, conclusion, and what couldn't be checked.

## Next action

None — this task is complete. The next actor (per decisions.md's pipeline)
is the **ui-designer**, who should read
`/Users/davidperkins/Code/personal/room-planner/research/panel-research.md`
before starting the redesign concept, particularly section 3 (recommends
option (c): merge the Furniture list and Selected item into one persistent
block, keep Doors & Windows visible, collapse Room setup and Share by
default) and section 6 (the one-paragraph recommendation).

## Then

Gate 1 review of this research brief, then ui-designer builds the redesign
concept on the `redesign` branch, per decisions.md.

## Learned along the way

- The panel is currently 2,742px tall in six equal-weight sections (Room
  416px, Doors & windows 601px, Furniture 474px, Selected item 344px, Add
  furniture 299px, Share 607px) against a 720px viewport — confirmed by
  reading `.planner-panel` and its children directly in the browser console,
  not estimated from screenshots.
- The app already does several things well that the redesign must not lose:
  a live status line anchored near the canvas for every action, GOV.UK-style
  focus and auto-focus on "New room", auto-placement of new furniture into
  free space, and dashed-outline flagging for furniture-on-furniture overlap.
- Nearly every comparable "furniture fit" tool (RoomSketcher, Planner 5D,
  SketchUp Free, IKEA Kreativ/PAX, Canva's whiteboard editor) gates its
  actual editor behind an account sign-up. Only Excalidraw and a lighter demo
  mode of Floorplanner were reachable without one — worth knowing for any
  future research pass on this topic, so time isn't lost trying the same
  dead ends again.

## Open questions

For the design lead, batched, not blocking:
- Should the two overlap gaps found (furniture stranded by a corner cut;
  overlapping doors) be fixed as part of this redesign, or filed as a
  separate follow-up? They're logic gaps, not panel-layout issues, so
  they're outside the strict brief but were found during the same testing
  pass.
- The L-shaped sample room was not tested to the same depth as the
  rectangular one (only confirmed it loads via the room picker) — worth a
  pass during design-qa if the L-shape has layout quirks the rectangle
  doesn't.

## Continuation notes for a non-Claude LLM

All work is desk research, browser testing, and writing; no Claude-specific
tooling was required beyond a browser capable of driving the live app and
viewing other sites. If continuing this without browser access, the
heuristic-review findings are already captured in full in
`research/panel-research.md` section 1 and don't need re-testing unless the
app has changed since 8 September 2026. The comparable-tools table (section
2) already states plainly which claims were directly observed vs. inferred
from marketing pages — a continuation should preserve that distinction
rather than upgrading inferred claims to observed ones.
