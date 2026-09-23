# Knowledge wiki

This repository keeps a knowledge wiki maintained by Claude Code, set up with the [LLM Wiki Kit](https://github.com/vellumlabs/llm-wiki-kit).

```
raw/        immutable sources (docs, exports, code submodules)   ← humans add, agent reads
wiki/       markdown the agent maintains, with citations          ← agent writes, humans read
schema/     the rules, split by situation                         ← short, evolvable
CLAUDE.md   the contract Claude Code reads every session
```

- Add a source: drop a file into `raw/docs/` and ask a question about it.
- Record a decision: `/llm-wiki:capture` right after it is made (the agent also does this on its own when a capture trigger fires).
- Ask: `/llm-wiki:query <question>` answers from `wiki/` first, falls back to `raw/`, and writes what it learned back.
- Browse: start at `wiki/index.md`; `wiki/log.md` is the append-only history.
