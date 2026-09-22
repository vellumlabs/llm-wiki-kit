# schema/capture-workflow.md — Capture (primary operation)

The LLM records confirmed knowledge **autonomously, without asking permission**, the moment a trigger fires.

---

## 1. Triggers (record if ANY applies)

1. **Choice among alternatives** — the LLM proposed two or more options and the human picked one.
2. **Reversal** — an existing policy or earlier decision was overturned or redirected.
3. **Human-stated rule** — the human stated a business rule, constraint, or fact that cannot be read from code or documents.
4. **Measured result** — an experiment, test, or process produced a number or outcome worth keeping.

### Do NOT record
- Trade-off-free implementation details
- Temporary bug fixes
- Confirmations that do not change anything

When in doubt, lean toward *not* recording (noise control), but never skip a clear trigger.

## 2. Routing

| Content | Destination |
|---|---|
| Why something was decided | `<decisions-category>/<domain>.md` (append, newest first) |
| Definition of a term that came up | `concepts/<term>.md` (or append to an existing page) |
| A measured result | the page for that experiment / topic, plus the raw ledger or data file if one exists |

## 3. Conversation transcripts (volatile hybrid)

- **Default: volatile.** Summarize into the wiki; do not store the transcript. Cite as "from conversation, summarized (no transcript)".
- **Exception: pin.** For decisions likely to be disputed later or with complex background, copy *only the decisive lines* to `raw/conversations/<topic>.md` and cite that. Never paste the whole conversation.
- The LLM decides volatile vs. pin on its own.

## 4. Code citations

Cite as `raw/code/<repo>/<path>:<lines> @<short-commit>` so the reference is pinned to a point in time. Bump the submodule first if needed. Never edit submodule code.

## 5. After every capture

1. Update `wiki/index.md` (new page or changed summary).
2. Append one line to `wiki/log.md`: `- YYYY-MM-DD [capture] <page>: <one-line summary>`.
3. Commit and push.
