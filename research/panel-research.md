# Panel research — right-hand panel reorganisation

**Question.** How should the right-hand panel of the room planner be organised so it reads as a small number of clear places rather than one long form, without breaking the Markdown interchange format or the accessibility work already done?

**What I looked at.** The live app itself, used as a person planning a room would (both the rectangular and L-shaped sample rooms). Then six comparable tools, viewed live in the browser on 8 September 2026 unless stated otherwise, plus the GOV.UK Design System as the sector reference the brief calls for.

---

## 1. Heuristic review of the current app

I loaded the sample plan, added a "Bookshelf" (80×30) via Add Furniture, dragged it, rotated it with the toolbar button, cut a corner off the rectangular room, added a second door that overlaps an existing one, and checked the empty state via "New room".

**What already works well, and should not be lost:**

- **Direct manipulation on the canvas is good.** Dragging the bookshelf gave an immediate live status line under the canvas ("Bookshelf · 80 × 30 cm · at 280, 245") and it snapped cleanly. The toolbar's **Rotate** button turned it 90° in one click without needing to scroll to a form.
- **Furniture collision detection is genuinely good.** Dragging the bookshelf onto the bed drew a dashed berry-red outline on both pieces and updated the status line to "… overlaps King bed" — in place, immediate, no scrolling required.
- **The empty state coaches well.** Clicking "New room" drops in a ready 4×3 m rectangle, auto-focuses and selects the Name field with the GOV.UK yellow focus style, and prints "Room added. Give it a name and a size." That's exactly the kind of quiet, specific guidance a non-technical family member needs.
- **Auto-placement on add is sensible.** Adding "Bookshelf" through the Add Furniture form placed it in a free corner immediately, with a confirming toast ("Bookshelf added and placed"), rather than dropping it centre-screen on top of the bed.

**Where it slowed me down or confused me:**

- **Cutting a corner does not warn about furniture already sitting in the cut zone.** I placed the bookshelf, then cut a 100×100 cm corner over the same spot. The room polygon updated correctly underneath (confirmed in the SVG), but the bookshelf kept no flag — it's now sitting outside the room, and I'd only notice by reading the plan carefully. Furniture-on-furniture overlap is flagged; furniture-outside-the-room isn't. A real trap when reshaping a room mid-session.
- **Door/window overlap isn't checked at all.** I added a second door on the south wall (100 cm wide, 30 cm from the corner) directly overlapping the existing one (80 cm wide, 40 cm from the corner). The app said "Door added to the south wall" and left both in place with no flag whatsoever (`class="opening"` on both, no warning class). The same product clearly has the concept of "flag things that shouldn't overlap" and simply hasn't applied it to openings.
- **The panel is a genuinely long, undifferentiated scroll.** I measured it directly: with one room selected, the panel (`.planner-panel`) is 2,742px tall across six `panel-section` blocks — Room (416px), Doors and windows (601px), Furniture (474px), Selected item (344px, only present once something is selected), Add furniture (299px), Share (607px) — against a 720px-tall viewport. That's roughly 3.8 screens of scroll, and every section carries the same heading weight, so nothing tells you Room and Share are once-a-visit jobs while Furniture and Selected item are what you touch constantly.
- **"Selected item" is buried below the two list sections.** Clicking an item on the canvas doesn't scroll the panel to its properties — the numeric X/Y/Turn fields sit fourth of six sections down, so nudging an item to an exact centimetre means scrolling past Doors & Windows and the Furniture list every single time, which is the single most frequent action in the tool.

## 2. Pattern research — six comparable tools

| Tool | Layout pattern observed | Selected object's properties |
|---|---|---|
| **Excalidraw** (excalidraw.com, live, 8 Sep) | Floating tool palette top-centre; no persistent library (it's a whiteboard). | A **left contextual panel** (stroke, fill, width, opacity, layers) renders only on selection and vanishes otherwise — confirmed by drawing a rectangle. |
| **Floorplanner** (floorplanner.com, live demo "Example House", 8 Sep) | Marketing screenshot shows a left vertical icon rail + top dropdown toolbar. The lighter "Roomstyles" demo I could open without an account instead used floating overlays (zoom, 2D/3D toggle, undo/redo) and a **bottom tab bar** for the style library. | Not reachable without an account — **labelled uncertain**. |
| **RoomSketcher** (roomsketcher.com, 8 Sep) | Marketing pages only; editor is account-gated. Copy names "Draw Yourself", a Measurement Wizard, and asset libraries as separate modes. | **Not observed** — labelled uncertain. |
| **SketchUp Free / Trimble** (sketchup.trimble.com, 8 Sep) | Marketing page only, editor sign-in gated. | **Not observed** — labelled uncertain. |
| **IKEA (incl. Kreativ)** (ikea.com, 8 Sep) | Landing page runs each planner (Kreativ, PAX, BILLY, kitchen tools…) as a **separate tool per job** rather than one form with everything visible — itself a useful data point even though the editors are gated behind an account/app. | **Not observed** — inference only. |
| **GOV.UK Design System — Accordion/Details** (design-system.service.gov.uk, 8 Sep) | Not a furniture tool, but the brief's sector reference. Quoted live: accordions suit content users don't all need and "familiar tasks" done repeatedly, but **"Do not use an accordion for content that all users need to see"** — test plain, well-headed content first. | N/A |
| **Canva Whiteboard** (canva.com/whiteboards, templates gallery, 8 Sep) | Gallery of ready-made templates observed directly; the editor itself needs a signed-in session, not opened. | **Not observed live** — labelled uncertain. |

Honest takeaway: every tool I could actually operate without an account (Excalidraw, this app, Floorplanner's lighter demo) keeps the object palette out of a stacked sidebar and shows properties only contextually. The account-gated tools may well use a left rail plus a contextual properties area — that's the common reputation — but I didn't get past their sign-up walls this session, so it isn't cited as observed fact here.

## 3. How should the right side be organised?

- **(a) Tabbed/segmented inspector.** Would hide Doors & Windows behind a tab while working Furniture — wrong here, since checking "does the wardrobe fit near the door" needs both without an extra click. Adds navigation cost, saves no real space on a single-room task.
- **(b) Accordion, one section open at a time.** GOV.UK's own guidance supports this *only* for content not everyone needs and for repeat, familiar tasks. Room setup and Share qualify (once-a-visit); Furniture and Selected item don't — a first-time family member needs to see the furniture list immediately. A blanket accordion would hide the thing people open the link to look at.
- **(c) Split — persistent object list + contextual inspector for the selection only.** Excalidraw's contextual panel, crossed with what this app already half-does (Selected Item only renders when something's selected). Fixes the worst finding above directly: put Selected Item where you land, not fourth of six. **The strongest option** — the furniture list must stay visible (it's the plan's inventory) and the selected item's properties are the most-used control surface, so the two belong together at the top.
- **(d) Move tools to a left rail or top toolbar.** Already half-true — Grid/Snap/Rotate/Duplicate/Delete/Undo already sit above the canvas, not in the panel. Add Furniture and Add opening are forms, not one-click tools, so they don't move as cleanly, but the Doors & Windows *list* could become read-only chips near the canvas.
- **(e) Bottom-anchored library drawer.** Fits touch use (Floorplanner's bottom tab bar) but the brief's audience is "non-technical, on laptops mostly" — a touch-reach drawer doesn't solve this audience's actual problem, which is scroll depth on a desktop sidebar.

**Recommendation: (c), with pieces of (d) already banked.** Restructure the panel as: **Furniture list** and **Selected item** merged into one persistent block at the top (list stays visible; a selection expands its own row into the editable fields in place, rather than jumping to a separate section) — then Doors & Windows, then Room setup and Share collapsed by GOV.UK's own accordion rule (infrequent, not needed by everyone at once). Keep the toolbar exactly where it is.

## 4. Task frequency

1. **Move furniture** — happens continuously, every few seconds during a session; this is the point of the tool.
2. **Turn furniture** — nearly as frequent as moving; already a one-click toolbar action, correctly.
3. **Add furniture** — several times per room, in bursts, but not continuous.
4. **Switch rooms** — a handful of times per session (checking each room in turn), cheap already (one combobox in the header).
5. **Edit sizes** (of furniture or room) — occasional, mostly at the start of a room or when a family member disputes a measurement.
6. **Doors and windows** — a handful of edits per room, front-loaded early in a session.
7. **Room setup** (name, dimensions, corner cut) — once per room, essentially never touched again once right.
8. **Share** — once or twice at the end of a session, sometimes not at all if just checking a fit alone.

Reasoning: matches the brief's own framing and my own session — of roughly a dozen actions performed, half were drags/rotates on already-placed furniture, one was room setup, one a door edit, and Share untouched until sought out. The panel should mirror this: cheapest reach for #1–#3, easy but not first for #4–#6, tucked away for #7–#8.

## 5. Ideas worth stealing

- **Contextual properties, shown only when something is selected.** *Source:* Excalidraw. *Effort:* low — the app already conditionally renders "Selected item"; this just relocates it. Fits: yes, layout-only, no Markdown impact.
- **A short, specific status line anchored near the canvas.** *Source:* this app already does this well ("Bookshelf added and placed", "… overlaps King bed") — protect it, don't lose it in the redesign. *Effort:* none, already built.
- **Warn before letting a change strand something.** *Source:* inference from the corner-cut/door-overlap gaps found above, not another product. *Effort:* medium — extend the existing flagging logic (`.is-flagged`, already used for furniture overlap) to furniture stranded outside the room polygon and to overlapping openings. Fits: yes, internal logic only.
- **Progressive disclosure only for once-a-visit sections, evidence-based.** *Source:* GOV.UK accordion guidance. *Effort:* low, `<details>` is native, no dependency. Fits: yes — exactly the "move a few steps from brand if it improves clarity" the brief permits.
- **Separate planner per job (IKEA's catalogue approach).** *Source:* inference from IKEA's landing-page structure only. **Not recommended** — this app is already the single planner for one house move; splitting it would fight "keep the plan the biggest thing on screen."
- **Floating/bottom drawer for the object library.** *Source:* Floorplanner's demo. *Effort:* medium, and risks the accessibility work already done (44px targets, focus restoration) if built as an overlay rather than in-flow content.

None of these change what's stored or how a plan round-trips through Markdown; all are presentation-only.

## 6. Recommendation

Keep the toolbar above the canvas exactly as it is — it already does the "move tools out of the long form" job the brief is asking for — and spend the redesign effort on collapsing the six flat panel sections into three: a **persistent, always-visible Furniture block** where the list and the selected item's editable fields live in the same place (fixing the single biggest friction found: properties buried fourth of six sections below the thing you just clicked); a **Doors & Windows block** kept visible since it's referenced constantly while placing furniture near a door; and a **collapsed-by-default Room setup and Share section**, following GOV.UK's own accordion rule, since both are once-a-visit tasks that shouldn't cost scroll depth on every other visit. The single biggest win is moving Selected Item to sit with the Furniture list — that alone removes most of the scrolling a person does while actually using the tool, which per the task-frequency ranking above is nearly the entire session.

---

### Sources
- This app, https://dave-perkins-studio.github.io/room-planner/, tested live 8 Sep 2026.
- Excalidraw, https://excalidraw.com, tested live 8 Sep 2026.
- Floorplanner, https://floorplanner.com and its "Example House" demo, tested live 8 Sep 2026.
- RoomSketcher, https://roomsketcher.com and https://roomsketcher.com/features/floor-plans, viewed 8 Sep 2026 (marketing pages only — editor gated).
- SketchUp Free / Trimble, https://sketchup.trimble.com/plans-and-pricing/sketchup-free, viewed 8 Sep 2026 (marketing page only — editor gated).
- IKEA design and planning tools, https://www.ikea.com (design & planning landing, redirected from Kreativ deep links), viewed 8 Sep 2026 (landing page only — planners gated behind account/app).
- Canva Whiteboards, https://www.canva.com/whiteboards/templates/, viewed 8 Sep 2026 (template gallery only — editor gated).
- GOV.UK Design System, Accordion — https://design-system.service.gov.uk/components/accordion/ and Details — https://design-system.service.gov.uk/components/details/, both read live 8 Sep 2026.
