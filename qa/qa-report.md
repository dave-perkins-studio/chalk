# QA report: room planner app shell redesign

**Target:** `index.html` on branch `redesign`, served at `http://127.0.0.1:8733/index.html`
**Judged against:** `perkins-brand/reference/excursion-web.md` (§6, §9) and this project's `brand.md`
**Compared against:** `master` (`git show master:index.html`) where relevant
**Method:** Live in Chrome via the browser tool: DOM/JS inspection, real
keyboard and mouse input, computed-style contrast checks, viewport resizes.
No files changed.

## Findings

| # | Severity | What | Where | WCAG | Evidence |
|---|---|---|---|---|---|
| 1 | **Major** | After deleting the selected item while focus is in one of its editor fields, focus is left on that field even though `#selected-section` has just been set `hidden` (`display:none`). The field is present in the DOM and registered as focused, but invisible and unreachable by further Tab presses in the normal way. | `index.html`, in `restoreRowFocus()` (~line 968–980), specifically the `if (at.editor)` branch, which calls `f.focus()` without checking whether `f` is still visible. Triggered from `renderItemRows()` (~1402–1460) via `deleteSelected()` (~1294). Same shared function is used by `renderOpeningRows()`, and the same class of bug applies to the toolbar **Undo** button after a delete, for the same reason. | 2.4.7 Focus Visible (no visible focus indicator exists anywhere on screen once this happens); 2.4.3 Focus Order (focus is not left in an operable, perceivable place) | **Reproduced.** `select(item, true); $('sel-x').focus(); $('btn-delete').click();` → `document.activeElement.id === 'sel-x'`, `offsetParent === null`, `closest('#selected-section').hidden === true`. Screenshotted before/after: focus ring visible on `#sel-x` before delete, no focus ring anywhere on screen after. **Scope correction to the brief:** this does not reproduce on an ordinary mouse click or a Tab-then-Enter keyboard activation of the Delete button, because Chrome moves DOM focus to the button on `mousedown`, before the click handler (and its `restoreRowFocus` call) runs. Verified with a real `computer` click, which left focus on `#btn-delete`, not `#sel-x`. It does reproduce on any activation that invokes the button without a native mousedown first: scripted/automated clicks, and by the same mechanism switch-access or voice-control "invoke" activation, which several real assistive-technology users rely on instead of a physical click. |
| 2 | Minor | The room switcher `<select>` clips its option text mid-character below 560px width, with no ellipsis: "Bedroom 1 · 4.20 m × 3.50 n" (final "m" cut off). | `index.html`, in the CSS `.room-switch select{flex:1 1 0;min-width:0}` under `@media (max-width:560px)` (~line 235), text built in `renderRoomSwitcher()` (~1356–1364) | 1.4.10 Reflow is not failed (no scroll/loss of content), but the label is not fully perceivable, closest to 1.3.1 in spirit rather than letter | **Reproduced** at 375px width (screenshot). This is a direct consequence of the redesign: the switcher moved from the panel into the header, where it now shares a flex row with the masthead and "New room" below 560px. `master`'s stacked panel never had to fit it into a shared row this narrow. Native `<select>` doesn't support `text-overflow:ellipsis` reliably across browsers, so the fix is a shorter string (drop " m" units, or drop dimensions entirely below ~400px) or a two-line stack, not a CSS tweak alone. |

No Blocker or Nit-level findings.

## What passed (verified, not inferred)

**Keyboard and focus** (priority 1)
- Rotate (`R`), nudge (arrow keys, with Shift for 10cm), Duplicate (`Cmd/Ctrl+D`), and Undo (`Cmd/Ctrl+Z`) all keep focus on the SVG item (`g.item`) that was focused, both immediately and across re-render.
- Editing a field in the row editor (name, width, notes) does an in-place text refresh (`refreshItemRows`) that never disturbs the focused node, confirmed by editing `#sel-name` mid-typing and checking `document.activeElement` was unchanged.
- Toggling **Place/Remove** on a row keeps focus on the clicked toggle button (same shape, cheap-refresh path).
- Clicking the toolbar Rotate and Undo buttons with real mouse input keeps focus on those buttons.
- Deleting the second-to-last opening, and adding a new opening, both restore focus sensibly (`restoreRowFocus` clamps its index correctly when the list shrinks; the static Add-opening button never moves).
- Switching rooms with an item selected clears the selection cleanly (`selectedId = null`) and leaves focus on the room switcher, with no dangling reference to a control that no longer applies.
- **New room** and **Delete room down to the last room** both call `reveal()` before focusing, so focus never lands inside a still-collapsed `<details>`; deleting the second-to-last room correctly redirects focus to the room switcher when the Delete button becomes disabled.
- Tabbing the whole page end to end (skip link → toolbar → SVG items → furniture rows → add block → openings → Room setup → Share → footer) reaches everything with no trap, matching the ui-designer's own test.

**The app shell** (priority 2)
- No horizontal overflow at any tested width: 1280, 900, 899, 640, 375px (`document.documentElement.scrollWidth === window.innerWidth` at each).
- At exactly 900px and above, `html`/`body` are `overflow:hidden` as documented; the panel's own scroll reaches its foot (ticket line + Clear saved plan button) confirmed by scrolling `.planner-panel` to `scrollHeight` and screenshotting.
- Two short-viewport cases specified in the brief both hold: 900×600 (clean two-column shell, panel scroll reaches the footer) and 1280×500 (same).
- Below 900px (899px tested) the layout correctly falls back to a single stacked column with ordinary page scroll, no clipping.
- A viewport-resize approximation of 200% zoom (640×400, roughly what a 1280-wide screen's layout viewport becomes at 200%) showed no breakage. This is an approximation, not a true zoom test; see Gaps below.

**Screen reader semantics** (priority 3)
- Heading outline is clean: one `h1` ("Room planner", visually hidden), five `h2`: plan caption, Furniture, Doors and windows, and one inside each of the two `<summary>` elements (Room setup, Share). No skipped levels.
- `role="status"` present and populated correctly on `#canvas-status` and `#share-status`; messages are pushed into them by `say()`, which also calls `reveal()` so a status text never lands inside a still-collapsed section.
- Every form field has a proper `<label for>`. The SVG furniture items carry `role="button"`, `tabindex="0"` and an `aria-label` that includes name, size, position, rotation and any flags, for example `"King bed, 150 by 200 centimetres, at 130, 0, blocks the door"`, so a flag is spoken, not just coloured.
- The new overlapping-opening flag is exposed the same way: `.row-flag` text ("overlaps 2 other openings on the same wall" / "overlaps the door on the same wall" for the singular case) is present in both the panel row and, where applicable, folded into the SVG item's `aria-label` for anything blocked by it.

**Contrast, light and dark** (priority 4), computed from the live tokens via a WCAG relative-luminance script, not from memory or the stylesheet doc:

| Pair | Light | Dark |
|---|---|---|
| `--ink` / `--paper` | 13.61:1 | 13.73:1 |
| `--ink-muted` / `--paper` | 6.13:1 | 6.18:1 |
| `--berry-ink` / `--paper` | 6.08:1 | 6.18:1 |
| `--berry-ink` / `--paper-tint` | 5.51:1 | 5.52:1 |
| `--blue` / `--paper` | 6.06:1 | 6.70:1 |

All comfortably clear AA (4.5:1) for text. `.row-flag` (the clash/off-wall/blocks-the-door flag text) uses `--berry-ink`, the text-safe token, not the non-text `--berry` used for the SVG stroke, confirmed in CSS (line 168) and by reading the live computed token values, which matched `excursion-web.md` exactly (no hand-typed hex anywhere in the page's `<style>` block).

**Responsive** (priority 5): covered under "The app shell" above. 1280, 900 and 375 all clean, no horizontal scroll anywhere.

**Regression check against `master`** (priority 6)
- `.opening` rendering (window/door/arc/hit-line construction) is byte-for-byte identical to `master` apart from the new `is-clashing` class, confirmed by diffing `renderOpening()`.
- SVG furniture items keep the same `tabindex`, `role`, and aria-label pattern as `master`; openings are still not independently keyboard-focusable on the canvas in either version, which is not a regression, just an existing limit shared with `master`.
- Copy text and Load plan both work correctly once the Share `<details>` is opened. (They cannot be triggered while it's collapsed because the buttons aren't rendered or clickable then: correct native `<details>` behaviour, not a defect.) Clipboard write/read is blocked in this headless browser context; the app's own documented fallback (write the plan into the paste box and announce "Couldn't reach the clipboard..." via `role="status"`) was exercised directly and works.
- Markdown round-trip was not re-verified per the brief (already confirmed byte-identical).

**Usability of the new arrangement** (priority 7)
- Empty state is genuinely useful: with a fresh room, `#empty-note` reads "Nothing in this room yet. Add a piece of furniture from the list, or open Common sizes for the usual ones, then drag it around the plan," and the list itself says "Nothing on the list yet." Both are plain, specific, and point at the next action.
- Selecting an item deep in a 14-row furniture list does not cause an unexpected scroll jump. `select()` doesn't call `scrollIntoView`, so the row editor expanding in place doesn't yank the list under the pointer if the clicked row was already in view. (Not tested: whether a keyboard/AT user Tab-ing to a row below the fold gets a jarring native `scrollIntoView` from the browser's own focus behaviour. That's normal browser behaviour, not app-specific, and wasn't flagged as a problem in the brief.)
- The `<details>` marker (drawn square-plus, losing its upright stroke when open) is legible and its focus state switches to `--focus-ink` as documented in `brand.md` §4, avoiding disappearing into the yellow focus field.

## Brand deviations

All checked deviations match `brand.md` exactly: full-width shell (no `.hold`), the app-shell scroll model, the panel/canvas hairline divider instead of a second surface colour, `<details>` carrying an `<h2>`, and the row editor living inside its own `<li>`. Live computed tokens confirmed no hand-typed hex anywhere in the page's own `<style>` block. Nothing found that deviates from Excursion without being recorded in `brand.md`.

## Gaps in coverage: checked lightly or not at all

- **True browser zoom** was not tested; a 640×400 viewport resize was used as an approximation of 200% zoom on a 1280 screen, which is not the same mechanism (text and layout reflow differently under real zoom than under a shrunk layout viewport). No problems found in the approximation, but this should be spot-checked with real zoom before shipping.
- **A real screen reader** (VoiceOver, NVDA) was not run. Findings on semantics rest on the accessibility tree, ARIA attributes, roles and heading structure read programmatically, which is a good proxy but not the same as hearing it.
- **PNG export** binary output was not re-verified in this pass; the ui-designer's own handoff already confirmed a real 3200×2600 `image/png`, and nothing in this redesign touches that code path, so the risk is low.
- **`file://` origin** was not exercised, per the same limitation noted in the ui-designer's handoff: the browser tool can't script local files. Nothing in the change depends on origin.
- Did not attempt to reproduce a focus-visible failure via an actual screen reader's "activate" gesture or Switch Control/Voice Control. Finding 1's assistive-technology scope is inferred from how the code path works (no native mousedown before the click), not directly observed on real AT.

## Fix for Finding 1

In `restoreRowFocus()`, guard the editor-focus branch with a visibility check before calling `.focus()`:

```js
if (at.editor) {
  const f = $(at.editor);
  if (f && document.body.contains(f) && f.offsetParent !== null) {
    f.focus(); setCaret(f, at.caret);
  }
  return;
}
```

This is the minimal fix: if the field the caller remembers is no longer visible (because the item it belonged to is gone), don't focus it. Leave focus wherever the actual activation already put it. In the affected cases there is nowhere better to redirect to automatically, but unfocused is strictly better than focused-and-invisible, which is the brief's own framing. Because `restoreRowFocus` is shared by `renderOpeningRows`, this one change also closes the equivalent path for openings and for the toolbar Undo button, without touching either of those call sites.
