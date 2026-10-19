# LLM Wiki Kit for Claude Code

*by [Vellum Labs](https://github.com/vellumlabs)*

**Keep the decisions your code can't show — why you chose them, what you rejected, and what they replaced — in a cited markdown wiki that Claude Code maintains in your repo. One command to set up, no database.**

Give Claude Code a **persistent, self-maintained knowledge wiki** — the setup Andrej Karpathy sketched in his [LLM wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), turned into a drop-in repository template.

The agent builds the wiki from your sources, answers from it with citations, and records the decisions you make in conversation, so the next session knows *why* invoice numbers are sequential and not just *that* they are. That last part is what no retrieval system can give you: it is knowledge that exists in nobody's source files. Facts stay in an immutable `raw/`; the agent owns `wiki/`; `CLAUDE.md` + `schema/` tell it exactly how to behave.

```
raw/        immutable sources (docs, exports, code submodules)   ← humans add, agent reads
wiki/       markdown the agent maintains, with citations          ← agent writes, humans read
schema/     the rules, split by situation                         ← short, evolvable
CLAUDE.md   the contract Claude Code reads every session
```

Extracted from wikis running beside a production app, an investment journal, a daily diary, and an autonomous revenue experiment.

## What it is not (measured)

It is **not a faster or cheaper RAG**. On a 1,200-line library (pallets/itsdangerous, Claude Code 2.1.286, 2026-10-01, each step its own session) answering four code questions by reading the source directly took 124 s / $1.93; the same four through the wiki took 191 s / $2.59, because the agent checks its answer against the source anyway, and building the wiki first cost 7 min 51 s / $4.23 on top. Where the wiki won: a question whose answer is a *decision*, not code. Without the wiki the agent correctly said it could not answer; after one `/llm-wiki:capture`, the next session answered it in 22 s / $0.47 with a dated citation. Use the kit for the decisions, rules and measured results that are not in your source, and let the agent read the source for the rest. Full write-up with the commands: https://dev.to/vellumkasane/llm-wiki-vs-rag-what-each-one-is-and-what-i-measured-when-i-used-a-wiki-as-a-rag-replacement-it-5fg3

It is also **not a better memory for a handful of current rules**. Recording the same 30 design decisions in `CLAUDE.md`, in Claude Code's auto-memory and in this wiki (Claude Code 2.1.288, 2026-10-03, each question in a fresh session), all three got the facts right on all three recall questions, and `CLAUDE.md` was the fastest and cheapest every time (6–17 s / $0.12–0.13 per question, against 12–39 s / $0.17–0.29 for the wiki). The difference showed when a decision changed: `CLAUDE.md` rewrote the section and dropped the reason the old option had been rejected; the wiki kept the superseded entry, dated and struck through, and flagged a conflict nobody had mentioned (unsubscribe links signed with a rotating key stop working 90–120 days after sending). If you only need today's rules, use `CLAUDE.md`. Use the kit when the history of a decision matters and the team shares it through git.

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

`/llm-wiki:init` never stops to ask (plugin 1.0.5, verified 2026-10-05 on 2.1.289 with `claude -p` in three fresh repos that had a README, a `.gitignore` and uncommitted work): with no description it reads the repository name and README, fills every placeholder, commits, and lists the values it chose (4–5 turns, $0.23–0.26); if `CLAUDE.md`, `WIKI.md`, `wiki/` or `schema/` already exist it changes nothing and says which file to move. Before 1.0.5 init without a description copied the template and then waited for an answer, leaving the scaffold unfilled and uncommitted in headless runs.

Pinning is a rule, not a mood (plugin 1.0.4, verified 2026-10-02 on 2.1.287 in a fresh repo, three separate sessions): a decision that rejected a named alternative landed the owner's decisive lines in `raw/conversations/invoice-numbering.md` (11 lines, cited from the wiki entry); a load-test result stayed a volatile summary with no transcript; a later reversal on the same topic was appended to the existing file as a second dated section instead of creating another file. Before 1.0.4 the same decision was pinned one day and only summarized the next.

Two things learned from running the kit against a codebase that is not ours (verified 2026-09-30 on 2.1.285):

- **Push only goes to remotes you can write to.** If the repository is a plain clone of someone else's project (not your fork), the first push after a capture is rejected; the commands then set `git config llmwiki.push false` and stop trying, and every wiki change stays as local commits. Unset it (`git config --unset llmwiki.push`) once you point `origin` at your own fork.
- **Build from a full clone.** With `git clone --depth 1` there is no history, so the agent cannot cite *when* something was introduced (`git log -S`) and has to mark those claims as unsourced. Use a full clone (or `git fetch --unshallow`) when you want dated citations.

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
