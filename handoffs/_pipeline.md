# Pipeline — Room planner redesign

Updated: 2026-09-08

| Stage | Status | Deliverable |
|---|---|---|
| Research | done | `research/panel-research.md` |
| Gate 1, Frame | done, logged in decisions.md | recommendation (c) accepted |
| Concept, ui-designer | done, commit dab31dc | restructured `index.html` + `brand.md` |
| Gate 2, Direction | not started | clickable prototype |
| QA, design-qa | done, 1 major + 1 minor | `qa/qa-report.md` |
| Fix cycle | done, commit b953dbc | both findings fixed and re-verified |
| Gate 3, Ship | done, merge b41566b | merged and pushed; Pages rebuilding |

**Next action:** confirm the live URL serves the redesign, then the project is
closed. `improvements.md` is the reading copy of what changed and why.

**Worth carrying forward.** QA's correction on the focus bug is the most useful
thing in this project: a mouse click never triggered it, because the pointer
moves focus to the button before the handler runs. Only switch access and voice
control hit it. Testing that class of bug with a mouse will always say it is
fine.
