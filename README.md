# Try Omarchy roadmap: a proposal for discussion

A community-facing proposal for taking [Try Omarchy](https://github.com/omacom/try-omarchy), the macOS app that runs the Omarchy Linux desktop in a virtual machine on Apple silicon, from "try it" to a daily driver for people whose home base is macOS.

Status: first draft, for discussion. It exists to start the conversation that leads to a more refined roadmap; nothing in it is settled. The maintainer decides; this repository proposes.

| Document | What it is |
|---|---|
| [`roadmap/01-exec-briefing.md`](roadmap/01-exec-briefing.md) | The executive briefing: TL;DR, terms, who it is for, what main already is, the three contracts, five decisions, three horizons, non-goals, and the asks. Ten pages. Read this first. |
| [`roadmap/02-technical-roadmap.md`](roadmap/02-technical-roadmap.md) | The technical roadmap: a problem graph ordered by what blocks what, with owner, horizon, evidence anchor, and risk on every row; the two update pipes; the capability matrix; a liftable draft of a Mac readiness document; open questions. |
| [`roadmap/appendix-source-index.md`](roadmap/appendix-source-index.md) | Every file anchor, issue, pull request, and URL the two documents cite, with the date each was read. |
| [`roadmap/README.md`](roadmap/README.md) | Conventions used in the documents. |

Rendered page: https://claude.ai/artifact/NCzK1pkSpjGxpyrVmcNABa

## How to give feedback

Read the executive briefing first (about ten pages). Then pick the shape that fits:

1. **A decision.** The briefing proposes five. Each has its own issue: reply accept, amend (say how), or reject (say why).
   [#1 Product contract](https://github.com/rdtiv/top/issues/1) · [#2 Update contract](https://github.com/rdtiv/top/issues/2) · [#3 Packaging ownership](https://github.com/rdtiv/top/issues/3) · [#4 VMM bet](https://github.com/rdtiv/top/issues/4) · [#5 v1 definition](https://github.com/rdtiv/top/issues/5)
2. **A row.** The technical roadmap is numbered rows (2.1, 3.4, ...). Open an issue with the `row` label naming the row, or edit the row in a pull request.
3. **A correction.** Every claim carries an anchor. If one is wrong, open a `correction` issue with the anchor and the fix, or send a pull request that changes the text and the source index together.
4. **Something missing.** Anything the documents do not cover goes in [#6 What did the documents miss?](https://github.com/rdtiv/top/issues/6)

Discussions are open for anything that is not one of those. [CONTRIBUTING.md](CONTRIBUTING.md) has the conventions edits should follow. The maintainer decides; rejected proposals stay in the documents as "declined, because" so the reasoning is kept.
