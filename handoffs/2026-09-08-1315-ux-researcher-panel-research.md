# Handoff — Panel research for room planner redesign

- Status: in progress
- Role: ux-researcher · Last updated: 2026-09-08 13:15
- Project: /Users/davidperkins/Code/personal/room-planner
- Read first: brief.md · decisions.md · latest qa/ report if any

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
- Confirmed no brand.md deviation file yet exists in this folder — none
  needed for research, only for design stage.
- About to open https://dave-perkins-studio.github.io/room-planner/ in the
  browser tool and run the sample-load console snippet from the task.

## Next action

Open the live app in the browser, run:
`const md=await fetch('samples/example-plan.md?b='+Date.now()).then(r=>r.text()); loadMarkdown(md);`
in the console, then work through the heuristic review task list (load
sample, add wardrobe, move it, turn it, cut a corner, misplace a door,
export) on both the rectangular and L-shaped room, taking notes on friction
points and things that already work well.

## Then

1. Heuristic review write-up.
2. Browse 6–10 comparable tools (IKEA Kreativ, RoomSketcher, Floorplanner,
   Planner 5D, Magicplan, SketchUp Free, Canva whiteboard, Figma/FigJam,
   Excalidraw, plus any dedicated furniture-fit tool found), screenshot/note
   URL + date for each.
3. Synthesise into the five named layout options with trade-offs.
4. Task-frequency ranking with reasoning.
5. Ideas-worth-stealing list, each with source/effort/fit with Markdown
   interchange format.
6. One-paragraph recommendation.
7. Write research/panel-research.md, keep under ~2,000 words.
8. Write journal entry to handoffs/research-journal.md.
9. Mark this handoff complete.

## Learned along the way

(none yet)

## Open questions

(none yet)

## Continuation notes for a non-Claude LLM

All work is desk research + writing; no Claude-specific tooling required
except the browser to view the live app and comparable products. If browser
access is unavailable, the heuristic review can still be done by reading
`index.html` directly (single file, ~1800 lines) instead of driving the UI,
and the comparable-tools research can fall back to documented public
knowledge with explicit "not verified live, from training knowledge, dated
before 2026" labelling — this is a degraded fallback, not preferred.
