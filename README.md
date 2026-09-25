# LLM Wiki Kit for Claude Code

*by [Vellum Labs](https://github.com/vellumlabs)*

**Claude Code forgets your project's decisions between sessions; this kit makes it keep a cited, self-maintained markdown wiki in your repo instead — one command to set up, no database.**

Give Claude Code a **persistent, self-maintained knowledge wiki** — the setup Andrej Karpathy sketched in his [LLM wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), turned into a drop-in repository template.

Instead of re-reading your documents every session, the agent builds a wiki once, answers from it, and writes new knowledge back. Facts stay in an immutable `raw/`; the agent owns `wiki/`; `CLAUDE.md` + `schema/` tell it exactly how to behave.

```
raw/        immutable sources (docs, exports, code submodules)   ← humans add, agent reads
wiki/       markdown the agent maintains, with citations          ← agent writes, humans read
schema/     the rules, split by situation                         ← short, evolvable
CLAUDE.md   the contract Claude Code reads every session
```

Extracted from wikis running beside a production app, an investment journal, a daily diary, and an autonomous revenue experiment.

## Quick start

**As a Claude Code plugin** (recommended): inside Claude Code, run

```
/plugin marketplace add vellumlabs/llm-wiki-kit
/plugin install llm-wiki@vellumlabs
/llm-wiki:init a wiki that captures the decisions behind this app
```

`/llm-wiki:init` scaffolds `CLAUDE.md`, `WIKI.md`, `schema/`, `wiki/` and `raw/` into the current repository and fills the placeholders. It leaves your `README.md` alone, merges into an existing `.gitignore` instead of replacing it, and commits the scaffold as one commit that touches only its own files (your other uncommitted work stays uncommitted). Then use `/llm-wiki:capture` after a decision and `/llm-wiki:query <question>` to answer from the wiki.

What you should see (verified 2026-09-23 on Claude Code 2.1.280 in a fresh repo that already had its own `README.md` and `.gitignore`; commit behaviour re-verified 2026-09-24 on 2.1.281 with uncommitted work present):

```
> /llm-wiki:init a wiki that captures the decisions behind this invoicing app
  .gitignore     merged (your lines kept)
  CLAUDE.md      placeholders filled: project name, purpose, categories decisions/ + billing-rules/
  WIKI.md        short how-to for humans
  README.md      yours, untouched
  raw/  schema/  wiki/index.md  wiki/log.md
  committed 8559093 "Add LLM wiki scaffold" (10 files; app.py edits and notes.txt left alone, not pushed)

> We decided invoice numbers are per-tenant sequential with no gaps; we rejected UUIDs. /llm-wiki:capture
  wiki/decisions/invoice-numbering.md   dated entry, alternatives rejected, open questions
  wiki/log.md                           + [capture] line
  committed "wiki: record decision ..." (wiki files only); pushed only if the branch has an upstream

> /llm-wiki:query why don't we use UUIDs for invoice numbers?
  answer from wiki/decisions/invoice-numbering.md, cited as conversation-derived; + [query] line in wiki/log.md
```

**As a template**:

```bash
git clone https://github.com/vellumlabs/llm-wiki-kit my-wiki && cd my-wiki && rm -rf .git plugin .claude-plugin && git init
grep -rn "<PROJECT NAME>\|<CATEGORY-1>\|<CATEGORY-2>\|<LANGUAGE>" . --include=*.md   # fill these in
claude
```

Then in Claude Code: drop a file into `raw/docs/` and ask a question. After a decision in conversation, run `/wiki-capture`.

## What's in the free kit

- `CLAUDE.md` — three-layer contract, ground rules, hard rules
- `schema/wiki-conventions.md` — frontmatter, naming, citations, cross-links, ADR entry format
- `schema/capture-workflow.md` — the four capture triggers and the "do not record" list (noise is the #1 failure mode)
- `/wiki-capture`, `/wiki-query` — two slash commands (as a plugin: `/llm-wiki:init`, `/llm-wiki:capture`, `/llm-wiki:query`)

## Starter and Pro

The paid kits add what makes the wiki hold up over months:

| | Free | Starter ($19) | Pro ($49) |
|---|---|---|---|
| Commands | capture, query | + ingest, lint, status, handover | same |
| Schema files | 2 | 6 (ingest, query, lint, glossary-pointers) | same |
| `raw/` protection hook | | ✓ blocks edits and overwrites, allows new files and appends | ✓ |
| Unpushed-changes Stop hook | | ✓ never lose a session's work | ✓ |
| `scripts/lint.py` | | ✓ frontmatter, orphans, broken links, stale pages; CI exit code | ✓ |
| Examples | | ✓ decision-capture, investment, diary, revenue-experiments | ✓ |
| Guide | | ✓ setup, customization, sibling-repo pattern, troubleshooting | ✓ |
| **Finished, verified wikis** | | | ✓ codebase-decisions, research-notes, team-runbook, life-log (a personal diary: a year of entries, receipts, money and people pages), each with a `VERIFIED.md` transcript (lint 0, real ingest/query/capture runs) |
| Updates | | 1.x line | 12 months, including the finished wikis |

**→ [Get Starter ($19)](https://buy.polar.sh/polar_cl_6VoQbr5434gNyp4AQx8dKFazrJsMwdm92EtVT1BVTa3)** · **→ [Get Pro ($49)](https://buy.polar.sh/polar_cl_kRvZ7bzOE2EGSzefIcOJRKqVxfohNfm8kIJvJ4AkfA0)**

## Why this shape

- **Autonomy is the design.** The agent decides when to record; it never asks "should I write this down?"
- **Citations or it didn't happen.** Every claim points at `raw/…`, a URL with a date, or is labeled as conversation-derived.
- **Domain pages, not event pages.** `decisions/billing.md` with dated entries beats fifty files.
- **Immutable raw.** Corrections are new files; history is never rewritten.

## License

MIT for the free kit. The Pro kit has its own license (use in unlimited own projects; no redistribution).
