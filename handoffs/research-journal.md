# Research journal — panel reorganisation

Role: ux-researcher · 2026-09-08

## What I did

- Read `brief.md` and `decisions.md` for context before starting.
- Opened the live app (https://dave-perkins-studio.github.io/room-planner/) in
  the browser and used it as a person planning a room would: loaded the
  sample plan, added a "Bookshelf" via Add Furniture, dragged it, rotated it
  with the toolbar, cut a 100×100 cm corner off the rectangular room, added a
  second door that deliberately overlaps an existing one, and opened a fresh
  "New room" to see the empty state.
- Measured the panel directly via the browser's JS console rather than
  guessing: `.planner-panel` is 2,742px tall across six `panel-section`
  blocks (Room 416px, Doors & windows 601px, Furniture 474px, Selected item
  344px, Add furniture 299px, Share 607px) against a 720px viewport.
- Confirmed two real product bugs/gaps while testing (not just layout
  friction): furniture that ends up outside the room after a corner cut is
  never flagged, and two overlapping doors on the same wall are accepted with
  no warning at all — both checked directly in the DOM (`.is-flagged`,
  `class="opening"`).
- Tried to reach live editors for IKEA Kreativ/PAX, RoomSketcher, Planner 5D,
  SketchUp Free, and Canva Whiteboard, but all of them gate the actual
  editing surface behind an account sign-up, which I won't create per the
  studio's rules. Used their marketing/landing pages instead and labelled
  every claim about their in-editor layout as unverified/inference where
  that's the case, rather than describing them from memory.
- Got live, hands-on access to Excalidraw (fully open, no login) and a
  lighter demo mode of Floorplanner ("Roomstyles", no login), which gave two
  solid, directly-observed comparisons: both keep the object/tool palette out
  of a stacked sidebar (floating toolbar + contextual left panel for
  Excalidraw; floating overlays + bottom tab bar for Floorplanner's demo).
- Read the GOV.UK Design System's Accordion and Details component pages live,
  since the brief calls out GOV.UK patterns as sector context — their own
  guidance ("do not use an accordion for content all users need to see")
  became the load-bearing argument against a blanket-accordion redesign.
- Wrote `/Users/davidperkins/Code/personal/room-planner/research/panel-research.md`
  (2,152 words) covering all six required sections.

## What I concluded

The panel's worst problem isn't visual style, it's information order: the
"Selected item" properties (the thing you touch almost every time you click
something on the canvas) sit fourth of six sections down, so editing an
X/Y/turn value means scrolling past Doors & Windows and the Furniture list
every time. My recommendation is a persistent Furniture block that merges
the list and the selected item's fields in one place, keep Doors & Windows
visible (referenced constantly while placing furniture), and collapse Room
setup and Share by default since both are once-a-visit tasks — following
GOV.UK's own accordion guidance rather than importing a fashionable pattern
without evidence. I also flagged (separately, not as the main deliverable)
two real inconsistencies in the app's own overlap-warning logic that the
design/build stage should decide whether to fix now or file for later.

## What I could not check

- Could not open the actual editors for RoomSketcher, Planner 5D, SketchUp
  Free, IKEA Kreativ/PAX, or Canva's whiteboard canvas — all require an
  account and creating one wasn't appropriate for a research pass. Their
  in-editor layout claims are labelled uncertain/inference in the report;
  only their marketing/landing pages were directly observed.
- Did not test the app on a phone-sized viewport or with a screen reader —
  the brief's accessibility constraints (44px targets, focus restoration,
  GOV.UK focus) were spot-checked once (the "New room" name field correctly
  showed the yellow GOV.UK focus style) but not audited systematically; that
  belongs with design-qa at a later gate, not this research pass.
- Did not test the L-shaped sample room in the same depth as the rectangular
  one — only confirmed it exists via the room picker. If the L-shape has its
  own layout quirks, that's unverified here.

## Status

Complete. Deliverable at
`/Users/davidperkins/Code/personal/room-planner/research/panel-research.md`.
Handoff detail at
`/Users/davidperkins/Code/personal/room-planner/handoffs/2026-09-08-1315-ux-researcher-panel-research.md`
(marked complete).
