Record a confirmed decision, rule, or result into the wiki. Argument: a one-line description (optional; if omitted, scan the current conversation for capture triggers).

1. Read `schema/capture-workflow.md`. Confirm at least one trigger applies; if none does, say so and stop.
2. Route per section 2. Prefer appending to an existing domain page (newest entry first) over creating a page.
3. Write the entry with a citation. If the source is this conversation, decide volatile vs. pin (section 3).
4. Update `wiki/index.md`; append to `wiki/log.md`.
5. Commit only the wiki files you touched (not `-A`). Then push **once**, and only if the branch has an upstream and `git config --get llmwiki.push` is not `false`. If the push is rejected (403, permission denied, read-only remote), do not retry, do not add or change remotes, run `git config llmwiki.push false` so later commands skip the push, and say in one line that the commit is local. Report what was recorded and where, in two lines.
