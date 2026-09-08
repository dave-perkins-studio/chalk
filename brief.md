# Brief — Room planner redesign

**Project:** Room planner (personal tool)
**Folder:** `~/Code/personal/room-planner/`
**Live:** https://dave-perkins-studio.github.io/room-planner/
**Date:** 8 September 2026
**Client:** David Perkins, for his own house move

## The brief as given

> Improve the design and layout of this app. It should be a clean, usable,
> full page experience. The right side should be easier to use and more
> intuitively laid out. There should be clearer sections to each part of the
> interface, you can feel free to move a few steps away from the brand, if it
> improves clarity. Suggest improvements to the design and use of the app,
> suggest amends to the plan and concept if there are good ideas.

## What exists

A working single-file HTML tool, built 8 September 2026. To-scale 2D plans of
rectangular or L-shaped rooms, furniture dragged and turned on a grid, doors
and windows on walls, warnings for overlaps and blocked doors, multiple rooms,
and Markdown as the interchange format so family can edit a plan and send it
back. Full history in `README.md` and the git log.

## Audience

David, and family members deciding what furniture fits. Non-technical, on
laptops mostly, occasionally reading a shared plan on a phone. Used in short
bursts while looking at a room, or round a table arguing about a sofa.

## The problem to solve

The right-hand panel has grown to roughly 1,700px of stacked forms with no
hierarchy: Room, Doors and windows, Furniture, Selected item, Add furniture,
Share. Everything is visible at once and everything looks equally important.
The canvas is sticky, so the panel is a long scroll against a fixed plan.

## Constraints

- Single self-contained HTML file, no build step, no dependencies.
- Must keep working from `file://` and from GitHub Pages.
- Must not break the Markdown format. Plans already exist in that shape and
  the format is the product's whole sharing story.
- Keep the keyboard path and the accessibility work already done: focus
  restoration after re-render, 44px targets, heading outline, GOV.UK focus.
- Excursion web brand is the starting point. Deviation is explicitly allowed
  where it buys clarity, and must be written down rather than drifted into.

## What good looks like

- The panel reads as a small number of clear places, not one long form.
- The most common actions (add furniture, move it, share the plan) are the
  easiest to reach.
- The plan itself is the biggest thing on screen.
- A family member opening the link understands what to do without being told.
- No regression in the accessibility or the file format.

## References

- Live app: https://dave-perkins-studio.github.io/room-planner/
- Repo: https://github.com/dave-perkins-studio/room-planner
- Brand: `perkins-brand` skill, `reference/excursion-web.md`, `assets/excursion-web.css`
- Prior QA: findings fixed in commit 42b49a9
