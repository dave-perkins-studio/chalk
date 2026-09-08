# Brand — room planner

Excursion for the web is the system. The `perkins-brand` skill's
`reference/excursion-web.md` and `assets/excursion-web.css` are the source of
truth, and the copy of the stylesheet in this folder is never edited.

This file records where the app departs from it, and why. It's a delta, not a
replacement. Anything not listed here follows Excursion as written.

Last updated 8 September 2026, on branch `redesign`.

## What stays

- Warm cream paper, warm near-black ink, dark mode through
  `prefers-color-scheme` only. No hex is typed by hand anywhere in the page's
  own `<style>` block; every colour is a token from the stylesheet.
- The frame: two hairlines with graph paper in the gutters outside them, fixed
  and never scrolling. The app runs between the lines, not over them.
- Three type families in their Excursion roles. Jost caps for wayfinding
  (kickers, buttons, field labels), Petrona for the masthead and content,
  Inclusive Sans for reading, Inclusive Sans 300 caps as the data voice for
  every measurement in the panel and on the plan.
- Squares. The masthead totem is the only round thing on the page.
- The weighted bottom edge on every button, flattening 3px on press.
- The GOV.UK focus block, verbatim, on everything focusable, including the SVG
  furniture and the two new `<summary>` elements.
- The 200ms motion budget, and `prefers-reduced-motion` honoured.
- Berry for flags, always with words alongside. Colour is never the only signal.

## Deviations

### 1. Full width, no `.hold` column

**What.** The app no longer sits in the 1120px `.hold` column. The header and
the two-column body run the full width between the frame lines.

**Why.** `.hold` is a reading measure for prose. This is a canvas tool, and the
plan is the thing people came to look at. On a 1280px screen the old layout
gave the plan about 700px and left roughly 160px of empty paper down each side.
Sanctioned in `decisions.md` at Gate 1 as the main brand deviation.

**How far it goes.** Only the column. Paper, frame, gutters, rules and type are
untouched, so the page still reads as the same document family.

### 2. An app shell instead of a scrolling page

**What.** On screens 900px and wider the shell is exactly the viewport height,
`html` and `body` are `overflow:hidden`, and the panel scrolls inside its own
column. Below 900px it stacks and the page scrolls normally.

**Why.** The old page scrolled and the canvas was pinned with `position:sticky`,
which only works because the page scrolls; two scroll models fighting over one
screen. Now there's one scrolling thing and it's the panel.

**Cost.** The page footer had nowhere to live. The ticket line and the Clear
saved plan button moved to the foot of the panel's own scroll, under a dashed
rule. Same content, same order, different place.

### 3. A hairline instead of a second surface colour

**What.** The panel sits on the same `--paper` as the canvas, separated by a
1px `--rule` down its left edge.

**Why.** The brief allowed a new surface colour taken from the tokens.
`--paper-tint` is the only candidate and it already means 'interactive' across
the whole system: it's the hover and pressed fill, and the selected furniture
row uses it. Painting a whole column in it would say the column is a button.
Excursion's own answer is that lines do the layout, so the columns are divided
with a rule.

### 4. `<details>` carrying an `<h2>`

**What.** Room setup and Share are native `<details>`, collapsed by default,
each with its `<h2 class="kicker">` inside the `<summary>`.

**Why.** GOV.UK's accordion guidance says to hide what not everyone needs and
what's done repeatedly, and to leave everything else in plain sight. Room setup
is once per room and Share is once at the end. Furniture and the openings stay
open, because a family member opening a shared link needs to see them.

The heading inside the summary keeps the page's heading outline intact: one
`h1`, one `h2` per place.

**The marker.** Excursion has no chevron glyph outside the drawn, decorative one
on a stream row, and the browser's default triangle is a chevron. The marker is
hidden and replaced with a drawn square plus, which loses its upright stroke
when the place opens. Two 2px bars, in `--blue`, turning to `--focus-ink` under
focus so it doesn't disappear into the yellow field.

### 5. A list row that grows

**What.** Selecting a piece of furniture expands its own row to show name,
width, depth, X, Y, turn and notes. There's no separate 'Selected item'
section.

**Why.** It's the finding the research led with: those fields are what you touch
every time you click something, and they used to sit fourth of six sections
below the list. The Excursion stream row is a link that takes you somewhere; this
one acts in place, so it isn't a stream row and doesn't get the arm, the chevron
or the kind-coloured hover edge. It keeps the row's dashed rules, the tint fill
and the 3px blue bottom edge, which now reads as 'this whole block is the
selection'.

### 6. Form fields, which Excursion doesn't have

Carried over from the original build, unchanged. The stylesheet has no field
rules, so the page adds them in the same grammar: square, hairline ring, 2px
bottom edge, 44px minimum, GOV.UK focus. `.btn-small` is a 44px version of
`.btn`, for the same reason.

## Rations

Excursion's device rations are a page budget, and this page spends almost
nothing:

- One bolted rule, under the header. The stylesheet allows two or three.
- No fingerpost arm. Nothing here navigates away.
- No chips, no stipple bands, no boop, no pull quotes.
- One curve, the totem.
- No italics.

That's deliberate. A tool you use for an hour shouldn't have charm devices in
it that you have to look past every time.

### 7. Its own icon and name, not the Perkins totem

**What.** The masthead carries a tile icon and the name Chalk, with 'Room
planner' beside it in the wayfinding face. The totem plate is gone from this
app.

**Why.** Hum, Redpen and Help Me Listen all do this, and their icon files say
why: at 32 points a cream tile with a totem on it reads as a beige smudge. A
browser tab is the same problem. The tile follows the family exactly, same
0.225 corner radius, same shallow three-stop gradient with the light at the
top, same paper stroke with round caps, same trick of fitting the mark to its
own ink. The mark is an L-shaped room with one piece of furniture against two
walls: a plain rectangle would read as a box or a button, and the notch is what
makes it a floor plan at a glance.

**The hue.** Excursion's taupe, taken deeper the way Redpen takes the rose
deeper. Hum and Help Me Listen are both blue and Redpen is rose, so the fourth
tile in the Dock had to be neither, and plain taupe is the beige smudge the
others warn about.

### 8. Contextual actions, and settings behind a toggle

**What.** Rotate, Duplicate and Delete only appear once something is selected.
Grid and snap sit behind a quiet Settings toggle at the end of the toolbar.

**Why.** Six buttons of equal weight said all six were equally useful. Three of
them do nothing without a selection, and two change how the plan is drawn
rather than what is in it. Undo is the only one that always applies, so it is
the only one always shown.
