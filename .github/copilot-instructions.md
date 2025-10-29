# Copilot instructions for Assignment 7 (Data Visualization)

This repository is an assignment scaffold. Agents should be conservative: make minimal, focused edits to notebooks and preserve exact filenames and output behaviors required by the autograder.

Key files & layout
- `assignment.ipynb` — primary notebook where students implement TODOs and save plots to `output/`.
- `data_generator.ipynb` — run once to create `data/*.csv` used by the notebook.
- `data/` — contains CSVs: `sales_data.csv`, `customer_data.csv`, `product_data.csv` (must exist for tests).
- `output/` — target location for saved figures. Tests expect exact filenames (see below).
- `.github/test/test_assignment.py` — autograder. Read it to learn exact file names and size expectations.

Important constraints (do not change)
- Preserve output filenames: `q1_matplotlib_plots.png`, `q1_multi_panel.png`, `q2_seaborn_plots.png`,
  `q2_correlation_heatmap.png`, `q3_pandas_plots.png`, `q3_data_overview.png`.
  The tests assert files exist and are within size bounds (>=1KB and <10MB).
- Do not move or rename files in `data/` — the notebook, tests, and helper code reference these paths directly.
- Many notebook cells contain TODO placeholders; prefer editing only those cells when implementing solutions.

Project-specific patterns and gotchas
- Notebooks set plotting styles explicitly: `plt.style.use('default')` and `sns.set_style('whitegrid')`.
  Respect those settings unless an assignment cell explicitly asks to change them.
- The notebook creates the output directory via `os.makedirs('output', exist_ok=True)` — use `plt.savefig('output/<name>.png', dpi=300, bbox_inches='tight')` to match examples.
- After merging CSVs, pandas may create `_x`/`_y` suffixes for duplicate column names — tests and sample code expect you to inspect `merged_df.columns` and select numeric columns accordingly.
- Time-series tasks expect converting `transaction_date` with `pd.to_datetime(...)` and using resample (e.g. `resample('ME')`) or rolling windows (`rolling(7).mean()`).

Developer workflows
- Create & activate venv, then install requirements:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
- Run the data generator before running assignment cells (creates `data/` CSVs):
```bash
# open interactively
jupyter notebook data_generator.ipynb
# or run headless (needs matching kernel/venv)
jupyter nbconvert --to notebook --execute data_generator.ipynb --inplace
```
- Run the notebook interactively in Jupyter or VS Code with the `.venv` kernel selected. To execute the assignment end-to-end headless:
```bash
jupyter nbconvert --to notebook --execute assignment.ipynb --inplace
```
- Autograder/tests: run locally with pytest (install test deps if present in `.github/test/requirements.txt`):
```bash
pip install -r .github/test/requirements.txt  # if provided
pytest .github/test/test_assignment.py -q
```

When editing code
- Keep changes small and local to TODO cells. Avoid refactoring global notebook structure unless necessary.
- Ensure saved figures are not empty and use reasonable DPI (>=150, example code uses 300).
- If modifying data-processing logic (merges, groupbys), double-check column names and dtypes because tests rely on non-empty CSVs and basic numeric columns.

Where to look for examples
- `assignment.ipynb` — contains the exact plotting calls and filename `plt.savefig(...)` examples.
- `README.md` — contains environment and run instructions; follow it for venv and kernel selection.
- `.github/test/test_assignment.py` — authoritative source of what outputs the autograder expects.

If anything is unclear
- Ask for the specific workflow you'd like to change (e.g., different output format, new tests) before making broad edits.
- If you need to add dev tooling (tests, CI), keep the original output filenames and data-generator behavior backward-compatible.

---
This file is intentionally concise. If you'd like, I can expand sections with more concrete code snippets or add a checklist for verifying outputs locally.
