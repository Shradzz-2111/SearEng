# docs/

Short, factual notes about **this repo**: how to set it up, what the data looks like, and the decisions we made.

| File | What it covers |
|---|---|
| [SETUP.md](SETUP.md) | Editor, Python env, tools, and how data is handled |
| [DATA.md](DATA.md) | The ESCI dataset: files, relationships, labels, key numbers, pitfalls |

## What goes where

- **docs/**: facts about our code and data, plus decisions with a one-line reason. Update them in the same PR as the change.
- **ROADMAP.md**: tasks and status.
- **Notion**: concepts (BM25, NDCG, …), learning resources, quizzes. Don't copy them here.

## Decision log

| Date | Decision | Why |
|---|---|---|
| 2026-10-13 | Data is not committed to git; raw files are downloaded from a pinned upstream commit | Files are 1.1 GB+, over GitHub's 100 MB limit, and would bloat the history permanently |
| 2026-10-13 | VS Code + uv + DuckDB as the default toolset | Light, free, good notebook support, and both of us use the same setup |
| _open_ | Locale scope: `us` only vs all locales | Proposed: `us` only. To be confirmed in Phase 0 (see DATA.md) |
