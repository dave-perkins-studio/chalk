# Chalk

A room planner. A flat, to-scale plan of a room, so you can work out whether the sofa fits
before it turns up on a lorry.

Draw the room in centimetres, add furniture at its real size, then drag the
pieces around. You can send the plan to someone as a picture or as a block of
text, and paste theirs back in.

One HTML file, no build step, no account, nothing sent anywhere. Open
`index.html` in a browser, or use the hosted copy.

## Using it

Pick a room from the switcher at the top, or add one with New room. The panel
on the right has three parts: the furniture in this room, the doors and
windows, then Room setup and Share, which stay folded away until you need them.

Set the room's width and depth under Room setup. Width runs left to right,
depth runs top to bottom, both in centimetres.

The panel can be put away with the button beside Settings, which gives the
plan the whole window. It stays put away until you bring it back.

Under Share you can name the house, send this room on its own, or send every
room in one file. Loading works either way round: paste a single room or a
whole house and it takes whichever it is given.

Add furniture from Common sizes or by typing a name and a size. Items land on
the first free patch of floor. Drag them where you want them. Click one and its
row opens up so you can type an exact position, size or turn, and Rotate,
Duplicate and Delete appear above the plan. Undo and redo sit at the right of
that row as a pair of arrows, with the grid and snap switches under Settings
beside them.

For an L-shaped room, cut a corner under Room setup: pick which one, then give
the notch its two lengths. That covers a chimney breast, a stair bulkhead or a room that
wraps round one. Set the corner back to None to get the rectangle again.

Doors and windows go on a wall, measured from a corner: top and bottom from the
left, left and right from the top. Drag one along its wall, or round onto any
other wall, and the measurements follow. Click one and its row opens so you can
type an exact position instead, which moves it as you type. Two openings
overlapping on the same wall are flagged. A door draws its swing, and anything
sitting in the way gets flagged. Cutting a corner shortens two of the walls, so
an opening left stranded past the end of one is flagged with the range that
wall now covers.

Items turn red and dashed when they're outside the room, overlapping something
else, or blocking a door. The reason is written out under the plan and next to
the item in the list, so you don't have to go by colour.

### Keys

| Key | What it does |
|---|---|
| Drag | Move an item, snapping to 5 cm |
| Alt while dragging | Snap to 1 cm instead |
| R | Turn the selected item 90° |
| Arrow keys | Nudge 1 cm |
| Shift + arrows | Nudge 10 cm |
| Delete | Remove the selected item |
| Cmd + D | Duplicate it |
| Cmd + Z | Undo |
| Cmd + Shift + Z | Redo |
| Cmd + V | Load a plan from the clipboard |
| Esc | Deselect |

You can do all of it from the keyboard. Tab moves through the items on the
plan, then into the panel.

Every room you make is kept, so a whole house lives in one place. Share
buttons come in two kinds: this room on its own, or all of them in a single
file under a house name.

Your plans are saved in the browser you're using, and nowhere else, so a
different browser or another computer won't have them and you're probably best
sending yourself the text if you want it on both.

## The file format

Plans travel as Markdown, because a table survives being pasted into WhatsApp
and anyone can edit a row by hand. There's no hidden data: what you see is the
whole plan.

```markdown
# Room plan: Bedroom 1

Room: 420 × 350 cm
Cut: top-right 150 × 120 cm

| Opening | Wall | From | Width | Swing | Hinge |
|---|---|---|---|---|---|
| Door | S | 40 | 80 | in | left |
| Window | N | 120 | 150 |  |  |

| Item | Width | Depth | X | Y | Rotation | Notes |
|---|---|---|---|---|---|---|
| King bed | 150 | 200 | 130 | 0 | 0 | headboard on the north wall |
| Bedside table | 40 | 40 |  |  |  | only if it fits |
```

X and Y are the distance from the top-left corner of the room to the top-left
corner of the item, after it's been turned. Leave them empty and the item stays
on the list without going on the plan.

The Cut line is optional and only appears for an L-shaped room. It names the
corner taken out and the size of the notch, so `Cut: top-right 150 × 120 cm`
removes a 150 by 120 block from the top right. Compass names work too, so
`NE` and `north-east` both load.

The parser is fairly forgiving. It takes metres or centimetres, `x` or `×`, wall names
or letters, columns in any order, extra columns it doesn't recognise, and
tables that have lost their outer pipes. A row it can't read is skipped and
counted, rather than failing the lot. `samples/mangled-plan.md` is the same
plan after a round trip through a chat app, and it loads.

Several rooms go in one file under `##` headings, below a `# Room plans: <house>`
line that names the house. Loading one of those brings in every room at once.
Any room whose name matches one of yours is replaced, and the rest are added,
after a single confirmation that names them.

## What it doesn't do

- Rooms are a rectangle, optionally with one corner cut out. No U or T shapes.
- Doors and windows go on the four outer walls, not on the two short walls a
  cut leaves behind.
- Furniture turns in 90° steps, not freely.
- The exported picture covers one room, and uses a system font rather than the
  page's, because web fonts don't survive the trip into an image.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole tool |
| `excursion-web.css` | The Excursion stylesheet, copied from `personal-site-studio`. Don't edit it here |
| `samples/example-plan.md` | A two-room house, one of them L-shaped |
| `samples/mangled-plan.md` | The same plan after a chat app has had a go at it |

Actual house plans don't belong in this repo. They go in `personal/life/house/`.
