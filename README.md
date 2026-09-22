# LLM Wiki Kit for Claude Code

*by [Vellum Labs](https://github.com/vellumlabs)*

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

```bash
git clone https://github.com/vellumlabs/llm-wiki-kit my-wiki && cd my-wiki && rm -rf .git && git init
grep -rn "<PROJECT NAME>\|<CATEGORY-1>\|<CATEGORY-2>\|<LANGUAGE>" . --include=*.md   # fill these in
claude
```

Then in Claude Code: drop a file into `raw/docs/` and ask a question. After a decision in conversation, run `/wiki-capture`.

## What's in the free kit

- `CLAUDE.md` — three-layer contract, ground rules, hard rules
- `schema/wiki-conventions.md` — frontmatter, naming, citations, cross-links, ADR entry format
- `schema/capture-workflow.md` — the four capture triggers and the "do not record" list (noise is the #1 failure mode)
- `/wiki-capture`, `/wiki-query` — two slash commands

## Pro

The Pro kit ($19, one-time, includes 1.x updates) adds what makes the wiki hold up over months:

| | Free | Pro |
|---|---|---|
| Commands | capture, query | + ingest, lint, status, handover |
| Schema files | 2 | 6 (ingest, query, lint, glossary-pointers) |
| `raw/` protection hook | | ✓ blocks the agent from editing raw |
| Unpushed-changes Stop hook | | ✓ never lose a session's work |
| `scripts/lint.py` | | ✓ frontmatter, orphans, broken links, stale pages; CI exit code |
| Examples | | ✓ decision-capture (wiki beside an app), investment, diary, revenue-experiments |
| Guide | | ✓ setup, customization, sibling-repo pattern, troubleshooting |

**→ [Get Pro ($19, one-time)](https://buy.polar.sh/polar_cl_6VoQbr5434gNyp4AQx8dKFazrJsMwdm92EtVT1BVTa3)**

## Why this shape

- **Autonomy is the design.** The agent decides when to record; it never asks "should I write this down?"
- **Citations or it didn't happen.** Every claim points at `raw/…`, a URL with a date, or is labeled as conversation-derived.
- **Domain pages, not event pages.** `decisions/billing.md` with dated entries beats fifty files.
- **Immutable raw.** Corrections are new files; history is never rewritten.

## License

MIT for the free kit. The Pro kit has its own license (use in unlimited own projects; no redistribution).
