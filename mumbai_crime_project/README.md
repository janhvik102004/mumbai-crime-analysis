# Analysing Crime in Mumbai — Python version

Rebuilds the whole project (cleaning, analysis tables, charts, validation, Excel export) from the raw files with code.

## What is in this folder
| Path | What it is |
|---|---|
| `Mumbai_Crime_Analysis.ipynb` | The notebook — already run, so every table and chart is visible |
| `mumbai_crime_analysis.py` | The same code as a plain script |
| `data/raw/` | Inputs: 3 Mumbai Police CSVs (untouched) + 4 Praja Foundation tables |
| `data/cleaned/` | Tidy tables written by the code |
| `outputs/` | `dashboard.png`, 7 charts in `charts/`, and `Mumbai_Crime_Analysis_Python.xlsx` |

## How to run it

### Option A — Google Colab (nothing to install)
1. Go to https://colab.research.google.com → **File → Upload notebook** → pick `Mumbai_Crime_Analysis.ipynb`.
2. Click the folder icon on the left, then the upload icon, and upload `mumbai_crime_project.zip`.
3. Add a new cell at the very top and run it:
   ```
   !unzip -o mumbai_crime_project.zip
   %cd mumbai_crime_project
   ```
4. **Runtime → Run all**.
5. Download results from the `outputs/` folder in the file panel.

### Option B — your own laptop (VS Code or Jupyter)
1. Install Python 3.10+ from python.org.
2. Unzip the project, open a terminal inside the folder and run:
   ```
   pip install -r requirements.txt jupyter
   ```
3. Either `jupyter notebook Mumbai_Crime_Analysis.ipynb` and **Run All**, or simply `python mumbai_crime_analysis.py`.

## What the code does, step by step
1. **Load** the three raw Mumbai Police CSVs as text and print their problems.
2. **Clean IPC crimes** — drop `_id`, remove thousands separators (`"1,073"` → 1073), reshape wide → long
   (one row per crime head per year), recompute detection rate from the counts, flag the source total row.
3. **Clean cyber crimes** — keep `S No.` as text (so 8.10 is not read as 8.1), tag each row Main /
   Sub-category / Total, strip the index from sub-category names, drop the unused `PA` column, and assert
   that sub-categories sum to *Cheating* and main rows sum to the total.
4. **Clean crimes against women** — name the unlabelled column, drop the empty column and blank rows, repair the
   label broken across two lines, map each raw label to Category + Sub-head with a rule table
   (`(ii) Major` inherits the category above it), reshape wide → long.
5. **Load the Praja tables** — enforce types, check for duplicates and that all 500 / 324 combinations exist,
   derive staffing shortfall and female share.
6. **Analyse** — A1 hotspots (region × year), A2 offence mix (2013 vs 2022), A3 region profile (2022),
   A4 detection rates, plus the headline figures.
7. **Visualise** — 7 charts and a 4-panel dashboard, saved as PNG.
8. **Validate** — recomputed changes for rape, thefts and molestation against Praja's published figures; murder cross-check.
9. **Export** — everything written to one Excel workbook.

## Notes
- The Praja tables in `data/raw/` are the figures already extracted from the Praja PDF (Tables 31–42, Annexure 3).
  The PDF itself is not in this folder, so the PDF-extraction step is not re-run here.
- The Kaggle screening appendix runs only if you add `crime_dataset_india.csv` to `data/raw/`.
- The Excel export holds values, not live formulas, and the charts are PNG images rather than Excel charts.
