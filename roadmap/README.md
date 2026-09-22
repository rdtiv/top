# Try Omarchy daily-driver briefing 

## (2026-09-16)

Files in this folder, in reading order:

| File | What it is | Who it is for |
|---|---|---|
| `01-exec-briefing.md` | Executive briefing. Ten pages max. Opens with a TL;DR and a terms list so it reads without project context; then thesis, personas, five decisions, three horizons, the asks, non-goals. | Eduardo (primary). Fail-Safe and Omarchy M owners may read. |
| `02-technical-roadmap.md` | Technical roadmap. Opens with a TL;DR and an 89-term glossary. A problem graph ordered by blocking relationship, with owner, horizon, evidence anchor, and risk on every row, plus a proposed Mac V1-READINESS. | Whoever sequences issues. Written so Eduardo could lift it into `docs/V1-READINESS.md`. |
| `appendix-source-index.md` | Every source the two documents cite: repository file anchors, issues, PRs, and URLs, with the date each was read. | Verification. |

Conventions used in both documents:

- File anchors are `path:line` in `omacom/try-omarchy` at commit `58cbac5` (main on 2026-09-15) unless another repository is named.
- "Try" means Try Omarchy for macOS. "try-omarchy-windows" is the Windows project used as the process template. "Parallels" is the Windows VM that runs beside Try on the Mac.
- Horizons: Horizon 0 unblocks the daily-driver claim; Horizon 1 is what a Mac V1-READINESS gates; Horizon 2 is after v1.
- "Your call" marks a place where the maintainer decides. The documents propose; they do not assign.

These files live in the `rdtiv/top` roadmap repository, outside try-omarchy, on purpose. Nothing here is a pull request.
