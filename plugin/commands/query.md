---
description: Answer a question from the wiki first, falling back to raw/ and writing back
---
Answer a question from the wiki. Argument: the question.

1. Start from `wiki/index.md`; read the pages in the relevant categories. Answer with citations to wiki pages.
2. Only if the wiki is insufficient: consult `raw/` (or the web if allowed), then **write what you learned back** into the wiki with citations so the next query does not need raw again.
3. If the answer reveals a contradiction between pages, fix it now (raw wins).
4. Append to `wiki/log.md`: `- YYYY-MM-DD [query] <question>: <pages used>`. If anything changed, commit only the wiki files you touched (not `-A`). Then push **once**, and only if the branch has an upstream and `git config --get llmwiki.push` is not `false`. If the push is rejected (403, permission denied, read-only remote — typical when the repository is a clone of someone else's project rather than your fork), do not retry, do not add or change remotes, run `git config llmwiki.push false` so later commands skip the push, and say in one line that the commit is local.
