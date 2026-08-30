# ☕ Cafe Sales Data Cleaning

A data cleaning project on a cafe sales transactions dataset containing intentional data quality issues — practicing decision-driven cleaning rather than blanket fixes.

## Dataset

[Cafe Sales Dirty Data (Kaggle)](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training) — 10,000 rows of cafe sales transactions with:

- Missing values
- Invalid placeholder strings (`"ERROR"`, `"UNKNOWN"`) hiding as valid data
- Incorrect data types (numeric columns stored as text)
- Corrupted dates

## Approach

Rather than applying one generic imputation method to every column, each column was handled based on its actual relationship to the rest of the data:

| Column | Issue | Strategy |
|---|---|---|
| `Item` | 9.7% missing | Inferred from `Price Per Unit` where the price uniquely identifies a product; labeled `"Unknown Item"` otherwise |
| `Quantity` / `Price Per Unit` / `Total Spent` | ~5% missing each | Calculated from the other two using `Total = Quantity × Price`, verified against 0 mismatches in known rows; remaining gaps filled using per-item average price |
| `Payment Method` / `Location` | 32–40% missing | No reliable column to infer from — left explicitly as `"Unknown"` instead of fabricating a majority-value fill |
| `Transaction Date` | Missing/invalid | Converted to proper `datetime`, invalid entries become `NaT` |

Values that couldn't be reliably inferred were left missing on purpose rather than dropped or guessed — preserving the rest of each row's valid data and avoiding distortion of any downstream analysis.

## Result

- **99.4% overall completeness** after cleaning
- **0 rows dropped**
- Every fill (or non-fill) decision is documented with its reasoning in the notebook

## Files

- `cafe_sales_cleaning_en.ipynb` — full cleaning notebook with step-by-step explanation
- `dirty_cafe_sales.csv` — original raw dataset
- `cafe_sales_cleaned.csv` — cleaned output

## Tools

Python, pandas, NumPy
