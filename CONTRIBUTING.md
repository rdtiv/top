# Conventions

These are working notes, not a project seeking contributions; discussion with the maintainer is under way. The conventions below keep the documents consistent for anyone reading or citing them. If you spot a factual error, an issue with the anchor and the corrected wording is welcome.

- Anchors dated 2026-09-16 point at commit `58cbac5`; anchors marked "main" point at `e1a0dbe` (2026-09-30). Anything newer says which commit it was read at.
- "Try" means Try Omarchy for macOS. "try-omarchy-windows" is the Windows project. "Parallels" is the Windows VM that runs beside Try on the Mac.
- Horizons: 0 unblocks the daily-driver claim; 1 is what a v1 release gates on; 2 is after v1. There is no other scheme.
- Rows keep their eight fields (Today, Why it matters, Candidate path, Owner, Dependency, Horizon, Evidence anchors, Risk). Horizon 0 rows also keep "If daily driver ships without it".
- Every factual claim carries an anchor: a `path:line` in omacom/try-omarchy, an issue or pull request number, or a URL, listed in the source index with the date it was read.
- Nothing here assigns work to anyone. Propose, do not volunteer others.

Decisions the maintainer declines stay in the documents as a one-line "declined, because", not a deletion, so later readers see the reasoning.
