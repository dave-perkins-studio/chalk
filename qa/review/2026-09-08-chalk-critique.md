# Chalk, a critique

Method: two-agent (A: judgement, opus · B: mechanical, impeccable 3.6.1)
Target: https://dave-perkins-studio.github.io/chalk/ → `/Users/davidperkins/Code/personal/chalk/index.html`   Kind: url + html
Brand judged against: Excursion web, plus `brand.md` in this folder (ten agreed deviations)
Captures: `qa/review/captures/chalk-desktop.png`, `chalk-mobile.png` (first run), plus live walks at 1100×760, 1000×420, 375 and 320, both schemes
Detector: impeccable 3.6.1

## Design health score

| # | Heuristic | Score |
|---|---|---|
| 1 | Visibility of system status | 2 |
| 2 | Match with the real world | 3 |
| 3 | User control and freedom | 3 |
| 4 | Consistency and standards | 2 |
| 5 | Error prevention | 3 |
| 6 | Recognition over recall | 2 |
| 7 | Flexibility and efficiency | 3 |
| 8 | Aesthetic and minimalist | 3 |
| 9 | Error recovery | 3 |
| 10 | Help and documentation | 3 |

**27 of 40. That is 68%, which the rubric bands as Acceptable.** Real work needed before the weak rows stop hurting.
Cognitive load: 4 failures of 8, which is a critical finding in its own right.

## Specificity verdict

The drawing is unmistakably this product. The panel around it could be any
sidebar. Graph paper at 10 and 100 cm, dimension lines in the data voice, a
door drawn as a leaf and a swing arc, the notch labelled inside itself, and
flag copy no other product would write: `past the end of that wall · it runs 0
to 420 cm`. To the right of the hairline, though, it is a kicker, a list of
rows with a button each, an add form and two disclosures. Swap the nouns and
it is a form builder.

Three missed chances, all concrete. Rooms are a bare `<select>` in a tool whose
whole question is which room the sofa goes in. The catalogue of sizes you might
buy is drawn identically to the furniture you own. The name earns nothing: it
appears once and nothing in the interface is chalk-like.

## Overall impression

The plan itself is excellent and the warnings are the best thing in the
product. The panel is still nearly two screens with every disclosure shut, and
the plan it exists to serve gets 20% of the viewport. The biggest opportunity
is the one the brief already named and the redesign only half delivered: make
the plan the biggest thing on screen.

## What's working

**The flag copy.** `past the end of that wall · it runs 0 to 420 cm` names the
problem and hands you the valid answer in one line, in the product's own units,
without a modal. Almost nothing achieves that.

**The clipboard fallback.** When Copy text failed, the plan was already in the
box below with one sentence saying so. The failure path preserves the work and
states the recovery.

**Focus restoration.** Every render tears down the focused node and puts both
the element and its caret back, verified across four re-renders while typing.
The visibility guard exists because someone thought about switch access.

## Priority issues

| # | Severity | What | Where | Verified by | Ref | Fix direction |
|---|---|---|---|---|---|---|
| 1 | P0 (Blocker) | The three `Remove` buttons have accessible names that omit their visible word, so voice control cannot activate them by label | `index.html`, `.rows li` remove buttons | Read back: visible `Remove`, name `Take King bed off the plan`, ×3. `Place` and `Add` pass | WCAG 2.2 SC 2.5.3 Label in Name (A) | Start the name with the visible word |
| 2 | P1 (Major) | Two of four disclosures have no affordance and read as headings. `Common sizes` is the only quick way to add furniture | `index.html` `details.presets summary`, no `::after` | Computed: heading is Jost 500 13px uppercase `#495C7C`; the summary is Jost 500 14px uppercase `#495C7C`. Only `cursor` differs | Heuristic 6 | Give `.presets` the drawn plus `.place` already has |
| 3 | P1 (Major) | 15 controls lose the GOV.UK focus field, including every destructive one | `index.html:238` `.btn-small.ghost{background:transparent}` beats `excursion-web.css:678` | Specificity (0,2,0) over (0,1,1); the page never re-asserts it. Confirmed the ghost rule exists and no `.ghost:focus-visible` does | Excursion §9.6; `brand.md` "What stays" | Add `.btn-small.ghost:focus-visible{background:var(--focus-yellow)}` |
| 4 | P1 (Major) | The status line goes stale, and reports the panel's errors 976px away in the other column | `index.html:1828` field handler, no `announceSelection()` | Typed 260 into `sel-x`: the item moved to 260, the status still read `at 130, 0` | Heuristic 1; SC 4.1.3 | Announce from the field handler; give the panel its own status line |
| 5 | P1 (Major) | The plan is not the biggest thing on screen, and collapses at 200% zoom | `index.html:581` fixed `MARGIN = 60`; width-only 900px breakpoint | 452×377px in a 1100×760 viewport: 20% of the screen, 50% of its own canvas. A 180×150cm room drew at 28% of the canvas. At 1000×420 the room drew 148×124px | Heuristic 8; the brief | Scale the margin to the room; add a height condition to the shell |

## Brand

Excursion §9, seven points: five pass. Paper and dark correct, frame present, one
bolted rule, zero arms, zero chips, zero italics, one curve (the documented icon
tile), every colour a token with no literal hex in the page's own styles.

Two fail. Point 6, the focus block, is issue 3 above. Point 5 is a **chevron**:
`.settings > summary::after` is a rotating CSS chevron, three lines from the
drawn plus the page invented specifically to avoid one. `brand.md` deviation 4
rules chevrons out by name. Undocumented deviations found: one.

## Craft floor

| Row | Verdict |
|---|---|
| Contrast | **Pass** on reconciliation. A read the plan's dimension labels at 5.21:1; measured live they are `#64594B` on paper at 6.13:1, and the token on tint is 5.56:1. Both clear the 5.5 floor |
| Depth | Exempt by brand |
| Spacing | Pass, ~63px above a section heading and ~40px below |
| Type | **Fail.** plan labels compute to 7.8px at the smallest, and an item can be too small to label at all |
| Motion | Pass, one device at 0.18s with a reduced-motion guard |
| States | Pass, and unusually full |
| Browser surfaces | **Fail.** no `::selection`, no `caret-color`, no scrollbar theming. The panel's scrollbar is the most visible chrome the app shell created and it is OS default grey |
| Copy | **Fail.** `Cmd` is hard-coded in five user-facing strings, for an audience explicitly including family on Windows |
| Coverage | Pass |

## Persona red flags

**Alex, the impatient expert.** Furniture on the plan is `role="button"
tabindex="0"` with no keydown handler, so Enter and Space do nothing and he must
leave the plan to use the list. The arrow nudge ignores the Snap setting
entirely: set snap to 5cm, nudge, get 1cm. No multi-select and no way to copy one
room's furniture into another, which is the obvious bulk job in a house move.

**Sam, using assistive tech.** The three `Remove` buttons fail Label in Name.
Fifteen ghost buttons, including both Deletes, focus without the yellow field.
Two landmarks share the name "Plan controls", one being the whole panel. The plan
is `role="img"` with `role="button"` children, whose children are
treated as decoration by the ARIA spec, so a browse-mode reader may not enumerate the furniture
at all. Against that, the accessible names on the furniture are excellent and
focus survives every re-render.

**Jordan, the first-timer**, derived from the brief's family who are sent the
link cold. `Common sizes` looks exactly like the heading beside it, so the fast
path is invisible and the alternative is typing three fields. `Cmd + Z puts it
back` is a lie on Windows. `X, cm` and `Y, cm` never say which corner they count
from. A small bookcase draws as a blank rectangle and stays blank in the export.

## Detector-only findings

| Rule | Count | Verdict |
|---|---|---|
| `clipped-overflow-container` | 2 | **False positive.** The hits are `html` and `body`, which is the app shell. Verified at 1280×500: the settings panel is not cut off and the furniture panel still scrolls. The rule is aimed at a card or menu wrapper cutting off a popover |
| `flat-type-hierarchy` | 1 | **Stands, at P2.** Six sizes between 12.5 and 21px, a range of 1.7:1. It supports A's finding that headings and disclosure buttons are indistinguishable |

Suppressed for Excursion web: `repeating-stripes-gradient` (1 hit, the graph-paper
gutters). The other five ignored ids produced nothing.

Contrast: every text pair on both real surfaces clears 5.5:1 in both schemes. The
apparent failures are tokens the stylesheet marks non-text, used as marked.

## Minor observations

- `Clear saved plan` is the only irreversible action, wipes both history stacks,
  and has the mildest wording of the three confirms. P2.
- The exported PNG carries no room name, so two rooms sent to family are two
  anonymous drawings. P2.
- Switching rooms keeps the panel's scroll position and clears the status line,
  so nothing confirms the switch. P2.
- The PNG inherits the exporter's colour scheme, so a dark-mode user sends a dark
  plan. P3.
- No upper bound on item size; a 99999cm piece is accepted. P3.
- `blocks the door` stays singular with two doors, where the overlap flag names
  each item. P3.
- Rotate, Duplicate and Delete sit 417px from the item they act on. P3.
- Inclusive Sans's slashed zero renders every measurement as `15Ø × 2ØØ` in
  editable number fields. Brand-correct, but worth a decision in a tool whose
  content is numbers. P3.

## Checked and found clean

Overlapping openings on one wall are flagged in words. Undo and redo are one step
per action with no drift, across rotate, duplicate, delete and three undos. The
corner-cut flow reveals its fields, prefills a third of each dimension and moves
focus with the text selected. The L-shaped room draws correctly with the notch
labelled. No horizontal overflow at 320 or 375. Zero italics, one bolted rule,
squares throughout but the documented icon tile. The reduced-motion guard is
present and the only transition is 0.18s. Both `role="status"` regions are
correct and the Grid switch and Snap group are properly labelled.
