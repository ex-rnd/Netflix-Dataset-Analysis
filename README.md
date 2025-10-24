# 🔆 Project Overview
### ✨ Purpose
Provide a compact, reproducible demonstration of common data-preparation tasks that appear in analytics pipelines and model-ready preprocessing.

### 📚 Datasets in the Notebook
- Netflix-style dataset: demonstrates missing-value reporting, targeted fill strategies for categorical text columns, and moving mis-typed numeric/duration content into correct columns.
- Olympics medals dataset: demonstrates dtype correction, normalization (min-max), skew/kurtosis diagnostics, Z-score based outlier detection, and aggregation (medals per country).
- Synthetic relational datasets: student streams A/B, subjects, and tests used to illustrate concat and merge behavior and to produce a joined student→subject→test table.

### 🛠️ Core techniques demonstrated
- Missing-value detection and handling: df.isna(), df.fillna(), df.dropna()
- Type correction and coercion: pd.to_numeric(..., errors='coerce'), .astype(int)
- String cleaning: .str.strip(), .str.upper(), .str.replace()
- Normalization: custom min-max normalizer function
- Outlier detection: scipy.stats.zscore, filter with abs(z) > threshold
- Aggregation: df.groupby(...).sum().reset_index()
- Joins: pd.concat (axis=0 and axis=1) and pd.merge with how = {inner, left, right, outer}


## ▶️ Usage and Key Cells to Inspect 

### 🧹 Part 1 Netflix cleaning
- Load dataset and inspect with df.info() and df.describe()
- Check and report missing values
- Fill director and cast using fillna with clear tokens (e.g., 'Unspecified', 'Unlisted')
- Move incorrectly entered rating values into the duration column and null out rating entries that held durations

### 🏅 Part 2 Olympics preparation
- Inspect: df.info(), df.describe()
- Missing-values check and early exit when none exist
- Convert Rank to numeric: pd.to_numeric(..., errors='coerce'), fillna and cast to int
- Standardize Country names and do targeted replacements (trim Unicode whitespace, remove stray annotations)
- Normalize Gold with min-max; create Gold_Normalized
- Compute skew and kurtosis for numeric medal columns
- Compute Z_score on Total with scipy.stats.zscore and flag outliers where abs(Z_score) > 3
- Aggregate medal totals: medal_totals = df.groupby('Country')['Total'].sum().reset_index()

### 🔗 Part 3 Joins and merges
- Vertical concat: pd.concat([...], axis=0).reset_index(drop=True)
- Horizontal concat: pd.concat([...], axis=1).reset_index(drop=True)
- Merge patterns: pd.merge(left, right, on='key', how='inner'/'left'/'right'/'outer')
- Multi-step pipeline: students → subjects (inner) → tests (left) to answer which tests each student will take
Command examples and immediate outputs are embedded inline in the notebook so you can run and inspect each transformation interactively.



## 📊 Notes and Findings
### 🎬 Netflix example
- Defensive defaults were used for missing textual metadata: director → 'Unspecified', cast → 'Unlisted'.
- Found and corrected rows where durations were stored in rating; such domain-specific fixes reduce downstream noise.

### 🥇 Olympics example
- Rank column contained non-numeric tokens; converting with coercion then filling missing numeric values allowed stable dtype conversion.
- Medal totals have high positive skew and kurtosis, so median-based summaries and robust outlier checks are necessary.
- Z-score filtering (abs > 3) surfaced top-performing country-event rows (e.g., United States Athletics) and several high-medal sport entries.

### 🧩 Joins example
- Left joins show how missing mappings produce NaNs for subject→test relationships; right/outer joins show how to detect subjects without students and vice versa.
- Multi-step merges are a practical pattern to assemble denormalized tables for reporting.

## 🤝 Contributing
### 🚀 Suggested next steps and improvements
• 	Factor notebook logic into small, testable Python modules under src/ and add unit tests under tests/.
• 	Replace hardcoded file paths with a small config (YAML/JSON) or CLI arguments.
• 	Add robust file-loading helpers that present clear errors and suggestions if CSVs are missing.
• 	Add visualization cells for diagnostics: histograms, boxplots, violin plots for distribution and outlier inspection.
• 	Add logging, argument parsing, and small wrappers to run the cleaning pipeline from the command line.

### 🧭 Style and process
• 	Follow PEP8 for any Python modules and use a pre-commit hook for linting.
• 	Prefer small, well-documented functions rather than large blocks of procedural code inside the notebook.
• 	Tests should call functions in src/ instead of running notebook cells.


### Thank you for your contributions! 🎉




