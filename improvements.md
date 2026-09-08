# What changed in the redesign, and why

A reading copy of the thinking behind the room planner redesign, 8 September
2026. The working notes are in `decisions.md`, the evidence is in
`research/panel-research.md`, and the brand deviations are in `brand.md`.

## The problem, measured

The right-hand panel had grown to six sections of stacked forms, all carrying
the same heading weight.

| Measured on the live app | |
|---|---|
| Panel height | 2,183px at 1280 × 900 |
| Screens of scrolling | 2.4 |
| Form controls visible at once | 25 |
| Sections, all equal weight | 6 |

Nothing there told you that Room setup happens once and never again, while
moving furniture happens every few seconds.

The sharpest version of the problem: the **Selected item** fields sat fourth of
six sections down. So clicking a piece of furniture on the plan put its settings
two sections below the thing you had just clicked. Nudging a wardrobe to an
exact centimetre meant scrolling past the doors list and the furniture list,
every single time, for the most frequent action in the tool.

## The improvements

### 1. The selected item's fields live in its own row

Click a piece of furniture and its row in the list expands to show name, size,
position, turn and notes. There is no separate settings section any more.

**Why.** This was the biggest single win on offer. Ranked by how often you do
them, the tasks go: move furniture, turn it, add it, switch rooms, edit sizes,
doors and windows, room setup, share. The first three are nearly the whole
session. Room setup is once per room. The old layout gave all eight the same
weight.

**Reference.** Excalidraw shows a panel of settings only while something is
selected, and hides it the rest of the time (excalidraw.com, used live, 8
September 2026). Of the tools we could open without making an account, every one
did this. None parked the settings in a stacked sidebar.

### 2. Room setup and Share collapse; the lists stay open

Both are now closed by default. The furniture list and the doors list are not.

**Why.** Room setup is a once-per-room job. Share is a once-at-the-end job.
Neither should cost you scrolling on every other visit.

**Reference.** The government design system says an accordion suits content
that not everyone needs, and warns directly against using one for content all
users do need (design-system.service.gov.uk/components/accordion, read 8
September 2026). That rule is why the furniture list stayed open. A family
member opening your link is there to look at the furniture. Hiding it behind a
toggle to make the page tidier would have been tidiness at the reader's expense.

### 3. Full width, and the page no longer scrolls

The app used to sit inside a 1,120px document column with large empty margins,
with the plan pinned by a CSS trick while the whole page scrolled under it. Now
the header sits across the top, the plan takes the space it can get, and the
panel scrolls inside its own column.

**Why.** The plan is the product. At 1440 wide it is now 964px across, and the
whole thing fits one screen. This is the main step away from the Excursion
brand. Its column is right for reading and wrong for a canvas.

### 4. Doors and windows: the list stays, the form folds away

The list of openings is always visible. The six-field form for adding one is
behind a toggle.

**Why.** The designer flagged that six controls always sat under a two-row
list, and wanted the whole section collapsed. The research said keep it visible.
Both were right about different things. You check where the door is constantly
while placing furniture near it, so the list stays. You add a door twice per
room, so the form folds. The furniture block already worked this way, with the
list open and Common sizes tucked away.

### 5. An empty state

A room with no furniture now says what to do first.

**Why.** You are sending this link to family. They land on an empty rectangle with nothing telling them what to do. The app already did this well in one place, printing "Room
added. Give it a name and a size." when you make a room, so this extends
something that already worked rather than inventing a new idea.

### 6. Overlapping doors are flagged

Two doors sharing a stretch of the same wall now say so, in words, in the row.

**Why.** The tool already had the concept of flagging things that should not
overlap, and applied it to furniture but not to openings. That gap quietly chips
away at trust in the warnings you do get.

## What was proposed and turned down

**A separate planner per job.** IKEA runs each of its planners as its own tool
rather than one screen with everything on it (ikea.com, landing page only, 8
September 2026, editors gated behind an account). Turned down: this is one house
move, and splitting it would fight the goal of keeping the plan the biggest
thing on screen.

**A bottom-anchored library drawer.** Floorplanner's lighter demo uses a bottom
tab bar for its style library (floorplanner.com, 8 September 2026). Turned down:
it suits touch, and this is used on laptops. Building it as an overlay would
also risk the keyboard and focus work for no gain.

**A tabbed inspector.** Turned down because checking whether the wardrobe fits
beside the door needs both the furniture and the doors visible at once. A tab
would charge you a click for something you do constantly.

## Two bugs found on the way

**The hidden attribute was not hiding.** A style rule was beating the
browser's own rule for hidden things, so two rows of fields that should come and
go never went away. Choosing Window still offered you a door's swing and hinge,
and a rectangular room still showed cut sizes carrying the previous L-shape's
numbers. Fixed.

**Sixty lines of JavaScript had been pasted into the stylesheet.** Mine, from
building the room switcher: a find-and-replace matched a comment in the styles
as well as the one in the script, and copied a block of code in there as junk.
Browsers skipped it, so nothing broke, but it has been shipping since. Removed.

## Where the evidence is thin

Four of the seven tools we looked at keep their editors behind a sign-up:
RoomSketcher, SketchUp Free, IKEA's planners and Canva's whiteboard. We read
their marketing pages and did not create accounts, so anything said about how
their editors are laid out is a guess, and the research labels it as one. What
is here rests on the three we could use, the government design system guidance,
and what we measured in your own app.

One claim in the research did not survive checking. It reported that furniture
stranded by a corner cut goes unflagged. Tested directly twice: a bookcase left
inside a cut corner is flagged "outside the room" and drawn dashed. The likely
cause is a cached older build, which caught this session out once already.
