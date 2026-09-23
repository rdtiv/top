# Contributing to the roadmap

This repository holds a proposal, not a product. The maintainer of Try Omarchy decides; the documents here propose. Feedback is welcome from anyone, in three shapes:

- **A decision.** The executive briefing proposes five. Each has an open issue labelled `decision`. Reply there with accept, amend (say how), or reject (say why). One issue per decision, so the answer is easy to find later.
- **A row.** The technical roadmap is a set of numbered rows (2.1, 3.4, and so on). Open an issue labelled `row`, name the row, and say what is wrong, missing, or already done. A pull request that edits the row directly is just as welcome.
- **Something missing.** Anything the documents do not cover at all goes in the `missing` issue (#6); if it grows into a real row, it gets its own `row` issue.
- **A correction.** Every factual claim carries an anchor: a `path:line` in omacom/try-omarchy, an issue or pull request number, or a URL. If a claim is wrong, open an issue labelled `correction` with the anchor and the corrected wording, or send a pull request that changes both the text and the source index.

Conventions the documents follow, so that edits fit:

- Anchors dated 2026-09-16 point at commit `58cbac5`; anchors marked "main" point at `7f3ce66`. When you cite something newer, say which commit.
- "Try" means Try Omarchy for macOS. "try-omarchy-windows" is the Windows project. "Parallels" is the Windows VM that runs beside Try on the Mac.
- Horizons: 0 unblocks the daily-driver claim; 1 is what a v1 release gates on; 2 is after v1. There is no other scheme.
- Rows keep their eight fields (Today, Why it matters, Candidate path, Owner, Dependency, Horizon, Evidence anchors, Risk). Horizon 0 rows also keep "If daily driver ships without it".
- Nothing here assigns work to anyone. Propose, do not volunteer others.

Decisions the maintainer rejects stay in the documents as a one-line "declined, because", not a deletion, so later readers see the reasoning.
