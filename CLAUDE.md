# CLAUDE.md — <PROJECT NAME> knowledge wiki

This file is the **entry point for the LLM (Claude Code) working in this repository.**
Human-facing notes live in `README.md`. Detailed, situation-specific rules live in `schema/`.

The design follows Andrej Karpathy's [LLM wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). The one idea that matters:

> Instead of re-reading raw sources every time and synthesizing an answer,
> the LLM **incrementally builds and maintains a persistent wiki** and uses it as the primary reference layer.

---

## Purpose of this repository (most important)

<!-- Replace this paragraph. One or two sentences: what knowledge does this wiki hold, and who reads it? -->
<!-- Example: "Capture the *why* behind decisions made while developing ../my-app, so the team and future agents never re-litigate them." -->

- The star categories are `<CATEGORY-1>/` and `<CATEGORY-2>/`. Grow those first.
- Do not bulk-import documents that nobody asked for. Add raw sources when they answer a real question.

---

## Ground rules for the LLM (read every session)

- **You decide when to record.** Never ask "should I write this down?" — if a capture trigger fires (see `schema/capture-workflow.md`), write it and report afterwards.
- **`raw/` is immutable.** Never edit it. To update, add a new file (or bump a submodule pointer).
- **`wiki/` is yours.** Humans rarely write there directly; they add raw sources or ask you.
- **Every claim cites a source.** Code → `raw/code/<repo>/<path>:<lines> @<commit>`; documents → `raw/<path>`; web → URL + retrieval date; conversation-derived → mark "from conversation, summarized (no transcript)".
- **Prefer editing an existing page over creating a new one.**
- **Write in <LANGUAGE>.** File and directory names stay ASCII kebab-case regardless of language.

---

## Three-layer architecture

```
raw/        Immutable external sources (documents, exports, code submodules, conversation excerpts).
            LLM reads only. This is the source of truth for facts.

wiki/       Markdown the LLM generates and maintains. The primary reference layer for humans and agents.
            Answer from here first; fall back to raw/ only when the wiki is missing something,
            then write what you learned back into the wiki.

CLAUDE.md   Entry point for the LLM (this file)
  + schema/ Situation-specific rules (read the one that matches what you are about to do)
```

Information flows **raw → (ingest / capture) → wiki → (query) → answer**, and good answers are written back into the wiki so knowledge compounds.

---

## raw/ layout

| Path | Role | Policy |
|---|---|---|
| `raw/docs/` | Documents, exports, PDFs, CSVs | Append only |
| `raw/code/<repo>/` | Source code as a git submodule | Bump the pointer; never edit |
| `raw/conversations/<topic>.md` | Excerpts of decisive conversations | Volatile by default; pin only the lines that mattered |

---

## wiki/ layout

```
wiki/
├── index.md         # Category index. Every page is reachable from here.
├── log.md           # Append-only history of capture / query / lint operations.
├── <category-1>/    # ★ star category — describe it here
├── <category-2>/    # ★ star category — describe it here
├── concepts/        # Definitions of domain terms and metrics
├── sources/         # Summaries / chapter maps of ingested raw sources
└── ...              # Add categories only when a real need appears. No empty stubs.
```

Pick categories by asking: *why* → decisions, *what does the word mean* → concepts, *summary of a raw source* → sources. When a page could live in two places, pick the one a reader would open first and link from the other.

---

## Operations

| Operation | What it does | Rules | Command |
|---|---|---|---|
| **Capture** | Record a decision / fact / result the moment it is confirmed | `schema/capture-workflow.md` | `/wiki-capture` |
| **Query** | Answer from the wiki first; fall back to raw; write back | (inline in the command) | `/wiki-query` |

Ingest, Lint (with a script), Status and Handover commands, the raw-protection and unpushed-changes hooks, and four worked examples are in the Starter kit; Pro adds three finished, verified wikis: https://github.com/vellumlabs/llm-wiki-kit#starter-and-pro

---

## Hard rules

### Never
- Edit anything under `raw/`
- Paste large verbatim chunks of raw into the wiki (summarize + cite instead)
- Create a page that nothing links to
- Put non-ASCII characters in file or directory names
- Invent a new top-level category for the LLM's convenience
- Record trivia: trade-off-free implementation details, temporary bug fixes, throwaway confirmations

### Always
- Frontmatter on every wiki page (`type`, `title`, `status`, `updated` are required)
- A citation for every claim
- Update `wiki/index.md` and append to `wiki/log.md` after any create/update
- Commit and push the wiki when you are done

Details: `schema/wiki-conventions.md`.

---

## schema/ — which rule file to read, and when

| Situation | Read |
|---|---|
| Before creating or editing any wiki page | `schema/wiki-conventions.md` |
| Before recording a decision / result ★ | `schema/capture-workflow.md` |

CLAUDE.md and `schema/` co-evolve with use. When you find a contradiction or a gap, propose a change; the human reviews it.
