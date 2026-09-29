# Try Omarchy roadmap: working notes

My working notes on a roadmap for taking [Try Omarchy](https://github.com/omacom/try-omarchy), the macOS app that runs the Omarchy Linux desktop in a virtual machine on Apple silicon, from "try it" to a daily driver for people whose home base is macOS. I keep them here so my thinking stays current and in one place. Anyone is welcome to read them.

Try Omarchy is created and maintained by Eduardo ([@themartiano](https://github.com/themartiano)). These notes exist because of what he and the project's contributors have already built, in a few weeks, at a pace few projects manage: the VM, the graphics chain, the host bridges, and a release a week. Thank you. The maintainer decides; these notes propose.

Status: First draft, for discussion. The discussion will happen in the Omarchy dev Discord once its channel opens; nothing here is settled.

| Document | What it is |
|---|---|
| [`roadmap/01-exec-briefing.md`](roadmap/01-exec-briefing.md) | The executive briefing: TL;DR, terms, who it is for, what main already is, the three contracts, five decisions, three horizons, non-goals, and the asks. Ten pages. Read this first. |
| [`roadmap/02-technical-roadmap.md`](roadmap/02-technical-roadmap.md) | The technical roadmap: a problem graph ordered by what blocks what, with owner, horizon, evidence anchor, and risk on every row; the two update pipes; the capability matrix; a liftable draft of a Mac readiness document; open questions. |
| [`roadmap/appendix-source-index.md`](roadmap/appendix-source-index.md) | Every file anchor, issue, pull request, and URL the two documents cite, with the date each was read. |
| [`roadmap/README.md`](roadmap/README.md) | Conventions used in the documents. |

Rendered page: https://claude.ai/artifact/NCzK1pkSpjGxpyrVmcNABa

## Where things are

The issues are working notes too. Each tracks where my thinking on one item stands and what is open for the maintainer; none of them waits on a reply.

- **Decisions.** The five the briefing proposes: [#1 Product contract](https://github.com/rdtiv/top/issues/1) · [#2 Update contract](https://github.com/rdtiv/top/issues/2) · [#3 Packaging ownership](https://github.com/rdtiv/top/issues/3) · [#4 VMM bet](https://github.com/rdtiv/top/issues/4) · [#5 v1 definition](https://github.com/rdtiv/top/issues/5)
- **Rows.** Proposed changes to specific roadmap rows: [#7](https://github.com/rdtiv/top/issues/7) · [#8](https://github.com/rdtiv/top/issues/8) · [#9](https://github.com/rdtiv/top/issues/9) · [#10](https://github.com/rdtiv/top/issues/10)
- **Known gaps.** What the notes do not cover yet: [#6](https://github.com/rdtiv/top/issues/6)

If you spot a factual error, an issue is welcome, but nothing here waits on replies. [CONTRIBUTING.md](CONTRIBUTING.md) has the conventions the documents follow. Proposals the maintainer declines stay in the documents as "declined, because" so the reasoning is kept.
