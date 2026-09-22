# schema/wiki-conventions.md — Page conventions

Read before creating or editing any wiki page.

---

## Frontmatter (required)

```yaml
---
type: decision | concept | source | <your-category-types>
title: <Human-readable title>
status: draft | active | superseded
updated: YYYY-MM-DD
---
```

- `type`, `title`, `status`, `updated` are required. (the Pro kit ships `scripts/lint.py`, which fails without them).
- A `superseded` page must link to its successor with `[[page-name]]`.

## Naming

- File and directory names: **ASCII kebab-case** only. Body text and `title` may be in any language.
- One page per domain (`decisions/<domain>.md`) or per term (`concepts/<term>.md`). Do not create one page per decision; append entries to the domain page instead.

## Citations

| Source | Format |
|---|---|
| Code (submodule) | `raw/code/<repo>/<path>:<lines> @<short-commit>` |
| Document | `raw/docs/<file>` (+ page or section) |
| Pinned conversation | `raw/conversations/<file>:<lines>` |
| Volatile conversation | "from conversation, summarized (no transcript)" |
| Web | URL + `retrieved YYYY-MM-DD` |

A claim without a citation is a lint finding. If you must speculate, label it **speculation** and say how it will be verified.

## Cross-links

- No orphan pages. Every new page is reachable from `index.md` or a related page.
- Link with `[[page-name]]` (the file name without `.md`) or a relative path.

## Entry format for decision-style pages (lightweight ADR)

```markdown
## YYYY-MM-DD <Decision title>

- **Context:** why a decision was needed
- **Decision:** what was chosen
- **Why:** reasons; why alternatives were rejected
- **Consequences:** impact, alternatives considered, open questions

Citation: ...
```

Newest entry at the top of the page.
