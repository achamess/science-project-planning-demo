# CLAUDE.md

## Project Overview
Science project planning demo repository. Uses Python for analysis pipelines and Jupyter notebooks for exploration.

## Structure
- `data/raw/` — immutable raw data (not committed)
- `data/processed/` — cleaned data
- `notebooks/` — Jupyter notebooks
- `src/analysis/` — analysis pipelines
- `src/utils/` — shared utilities
- `docs/` — documentation and protocols
- `figures/` — generated figures
- `results/` — final outputs

## Conventions
- Python 3.10+
- Use `src/` for importable code, `notebooks/` for exploration
- Raw data never modified; processing scripts produce `data/processed/` outputs
- Figures saved as both PNG (300 dpi) and SVG
