# AI-216: Programming for Artificial Intelligence
## Lab 08 — Data Preprocessing & Exploratory Data Analysis (EDA)

**Semester:** Fall 2026  
**Week:** 08  
**Topic:** Missing values, duplicate records, categorical data, descriptive statistics, outlier awareness, visualization, basic scaling, and reproducibility

---

## 1. Objective

In Week 6, you learned how to manipulate tables using **Pandas**. In Week 7, you learned how data arrives through files, APIs, websites, and databases. The Week 8 lecture asks an equally important question:

> **Once data has arrived, can we establish what it means, identify quality problems, justify our changes, and explore it without misleading ourselves?**

In this lab, you will work with a deliberately imperfect dataset. The goal is **not** to make every number look clean, nor to train an ML model. The goal is to produce an **auditable, reproducible preprocessing and EDA workflow**.

By the end of Lab 08, you should be able to:

1. Audit a dataset before editing it, including its shape, types, missingness, and duplicates.
2. Distinguish **required missing fields** from optional missing measurements.
3. Explain and implement a defensible missing-value policy, retaining missingness evidence.
4. Distinguish identical rows, conflicting identifiers, and legitimate repeated observations.
5. Normalize categorical labels without silently inventing mappings for unknown categories.
6. Apply explicit range and consistency rules and record why observations were excluded.
7. Compute descriptive and grouped statistics and investigate potential outliers.
8. Create interpretable, clearly labeled visualizations and describe their limitations.
9. Demonstrate basic feature scaling and categorical encoding without assuming that a transformation is universally appropriate.
10. Turn the analysis into a rerunnable project with saved outputs, documentation, and Git history.

**Libraries:** `pandas`, `numpy`, `matplotlib`. You may use Python standard-library modules such as `pathlib`, `json`, and `hashlib`. **No scikit-learn, model training, or external data collection is required.**

---

## 2. Before You Start

### Environment

Use the Python and Git/GitHub environment established in previous weeks.

```bash
python --version
python -m pip show pandas numpy matplotlib
git status
```

Install a missing package in the environment you are actually using:

```bash
python -m pip install pandas numpy matplotlib
```

In scripts, use the standard imports when needed:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

If your system uses `python3`, use that command instead of `python`.

### Files Provided

The complete package includes:

```text
data/
├── study_space_observations.csv
└── README.md
```

`data/README.md` documents the schema, provenance, and special cases. **Read it before modifying or analyzing the data.** The CSV contains synthetic teaching observations, not real students or real campus measurements.

### Suggested Student Repository Structure

In your personal coursework repository:

```text
labs/week08/
├── data/
│   ├── study_space_observations.csv  # provided, never edited
│   └── README.md                      # provided provenance
├── task01_audit.py
├── task02_missing_values.py
├── task03_duplicates.py
├── task04_validation.py
├── task05_statistics.py
├── task06_visualization.py
├── task07_features.py
├── task08_eda_pipeline/
│   ├── main.py
│   ├── audit.py
│   ├── preprocess.py
│   └── explore.py
├── outputs/                        # student-generated reports and plots
└── README.md                       # student-written documentation
```

You may use a slightly different structure if you can explain why. Keep lab data, code, and generated results distinguishable.

**Important:** `labs/week08/README.md` in the instructor's repository is the lab handout. `labs/week08/README.md` in **your personal solutions repository** documents *your implementation, tests, and findings*.

Avoid machine-specific absolute paths. Use `pathlib.Path(__file__)` where appropriate to locate your source files reliably.

---

## 3. Dataset: Study-Space Occupancy and Comfort

### Scenario

A hypothetical smart-campus team receives observations from different study spaces. The team wants to understand:

- How much of each space's seating capacity is used?
- Do occupancy patterns differ by type of space?
- How do noise levels and temperature vary?
- Which records are incomplete, inconsistent, or unusually high?
- What can reasonably be said from this small sample, and what remains uncertain?

The dataset is **synthetic** and has intentionally inconsistent categories, missing fields, duplicate observations, invalid readings, and unusual-but-allowable values. Treat these as **engineering conditions** rather than inconveniences to hide.

### Data Dictionary

| Field | Meaning | Contract |
| --- | --- | --- |
| `observation_id` | ID of one logical observation | Required text; unique after reconciliation |
| `space_id` | Study-space identifier | Required text; **may repeat** across observations |
| `space_type` | Type of study space | Required; canonical categories are `quiet`, `group`, `computer` |
| `observed_date` | Date of reading | Required ISO date, `YYYY-MM-DD` |
| `hour` | Hour of observation | Required whole number from `0` to `23` |
| `occupied_seats` | Number of seats being used | Required whole number from `0` through `seat_capacity` |
| `seat_capacity` | Available seats | Required positive whole number |
| `noise_db` | Illustrative noise measurement | Optional; if observed, must be in `[20, 100]` |
| `temperature_c` | Illustrative temperature measurement | Optional; if observed, must be in `[15, 40]` |

For this lab, normalize category labels by trimming spaces, making case consistent, and applying only these approved aliases:

| Input after `.str.strip().str.lower()` | Canonical type |
| --- | --- |
| `quiet`, `quiet zone` | `quiet` |
| `group`, `group study` | `group` |
| `computer`, `computer lab` | `computer` |

An unrecognized category (for example, `event`) must be **flagged**; it must not silently become `group` or `quiet`.

`N/A` and blank cells in **optional** measurement columns are missing readings, not zero readings. A missing `occupied_seats` value is different: it prevents the main occupancy calculation and should invalidate that row under the specified contract.

### Source Order and Duplicate Policy

Apply a deterministic policy so that results can be checked:

1. Preserve the original source rows and their order.
2. Detect and log **exact duplicate rows**.
3. For the same `observation_id` with different content, log a **conflicting identifier** and keep the **first** occurrence for this lab's main analysis.
4. Only then apply categorical and numeric validation to the retained observations.
5. Do **not** treat a repeated `space_id` as a duplicate; a room can be observed at several times.

A different conflict policy may be worth discussing, but use the policy above for the required reproducible output.

### Workflow

```text
Original CSV (preserved unchanged)
            ↓
Raw-data audit
            ↓
Exact duplicates / conflicting IDs
            ↓
Category normalization + domain validation
            ↓
Valid observations + excluded-row audit
            ↓
Missingness flags and documented treatment
            ↓
Descriptive statistics + plots
            ↓
Saved report and reproducible outputs
```

---

## 4. General Coding Requirements

For every task:

1. **Inspect before changing.** Report what you observed in the source dataset.
2. Work on a separate DataFrame; do not overwrite the instructor's original CSV.
3. Make assumptions and data-validation rules explicit.
4. Preserve evidence of missing values and excluded records.
5. Use readable Pandas operations rather than unnecessarily complex loops.
6. Use meaningful names and functions with one main responsibility.
7. Treat an **outlier flag** as a reason to investigate, **not** automatic permission to delete a row.
8. Label plot axes, provide units, and interpret figures carefully.
9. Test normal, boundary, missing, and duplicate conditions.
10. Be prepared to explain every transformation and choice, including those suggested by an AI tool.

---

# 5. Task 1 — Initial Data Quality Audit (Basic)

**File:** `task01_audit.py`  
**Source:** `data/study_space_observations.csv`

### Part A — Load and Observe

Read the CSV into Pandas. Use string types for identifiers such as `observation_id` and `space_id` so identifiers are not treated as quantities.

Report:

- number of rows and columns
- column names and data types
- first and last few rows
- missing-value count **and percentage** for each column
- unique `space_id` values
- raw frequencies of `space_type` including whitespace/case variations

Useful inspection operations include:

```python
df.shape
df.head()
df.dtypes
df.isna().sum()
df["space_type"].value_counts(dropna=False)
```

### Part B — Distinguish the Types of Problems

Report the number of:

- rows that are exact duplicates (all original columns match)
- rows whose `observation_id` has already appeared
- missing **required** measurements
- missing **optional** measurements

Use `duplicated()` in two distinct ways. One is about identical *rows*; the other is about repeated *keys*.

### Part C — Write an Initial Audit Statement

In your own `README.md`, record three observations about raw-data quality **before** any cleaning. Include actual counts and the commands you used to obtain them.

**Sanity check:** The provided CSV has **46 source rows and 9 columns**. Do not use the total number of unique `space_id` values as the number of unique observations.

**Concepts:** data-quality profiling, schema awareness, `isna()`, category frequencies, duplicate definitions.

---

# 6. Task 2 — Missing Values and Defensible Treatment (Basic → Intermediate)

**File:** `task02_missing_values.py`

### Part A — Compare Strategies on a Small Example

Start with this **separate teaching example**:

```python
import pandas as pd

example = pd.DataFrame({
    "room": ["Q101", "Q102", "G201", "C301"],
    "noise_db": [38.0, None, 55.0, 90.0],
    "occupied_seats": [8, 10, None, 15],
})
```

Create and compare:

1. The original table with missing values retained.
2. A table that drops rows missing `occupied_seats`.
3. A separate copy where missing `noise_db` is filled with the median of observed noise readings.
4. A Boolean column `noise_was_missing` that records **which values were originally absent**.

Answer: How many rows remain after each operation? Why is a measured `noise_db=0` different from no noise measurement?

### Part B — Examine the Provided Dataset

Inspect missing values in `occupied_seats`, `noise_db`, and `temperature_c`. Distinguish the implications of each:

- A missing occupied-seat count prevents an occupancy-ratio calculation.
- Missing noise or temperature does **not** necessarily make an occupancy observation unusable.
- Blindly calling `df.dropna()` may remove useful records.

**Do not apply a final imputation to the full raw dataset yet.** First resolve keys, categories, and invalid measurement ranges in Tasks 3–4. Otherwise, bad readings may distort the median used for filling.

### Part C — State Your Policy

Write a short policy for the integrated workflow:

- Which fields are mandatory?
- Which missing measurements remain missing initially?
- If you later fill optional measurements for a derived feature table, what statistic will you use?
- How will you preserve a missingness indicator?

**Concepts:** `isna()`, `dropna(subset=...)`, `fillna()`, median imputation, information loss, missingness flags.

---

# 7. Task 3 — Exact Duplicates vs Conflicting Identifiers (Intermediate)

**File:** `task03_duplicates.py`

### Part A — Exact Duplicate Records

Find rows whose **entire source record** has already appeared. Print the related `observation_id` values and explain why they are exact duplicates.

Make a DataFrame of distinct complete rows, preserving the first occurrence.

### Part B — Key Conflicts

Now identify any remaining **repeated `observation_id`** values. Compare the corresponding rows to establish whether the content differs.

Follow the specified lab policy: **first source occurrence wins**; the later conflicting row is excluded from the main analysis and recorded in an audit output.

### Part C — Important Distinction

Find a `space_id` with multiple observations. Explain why these records are *not* duplicates when their observation IDs, dates, or times differ.

### Required Audit Fields

Your working audit should preserve enough information to identify:

```text
source row / observation_id / exclusion reason
```

Useful reason categories include:

```text
exact_duplicate
conflicting_observation_id
```

### Required Checks

| Stage | Expected row count |
| --- | ---: |
| Original dataset | 46 |
| After exact-row deduplication | 45 |
| After resolving one conflicting ID | 44 |

Do not confuse the two excluded duplicate-related rows with the eight separate **invalid records** found in Task 4.

**Concepts:** `duplicated()`, `drop_duplicates()`, key design, stable source order, conflict policy.

---

# 8. Task 4 — Categories, Types, and Domain Validation (Intermediate → Advanced)

**File:** `task04_validation.py`

This task converts the distinct observations from Task 3 into a **valid observation table** and a **reasoned rejection table**.

### Part A — Normalize Categories Carefully

Normalize `space_type` by removing surrounding whitespace, converting case, and applying **only** the approved category aliases from Section 3.

Report:

- categories before normalization
- categories after normalization
- any unrecognized categories
- number of affected rows

Do **not** guess what an unknown category means. Document it and exclude that observation from the required valid subset.

### Part B — Convert Types and Enforce Rules

Check every field against the contract in Section 3:

- required identifiers and ISO date
- whole-number `hour`, `occupied_seats`, and `seat_capacity`
- `hour` in the range 0–23
- positive `seat_capacity`
- `occupied_seats` between 0 and capacity, inclusive
- `noise_db` in `[20, 100]` **when present**
- `temperature_c` in `[15, 40]` **when present**

`pd.to_numeric(..., errors="coerce")` can help identify invalid numerical text, but **compare the converted values to the original text** when needed. Unexpected text should not silently be reclassified as an ordinary missing measurement.

Keep optional missing noise/temperature values as missing at this stage.

### Part C — Explain Rejections

Create a rejected-observations DataFrame containing at least:

```text
observation_id
source_row
reason
```

If a record fails multiple rules, record all meaningful reasons, for example as a semicolon-separated list. Do not hide the evidence simply because one condition already failed.

### Part D — Validate the Boundary Rules

Test small additional records of your own, including:

1. `occupied_seats = 0` with positive capacity (valid).
2. `occupied_seats = seat_capacity` (valid).
3. Negative occupied seats (invalid).
4. Occupied seats greater than capacity (invalid).
5. Missing optional noise but otherwise valid fields (valid).
6. A noise reading below 20 (invalid).
7. An unrecognized category (invalid).

### Expected Checks

Using the required duplicate policy and the stated domain rules:

```text
Unique observations after duplicate handling: 44
Invalid unique observations:               8
Valid unique observations:                36
```

The eight invalid unique observation IDs are `OBS-037` through `OBS-044`. Your code must **detect** them based on the rules, not hard-code these IDs as deletions.

**Important:** `OBS-022` has an unusual but *allowed* noise reading of `94 dB`; `OBS-027` has an unusual but *allowed* temperature of `37 °C`. Investigate them rather than dropping them just for being high.

**Concepts:** categorical normalization, domain validation, type conversion, rejection auditing, boundary testing.

---

# 9. Task 5 — Descriptive Statistics and Outlier Awareness (Intermediate → Advanced)

**File:** `task05_statistics.py`  
**Input:** the valid unique subset from Task 4

### Part A — Create a Useful Derived Measure

Create an `occupancy_ratio` column:

```text
occupancy_ratio = occupied_seats / seat_capacity
```

The result should be between 0 and 1 for every valid record. Use a descriptive name and do not divide by zero.

### Part B — Describe the Numerical Columns

For `occupied_seats`, `occupancy_ratio`, `noise_db`, and `temperature_c`, report meaningful statistics such as:

- number of observed (nonmissing) values
- mean and median
- minimum and maximum
- standard deviation
- 25th, 50th, and 75th percentiles
- interquartile range (IQR)

Explain at least one case where the **median** is more informative than the **mean**.

### Part C — Compare Groups

Build a grouped summary by canonical `space_type` containing:

```text
space_type
observations
mean_occupancy_ratio
median_occupancy_ratio
median_noise_db
mean_temperature_c
```

Pay attention to the difference between `size` (rows) and `count` (nonmissing values). Sort category labels to make the output stable.

### Part D — Flag Possible Outliers

For at least one numerical feature (such as `noise_db`), calculate the quartiles and IQR and identify values outside:

```text
Q1 - 1.5 × IQR
Q3 + 1.5 × IQR
```

Report the flagged observation IDs, if any. Then explain why a statistical flag **does not prove that a value is incorrect**.

**Do not automatically delete any valid observation based solely on IQR.**

**Concepts:** `describe()`, `quantile()`, `groupby()`, `agg()`, outlier investigation, sensible denominators.

---
# 10. Task 6 — Exploratory Data Analysis with Visualizations (Intermediate → Advanced)

**File:** `task06_visualization.py`  
**Input:** the valid unique subset from Task 4, with `occupancy_ratio` from Task 5

Create and **save** at least three different plots using Matplotlib. Use the original observed measurements when plotting missing-sensitive variables, unless you explicitly label a plot as using imputed data.

### Plot 1 — Distribution of Occupancy

Create a histogram of `occupancy_ratio`.

Questions to consider:

- Are most observations at low, moderate, or high occupancy?
- Are values clustered or widely spread?
- What does a histogram **not** tell us about time or room identity?

Suggested name:

```text
outputs/plots/occupancy_ratio_histogram.png
```

### Plot 2 — Noise by Space Type

Create a boxplot comparing observed `noise_db` values across the three canonical space types.

Questions to consider:

- Which group appears to have a different center or spread?
- Is there a possible unusually high reading?
- How many actual noise observations contribute to each group?

Suggested name:

```text
outputs/plots/noise_by_space_type_boxplot.png
```

### Plot 3 — Relationship Between Temperature and Occupancy

Create a scatter plot with:

```text
x-axis: temperature_c

y-axis: occupancy_ratio
```

You may distinguish space types using a legend. Exclude pairs where the plotted temperature is missing, **not** the entire observation from every other analysis.

Suggested name:

```text
outputs/plots/temperature_vs_occupancy_scatter.png
```

### Plot Requirements

Each chart must have:

- a meaningful title
- labeled axes with units where relevant
- a readable figure size
- a clear legend when multiple groups appear
- no misleading axis labels or implied causal claims

Save plots using a pattern such as:

```python
fig, ax = plt.subplots()
# Your own plotting code here
fig.tight_layout()
fig.savefig(output_path)
plt.close(fig)
```

Do not save the same chart multiple times with random filenames.

### Interpretation Requirement

In `README.md`, write **at least three specific observations**, one based on each required figure. For every observation, state:

1. What the plot appears to show.
2. Which rows or measurements were included/excluded.
3. What the plot **does not** establish.

**Important:** A scatter plot showing association does not prove causation. This synthetic data also does not establish real-world campus behavior.

**Concepts:** histograms, boxplots, scatter plots, distributions, missing-data awareness, evidence-based interpretation.

---

# 11. Task 7 — Encoding, Scaling, and Reproducibility (Advanced)

**File:** `task07_features.py`  
**Input:** valid unique observations from Task 4

### Part A — Prepare Categorical Indicators

Represent the normalized `space_type` categories as indicator columns using Pandas, for example with `pd.get_dummies()`.

Verify that:

- only expected categories are represented
- category names are clear
- the number of rows is unchanged
- the original `space_type` column can still be used for reporting

Do **not** imply an ordering among `quiet`, `group`, and `computer` by arbitrarily assigning ranks such as `0`, `1`, `2`.

### Part B — Preserve Missingness Before Filling

On a **copy** of the valid table, create:

```text
noise_was_missing
temperature_was_missing
```

These should be computed **before** any imputation.

For a derived demonstration table, you may fill missing `noise_db` and `temperature_c` with a clearly documented median. Specify whether the median is overall or category-specific, and explain what happens if a group has no observed values.

Retain a second table with the original missingness unchanged, so an analyst can distinguish measured values from imputed ones.

### Part C — Basic Numerical Scaling

Demonstrate both transformations using one appropriate numeric column:

**Min–max scaling**

\[
x_{scaled} = \frac{x - x_{min}}{x_{max} - x_{min}}
\]

**Standardization**

\[
z = \frac{x - \mu}{\sigma}
\]

You may choose `noise_db` or `temperature_c`, provided you explain how missing values are treated. Handle a constant column without dividing by zero.

### Part D — Engineering Explanation

In your README, explain:

- What scaling changes, and what it does not change.
- What happens to a value beyond previously observed min–max bounds.
- Why future model-training workflows should learn scaling/imputation parameters from **training data only**, then reuse them elsewhere.
- Why Week 8 EDA does **not** require a train/test split or model fitting.

**Concepts:** one-hot encoding, missingness indicators, min–max scaling, standardization, constant-column guard, reproducibility, leakage awareness.

---

# 12. Task 8 — Integrated Preprocessing & EDA Case Study (Advanced)

This is the main Week 8 engineering task. Integrate the important steps from Tasks 1–7 into one program that another engineer could rerun.

**Suggested folder:** `task08_eda_pipeline/`

### Scenario

A campus analytics team receives a new CSV export of study-space observations and needs a concise answer to three questions:

1. **Can we trust the source records enough to summarize them?**
2. **Which patterns are visible in the valid measurements?**
3. **Can we reproduce the same result and explain every exclusion or replacement?**

### Expected Project Structure

```text
task08_eda_pipeline/
├── main.py
├── audit.py
├── preprocess.py
└── explore.py
```

You may use additional small functions, but do not create an unnecessarily elaborate framework.

### A. `audit.py` — Inspect Before Modifying

Design functions that:

- read the source without modifying it
- produce a column-level missingness report
- report raw category variants
- identify exact duplicates and repeated observation IDs
- record original row numbers or another stable source reference
- build a basic source-quality summary

### B. `preprocess.py` — Apply a Declared Contract

Design functions to:

- preserve the first occurrence when reconciling duplicate IDs
- classify exact-row duplicates separately from conflicting keys
- normalize recognized categories
- validate required fields, numeric ranges, and capacity consistency
- return a valid unique DataFrame **and** a table of excluded rows/reasons
- create `occupancy_ratio`
- create optional-measurement missingness flags

If you also produce an imputed feature table, make it a **separate** output. Do not overwrite the evidence-preserving valid table.

### C. `explore.py` — Summarize and Visualize

Design functions to:

- compute useful overall statistics
- calculate grouped metrics by canonical space type
- flag potential outliers without auto-deleting them
- produce the required histogram, boxplot, and scatter plot
- write chart files with clear, stable names

### D. `main.py` — Coordinate, Don't Reimplement

`main.py` should control the workflow:

```text
Load source
    ↓
Audit raw data
    ↓
Identify exact duplicates and ID conflicts
    ↓
Normalize and validate
    ↓
Save valid rows and excluded rows
    ↓
Calculate statistics / grouped summaries
    ↓
Save plots and structured quality report
```

Keep the detailed logic in the appropriate modules. Do not paste a second, independent copy of the validation rules into `main.py`.

### Required Output Files

Save these under `labs/week08/outputs/`:

```text
outputs/
├── valid_observations.csv
├── excluded_rows.csv
├── summary_by_space_type.csv
├── quality_report.json
└── plots/
    ├── occupancy_ratio_histogram.png
    ├── noise_by_space_type_boxplot.png
    └── temperature_vs_occupancy_scatter.png
```

`valid_observations.csv` should contain the **36 valid unique observations**, with normalized categories, numeric columns, and `occupancy_ratio`. Preserve the observed missingness in optional measurements and include the missingness flags.

`excluded_rows.csv` should identify **every excluded source row**, including exact duplicates, conflicting IDs, and invalid observations, with reasons. Under the specified policy there should be **10 excluded source rows** in total. Multiple reasons for one row do not turn it into several excluded rows.

`summary_by_space_type.csv` should show grouped descriptive statistics with clearly named columns and a predictable category ordering.

`quality_report.json` should include at least:

```json
{
  "source_rows": "compute from the CSV",
  "exact_duplicate_rows": "compute",
  "conflicting_id_rows": "compute",
  "invalid_unique_rows": "compute",
  "valid_unique_rows": "compute",
  "missing_noise_valid_before_fill": "compute",
  "missing_temperature_valid_before_fill": "compute",
  "policy_notes": "describe decisions"
}
```

The strings above illustrate **report fields**, not sample numerical answers or executable output. Save actual counts as JSON numbers rather than strings.

### Expected Sanity Checks

The following results must arise from applying the **documented rules**, not from hard-coded row deletions:

| Check | Expected |
| --- | ---: |
| Source rows | 46 |
| Source columns | 9 |
| Exact duplicate rows excluded | 1 |
| Conflicting-ID rows excluded | 1 |
| Invalid distinct observations excluded | 8 |
| Valid unique observations | 36 |
| Missing noise values among valid rows, before any filling | 4 |
| Missing temperature values among valid rows, before any filling | 3 |
| Canonical space types represented | 3 |

Also verify:

- Every kept `observation_id` is unique.
- Every kept `occupied_seats` value is between zero and capacity.
- Every kept `occupancy_ratio` is between 0 and 1.
- No unknown `space_type` remains in the valid table.
- `OBS-022` and `OBS-027` remain present; high-but-valid values are not silently removed.
- `36 + 8 + 1 + 1 = 46` reconciles the source rows.

### Rerun Requirement

Run the same pipeline **twice** without altering the raw CSV. Confirm that both runs produce the same row counts, category totals, summary metrics, and filenames, without accidentally appending duplicate output rows.

Do not write machine-specific absolute paths or depend on an interactive notebook's execution order.

### Engineering Questions (Answer in Your README)

1. Why does this workflow audit *before* preprocessing?
2. Which records were excluded, and for what reasons?
3. Why are repeated `space_id` values not automatically duplicates?
4. How would you distinguish a real unusual measurement from an input error?
5. What did you preserve so a reviewer can examine your decisions?
6. Which findings might be affected by missing values or the small sample size?
7. How would this workflow need to change before training a predictive model?

**Concepts:** modular data work, quality contracts, deterministic processing, transparent exclusions, reproducible analysis, engineering communication.

---
# 13. Optional Challenge — Sensitivity of EDA Conclusions

The same dataset can support different reasonable analysis decisions. For an optional extension, compare **two clearly documented treatments** of missing optional noise measurements:

- **Approach A:** summarize and plot observed noise values only.
- **Approach B:** fill missing noise values with a documented median in a separate derived table, then summarize and plot that table.

For each approach, report:

1. How many observations enter the noise summary.
2. Whether the mean, median, IQR, or boxplot changes.
3. Which conclusion becomes more or less convincing.
4. Why an imputation method should be chosen for a reason, rather than simply to produce a complete table.

If you choose an alternative duplicate conflict policy as an additional experiment, label those outputs separately and keep the required first-occurrence results intact.

This challenge is **not** a request to train or evaluate a machine-learning model.

---

# 14. Testing and Verification

A data pipeline is not complete merely because it prints a table without exceptions.

### Required Verification Categories

| Category | Example test | What you should verify |
| --- | --- | --- |
| Normal | Valid record | Kept with correct types and category |
| Boundary | Occupied seats equal capacity | Kept; occupancy ratio equals 1 |
| Boundary | Zero occupied seats | Kept; ratio equals 0 |
| Missing required | Empty occupied-seat cell | Excluded with a useful reason |
| Missing optional | Empty noise measurement | Kept; missingness flag is true |
| Invalid numeric | Negative noise or impossible temperature | Excluded |
| Category | Extra spaces/case in `space_type` | Normalized to allowed label |
| Unknown category | Unrecognized `space_type` | Excluded and explained |
| Exact duplicate | Complete repeated record | Counted separately from conflicts |
| Conflicting key | Same observation ID, changed value | Deterministically resolved and logged |
| Legitimate repeat | Same room at another time | Preserved |
| Outlier awareness | High but in-range reading | Kept, possibly flagged for review |
| Scale denominator | Constant numeric column | No division by zero |
| Reproducibility | Second pipeline run | Same logical outputs, no append duplication |

### Verification Techniques

You may use simple `assert` statements, small test functions, or your own documented checks. For example, the following is an illustrative **postcondition**, not a cleaning algorithm:

```python
assert valid_df["observation_id"].is_unique
assert valid_df["occupancy_ratio"].between(0, 1).all()
```

If a test fails, use the traceback and DataFrame inspection techniques from Weeks 3 and 6. Document at least **one genuine issue** you encountered and how you investigated it; if none occurred, explain one deliberately tested failure case instead.

---

# 15. Code Quality and Engineering Expectations

### Keep Source Data Untouched

Never overwrite `data/study_space_observations.csv`. Use separate DataFrames and place new outputs under `outputs/`.

### Explain the Data Contract

Use named constants where domain rules are important (permitted categories, numeric bounds, column names). Avoid scattering unexplained thresholds through several files.

### Avoid Silent Data Loss

Do not drop every incomplete record or every large measurement. Record exclusions and report the before/after counts.

### Make Missingness Visible

A filled value is **not** the same as an observed value. Preserve indicators or separate observed and imputed tables.

### Use Stable Processing Rules

Keep the chosen first-occurrence policy and predictable sort order. No random sampling is required. If you add randomness for exploration, use a fixed seed and explain why.

### Avoid Chained Assignment

Make a deliberate copy of a filtered DataFrame before changing its columns. Prefer clear `.loc[...]` assignments where appropriate.

### Match the Plot to the Question

- Histogram → distribution of one numerical variable.
- Boxplot → numerical distributions across groups.
- Scatter plot → relationship between two numerical variables.
- Bar chart → category counts or summarized category values.

A chart is not informative without an explanation of what was plotted.

### Don't Over-Engineer

Functions and modules should improve clarity, not create unnecessary complexity. A small CSV-based project does **not** need a database, an API, a web application, or a model-training framework in Week 8.

---

# 16. Git and GitHub Workflow

Commit progress regularly instead of making one large commit at the end. A reasonable sequence might be:

```bash
git add labs/week08/task01_audit.py labs/week08/task02_missing_values.py
git commit -m "Lab08: audit raw data and examine missing values"

git add labs/week08/task03_duplicates.py labs/week08/task04_validation.py
git commit -m "Lab08: reconcile identifiers and validate observations"

git add labs/week08/task05_statistics.py labs/week08/task06_visualization.py
git commit -m "Lab08: add grouped statistics and EDA figures"

git add labs/week08/task07_features.py labs/week08/task08_eda_pipeline/
git commit -m "Lab08: build reproducible preprocessing and EDA pipeline"

git add labs/week08/README.md
git commit -m "Lab08: document data decisions, tests and findings"

git push
```

Adjust paths and commit granularity to match your repository. Commit the original supplied data and your code. Include the required generated outputs if the instructor requests them; avoid committing temporary files, caches, or unrelated artifacts.

A useful `.gitignore` can exclude:

```text
__pycache__/
*.pyc
.venv/
.ipynb_checkpoints/
```

Do not commit secrets, private datasets, or personal information.

---

# 17. Your Week 8 README

Create your own:

```text
labs/week08/README.md
```

It should be sufficiently detailed that another student can understand how to reproduce your results without needing a live demonstration.

Suggested structure:

````markdown
# Lab 08 — Data Preprocessing & Exploratory Data Analysis

## Environment and Setup
- Python and package versions
- How to run the individual tasks and integrated pipeline

## Dataset and Provenance
- Source: instructor-supplied synthetic fixture
- Number of source rows and columns
- What each main field means

## Tasks Completed
- Tasks 1–8, with a short note for each

## Initial Data Quality Findings
- Missingness by column
- Raw categorical variants
- Exact duplicates and conflicting identifiers

## Cleaning and Validation Policy
- Required fields and valid ranges
- Category alias decisions
- How duplicates and conflicts were handled
- Why some observations were excluded

## Before/After Reconciliation
| Stage | Rows | Notes |
| --- | ---: | --- |
| Raw source | | |
| After duplicate reconciliation | | |
| After validation | | |

## Missing-Value and Outlier Decisions
- What stayed missing
- Any imputation in a separate derived table
- What you investigated rather than automatically removed

## Descriptive Statistics and Plots
- Grouped summary with interpretation
- Link or path to each saved plot
- At least three evidence-based observations
- Caveats and uncertainties

## Feature Transformations
- One-hot encoding decision
- Scaling method and edge-case handling
- Why preprocessing parameters matter for future ML

## Tests and Verification
- Normal, boundary, invalid, missing, duplicate, rerun checks
- Counts verified against expected sanity checks

## Challenges and Debugging
- A real issue found and corrected, or a deliberate failure test

## AI Engineering Relevance
- How validation, auditability and reproducibility support later ML work

## AI Usage Log
- Complete this section if AI assistance was used
````

Do not write only “all tasks completed.” Explain the decisions and **show the evidence** supporting your conclusions.

---

# 18. Responsible AI-Assisted Learning

AI tools can help you understand techniques, but should not replace the reasoning behind your analysis.

Helpful prompts include:

- “Explain the difference between an exact duplicate and a repeated business key using my data contract; don't write my solution.”
- “Which of these fields are required for an occupancy-ratio calculation?”
- “Help me reason about when a missing numeric value should remain missing.”
- “Suggest test cases that could reveal errors in my validation rules, including boundaries.”
- “Why might a high but valid noise reading be flagged but retained?”
- “Review my plot titles, axes, and interpretation for misleading claims.”
- “Help me compare the meaning of mean, median, and IQR in this summary.”
- “Explain why fitting preprocessing parameters on held-out examples can cause data leakage.”
- “Review the responsibilities of my `audit.py`, `preprocess.py`, and `explore.py` modules without generating my complete code.”

If you use an assistant, document your use:

```markdown
## AI Usage Log

### Tool Used
-

### What I Asked
-

### What I Used
-

### What I Verified or Changed Myself
-
```

**You are responsible for every submitted line of code, claim, number, and visualization.** If an AI tool suggests dropping rows, ask whether the proposal is justified by the declared data contract.

---

# 19. Self-Study Learning Resources

Use these references to support independent reasoning. You do not need to memorize the entire documentation.

### 1. Pandas — Working with Missing Data

https://pandas.pydata.org/docs/user_guide/missing_data.html

Review `isna()`, `dropna()`, `fillna()`, and the distinction between missing and observed values.

### 2. Pandas — Duplicate Records

https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html

Focus on `subset`, `keep`, and why duplicate definitions depend on the data contract.

### 3. Pandas — Categorical Data

https://pandas.pydata.org/docs/user_guide/categorical.html

Focus on categories, categorical labels, and representing category information clearly.

### 4. Pandas — GroupBy Operations

https://pandas.pydata.org/docs/user_guide/groupby.html

Practice `size`, `count`, `mean`, `median`, and named aggregations.

### 5. Pandas — Descriptive Statistics

https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html

Review which statistics are returned and how missing values affect summaries.

### 6. Matplotlib — Quick Start

https://matplotlib.org/stable/users/explain/quick_start.html

Review figures, axes, titles, legends, and saving plots to a file.

### 7. Pandas — Visualization

https://pandas.pydata.org/docs/user_guide/visualization.html

Compare simple DataFrame plotting with direct Matplotlib use.

### 8. scikit-learn — Common Pitfalls (Preview Only)

https://scikit-learn.org/stable/common_pitfalls.html

Skim the explanation of data leakage and inconsistent preprocessing for future weeks. **Model training is not required in Lab 08.**

### Suggested Independent Study Path

```text
Audit the source
       ↓
Distinguish missingness and duplicate definitions
       ↓
Normalize categories + validate numeric rules
       ↓
Document exclusion decisions
       ↓
Summarize the valid data
       ↓
Investigate outliers and draw plots
       ↓
Demonstrate scaling/encoding carefully
       ↓
Re-run, compare, and document
```

---

# 20. Submission Checklist

Before submitting, verify:

- [ ] All required work is inside `labs/week08/` in your personal coursework repository.
- [ ] The supplied CSV and `data/README.md` remain unchanged.
- [ ] Task 1 includes a **raw-data audit** with shape, types, missingness, categories, and duplicates.
- [ ] Task 2 compares missing-value strategies and explains required vs optional fields.
- [ ] Task 3 differentiates exact duplicates, conflicting observation IDs, and legitimate repeated space readings.
- [ ] Task 4 applies the declared category aliases and numeric validation contract.
- [ ] Every excluded row has a recorded reason; nothing is silently discarded.
- [ ] Task 5 calculates `occupancy_ratio`, descriptive statistics, and grouped summaries.
- [ ] Potential outliers are flagged for investigation, not automatically deleted.
- [ ] Task 6 saves three clearly labeled plots and documents at least three grounded interpretations.
- [ ] Task 7 shows categorical encoding, numerical scaling, and a constant-column guard.
- [ ] Missingness flags are calculated before any optional imputation.
- [ ] Task 8 produces the seven required output artifacts (four tabular/report files and three plots).
- [ ] Integrated quality counts reconcile with the 46-row source.
- [ ] The pipeline runs twice with stable logical results.
- [ ] `labs/week08/README.md` explains the dataset, cleaning decisions, outputs, tests, and findings.
- [ ] AI assistance is disclosed if used.
- [ ] Meaningful Git commits are visible and the work has been pushed.
- [ ] You can defend why you kept, changed, or excluded any record.

---

# 21. Expected Learning Outcomes

After completing this lab, you should be able to:

- examine a dataset's quality before preprocessing it
- distinguish required and optional missing fields
- select a missing-value strategy and preserve missingness evidence
- diagnose duplicates using a meaningful logical key
- normalize category names and reject unknown categories transparently
- validate numerical values with explicit rules and boundary tests
- compute numerical and grouped summaries correctly
- interpret distributions and relationships through plots with appropriate caveats
- demonstrate basic categorical encoding and numerical feature scaling
- recognize the risk of data leakage in future ML pipelines
- produce a repeatable, auditable preprocessing and EDA workflow
- communicate design decisions and uncertainty clearly

---

## Lab 08 Key Message

> **A clean-looking dataset is not necessarily a trustworthy dataset. Trust comes from knowing what was observed, what changed, what was excluded, and why.**

Week 8 completes the course's first data-engineering sequence: data structures → NumPy → Pandas → ingestion → preprocessing and EDA. The next phase will build on this foundation when you begin machine-learning workflows.

**End of Lab 08**
