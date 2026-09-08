# Handoff — Design QA, room planner app shell redesign

- Status: complete
- Role: design-qa · Last updated: 2026-09-08 17:20
- Project: /Users/davidperkins/Code/personal/room-planner (branch `redesign`)
- Read first: brief.md · decisions.md · brand.md · handoffs/2026-09-08-1520-ui-designer-app-shell-redesign.md
- Output: qa/qa-report.md (this session's deliverable)

## Task

Gate 2 QA of the app-shell/three-place panel redesign, served at
http://127.0.0.1:8733/index.html. Verify the pre-confirmed focus-loss bug
(select item, focus #sel-x, press toolbar Delete), then audit keyboard/focus,
app shell reachability, screen reader semantics, contrast (incl. new berry
clash flag), responsive, regressions vs `master`, and usability of the new
arrangement.

## What was checked and found (see qa/qa-report.md for full detail)

1. **Focus-loss bug** — confirmed exactly as described, but with an important
   scope correction: it does NOT reproduce on an ordinary mouse click or a
   Tab+Enter keyboard activation of the toolbar Delete button, because a real
   pointer mousedown moves DOM focus to the button before its click handler
   runs (verified: `document.activeElement` after a real `computer` click was
   `btn-delete`, not `sel-x`). It reproduces on any activation path that
   invokes the button without a native mousedown first — scripted/automated
   clicks (`el.click()`), and by extension switch-access/voice-control style
   "invoke" activation. Root cause: `restoreRowFocus()` (line ~968) calls
   `f.focus()` on the captured editor field unconditionally, even when
   `renderSelectedForm()` has just set `#selected-section` to `hidden` (line
   ~1573). Fix: guard the call with a visibility check
   (`f.offsetParent !== null`, or `!f.closest('[hidden]')`) before focusing;
   same shared function also guards `renderOpeningRows`, and the same
   vulnerability class applies to the toolbar Undo button after a delete.
2. Rotate/nudge/undo/duplicate via keyboard on canvas-focused items: fine.
3. Editing panel fields, toggling Remove/Place: fine, cheap-refresh path
   preserves focus and DOM nodes.
4. Opening add/remove: fine, no shape-check optimisation there but
   `restoreRowFocus` clamps correctly.
5. New room / Delete room / switch room: fine, `reveal()` + explicit focus
   works, no dangling focus after deleting down to one room.
6. Heading outline: one h1, five h2 (incl. both inside `<summary>`), no
   skipped levels — clean.
7. Contrast computed from live tokens, light and dark: all text pairs clear
   AA (5.5:1–13.7:1). `.row-flag` correctly uses `--berry-ink`, not the
   non-text `--berry`. Overlapping-opening flag renders in words + colour in
   both themes — verified via screenshot.
8. Responsive: no horizontal scroll at 1280, 900, 899, 640, 375. App shell
   holds at 900×600 and 1280×500 (short viewports); footer/Clear-saved-plan
   reachable by scrolling the panel to its end in all of them.
9. Regression: `room-pick` `<select>` text is hard-clipped mid-character
   ("Bedroom 1 · 4.20 m × 3.50 n|") at ≤560px width. This is a genuine
   consequence of the redesign — the switcher moved into the header and
   shares a flex row with the masthead/New room button below 560px
   (`.room-switch select{flex:1 1 0;min-width:0}`), which master's stacked
   panel never had to do. Minor/polish severity; native `<select>` doesn't
   support `text-overflow:ellipsis` reliably, so the fix is a shorter label
   or a two-line stack below ~400px, not pure CSS.
10. Copy text / Load plan verified working once the Share `<details>` is
    opened (the buttons are unreachable while collapsed, which is correct
    native `<details>` behaviour, not a bug). Clipboard API is blocked in
    this headless context; the app's own fallback (write to the paste box,
    `role="status"` message) was exercised and works.
11. Usability: selecting an item deep in a 14-row list does not cause an
    unexpected scroll jump (`select()` doesn't call `scrollIntoView`).
    Empty state text is clear and points at both "Common sizes" and manual
    add.

## Not verified / out of scope this run

- PNG export binary correctness — not re-checked; ui-designer's handoff
  already confirmed a real 3200×2600 PNG and nothing in this redesign
  touches that code path.
- `file://` origin — refused by the browser tool per the brief; nothing in
  the change depends on origin (no fetch/modules/new requests).
- Real screen reader (VoiceOver/NVDA) pass — only the accessibility tree,
  aria-labels, roles and heading structure were checked programmatically.
- 200% *browser zoom* specifically — approximated with an equivalent-width
  viewport resize (640×400), not real browser zoom, since the tool can't
  drive native zoom. No breakage found at that width, but text reflow under
  true zoom (vs viewport shrink) was not independently confirmed.

## Next action

None outstanding from this pass. qa/qa-report.md is the deliverable; hand
back to whoever runs Gate 2 / the fix cycle.
