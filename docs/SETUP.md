# Setup

## Editor: VS Code

Use **VS Code** (Cursor works too, since it's built on VS Code). It handles Python, notebooks, Markdown, and Mermaid diagrams in one place.
When you open the repo, VS Code suggests the extensions listed in `.vscode/extensions.json`:

| Extension | Used for |
|---|---|
| Python + Pylance | running code, type hints, go-to-definition |
| Jupyter | `.ipynb` notebooks inside VS Code (no separate Jupyter install) |
| Ruff | linting and formatting |
| Data Wrangler | browsing DataFrames as tables, with quick column stats |
| Markdown Mermaid | rendering the diagrams in `docs/` in the Markdown preview |

PyCharm is fine if you prefer it. Just don't commit `.idea/`, which is already gitignored.

## Python environment: uv

- Python **3.12**, managed with [`uv`](https://docs.astral.sh/uv/).
- `pyproject.toml` + `uv.lock` (pinned dependencies) arrive with the Phase 0 repo task. Until then, run one-off scripts with `uv run --with duckdb,pandas python ...`.

## Looking at data quickly: DuckDB

DuckDB runs SQL directly on parquet files, without loading them into memory first. It's the fastest way to answer a question about the data:

```python
import duckdb
ex = "data/raw/shopping_queries_dataset_examples.parquet"
duckdb.sql(f"""
    SELECT esci_label, count(*) AS n
    FROM '{ex}' WHERE small_version = 1
    GROUP BY 1 ORDER BY 2 DESC
""").show()
```

Use pandas in notebooks once you've narrowed the data down to what you need.

## Data is not in git

**Decision:** raw and processed data never go into the repo.

- **Why:** the products file alone is 1.1 GB. GitHub rejects files over 100 MB, and anything committed stays in the history for good.
- **Instead:**
  1. `data/` is gitignored. Raw files are downloaded from [amazon-science/esci-data](https://github.com/amazon-science/esci-data), pinned to commit `7916cdf6ab75a462e77f20ab40428a10923998d5`, into `data/raw/`.
  2. Splits are built by code with a fixed seed into `data/splits/v1/`. If the split logic changes, that becomes `v2`, and `v1` is never edited.
  3. A small `data/manifest.json` is committed, listing each file with its sha256, row count, and the seed. If your hashes match the manifest, you have the same data as everyone else.
- **Later, if needed:** DVC or a shared bucket for large derived files (embeddings, models). Not needed yet.

Raw file checksums (upstream commit above):

| File | Size | sha256 |
|---|---|---|
| shopping_queries_dataset_examples.parquet | 51 MB | `4a735b693b4a424a6fc67f5be6e4c811495c488bbf66d02a602d308b2744263a` |
| shopping_queries_dataset_products.parquet | 1.1 GB | `25124442d064d64b26f74082d6fa09438d679efc0c183cf28d19064a2b65a265` |
| shopping_queries_dataset_sources.csv | 1.7 MB | `a5fed8ecc016443de40bf3c63098f0e3f23bbe4daa4236f1c38b8c3184778c50` |
