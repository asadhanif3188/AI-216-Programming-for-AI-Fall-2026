# AI-216: Programming for Artificial Intelligence
## Week 08 Lecture Handout — Data Preprocessing & Exploratory Data Analysis (EDA)

**Semester:** Fall 2026  
**Week:** 08  
**Theme:** From collected records to understandable, trustworthy data for AI applications

---

## 1. Why Week 8 Matters

In Week 5, NumPy helped us compute with numerical arrays. Week 6 introduced Pandas for working with labeled tables. In Week 7, we learned to obtain records from files, APIs, permitted web sources, and databases.

But **a successfully ingested dataset is not automatically ready for analysis or machine learning**. It may contain:

- missing measurements
- repeated observations or duplicated identifiers
- categories written in inconsistent ways
- impossible values or unusual observations
- numerical features measured on very different scales
- patterns that are difficult to notice by reading rows

This week connects data-quality decisions with evidence from descriptive statistics and visualization.

```text
Data collected or ingested (Week 7)
                ↓
Inspect structure and quality
                ↓
Define cleaning rules and document decisions
                ↓
Handle missing, duplicate and inconsistent records
                ↓
Describe distributions and relationships
                ↓
Visualize patterns and unusual cases
                ↓
Prepare understandable, reproducible data
                ↓
ML workflows (Weeks 10–11)
```

> **AI engineering question:** Can we explain what is in a dataset, which records we changed, what information may have been lost, and whether the resulting table is suitable for its intended use?

**Week 8 is not about training an ML model.** It is about understanding data before we trust it, and preparing for the ML workflows introduced after the midterm.

---

## 2. Week 8 Learning Outcomes

By the end of this lecture, you should be able to:

1. Explain the difference between **data validation**, **preprocessing**, and **exploratory data analysis**.
2. Inspect the number, location, and proportion of missing values.
3. Compare basic missing-value strategies and explain their trade-offs.
4. Distinguish exact duplicate rows from repeated identifiers or legitimate repeat measurements.
5. Clean and standardize categorical values without inventing new meanings.
6. Recognize basic ways to represent categories numerically.
7. Compute descriptive statistics and grouped summaries.
8. Use plots to examine numerical distributions, category frequencies, and relationships.
9. Identify potential outliers while avoiding unjustified automatic deletion.
10. Apply and explain basic min–max normalization and standardization.
11. Describe how exploratory findings influence data preparation decisions.
12. Make an analysis reproducible through explicit rules, deterministic operations, and recorded assumptions.

### Tools for this lecture

We use **Pandas**, **NumPy**, and **Matplotlib** (with no scikit-learn or model training required).

```bash
python -m pip install pandas numpy matplotlib
```

Use your existing Python environment if these libraries are already installed. On some systems, the executable is called `python3`.

The conventional imports are:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

**Example convention:** Each example is intended to be read as a separate short program unless a section explicitly says that it continues an earlier example. For readability, some snippets assume the three imports above have already been executed.

---

## 3. The Preprocessing and EDA Mindset

Three related activities serve different purposes:

| Activity | Main question | Typical action |
| --- | --- | --- |
| **Validation** | Is this record consistent with the declared rules? | Check required columns, ranges, types |
| **Preprocessing** | What transformations should make the data usable? | Handle missing values, standardize categories |
| **EDA** | What does this dataset appear to contain? | Compute summaries, inspect plots, compare groups |

There is no universal cleaning recipe. Rules depend on the meaning of the data and the task.

### 3.1 Basic Example — Observe Before Changing

```python
scores = pd.Series([80, 90, None, 75])

print("Rows:", len(scores))
print("Missing:", scores.isna().sum())
print("Average of observed scores:", scores.mean())
```

**Key idea:** Inspect what exists before deciding how to handle a missing value. Pandas' mean ignores missing entries by default.

### 3.2 Intermediate Example — Describe a Quality Problem

```python
readings = pd.DataFrame({
    "site": ["Lab", "lab", "Library", "Lab"],
    "energy_kwh": [110.0, 125.0, None, -20.0],
})

print(readings.isna().sum())
print(readings["site"].value_counts(dropna=False))
print(readings["energy_kwh"].describe())
```

**Key idea:** Missing values, inconsistent labels, and negative energy readings are different problems and may require different decisions.

### 3.3 Advanced Example — Write a Decision Rule Before Applying It

```python
readings = pd.DataFrame({
    "energy_kwh": [110.0, -20.0, 125.0, None]
})

# Example domain rule: energy consumption cannot be negative.
invalid = readings["energy_kwh"] < 0

print("Invalid negative readings:", int(invalid.sum()))

cleaned = readings.copy()
cleaned.loc[invalid, "energy_kwh"] = np.nan

print("Missing after invalid-value conversion:",
      int(cleaned["energy_kwh"].isna().sum()))
```

**Key idea:** An explicit rule tells us **why** data changed. We do not quietly replace all unusual observations with a preferred number.

---

## 4. Start with a Data Quality Audit

Before preprocessing, answer basic questions:

- How many records and columns are present?
- What does each column mean, and what unit does it use?
- Which columns are IDs, categories, features, measurements, or potential targets?
- Which values are missing?
- Which identifiers repeat?
- Which fields violate expected rules?
- What might be lost by filtering or filling values?

### 4.1 Basic Example — Shape, Types, and First Rows

```python
df = pd.DataFrame({
    "building": ["Lab", "Library", "Admin"],
    "energy_kwh": [120.0, 90.0, 100.0],
})

print(df.shape)
print(df.dtypes)
print(df.head())
```

### 4.2 Intermediate Example — Column-Level Missingness

```python
df = pd.DataFrame({
    "building": ["Lab", "Library", "Lab", None],
    "energy_kwh": [120.0, None, 95.0, 102.0],
    "occupancy": [40, 35, None, 48],
})

missing_counts = df.isna().sum()
missing_percent = df.isna().mean().mul(100)

print(pd.DataFrame({
    "missing_count": missing_counts,
    "missing_percent": missing_percent.round(1),
}))
```

### 4.3 Advanced Example — A Compact Audit Report

```python
def audit_table(df):
    return {
        "row_count": len(df),
        "column_count": len(df.columns),
        "missing_by_column": df.isna().sum().to_dict(),
        "exact_duplicate_rows": int(df.duplicated().sum()),
    }

sample = pd.DataFrame({
    "record_id": [1, 2, 2],
    "reading": [10.0, None, None],
})

print(audit_table(sample))
```

**Engineering connection:** A repeatable audit allows us to compare the source table with the processed table instead of only checking the final row count.

---

## 5. Recognizing Missing Data

Pandas can represent missing data using `NaN`, `None`, and, for certain types, `pd.NA`. Use `isna()` or `notna()` to detect missing entries rather than comparing values to `np.nan` with `==`.

An empty string (`""`) or placeholder such as `"unknown"` may **not** count as missing unless we explicitly define it that way.

### 5.1 Basic Example — Detect Missing Cells

```python
scores = pd.Series([72.0, None, 88.0, np.nan])

print(scores.isna())
print("Missing count:", scores.isna().sum())
```

### 5.2 Intermediate Example — Inspect Incomplete Rows

```python
df = pd.DataFrame({
    "sensor_id": ["S1", "S2", "S3"],
    "temperature_c": [22.5, None, 25.0],
    "humidity_pct": [55.0, 61.0, None],
})

incomplete_rows = df.loc[df.isna().any(axis=1)]
print(incomplete_rows)
```

`axis=1` here asks: **Does this row have a missing value in any column?**

### 5.3 Advanced Example — Convert Placeholder Text to Missing

```python
df = pd.DataFrame({
    "temperature_c": ["23.5", "N/A", "", "26.0", "bad"]
})

cleaned = df.copy()
cleaned["temperature_c"] = (
    cleaned["temperature_c"]
    .replace({"N/A": np.nan, "": np.nan})
)
cleaned["temperature_c"] = pd.to_numeric(
    cleaned["temperature_c"], errors="coerce"
)

print(cleaned)
print("Missing or invalid:", cleaned["temperature_c"].isna().sum())
```

**Important:** `errors="coerce"` also turns unparseable text such as `"bad"` into missing values. **Count and inspect those conversions**; do not silently hide data-quality problems.

---

## 6. Choosing a Missing-Value Strategy

Missingness does not have one universally correct treatment.

| Strategy | What it does | Possible concern |
| --- | --- | --- |
| Retain missing values | Preserve uncertainty | Some later operations cannot use them |
| Drop incomplete rows | Use only complete observations | Can remove many rows or bias the sample |
| Drop a column | Remove a heavily incomplete feature | May discard useful information |
| Fill with median/mean | Replace absent numeric values | Changes the distribution and hides original gaps |
| Fill with a category | Represent absence explicitly | "Missing" may have a different meaning from a real category |

A missing value is **not automatically zero**. In an energy dataset, no reported reading and a measured value of `0 kWh` are different statements.

### 6.1 Basic Example — Drop Rows Missing a Required Field

```python
df = pd.DataFrame({
    "record_id": [101, 102, 103],
    "energy_kwh": [120.0, None, 110.0],
})

complete = df.dropna(subset=["energy_kwh"])
print(complete)
```

### 6.2 Intermediate Example — Fill a Numeric Column

```python
df = pd.DataFrame({
    "energy_kwh": [100.0, None, 120.0, 500.0]
})

filled = df.copy()
median_energy = filled["energy_kwh"].median()
filled["energy_kwh"] = filled["energy_kwh"].fillna(median_energy)

print("Median used:", median_energy)
print(filled)
```

The median is less affected by a very high observation than the mean. That does **not** make it the correct choice in every situation.

### 6.3 Advanced Example — Preserve a Missingness Indicator

```python
df = pd.DataFrame({
    "building": ["Lab", "Library", "Admin"],
    "energy_kwh": [120.0, None, 160.0],
})

prepared = df.copy()
prepared["energy_was_missing"] = prepared["energy_kwh"].isna()

fallback = prepared["energy_kwh"].median()
prepared["energy_kwh"] = prepared["energy_kwh"].fillna(fallback)

print(prepared)
```

**Engineering connection:** An indicator helps distinguish *measured* values from values inserted by a preprocessing rule. When the data is later used for ML, the imputation value must be learned from training data only; we will revisit that after the midterm.

---

## 7. Understanding Duplicate Records

Two rows can look similar but mean different things:

- **Exact duplicate:** every stored field is identical.
- **Duplicate identifier:** two rows have the same key, but other information may differ.
- **Legitimate repeated measurement:** the same device or user appears again at another time.

A duplicate decision needs a **business key**, not just a command.

### 7.1 Basic Example — Detect Exact Duplicates

```python
df = pd.DataFrame({
    "record_id": [1, 2, 2],
    "energy_kwh": [90, 110, 110],
})

print(df.duplicated())
print("Exact duplicates:", df.duplicated().sum())
```

### 7.2 Intermediate Example — Compare Duplicate Keys

```python
df = pd.DataFrame({
    "record_id": [1, 2, 2],
    "energy_kwh": [90, 110, 130],
})

conflicting = df.loc[df.duplicated(subset=["record_id"], keep=False)]
print(conflicting)
```

The two records with ID `2` conflict. **Simply keeping the first may hide an upstream correction or error.**

### 7.3 Advanced Example — Check a Composite Key

```python
df = pd.DataFrame({
    "device_id": ["A", "A", "B", "A"],
    "day": ["2026-11-01", "2026-11-02", "2026-11-01", "2026-11-02"],
    "reading": [10.0, 11.0, 9.0, 11.0],
})

business_key = ["device_id", "day"]
print(df.loc[df.duplicated(subset=business_key, keep=False)])

# Only when one row per device per day is the declared rule:
unique_records = df.drop_duplicates(subset=business_key)
print(unique_records)
```

**Key idea:** The meaning of a record determines the correct duplicate rule. Keep an audit of how many records were removed and why.

---

## 8. Validating Ranges, Types, and Logical Rules

Missing and duplicated data are only part of data quality. A value can be present and still be invalid.

### 8.1 Basic Example — Detect an Impossible Negative Value

```python
df = pd.DataFrame({"energy_kwh": [110.0, -4.0, 125.0]})

invalid = df["energy_kwh"] < 0
print(df.loc[invalid])
```

### 8.2 Intermediate Example — Several Column Rules

```python
df = pd.DataFrame({
    "temperature_c": [22.0, 100.0, 24.0],
    "occupancy": [45, 50, -3],
})

invalid_temperature = ~df["temperature_c"].between(-20, 60)
invalid_occupancy = ~df["occupancy"].between(0, 100)

print(df.loc[invalid_temperature | invalid_occupancy])
```

The temperature and occupancy ranges are **illustrative domain assumptions**, not universal rules.

### 8.3 Advanced Example — Explain Rejection Reasons

```python
df = pd.DataFrame({
    "record_id": [1, 2, 3],
    "energy_kwh": [100.0, -5.0, 150.0],
    "occupancy": [40, 45, 120],
})

checks = df.copy()
checks["invalid_energy"] = checks["energy_kwh"] < 0
checks["invalid_occupancy"] = ~checks["occupancy"].between(0, 100)
checks["needs_review"] = (
    checks["invalid_energy"] | checks["invalid_occupancy"]
)

print(checks.loc[checks["needs_review"]])
```

**Engineering connection:** A transparent audit is more useful than a cleaned table that conceals why records disappeared.

---

## 9. Cleaning Categorical Data

Categorical columns describe groups, for example `"Lab"`, `"Library"`, and `"Admin"`. Inconsistent spacing or capitalization can split one real group into several labels.

### 9.1 Basic Example — Consistent Case

```python
buildings = pd.Series(["Lab", "lab", "LAB"])

print(buildings.str.lower())
```

### 9.2 Intermediate Example — Strip Spaces and Standardize Labels

```python
df = pd.DataFrame({
    "building": [" lab ", "LIBRARY", "Admin", "lab"],
})

cleaned = df.copy()
cleaned["building"] = cleaned["building"].str.strip().str.title()

print(cleaned["building"].value_counts())
```

### 9.3 Advanced Example — Controlled Mapping and Unknown Categories

```python
df = pd.DataFrame({
    "building": ["lab", "laboratory", "library", "admin", "other"],
})

mapping = {
    "lab": "Lab",
    "laboratory": "Lab",
    "library": "Library",
    "admin": "Admin",
}

prepared = df.copy()
raw_labels = prepared["building"].str.strip().str.lower()
prepared["building_clean"] = raw_labels.map(mapping)

unknown = prepared.loc[prepared["building_clean"].isna(), "building"]
print("Unrecognized labels:", unknown.tolist())
```

**Key idea:** Do not invent a mapping for an unfamiliar category. Preserve it for investigation or explicitly label it `Unknown` under a documented policy.

---

## 10. Representing Categories for Later Analysis

Categories can be represented as labels, ordered categories, or numerical indicator columns. **Numerical codes are not always meaningful numerical quantities.**

### 10.1 Basic Example — Count Categories

```python
building = pd.Series(["Lab", "Library", "Lab", "Admin"])
print(building.value_counts())
```

### 10.2 Intermediate Example — Ordered Categories

```python
priority = pd.Series(["low", "high", "medium", "low"])

ordered_type = pd.CategoricalDtype(
    categories=["low", "medium", "high"], ordered=True
)
priority = priority.astype(ordered_type)

print(priority.sort_values().tolist())
```

### 10.3 Advanced Example — One-Hot Indicator Columns

```python
df = pd.DataFrame({
    "record_id": [1, 2, 3],
    "building": ["Lab", "Library", "Admin"],
})

encoded = pd.get_dummies(df, columns=["building"], dtype=int)
print(encoded)
```

**Preview of ML:** One-hot encoding avoids treating labels such as `Lab=1`, `Library=2`, `Admin=3` as if one building were numerically larger. Future training workflows must also handle categories not observed during training.

---

## 11. Descriptive Statistics: Understand the Center

Descriptive statistics summarize the data you have. They do not prove that the same pattern holds for every population or future observation.

| Measure | Question |
| --- | --- |
| Count | How many observed values are available? |
| Mean | What is the arithmetic average? |
| Median | What is the middle observed value? |
| Mode | Which value occurs most frequently? |
| Min / Max | What is the observed range? |

### 11.1 Basic Example — Mean and Median

```python
energy = pd.Series([100, 110, 120, 130])

print("Mean:", energy.mean())
print("Median:", energy.median())
```

### 11.2 Intermediate Example — One High Observation

```python
energy = pd.Series([100, 110, 120, 130, 900])

print("Mean:", energy.mean())
print("Median:", energy.median())
print("Max:", energy.max())
```

**Key idea:** The mean moves strongly when an extreme value is present; the median is usually more robust to a single extreme observation.

### 11.3 Advanced Example — Compare Multiple Summaries

```python
df = pd.DataFrame({
    "energy_kwh": [100, 110, 120, 130, 900],
    "temperature_c": [21, 22, 24, 25, 27],
})

summary = df.agg({
    "energy_kwh": ["count", "mean", "median", "min", "max"],
    "temperature_c": ["count", "mean", "median", "min", "max"],
})

print(summary)
```

---

## 12. Variability, Quantiles, and the IQR

The average alone does not explain how spread out the values are.

- **Standard deviation (`std`)** describes variability around the mean.
- **Quantiles** divide ordered values into proportions.
- **Interquartile range (IQR)** is the difference between the 75th and 25th percentiles.

Pandas `Series.std()` uses the sample standard deviation (`ddof=1`) by default. That differs from the default population standard deviation computed by `np.std()`.

### 12.1 Basic Example — Standard Deviation

```python
scores = pd.Series([70, 75, 80, 85, 90])

print("Mean:", scores.mean())
print("Sample standard deviation:", scores.std())
```

### 12.2 Intermediate Example — Quartiles

```python
energy = pd.Series([95, 100, 110, 120, 130, 150])

q1 = energy.quantile(0.25)
q3 = energy.quantile(0.75)
iqr = q3 - q1

print("Q1:", q1, "Q3:", q3, "IQR:", iqr)
```

### 12.3 Advanced Example — An Outlier Flag, Not an Automatic Deletion

```python
energy = pd.Series([95, 100, 110, 120, 130, 150, 900])

q1 = energy.quantile(0.25)
q3 = energy.quantile(0.75)
iqr = q3 - q1

lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr
flagged = energy[(energy < lower) | (energy > upper)]

print("Review these observations:", flagged.tolist())
```

The 1.5 × IQR rule is a simple *screening heuristic*. It does not prove that a value is a mistake. A genuine high-usage building may be the most important observation.

---

## 13. Grouped Summaries and Comparisons

EDA often asks whether groups differ. Always consider group size and missingness before comparing averages.

### 13.1 Basic Example — Average by Building

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Library"],
    "energy_kwh": [110, 130, 90],
})

print(df.groupby("building")["energy_kwh"].mean())
```

### 13.2 Intermediate Example — Count and Median by Group

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Library", "Library"],
    "energy_kwh": [110.0, 900.0, 90.0, 100.0],
})

print(df.groupby("building")["energy_kwh"].agg(
    ["count", "mean", "median"]
))
```

### 13.3 Advanced Example — Named Aggregations

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Library", "Library", "Admin"],
    "energy_kwh": [110.0, 150.0, 90.0, 100.0, np.nan],
})

report = df.groupby("building", dropna=False).agg(
    observations=("building", "size"),
    observed_energy=("energy_kwh", "count"),
    mean_energy=("energy_kwh", "mean"),
    median_energy=("energy_kwh", "median"),
)

print(report)
```

**Key idea:** `size` counts records in a group; `count` for a specific column excludes missing values. These are not interchangeable.

---

## 14. What Is Exploratory Data Analysis?

**EDA** is an iterative process of asking questions, summarizing observations, visualizing evidence, and revising questions.

It is not just a final set of colorful charts.

A useful EDA sequence is:

```text
Question → Summary → Visualization → Interpretation → Quality Check → Next Question
```

### 14.1 Basic Example — Ask About the Typical Value

```python
energy = pd.Series([100, 105, 125, 130])
print("Typical value (median):", energy.median())
```

### 14.2 Intermediate Example — Ask About Categories

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Admin", "Library", "Lab"],
})

print(df["building"].value_counts())
```

### 14.3 Advanced Example — Compare Summary with Data Quality

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Library", "Library"],
    "energy_kwh": [100.0, np.nan, 90.0, 110.0],
})

print(df.groupby("building").agg(
    rows=("building", "size"),
    observed=("energy_kwh", "count"),
    mean_kwh=("energy_kwh", "mean"),
))
```

**Engineering lesson:** A group with just one observed measurement may have a numerical average but weak evidence. Counts give context to comparisons.

---

## 15. Visualizing Numerical Distributions

A **histogram** helps answer: *Where do measurements tend to fall, and is the distribution skewed?*

**Plotting convention:** The examples create figures using `fig, ax = plt.subplots()`, label axes, and call `plt.show()`. In a non-interactive environment you can instead save the figure with `fig.savefig("figure.png")` before calling `plt.close(fig)`.

### 15.1 Basic Example — Histogram

```python
energy = [95, 100, 110, 110, 120, 125, 140]

fig, ax = plt.subplots()
ax.hist(energy, bins=5)
ax.set(title="Energy readings", xlabel="Energy (kWh)", ylabel="Count")
plt.show()
```

### 15.2 Intermediate Example — Compare Observations and Typical Value

```python
energy = pd.Series([95, 100, 110, 120, 130, 140, 500])

fig, ax = plt.subplots()
ax.hist(energy, bins=6)
ax.axvline(energy.median(), linestyle="--", label="Median")
ax.set(xlabel="Energy (kWh)", ylabel="Count")
ax.legend()
plt.show()
```

### 15.3 Advanced Example — See the Effect of a Possible Outlier

```python
energy = pd.Series([95, 100, 110, 120, 130, 150, 900])

fig, ax = plt.subplots()
ax.boxplot(energy.dropna())
ax.set(title="Energy distribution and unusual observations", ylabel="Energy (kWh)")
plt.show()
```

**Key idea:** Boxplots and histograms provide complementary views; a point outside the boxplot whiskers is a candidate for investigation, not a proven error.

---

## 16. Visualizing Categories and Relationships

Choose a chart based on the question, not because it looks sophisticated.

| Question | Common chart |
| --- | --- |
| How frequently does each category occur? | Bar chart |
| What is the distribution of a numerical measurement? | Histogram or boxplot |
| Do two numeric measurements vary together? | Scatter plot |
| How does a measurement change over ordered time? | Line chart |

### 16.1 Basic Example — Bar Chart for Categories

```python
counts = pd.Series({"Lab": 5, "Library": 3, "Admin": 2})

fig, ax = plt.subplots()
ax.bar(counts.index, counts.values)
ax.set(xlabel="Building", ylabel="Number of records", title="Records by building")
plt.show()
```

### 16.2 Intermediate Example — Scatter Plot

```python
df = pd.DataFrame({
    "temperature_c": [18, 20, 22, 25, 28],
    "energy_kwh": [100, 108, 120, 127, 150],
})

fig, ax = plt.subplots()
ax.scatter(df["temperature_c"], df["energy_kwh"])
ax.set(xlabel="Temperature (°C)", ylabel="Energy (kWh)",
       title="Temperature versus energy use")
plt.show()
```

### 16.3 Advanced Example — Compare Groups without Hiding Sample Size

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Library", "Library", "Admin"],
    "energy_kwh": [110, 145, 90, 105, 125],
})

summary = df.groupby("building")["energy_kwh"].agg(["mean", "count"])

fig, ax = plt.subplots()
ax.bar(summary.index, summary["mean"])
ax.set(xlabel="Building", ylabel="Mean energy (kWh)", title="Mean energy by building")
plt.show()

print("Group sample sizes:")
print(summary["count"])
```

**Interpretation:** A bar showing a high mean should not be read without its sample size and the possibility of outliers or unequal operating conditions.

---

## 17. Relationships and Correlation

The purpose of a relationship analysis is to discover a pattern worth investigating, **not to declare cause and effect**.

### 17.1 Basic Example — Two Values Moving Together

```python
df = pd.DataFrame({
    "temperature_c": [18, 20, 24, 27],
    "energy_kwh": [90, 105, 130, 160],
})

print(df[["temperature_c", "energy_kwh"]].corr())
```

### 17.2 Intermediate Example — Compare Several Numerical Features

```python
df = pd.DataFrame({
    "temperature_c": [18, 20, 24, 27, 30],
    "occupancy": [30, 40, 35, 50, 55],
    "energy_kwh": [90, 105, 125, 150, 165],
})

print(df.corr(numeric_only=True).round(2))
```

### 17.3 Advanced Example — Check for Different Group Patterns

```python
df = pd.DataFrame({
    "building": ["Lab", "Lab", "Lab", "Library", "Library", "Library"],
    "temperature_c": [20, 22, 25, 20, 22, 25],
    "energy_kwh": [130, 140, 160, 90, 95, 110],
})

fig, ax = plt.subplots()
for building, subset in df.groupby("building"):
    ax.scatter(subset["temperature_c"], subset["energy_kwh"], label=building)

ax.set(xlabel="Temperature (°C)", ylabel="Energy (kWh)")
ax.legend()
plt.show()
```

**Key idea:** Two measurements may be correlated because of other factors. Grouping helps us ask better questions, but an observational pattern alone does not establish causation.

---

## 18. Normalization and Standardization: Why Scale Features?

Some numerical columns have very different ranges. For example:

```text
Temperature:    15–35 °C
Energy usage:  100–900 kWh
Occupancy:      0–100 people
```

In later ML work, some algorithms can be influenced strongly by feature scale. We introduce the **ideas and arithmetic** now; the training-safe pipeline implementation comes in Week 10.

| Method | Formula | Typical interpretation |
| --- | --- | --- |
| Min–max scaling | `(x - min) / (max - min)` | Maps observed range to 0–1 when fitted and applied on the same data |
| Standardization (z-score) | `(x - mean) / std` | Centers data around zero in units of standard deviations |

**Terminology note:** In Python/ML, the word “normalization” is used in several ways. Here we explicitly mean **min–max scaling**, not normalization to unit vector length.

### 18.1 Basic Example — Normalize Scores by a Known Maximum

```python
scores = pd.Series([40.0, 70.0, 100.0])
normalized = scores / 100.0

print(normalized)
```

### 18.2 Intermediate Example — Min–Max Scaling

```python
energy = pd.Series([100.0, 150.0, 200.0])

minimum = energy.min()
maximum = energy.max()
scaled = (energy - minimum) / (maximum - minimum)

print(scaled)
```

### 18.3 Advanced Example — Safe Standardization of a Feature Table

```python
df = pd.DataFrame({
    "energy_kwh": [100.0, 140.0, 180.0],
    "occupancy": [30.0, 45.0, 60.0],
    "constant_flag": [1.0, 1.0, 1.0],
})

means = df.mean()
stds = df.std(ddof=0)
safe_stds = stds.replace(0, 1.0)

standardized = (df - means) / safe_stds
print(standardized.round(3))
```

A constant feature has zero variance; dividing by zero would produce invalid values. The example makes its standardized values zero. **Remember:** The choice of scaling method depends on the downstream application; scaling is not automatically required for every task.

---

## 19. Handling Scaling Edge Cases

A transformation should behave deliberately for missing values, constant columns, and new observations.

### 19.1 Basic Example — Constant Column

```python
values = pd.Series([5.0, 5.0, 5.0])
spread = values.max() - values.min()

if spread == 0:
    scaled = pd.Series(0.0, index=values.index)
else:
    scaled = (values - values.min()) / spread

print(scaled.tolist())
```

### 19.2 Intermediate Example — One Missing Measurement

```python
values = pd.Series([100.0, np.nan, 200.0])
minimum, maximum = values.min(), values.max()

scaled = (values - minimum) / (maximum - minimum)
print(scaled)
```

The missing value remains missing; scaling is **not** a missing-value treatment.

### 19.3 Advanced Example — Reuse Existing Scaling Parameters

```python
reference = pd.Series([100.0, 150.0, 200.0])
new_values = pd.Series([125.0, 250.0])

minimum = reference.min()
maximum = reference.max()

scaled_new = (new_values - minimum) / (maximum - minimum)
print(scaled_new.tolist())
```

An unseen value can produce a scaled result **outside** the interval 0–1, as `250` does here. This is expected when using previously learned bounds; do not silently recompute the bounds on every new batch.

---

## 20. Reproducibility: From Notebook Exploration to Repeatable Analysis

An analysis should be understandable and repeatable by another student or engineer.

A reproducible workflow typically records:

- where the source data came from and when it was collected
- what every field represents, including units
- rules for missing, invalid, and duplicate values
- which rows were removed or changed, and why
- parameters used for scaling or imputation
- library versions, where relevant
- code that reconstructs derived tables and figures

### 20.1 Basic Example — Preserve the Source

```python
raw = pd.DataFrame({"energy_kwh": [100.0, -10.0, 120.0]})
prepared = raw.copy()

prepared.loc[prepared["energy_kwh"] < 0, "energy_kwh"] = np.nan

print("Original:", raw["energy_kwh"].tolist())
print("Prepared:", prepared["energy_kwh"].tolist())
```

### 20.2 Intermediate Example — Deterministic Row Ordering

```python
df = pd.DataFrame({
    "record_id": [3, 1, 2],
    "energy_kwh": [140, 100, 120],
})

report = df.sort_values("record_id").reset_index(drop=True)
print(report)
```

### 20.3 Advanced Example — Explicit Randomness for Reproducible Demonstrations

```python
rng = np.random.default_rng(216)

synthetic_occupancy = rng.integers(10, 75, size=5)
print(synthetic_occupancy)
```

The fixed seed makes the **random demonstration** repeatable. It does not repair data quality or guarantee full reproducibility across every environment and library version.

---

## 21. A Preview: Data Leakage in Future ML Workflows

One subtle risk is using information from a future test/validation dataset to choose preprocessing parameters. This is called **data leakage** and can make model evaluation appear better than it really is.

For now, learn the conceptual rule:

> **When preparing for ML, learn imputation values, scaling parameters, and category mappings from the training portion; reuse them to transform held-out or new observations.**

### 21.1 Basic Example — Training Median

```python
train_energy = pd.Series([100.0, 120.0, np.nan])
new_energy = pd.Series([np.nan, 200.0])

train_median = train_energy.median()
prepared_new = new_energy.fillna(train_median)

print("Training median:", train_median)
print(prepared_new)
```

### 21.2 Intermediate Example — Apply Training Bounds

```python
train = pd.Series([100.0, 150.0, 200.0])
validation = pd.Series([125.0, 175.0])

low, high = train.min(), train.max()
validation_scaled = (validation - low) / (high - low)

print(validation_scaled)
```

### 21.3 Advanced Example — Why Using All Rows Can Mislead

```python
train = pd.Series([100.0, 120.0, 140.0])
held_out = pd.Series([1000.0])

train_mean = train.mean()
all_rows_mean = pd.concat([train, held_out]).mean()

print("Training-only mean:", train_mean)
print("Mean after including held-out data:", all_rows_mean)
```

This is a **preview**, not an instruction to train an ML model now. Proper train/test splitting and leakage-resistant pipelines are taught in Weeks 10–11.

---

# 22. Integrated AI-Oriented Example — Campus Energy Data EDA

This worked example brings the Week 8 ideas together. Imagine a campus team collecting energy observations that might later support a demand-forecasting or anomaly-review application. We will **not** train a model.

The dataset below is **synthetic**, deliberately small, and designed to illustrate missing values, inconsistent categories, duplicates, invalid ranges, and a high but potentially genuine energy reading.

The process is:

```text
Raw observations
       ↓
Quality audit
       ↓
Normalize labels and check numerical ranges
       ↓
Handle an exact duplicate
       ↓
Keep missingness indicators and explain a simple fill rule
       ↓
Descriptive and grouped summaries
       ↓
Distribution and relationship plots
       ↓
Document what changed
```

## 22.1 The Raw Observations

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

raw = pd.DataFrame([
    {"record_id": 1, "building": " Lab ",   "energy_kwh": 120.0, "temperature_c": 23.0, "occupancy": 45},
    {"record_id": 2, "building": "lab",     "energy_kwh": 128.0, "temperature_c": 24.0, "occupancy": 48},
    {"record_id": 3, "building": "Library", "energy_kwh": 95.0,  "temperature_c": 22.0, "occupancy": 35},
    {"record_id": 4, "building": "library", "energy_kwh": None,  "temperature_c": 23.0, "occupancy": 39},
    {"record_id": 5, "building": "Admin",   "energy_kwh": 105.0, "temperature_c": 26.0, "occupancy": 30},
    {"record_id": 6, "building": "Lab",     "energy_kwh": -20.0, "temperature_c": 24.0, "occupancy": 50},
    {"record_id": 7, "building": "Admin",   "energy_kwh": 112.0, "temperature_c": 27.0, "occupancy": -3},
    {"record_id": 8, "building": "Lab",     "energy_kwh": 800.0, "temperature_c": 29.0, "occupancy": 60},
    {"record_id": 9, "building": "Library", "energy_kwh": 102.0, "temperature_c": None, "occupancy": 33},
    {"record_id": 10,"building": "Admin",   "energy_kwh": 109.0, "temperature_c": 25.0, "occupancy": 32},
    {"record_id": 11,"building": "Lab",     "energy_kwh": 132.0, "temperature_c": 25.0, "occupancy": 52},
    {"record_id": 11,"building": "Lab",     "energy_kwh": 132.0, "temperature_c": 25.0, "occupancy": 52},
])
```

**Important design assumptions for this illustration:** one `record_id` corresponds to one logical observation; negative `energy_kwh` and negative `occupancy` are invalid; a very high positive energy reading is unusual but **not automatically invalid**.

## 22.2 Audit Before Changing Anything

```python
print("Raw shape:", raw.shape)
print("Missing values:\n", raw.isna().sum())
print("Exact duplicates:", int(raw.duplicated().sum()))
print("Raw building labels:\n", raw["building"].value_counts(dropna=False))
```

Do not skip this step. You will need the original counts when you explain the outcome of preprocessing.

## 22.3 Apply Explicit Rules and Preserve Evidence

```python
prepared = raw.drop_duplicates().copy()

prepared["building"] = prepared["building"].str.strip().str.title()

invalid_energy = prepared["energy_kwh"] < 0
invalid_occupancy = prepared["occupancy"] < 0

prepared["energy_was_missing_or_invalid"] = (
    prepared["energy_kwh"].isna() | invalid_energy
)
prepared["occupancy_was_invalid"] = invalid_occupancy

prepared.loc[invalid_energy, "energy_kwh"] = np.nan
prepared.loc[invalid_occupancy, "occupancy"] = np.nan

print("Records after exact deduplication:", len(prepared))
print("Missing after validation:\n", prepared.isna().sum())
```

**Why not delete the 800 kWh observation?** It may be a genuine peak event. EDA should investigate its context rather than erase it without a domain-supported reason.

## 22.4 Explain a Simple Missing-Value Policy

For this **demonstration only**, we fill absent numeric values with the observed median in the current table. This helps illustrate the mechanics of Pandas preprocessing; it is **not** a universal recommendation and would need a training-only fit in a future ML pipeline.

```python
numeric_columns = ["energy_kwh", "temperature_c", "occupancy"]
fill_values = prepared[numeric_columns].median()

analysis_table = prepared.copy()
analysis_table[numeric_columns] = (
    analysis_table[numeric_columns].fillna(fill_values)
)

print("Medians used:\n", fill_values)
print("Remaining numeric missing:\n",
      analysis_table[numeric_columns].isna().sum())
```

Keep the `prepared` table, which still shows missingness, separate from the `analysis_table`, which contains filled values.

## 22.5 Summarize and Compare Buildings

```python
print(analysis_table[numeric_columns].describe())

group_summary = analysis_table.groupby("building").agg(
    observations=("record_id", "size"),
    mean_energy_kwh=("energy_kwh", "mean"),
    median_energy_kwh=("energy_kwh", "median"),
    mean_occupancy=("occupancy", "mean"),
).sort_index()

print(group_summary.round(2))
```

The high Lab energy observation may pull the **mean** away from the **median**. That is a useful discussion about the meaning and limitations of each statistic.

## 22.6 Visualize the Evidence

```python
fig, ax = plt.subplots()
ax.hist(analysis_table["energy_kwh"], bins=6)
ax.set(title="Campus energy observations", xlabel="Energy (kWh)", ylabel="Count")
fig.tight_layout()
plt.show()

fig, ax = plt.subplots()
for building, group in analysis_table.groupby("building"):
    ax.scatter(group["temperature_c"], group["energy_kwh"], label=building)

ax.set(xlabel="Temperature (°C)", ylabel="Energy (kWh)",
       title="Temperature and energy by building")
ax.legend()
fig.tight_layout()
plt.show()
```

In a script that needs reproducible saved plots, replace `plt.show()` with calls such as `fig.savefig("energy_histogram.png")` and then `plt.close(fig)`.

## 22.7 End with an Honest Process Report

```python
process_report = {
    "source_rows": len(raw),
    "exact_duplicates_removed": int(raw.duplicated().sum()),
    "rows_after_deduplication": len(prepared),
    "invalid_negative_energy": int(invalid_energy.sum()),
    "invalid_negative_occupancy": int(invalid_occupancy.sum()),
    "numeric_missing_after_fill": int(
        analysis_table[numeric_columns].isna().sum().sum()
    ),
}

print(process_report)
```

Expected process counts:

```text
source_rows: 12
exact_duplicates_removed: 1
rows_after_deduplication: 11
invalid_negative_energy: 1
invalid_negative_occupancy: 1
numeric_missing_after_fill: 0
```

### What This Example Demonstrates

- **Inspection precedes modification.**
- **Invalid values and genuine outliers are not the same.**
- **Row-level changes should be auditable.**
- **Imputed values are different from observed values.**
- **A single summary statistic rarely tells the whole story.**
- **Figures require labels and interpretation.**
- **ML-ready preprocessing needs additional safeguards**, including train/test separation and reusable transformations in later weeks.

---

# 23. Common Mistakes and Engineering Pitfalls

## 23.1 Replacing Missing Values with Zero Without Evidence

```python
# Questionable unless zero means the same thing as "not observed":
# df["energy_kwh"] = df["energy_kwh"].fillna(0)
```

A missing reading does not necessarily mean no energy was consumed.

## 23.2 Dropping Every Row with Any Missing Field

```python
# df.dropna() may remove records for missing fields irrelevant to your question.
```

First decide which fields are essential, then inspect how many rows the rule would remove.

## 23.3 Calling Every Repeated ID a Duplicate

A sensor ID can appear many times because a sensor collects readings repeatedly. The correct uniqueness rule might be **(sensor ID, timestamp)**, not sensor ID alone.

## 23.4 Removing All Unusual Values

A rare or extreme observation can be an error **or** the most meaningful event in the dataset. Flag, inspect, and document before removal.

## 23.5 Hiding Category Mismatches

```python
# "Lab", "lab", and " Lab " may refer to the same category.
```

Define a standardization rule and inspect unknown labels after applying it.

## 23.6 Computing Numerical Statistics on Text

Use `pd.to_numeric(..., errors="coerce")` with an audit of invalid conversions. Do not assume numeric-looking CSV text has a numeric type.

## 23.7 Confusing `count()` and `size()` in Groups

`count()` ignores missing values in the selected column; `size()` counts group rows. Neither is automatically the correct denominator for every question.

## 23.8 Treating Correlation as Causation

A scatter plot or correlation coefficient does not prove that temperature *caused* the difference in energy use.

## 23.9 Forgetting the Scaling Denominator

Min–max scaling has a zero denominator for constant columns. Standardization encounters the same problem for zero standard deviation.

## 23.10 Learning a Transformation from Held-Out Data

Avoid choosing a scaling range, imputation median, or category mapping from future test data. This can leak information into model development.

## 23.11 Mutating the Only Copy of Raw Data

Use clear copies or reproducible transformations. An undocumented change to the source table can make analysis impossible to audit.

## 23.12 Creating Misleading Plots

Always label units, check sample sizes, and avoid plotting choices that exaggerate differences or conceal missing data.

## 23.13 Confusing EDA with Model Evaluation

A dataset can be clean and well visualized yet still be unsuitable for predicting a target. That question requires later modeling, evaluation, and domain checks.

---

# 24. A Practical Preprocessing Decision Framework

Use a short sequence before changing any column:

| Step | Question | Example |
| --- | --- | --- |
| 1. Understand | What does this field mean? | kWh of energy, not a currency |
| 2. Inspect | What values and gaps appear? | 2 missing measurements |
| 3. Define | What rule does the domain support? | Negative consumption is invalid |
| 4. Apply | Can I express the rule clearly? | Set invalid negative values to missing |
| 5. Record | What changed and how much? | One invalid record flagged |
| 6. Recheck | Did the transformation meet the intention? | No negative values remain |
| 7. Interpret | Did the transformation alter the analysis? | Compare before/after summaries |

For each transformation, students should be able to finish this sentence:

> "I changed **this field** using **this rule** because **this meaning or evidence** supports it, and I checked **this result**."

---

# 25. Thinking Like an AI Engineer

### Understanding the Dataset

- What does each row represent?
- What is the unit and expected range of each measurement?
- Is an identifier unique, or can it repeat legitimately?
- Which fields are numeric, categorical, dates, or labels?

### Assessing Quality

- Is the missingness concentrated in particular groups?
- Are invalid values errors or legitimate exceptions?
- What evidence supports removing or replacing a record?
- Could a cleaning rule introduce bias?

### Exploring Patterns

- Which statistic answers the question: mean, median, or distribution?
- Which plot would communicate the answer most clearly?
- What alternative explanations could account for the pattern?
- Are comparisons fair when group sizes differ?

### Preparing for AI

- Which transformations must be repeated when new data arrives?
- What should happen when the data schema changes?
- Can we recover the original records and reconstruction steps?
- Which statistics must be learned only from training data later?

**The engineering shift:** Do not ask only, "Does the DataFrame look clean?" Ask, **"Can we justify each transformation, reproduce it, and explain what it means for future decisions?"**

---

# 26. Responsible Use of AI Tools

AI assistants can help with the reasoning and debugging of data preprocessing, but students remain responsible for the decisions and the meaning of the data.

Useful learning prompts:

- "Do not solve my exercise. Explain what information I should inspect before handling missing values."
- "Given my column meanings, what risks come with replacing missing values by the mean?"
- "Help me distinguish a duplicate record from a repeated measurement."
- "Explain why my category counts changed after `.str.strip()` and `.str.title()`."
- "What does the IQR outlier rule detect, and what does it **not** prove?"
- "Explain why the mean and median are different in this distribution."
- "Help me interpret this plot without claiming causation."
- "Why does my min–max scaling produce missing or infinite values?"
- "Review my preprocessing decisions for possible data leakage without writing the final program."
- "Suggest edge cases and audit checks for my cleaning functions."

**Responsible use:** Do not paste sensitive personal or institutional data into an external AI system without appropriate permission. Do not accept an AI-generated cleaning rule without checking it against the dataset's meaning.

---

# 27. Self-Check Questions

Before moving to the Week 8 lab, make sure you can answer:

1. How is data preprocessing different from data ingestion?
2. How does EDA differ from preprocessing?
3. Why should a data-quality audit precede cleaning?
4. What does `df.isna().sum()` tell us?
5. How would you calculate the percentage of missing values in a column?
6. Why can `"N/A"` or an empty string require explicit handling?
7. What is the danger of replacing every missing numerical value with zero?
8. How does dropping rows differ from filling values?
9. Why might the median be preferred to the mean for some numerical columns?
10. How can an indicator preserve information about imputation?
11. What is the difference between an exact duplicate and a duplicate identifier?
12. What is a composite business key?
13. Why might repeated sensor identifiers be legitimate?
14. What is a range-validation rule?
15. Why should an unrecognized category be reviewed before mapping?
16. How do `.str.strip()` and `.str.title()` help clean categories?
17. What is one-hot encoding?
18. Why may arbitrary integer codes be misleading for nominal categories?
19. What does the mean tell us that the median does not?
20. What is the difference between `count()` and `size()` when values are missing?
21. What do standard deviation and IQR describe?
22. Why is an IQR outlier flag not enough to justify deletion?
23. What question does a histogram answer?
24. When would you use a bar chart instead of a histogram?
25. What does a scatter plot help us investigate?
26. Why does correlation not imply causation?
27. How does min–max scaling transform a numerical value?
28. What happens when the min and max of a feature are equal?
29. How is standardization different from min–max scaling?
30. Why can a previously fitted min–max transformation produce values outside 0–1?
31. What does it mean for a preprocessing workflow to be reproducible?
32. Why should you keep a copy of the raw data?
33. What is data leakage in the context of preprocessing?
34. Why must training-derived preprocessing parameters be reused on new data?
35. What does a well-documented before/after processing report contain?
36. How can data-quality findings change your interpretation of an EDA plot?

---

# 28. Self-Study Learning Resources

Use the official documentation as a reference while practicing:

1. **Pandas — Working with missing data:** https://pandas.pydata.org/docs/user_guide/missing_data.html  
   Focus on `isna()`, `dropna()`, and `fillna()`.
2. **Pandas — Duplicates:** https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop_duplicates.html  
   Focus on `subset` and `keep`.
3. **Pandas — Categorical data:** https://pandas.pydata.org/docs/user_guide/categorical.html  
   Review ordered categories and categorical types.
4. **Pandas — GroupBy:** https://pandas.pydata.org/docs/user_guide/groupby.html  
   Practice grouped `count`, `mean`, and `median`.
5. **Matplotlib — Quick start:** https://matplotlib.org/stable/users/explain/quick_start.html  
   Review figures, axes, titles, labels, legends, and plotting types.
6. **Pandas — Visualization:** https://pandas.pydata.org/docs/user_guide/visualization.html  
   Compare high-level Pandas plots with direct Matplotlib calls.
7. **scikit-learn — Common pitfalls (preview for later weeks):** https://scikit-learn.org/stable/common_pitfalls.html  
   Read the discussion of inconsistent preprocessing and data leakage. Model training is **not required** in Week 8.

Suggested study sequence:

```text
Audit → Missing values → Duplicates → Categories
      → Statistics → Distributions → Relationships
      → Basic scaling → Reproducibility
```

---

# 29. Looking Ahead

**Week 9 is the midterm examination**, drawing on the concepts introduced in Weeks 1–8.

In **Week 10**, we begin Machine Learning Workflows with scikit-learn, including:

- features versus target
- train/test separation
- baseline models
- estimator interfaces
- preprocessing pipelines
- training and inference

Week 8 is the bridge between understanding **what a dataset contains** and designing an ML workflow that can use it correctly.

```text
Week 6: DataFrames and tabular operations
             ↓
Week 7: Acquire, validate, and store records
             ↓
Week 8: Preprocess, explore, explain, and document data
             ↓
Week 9: Midterm
             ↓
Week 10: Model training and ML pipelines
```

---

## Week 8 Key Message

> **Reliable AI engineering begins by understanding the data and being able to justify every change made to it.**

Missing values, duplicates, categories, descriptive statistics, visualizations, and scaling are not isolated Pandas commands. Together, they form a disciplined approach to turning raw records into data that is easier to inspect, explain, reproduce, and eventually use in AI applications.

**End of Week 08 Lecture**
