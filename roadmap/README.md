# Try Omarchy daily-driver roadmap: the documents

Files in this folder, in reading order:

| File | What it is |
|---|---|
| `01-exec-briefing.md` | Executive briefing. Opens with a TL;DR and a terms list so it reads without project context; then thesis, personas, five decisions, three horizons, the asks, non-goals. |
| `02-technical-roadmap.md` | Technical roadmap. A problem graph ordered by blocking relationship, with owner, horizon, evidence anchor, and risk on every row, plus a liftable draft of a Mac readiness document. |
| `appendix-source-index.md` | Every source the two documents cite: repository file anchors, issues, PRs, and URLs, with the date each was read. |

Conventions used in all three:

- File anchors are `path:line` in `omacom/try-omarchy` unless another repository is named. Bare anchors are read at `58cbac5` (main on 2026-09-15); anchors marked "main" are read at `898f920` (2026-09-28), cited by file only where line numbers have moved.
- "Try" means Try Omarchy for macOS. "try-omarchy-windows" is the Windows project used as the process template. "Parallels" is the Windows VM that runs beside Try on the Mac.
- Horizons: Horizon 0 unblocks the daily-driver claim; Horizon 1 is what a Mac readiness document gates; Horizon 2 is after v1.
- "Your call" marks a place where the maintainer decides. The documents propose; they do not assign.

These are my working notes, kept in `rdtiv/top` outside try-omarchy on purpose; nothing here is a pull request. The discussion will happen in the Omarchy dev Discord once its channel opens.
