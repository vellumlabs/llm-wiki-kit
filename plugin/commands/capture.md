---
description: Record a confirmed decision, rule, or result into the wiki
---
Record a confirmed decision, rule, or result into the wiki. Argument: a one-line description (optional; if omitted, scan the current conversation for capture triggers).

1. Read `schema/capture-workflow.md` (created by /llm-wiki:init; if it is missing, run /llm-wiki:init first). Confirm at least one trigger applies; if none does, say so and stop.
2. Route per section 2. Prefer appending to an existing domain page (newest entry first) over creating a page.
3. Write the entry with a citation. If the source is this conversation, decide volatile vs. pin (section 3).
4. Update `wiki/index.md`; append to `wiki/log.md`.
5. Commit and push. Report what was recorded and where, in two lines.
