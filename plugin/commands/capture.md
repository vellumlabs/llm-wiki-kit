---
description: Record a confirmed decision, rule, or result into the wiki
---
Record a confirmed decision, rule, or result into the wiki. Argument: a one-line description (optional; if omitted, scan the current conversation for capture triggers).

1. Read `schema/capture-workflow.md` (created by /llm-wiki:init; if it is missing, run /llm-wiki:init first). Confirm at least one trigger applies; if none does, say so and stop.
2. Route per section 2. Prefer appending to an existing domain page (newest entry first) over creating a page.
3. Write the entry with a citation. If the source is this conversation, decide volatile vs. pin (section 3).
4. Update `wiki/index.md`; append to `wiki/log.md`.
5. Commit only the wiki files you touched (`git add wiki/ raw/` paths you wrote, not `-A`) with a message like `wiki: <what was recorded>`. Then push **once**, and only if the branch has an upstream and `git config --get llmwiki.push` is not `false`. If the push is rejected (403, permission denied, read-only remote — typical when the repository is a clone of someone else's project rather than your fork), do not retry, do not add or change remotes, run `git config llmwiki.push false` so later commands skip the push, and say in one line that the commit is local. Report what was recorded and where, in two lines.
