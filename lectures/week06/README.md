# AI-216: Programming for Artificial Intelligence
## Week 06 Lecture Handout — Pandas & Data Manipulation

**Semester:** Fall 2026  
**Week:** 06  
**Theme:** From numerical arrays to labeled, tabular data for AI engineering

---

## 1. Why Week 6 Matters

In Week 4, we learned how to represent data using **lists, tuples, dictionaries, and sets**. In Week 5, we moved to **NumPy arrays** and vectorized numerical operations.

But real AI projects often begin with **tables** rather than neatly arranged numerical matrices. A table may contain:

- customer IDs and transaction amounts
- timestamps and sensor readings
- labels and prediction confidence scores
- text, categories, and numerical features together
- missing or incorrectly typed values

Writing individual loops and indexing positions for every operation becomes difficult as the dataset grows.

**Pandas** provides convenient tools for working with labeled, tabular data.

```text
Python Data Structures (Week 4)
                ↓
NumPy Arrays & Numerical Computing (Week 5)
                ↓
Pandas Series and DataFrames
                ↓
Read and Inspect Tabular Data
                ↓
Select, Filter and Transform Records
                ↓
Summarize and Group Data
                ↓
Recognize Data Quality Problems
                ↓
Reliable Data Preparation for AI Workflows
```

> **AI engineering question:** Can we take unfamiliar tabular data, understand its structure, and express the transformations we need in clear and reproducible code?

That is the focus of Week 6. We are **not training ML models yet**. We are learning how to prepare and understand the tables that later become model inputs.

---

## 2. Week 6 Learning Outcomes

By the end of this lecture, you should be able to:

1. Explain the purpose of Pandas and how it relates to NumPy.
2. Create and inspect `Series` and `DataFrame` objects.
3. Read tabular data from CSV and JSON using Pandas.
4. Inspect column names, shapes, data types, and representative records.
5. Select columns and rows using brackets, `.loc`, and `.iloc`.
6. Filter records with Boolean conditions and combine filters correctly.
7. Create or transform columns using vectorized expressions and clear names.
8. Compute overall summaries and grouped aggregations.
9. Recognize basic data-quality issues such as missing values, duplicates, invalid categories, and incorrect data types.
10. Explain how Pandas contributes to maintainable data preparation for AI applications.

---

## 3. The Pandas Mindset: Work With Labeled Data

A NumPy matrix typically identifies values by **position**: row number and column number. Pandas adds **labels**, making tables more expressive.

```text
      model_name       accuracy     latency_ms
0     baseline             0.82            90
1     tree                 0.88           120
2     linear               0.85            65

      ↑ row index          ↑ named columns
```

The two core data structures are:

| Structure | Concept | Typical use |
| --- | --- | --- |
| `pd.Series` | One-dimensional labeled sequence | A score column or label column |
| `pd.DataFrame` | Two-dimensional table with named columns | A dataset of observations/records |

We will use the conventional import throughout:

```python
import pandas as pd
```

The examples assume this import has already been executed unless an example explicitly includes it.

### 3.1 Basic Example — From a List to a Series

```python
scores = [70, 80, 90]
series = pd.Series(scores)

print(series)
```

**Key idea:** Values now have an index as well as a position.

### 3.2 Intermediate Example — Labeled Values

```python
scores = pd.Series(
    [0.81, 0.88, 0.91],
    index=["baseline", "tree", "neural"]
)

print(scores["tree"])
```

**Key idea:** A label can be more meaningful than a position.

### 3.3 Advanced Example — A Table for AI Evaluation

```python
results = pd.DataFrame({
    "model": ["baseline", "tree", "linear"],
    "accuracy": [0.82, 0.88, 0.85],
    "latency_ms": [90, 120, 65]
})

print(results[results["accuracy"] >= 0.85])
```

**Key idea:** Named columns make filtering and comparisons easier to read.

---

# 4. Pandas Series

A `Series` contains values with an associated index. A Series normally has a single dtype, and many operations work element-wise.

### 4.1 Basic Example — Creating a Series

```python
import pandas as pd

marks = pd.Series([72, 85, 91, 67], name="marks")

print(marks)
print("Mean:", marks.mean())
```

### 4.2 Intermediate Example — Index Labels

```python
marks = pd.Series(
    [72, 85, 91],
    index=["Ali", "Sara", "Ahmed"],
    name="marks"
)

print("Sara:", marks.loc["Sara"])
print("First value:", marks.iloc[0])
```

`.loc` selects by **label**; `.iloc` selects by **integer position**.

### 4.3 Advanced Example — Vectorized Series Logic

```python
accuracy = pd.Series(
    [0.79, 0.88, 0.91, 0.83],
    index=["run_a", "run_b", "run_c", "run_d"]
)

qualified = accuracy >= 0.85

print("Qualified flags:")
print(qualified)
print("Qualified runs:")
print(accuracy.loc[qualified])
```

**AI connection:** Model scores, confidence values, feature columns, and predictions are frequently handled as Series.

---

# 5. Pandas DataFrames

A `DataFrame` represents a table with named columns. Different columns can have different dtypes; for example, text in one column and numbers in another.

## 5.1 Basic Example — A Small Table

```python
students = pd.DataFrame({
    "name": ["Ali", "Sara", "Ahmed"],
    "marks": [72, 91, 65]
})

print(students)
```

## 5.2 Intermediate Example — Columns and Shape

```python
students = pd.DataFrame({
    "name": ["Ali", "Sara", "Ahmed"],
    "marks": [72, 91, 65],
    "attendance": [80, 95, 77]
})

print("Columns:", students.columns.tolist())
print("Shape:", students.shape)
print("Rows:", len(students))
```

Here, `shape` means `(number_of_rows, number_of_columns)`.

## 5.3 Advanced Example — A Prediction Log

```python
prediction_log = pd.DataFrame({
    "request_id": [101, 102, 103, 104],
    "label": ["spam", "ham", "spam", "ham"],
    "confidence": [0.96, 0.74, 0.88, 0.69],
    "latency_ms": [70, 95, 81, 130]
})

print(prediction_log.dtypes)
print("Average latency:", prediction_log["latency_ms"].mean())
```

**Key idea:** A DataFrame can hold mixed data types while still supporting vectorized column operations.

---

# 6. Creating DataFrames From Python Structures

In Week 4, we worked with lists of dictionaries. Pandas can convert that representation directly into a table.

## 6.1 Basic Example — Dictionary of Lists

```python
frame = pd.DataFrame({
    "city": ["Islamabad", "Lahore"],
    "temperature_c": [25, 31]
})

print(frame)
```

## 6.2 Intermediate Example — List of Dictionaries

```python
records = [
    {"id": 1, "label": "spam", "confidence": 0.91},
    {"id": 2, "label": "ham", "confidence": 0.83},
    {"id": 3, "label": "spam", "confidence": 0.78}
]

frame = pd.DataFrame(records)
print(frame)
```

## 6.3 Advanced Example — DataFrame From a NumPy Array

```python
import numpy as np

features = np.array([
    [21.5, 44.0, 88.0],
    [22.0, 47.0, 91.0],
    [24.5, 50.0, 86.0]
])

frame = pd.DataFrame(
    features,
    columns=["temperature_c", "humidity_pct", "signal_strength"]
)

print(frame)
```

**AI connection:** NumPy provides the numerical array; Pandas adds column names and table-oriented operations. The two are complementary, not competitors.

---

# 7. Reading Datasets With Pandas

Real data is commonly stored outside the Python script. Pandas can load tabular files, including CSV and JSON.

Typical calls are:

```python
pd.read_csv("data/measurements.csv")
pd.read_json("data/records.json")
```

These paths are illustrative: the file must exist for the call to work. For self-contained lecture demonstrations, we can use in-memory text.

## 7.1 Basic Example — Reading CSV Text

```python
from io import StringIO

csv_text = """id,score
1,72
2,85
3,91
"""

frame = pd.read_csv(StringIO(csv_text))
print(frame)
```

**Key idea:** A CSV header becomes the DataFrame's column names.

## 7.2 Intermediate Example — Reading Records From JSON

```python
from io import StringIO

json_text = """[
  {"id": 1, "label": "spam", "confidence": 0.91},
  {"id": 2, "label": "ham", "confidence": 0.83}
]"""

frame = pd.read_json(StringIO(json_text), orient="records")
print(frame)
```

JSON is not always a simple table; nested JSON may require additional normalization. Deeper ingestion techniques come in Week 7.

## 7.3 Advanced Example — Read With Explicit Data Types

```python
from io import StringIO

csv_text = """customer_id,amount,is_priority
0012,1250.5,True
0035,800.0,False
"""

orders = pd.read_csv(
    StringIO(csv_text),
    dtype={"customer_id": "string"}
)

print(orders)
print(orders.dtypes)
```

**Why does this matter?** An identifier such as `0012` is not a numerical quantity. Treating it as an integer would discard meaningful leading zeros.

---

# 8. First Look at an Unfamiliar Dataset

Before transforming any dataset, understand what you have.

Useful methods and attributes:

| Pandas API | Purpose |
| --- | --- |
| `df.head()` | First rows |
| `df.tail()` | Last rows |
| `df.shape` | Number of rows and columns |
| `df.columns` | Column names |
| `df.dtypes` | Column data types |
| `df.info()` | Non-null counts and types |
| `df.describe()` | Basic summary statistics for numeric columns by default |
| `df.sample(n=..., random_state=...)` | Random sample of rows |

## 8.1 Basic Example — First Rows

```python
frame = pd.DataFrame({
    "id": [1, 2, 3, 4],
    "score": [72, 88, 63, 91]
})

print(frame.head(2))
print("Shape:", frame.shape)
```

## 8.2 Intermediate Example — Inspect Types

```python
frame = pd.DataFrame({
    "id": [101, 102, 103],
    "city": ["Islamabad", "Lahore", "Peshawar"],
    "sales": [1250.0, 980.0, 1140.0]
})

print(frame.dtypes)
frame.info()
```

`info()` prints its summary to the console; it does not return a DataFrame.

## 8.3 Advanced Example — Investigate Numeric Distributions

```python
frame = pd.DataFrame({
    "confidence": [0.91, 0.82, 0.74, 0.95, 0.67],
    "latency_ms": [68, 91, 135, 73, 210],
    "label": ["spam", "ham", "spam", "ham", "spam"]
})

print(frame.describe())
print("Category counts:")
print(frame["label"].value_counts())
```

**Key idea:** Inspect first. A transformation based on an incorrect assumption may quietly corrupt your analysis.

---

# 9. Selecting Columns

In Pandas, a single column usually gives a `Series`, while multiple columns give a `DataFrame`.

## 9.1 Basic Example — One Column

```python
frame = pd.DataFrame({
    "name": ["Ali", "Sara"],
    "score": [72, 91]
})

scores = frame["score"]
print(type(scores))
```

## 9.2 Intermediate Example — Several Columns

```python
frame = pd.DataFrame({
    "name": ["Ali", "Sara"],
    "score": [72, 91],
    "attendance": [85, 96]
})

selected = frame[["name", "score"]]
print(selected)
```

Note the **double brackets**: the inner brackets create a list of requested column names.

## 9.3 Advanced Example — Select AI Features, Exclude Labels

```python
frame = pd.DataFrame({
    "request_id": [101, 102, 103],
    "message_length": [120, 45, 220],
    "link_count": [2, 0, 4],
    "is_spam": [True, False, True]
})

feature_columns = ["message_length", "link_count"]
X = frame[feature_columns]
y = frame["is_spam"]

print("Features shape:", X.shape)
print("Target length:", len(y))
```

**AI connection:** Later, machine-learning workflows will separate **features** (`X`) from a **target** (`y`). This example introduces the data representation, not model training.

---

# 10. Selecting Rows: `.loc` and `.iloc`

A common source of confusion is mixing row **labels** with row **positions**.

- `.loc` works with index labels and column labels.
- `.iloc` works with integer positions.

## 10.1 Basic Example — Position-Based Selection

```python
frame = pd.DataFrame({
    "name": ["Ali", "Sara", "Ahmed"],
    "score": [72, 91, 65]
})

print(frame.iloc[0])
print(frame.iloc[0:2])
```

`.iloc[0:2]` returns positions `0` and `1`; position `2` is excluded.

## 10.2 Intermediate Example — Label-Based Selection

```python
frame = pd.DataFrame(
    {"name": ["Ali", "Sara", "Ahmed"], "score": [72, 91, 65]},
    index=["s01", "s02", "s03"]
)

print(frame.loc["s02", "score"])
print(frame.loc["s01":"s02", ["name", "score"]])
```

Unlike positional slicing, a `.loc` label slice includes **both** endpoints when the labels exist in order.

## 10.3 Advanced Example — Choose Rows and Columns Together

```python
frame = pd.DataFrame({
    "run_id": ["a", "b", "c", "d"],
    "accuracy": [0.82, 0.91, 0.86, 0.78],
    "latency_ms": [70, 120, 90, 60]
})

mask = frame["accuracy"] >= 0.85
selected = frame.loc[mask, ["run_id", "accuracy"]]

print(selected)
```

**Key idea:** `.loc[rows, columns]` is particularly useful when filtering and selecting specific fields in one operation.

---

# 11. Filtering Rows With Conditions

A Boolean mask is a sequence of `True` and `False` values identifying which rows meet a condition.

## 11.1 Basic Example — One Condition

```python
frame = pd.DataFrame({"score": [45, 70, 88, 39]})

passed = frame[frame["score"] >= 50]
print(passed)
```

## 11.2 Intermediate Example — Multiple Conditions

```python
frame = pd.DataFrame({
    "name": ["Ali", "Sara", "Ahmed", "Fatima"],
    "score": [72, 91, 65, 88],
    "attendance": [82, 93, 70, 78]
})

eligible = frame.loc[
    (frame["score"] >= 70) & (frame["attendance"] >= 75)
]

print(eligible)
```

Pandas uses `&` for element-wise AND and `|` for element-wise OR. Put each comparison in parentheses; Python's `and` and `or` do not work with a Series of Booleans.

## 11.3 Advanced Example — Filter by Categories and Numeric Rules

```python
frame = pd.DataFrame({
    "request_id": [101, 102, 103, 104],
    "label": ["spam", "ham", "unknown", "spam"],
    "confidence": [0.94, 0.76, 0.44, 0.89]
})

selected = frame.loc[
    frame["label"].isin(["spam", "ham"])
    & (frame["confidence"] >= 0.80)
]

print(selected)
```

**AI connection:** Filtering is useful when selecting valid records or reviewing a subset of model outputs. Filtering does not automatically make data representative or unbiased; the reason for excluding records matters.

---

# 12. Indexes: Labels, Not Necessarily Row Numbers

The DataFrame's index is its set of row labels. It need not be consecutive integers, and it is not always a unique identifier.

## 12.1 Basic Example — Default Index

```python
frame = pd.DataFrame({"score": [70, 85, 90]})
print(frame.index)
```

## 12.2 Intermediate Example — Set a Meaningful Index

```python
frame = pd.DataFrame({
    "student_id": [101, 102, 103],
    "score": [70, 85, 90]
})

indexed = frame.set_index("student_id")
print(indexed.loc[102, "score"])
```

`set_index()` returns a new DataFrame by default.

## 12.3 Advanced Example — Filter, Then Reset the Index

```python
frame = pd.DataFrame({
    "request_id": [11, 12, 13, 14],
    "confidence": [0.92, 0.61, 0.87, 0.54]
})

reviewed = frame.loc[frame["confidence"] >= 0.80].copy()
reviewed = reviewed.reset_index(drop=True)

print(reviewed)
```

**Key idea:** Filtering normally preserves original row labels. Resetting them is optional and depends on what the rest of the program expects.

---

# 13. Understanding Data Types (`dtypes`)

Data types affect which operations are possible and whether calculations are meaningful. A column that *looks* numeric might actually contain text.

## 13.1 Basic Example — Inspect Types

```python
frame = pd.DataFrame({
    "age": [19, 20, 21],
    "score": [0.82, 0.91, 0.88],
    "name": ["Ali", "Sara", "Ahmed"]
})

print(frame.dtypes)
```

## 13.2 Intermediate Example — Convert Numeric Text

```python
frame = pd.DataFrame({"amount": ["100", "250", "75"]})

frame["amount"] = pd.to_numeric(frame["amount"])
print(frame["amount"].sum())
```

## 13.3 Advanced Example — Detect Invalid Numeric Values

```python
frame = pd.DataFrame({
    "transaction_id": [101, 102, 103, 104],
    "amount_text": ["1200", "850", "unknown", "740"]
})

frame["amount"] = pd.to_numeric(
    frame["amount_text"],
    errors="coerce"
)

invalid_amounts = frame.loc[frame["amount"].isna()]

print(frame)
print("Invalid rows:")
print(invalid_amounts[["transaction_id", "amount_text"]])
```

`errors="coerce"` converts unparseable values to missing values; it **does not repair** the underlying source. Record or investigate unexpected conversions before making decisions.

---

# 14. Basic Data-Quality Inspection: Missing Values

Missing values may appear as `NaN`, `pd.NA`, or other missing markers depending on dtype and source. A blank field is not the same as zero.

Useful methods (assuming the table is named `df`):

```python
# df.isna()        # Missing-value mask
# df.isna().sum()  # Missing values per column
# df.notna()       # Non-missing-value mask
```

## 14.1 Basic Example — Find Missing Values

```python
frame = pd.DataFrame({
    "temperature_c": [25.0, None, 27.5]
})

print(frame["temperature_c"].isna())
```

## 14.2 Intermediate Example — Count Missing Values by Column

```python
frame = pd.DataFrame({
    "sensor_id": [1, 2, 3, 4],
    "temperature_c": [24.5, None, 26.0, 25.5],
    "humidity_pct": [45.0, 48.0, None, 50.0]
})

print(frame.isna().sum())
```

## 14.3 Advanced Example — Inspect Records Missing Critical Fields

```python
frame = pd.DataFrame({
    "request_id": [101, 102, 103, 104],
    "label": ["spam", None, "ham", "spam"],
    "confidence": [0.91, 0.79, None, 0.88]
})

critical_missing = frame[["label", "confidence"]].isna().any(axis=1)

print("Rows requiring review:")
print(frame.loc[critical_missing])
```

Week 8 will examine **how** to handle missing data, including the risks of dropping records or filling values. Week 6 focuses on **detecting and describing** the issue first.

---

# 15. Basic Data-Quality Inspection: Duplicates and Invalid Values

A duplicated row is not always an error, and duplicate IDs do not always mean duplicate events. You must understand the meaning of each record.

## 15.1 Basic Example — Exact Duplicate Rows

```python
frame = pd.DataFrame({
    "id": [1, 2, 2],
    "label": ["spam", "ham", "ham"]
})

print(frame.duplicated())
print("Duplicate rows:", frame.duplicated().sum())
```

## 15.2 Intermediate Example — Duplicate Business Keys

```python
frame = pd.DataFrame({
    "transaction_id": [201, 202, 202, 203],
    "amount": [150, 220, 225, 180]
})

suspected = frame.loc[
    frame.duplicated(subset=["transaction_id"], keep=False)
]

print(suspected)
```

Both rows with ID `202` are shown, which helps investigation.

## 15.3 Advanced Example — Detect Invalid Ranges and Labels

```python
frame = pd.DataFrame({
    "request_id": [101, 102, 103, 104],
    "label": ["spam", "ham", "other", "spam"],
    "confidence": [0.95, 1.20, 0.72, -0.10]
})

allowed_labels = {"spam", "ham"}

invalid = (
    ~frame["label"].isin(allowed_labels)
    | ~frame["confidence"].between(0, 1)
)

print(frame.loc[invalid])
```

**Engineering lesson:** Validation rules should reflect the domain. Confidence values outside `[0, 1]` are invalid **if** the column is intended to be a probability or normalized confidence score.

---

# 16. Column Transformations and Vectorized Operations

Like NumPy, Pandas supports element-wise operations. When a formula can be expressed across a column, a vectorized expression is often clearer than an explicit loop.

## 16.1 Basic Example — Convert Units

```python
frame = pd.DataFrame({"temperature_c": [0.0, 20.0, 30.0]})

frame["temperature_f"] = frame["temperature_c"] * 9 / 5 + 32

print(frame)
```

## 16.2 Intermediate Example — Calculate a Derived Feature

```python
frame = pd.DataFrame({
    "price": [1000, 1500, 800],
    "quantity": [2, 1, 4]
})

frame["line_total"] = frame["price"] * frame["quantity"]

print(frame)
```

## 16.3 Advanced Example — Prediction Review Flag

```python
frame = pd.DataFrame({
    "request_id": [101, 102, 103, 104],
    "confidence": [0.94, 0.63, 0.87, 0.55],
    "latency_ms": [80, 120, 240, 310]
})

MIN_CONFIDENCE = 0.70
MAX_LATENCY_MS = 250

frame["needs_review"] = (
    (frame["confidence"] < MIN_CONFIDENCE)
    | (frame["latency_ms"] > MAX_LATENCY_MS)
)

print(frame)
```

This is an **illustrative business rule**, not a universal quality standard for AI applications.

---

# 17. Transforming Categories and Text

Not every column is numerical. Many datasets contain strings, categories, and values that need consistent representation.

## 17.1 Basic Example — Normalize Case

```python
frame = pd.DataFrame({"label": ["Spam", "HAM", "spam"]})

frame["label_clean"] = frame["label"].str.lower()
print(frame)
```

## 17.2 Intermediate Example — Remove Extra Spaces

```python
frame = pd.DataFrame({
    "city": [" Islamabad ", "lahore", " KARACHI "]
})

frame["city_clean"] = frame["city"].str.strip().str.title()
print(frame)
```

## 17.3 Advanced Example — Map Codes to Descriptions

```python
frame = pd.DataFrame({
    "request_id": [101, 102, 103, 104],
    "status_code": [200, 422, 500, 200]
})

status_names = {
    200: "success",
    422: "invalid_request",
    500: "server_error"
}

frame["status"] = frame["status_code"].map(status_names)

print(frame)
```

`map()` returns a missing value for unmatched keys by default. If codes outside your mapping are possible, check for them rather than assuming every mapping succeeded.

---

# 18. `assign()`, `.copy()`, and Clear Transformation Steps

Some workflows benefit from explicitly creating a new DataFrame rather than changing the original.

## 18.1 Basic Example — Independent Copy

```python
raw = pd.DataFrame({"score": [70, 80, 90]})
processed = raw.copy()

processed["score"] = processed["score"] + 5

print("Raw:")
print(raw)
print("Processed:")
print(processed)
```

## 18.2 Intermediate Example — `assign()`

```python
raw = pd.DataFrame({
    "price": [100, 150],
    "quantity": [2, 3]
})

with_total = raw.assign(
    line_total=lambda df: df["price"] * df["quantity"]
)

print(with_total)
```

`assign()` returns a new DataFrame with the requested columns.

## 18.3 Advanced Example — Filter and Transform Safely

```python
raw = pd.DataFrame({
    "request_id": [101, 102, 103],
    "confidence": [0.90, 0.60, 0.82],
    "latency_ms": [80, 120, 95]
})

selected = raw.loc[raw["confidence"] >= 0.80].copy()
selected["latency_seconds"] = selected["latency_ms"] / 1000

print(selected)
print("Original columns:", raw.columns.tolist())
```

**Key idea:** Filtering and transforming through explicit copies and assignments can make data flow easier to reason about. Avoid ambiguous chained assignment such as `df[df["score"] > 50]["status"] = ...`.

---

# 19. Sorting and Ranking Records

Sorting helps identify extreme values, inspect outcomes, and present reports in a useful order.

## 19.1 Basic Example — Sort One Column

```python
frame = pd.DataFrame({"score": [72, 91, 65]})
print(frame.sort_values("score"))
```

## 19.2 Intermediate Example — Sort Descending

```python
frame = pd.DataFrame({
    "model": ["baseline", "tree", "linear"],
    "accuracy": [0.82, 0.91, 0.86]
})

ranked = frame.sort_values("accuracy", ascending=False)
print(ranked)
```

## 19.3 Advanced Example — Sort by Two Criteria

```python
frame = pd.DataFrame({
    "model": ["a", "b", "c", "d"],
    "accuracy": [0.88, 0.88, 0.91, 0.88],
    "latency_ms": [120, 65, 180, 90]
})

ranked = frame.sort_values(
    by=["accuracy", "latency_ms"],
    ascending=[False, True]
)

print(ranked)
```

**Key idea:** Sorting criteria must reflect the decision you intend to make. Highest accuracy does not always mean the best operational model.

---

# 20. Overall Aggregation and Summary Statistics

Aggregation reduces many values to summary numbers. Examples include total, average, minimum, maximum, and count.

## 20.1 Basic Example — Average

```python
frame = pd.DataFrame({"latency_ms": [80, 120, 100]})

print("Mean latency:", frame["latency_ms"].mean())
```

## 20.2 Intermediate Example — Several Metrics

```python
frame = pd.DataFrame({"sales": [1000, 1250, 950, 1400]})

summary = frame["sales"].agg(["count", "sum", "mean", "min", "max"])
print(summary)
```

## 20.3 Advanced Example — Multiple Columns, Different Statistics

```python
frame = pd.DataFrame({
    "confidence": [0.91, 0.84, 0.73, 0.95],
    "latency_ms": [80, 110, 240, 95]
})

summary = frame.agg({
    "confidence": ["mean", "min", "max"],
    "latency_ms": ["mean", "min", "max"]
})

print(summary)
```

Notice the distinction between **row-level data** and **summary-level data**. A single mean can hide meaningful differences among records.

---

# 21. Grouping Data With `groupby()`

A common analytical question is:

> How does a metric differ **between categories**?

Pandas `groupby()` is useful for split–apply–combine operations:

```text
Split records into groups
          ↓
Apply a calculation to each group
          ↓
Combine the group summaries
```

## 21.1 Basic Example — Group Mean

```python
frame = pd.DataFrame({
    "department": ["AI", "CS", "AI", "CS"],
    "score": [80, 70, 90, 86]
})

print(frame.groupby("department")["score"].mean())
```

## 21.2 Intermediate Example — Counts by Category

```python
frame = pd.DataFrame({
    "label": ["spam", "ham", "spam", "spam", "ham"],
    "confidence": [0.94, 0.81, 0.88, 0.75, 0.86]
})

summary = frame.groupby("label")["confidence"].agg(
    ["count", "mean", "min", "max"]
)

print(summary)
```

## 21.3 Advanced Example — Named Grouped Aggregations

```python
frame = pd.DataFrame({
    "model": ["a", "b", "a", "b", "a"],
    "latency_ms": [80, 120, 95, 110, 70],
    "confidence": [0.91, 0.83, 0.89, 0.79, 0.94]
})

summary = frame.groupby("model", as_index=False).agg(
    requests=("latency_ms", "size"),
    avg_latency_ms=("latency_ms", "mean"),
    avg_confidence=("confidence", "mean"),
    max_latency_ms=("latency_ms", "max")
)

print(summary)
```

**Key idea:** Named aggregations make the output easier to understand and use in later processing.

---

# 22. Category Frequencies With `value_counts()`

For a categorical column, one of the first questions is how often each value occurs.

## 22.1 Basic Example — Count Labels

```python
labels = pd.Series(["spam", "ham", "spam", "spam"])
print(labels.value_counts())
```

## 22.2 Intermediate Example — Fractions

```python
frame = pd.DataFrame({
    "label": ["spam", "ham", "spam", "ham", "spam"]
})

print(frame["label"].value_counts(normalize=True))
```

`normalize=True` returns proportions, not raw counts.

## 22.3 Advanced Example — Include Missing Categories

```python
frame = pd.DataFrame({
    "status": ["success", None, "error", "success", None]
})

print(frame["status"].value_counts(dropna=False))
print("Distinct non-missing statuses:", frame["status"].nunique())
```

**AI connection:** Category counts can reveal unexpected labels, uneven label frequencies, or missing classes. Understanding whether those patterns matter requires domain context and later evaluation concepts.

---

# 23. Pandas vs NumPy: When to Use Which?

Pandas and NumPy are designed for related but different tasks.

| Question | NumPy often fits well | Pandas often fits well |
| --- | --- | --- |
| Is the data mainly numerical with uniform types? | Yes | Sometimes |
| Are column names and row labels important? | Not central | Yes |
| Do we need numerical matrix operations? | Yes | Can interoperate |
| Are there mixed text/numeric columns? | Less convenient | Yes |
| Do we need grouped tabular summaries? | Possible with extra work | Very convenient |

## 23.1 Basic Example — NumPy Array

```python
import numpy as np

scores = np.array([70, 80, 90])
print(scores.mean())
```

## 23.2 Intermediate Example — Pandas Series

```python
scores = pd.Series([70, 80, 90], name="score")
print(scores.mean())
```

## 23.3 Advanced Example — Move From a DataFrame to a Matrix

```python
frame = pd.DataFrame({
    "age": [20, 25, 30],
    "monthly_spending": [1000, 1500, 1200],
    "segment": ["A", "B", "A"]
})

feature_columns = ["age", "monthly_spending"]
feature_matrix = frame[feature_columns].to_numpy(dtype=float)

print(feature_matrix)
print("Matrix shape:", feature_matrix.shape)
```

**AI connection:** Pandas is useful for organizing and inspecting a table. NumPy is useful for numerical computation on a selected matrix. Later, scikit-learn workflows will accept and work with these representations.

---

# 24. Integrated AI-Oriented Example — Inspecting Inference Logs

Imagine an AI service records requests, model predictions, confidence scores, and response times. An AI engineer wants to **inspect the data**, **identify suspicious records**, and **summarize basic service behavior**.

This is a conceptual walkthrough of the kinds of transformations a data-oriented AI application needs. It is not a Lab 06 submission task.

## 24.1 Start With a Small Raw Dataset

```python
import pandas as pd

logs = pd.DataFrame({
    "request_id": [1001, 1002, 1003, 1004, 1005, 1006, 1006],
    "model": ["v1", "v1", "v2", "v2", "v1", "v2", "v2"],
    "label": ["spam", "ham", "spam", "HAM", "unknown", "spam", "spam"],
    "confidence": [0.96, 0.83, 0.89, 0.72, None, 1.25, 1.25],
    "latency_ms": [70, 105, 88, 130, 95, 160, 160]
})

print(logs.head())
print("Shape:", logs.shape)
print(logs.dtypes)
```

The dataset intentionally contains problems:

- a missing confidence
- an out-of-range confidence
- inconsistent label capitalization
- an unexpected label
- a duplicated request ID

## 24.2 Inspect Quality Before Changing Anything

```python
print("Missing values:")
print(logs.isna().sum())

print("Duplicate IDs:")
print(logs.duplicated(subset=["request_id"]).sum())

print("Raw labels:")
print(logs["label"].value_counts(dropna=False))
```

Do not assume every duplicate is accidental; a real system might log legitimate retries. Here, we are identifying records that merit closer examination.

## 24.3 Normalize Categories and Identify Invalid Rows

```python
reviewed = logs.copy()
reviewed["label_clean"] = reviewed["label"].str.strip().str.lower()

allowed_labels = {"spam", "ham"}

reviewed["invalid_label"] = ~reviewed["label_clean"].isin(allowed_labels)
reviewed["invalid_confidence"] = (
    reviewed["confidence"].isna()
    | ~reviewed["confidence"].between(0, 1)
)

reviewed["duplicate_request_id"] = reviewed.duplicated(
    subset=["request_id"], keep=False
)

reviewed["needs_review"] = (
    reviewed["invalid_label"]
    | reviewed["invalid_confidence"]
    | reviewed["duplicate_request_id"]
)

print(reviewed.loc[
    reviewed["needs_review"],
    ["request_id", "label", "confidence", "needs_review"]
])
```

Rather than silently deleting questionable records, this example marks them for review.

## 24.4 Create a Valid Subset for an Illustrative Summary

```python
valid = reviewed.loc[~reviewed["needs_review"]].copy()

valid["latency_seconds"] = valid["latency_ms"] / 1000

print(valid[[
    "request_id", "model", "label_clean", "confidence", "latency_seconds"
]])
```

Here, “valid” means **passes these illustrative checks**. Real validation may require more rules and domain knowledge.

## 24.5 Group the Results

```python
if valid.empty:
    print("No valid requests available for summary")
else:
    summary = valid.groupby("model", as_index=False).agg(
        requests=("request_id", "size"),
        mean_confidence=("confidence", "mean"),
        mean_latency_ms=("latency_ms", "mean")
    )

    print(summary)
```

For these sample records, only requests `1001`–`1004` pass the illustrative checks.
The resulting grouped summaries are:

| Model | Valid requests | Mean confidence | Mean latency |
| --- | ---: | ---: | ---: |
| `v1` | 2 | 0.895 | 87.5 ms |
| `v2` | 2 | 0.805 | 109.0 ms |

These figures summarize the **selected records**, not the quality of the models overall.

## 24.6 What Have We Learned?

```text
Raw Inference Records
          ↓
Inspect Structure and Types
          ↓
Check Missing / Duplicate / Out-of-Range Values
          ↓
Normalize Category Representation
          ↓
Flag Questionable Records
          ↓
Select Records for a Specific Summary
          ↓
Group and Aggregate
          ↓
Interpretable Summary of Service Activity
```

This workflow is a foundation for more advanced topics later:

- Week 7: data ingestion from files, APIs, and databases
- Week 8: systematic preprocessing and EDA
- Week 10: ML workflows using scikit-learn
- Week 13: prediction services and APIs

---

# 25. Common Mistakes and Engineering Pitfalls

## 25.1 Mixing Up `Series` and `DataFrame`

```python
single = df["score"]       # Series
multiple = df[["score"]]   # DataFrame
```

The extra brackets change the result type.

## 25.2 Confusing `.loc` and `.iloc`

```python
# .loc -> labels
# .iloc -> integer positions
```

A row label of `10` does not necessarily mean the eleventh row.

## 25.3 Using `and` or `or` With a Series

Incorrect:

```python
# (df["score"] > 50) and (df["attendance"] > 75)
```

Correct:

```python
(df["score"] > 50) & (df["attendance"] > 75)
```

## 25.4 Forgetting Parentheses in Filters

Each comparison should normally be wrapped in parentheses when using `&` or `|`.

## 25.5 Treating Missing Values as Zero

Missing is not automatically zero. Filling values requires a reason and may change the meaning of the dataset.

## 25.6 Assuming All Numeric-Looking Values Are Numeric

```python
frame = pd.DataFrame({"amount": ["100", "250", "75"]})
print(frame.dtypes)
```

Read the dtype before computing a sum or average.

## 25.7 Silently Discarding Invalid Values

```python
pd.to_numeric(series, errors="coerce")
```

makes invalid values missing, which may be useful for inspection, but does not explain *why* they were invalid.

## 25.8 Chained Assignment

Avoid ambiguous code like:

```python
# df[df["score"] > 50]["status"] = "pass"
```

Prefer clear intent:

```python
df.loc[df["score"] > 50, "status"] = "pass"
```

If you need a filtered, independent table, use `.copy()`.

## 25.9 Averaging the Wrong Columns or Rows

Averaging a numeric score column is different from averaging every numeric field in a table. Know which dimensions your calculation uses.

## 25.10 Assuming Duplicated IDs Mean Duplicated Events

Investigate row meaning, retries, source systems, and identifiers before dropping records.

## 25.11 Grouping Without Understanding Missing Categories

`groupby()` excludes missing group keys by default. A group summary may therefore not account for all source records.

## 25.12 Mistaking Correlation or Summary for Model Quality

Averages and category counts are **descriptive summaries**. They do not establish whether an AI system is accurate, fair, calibrated, or production-ready.

---

# 26. A Practical Mental Model for Pandas

Before writing an operation, identify its level:

| Level | Typical question | Example |
| --- | --- | --- |
| Cell | What is this one value? | `.loc[row_label, "score"]` |
| Column | What transformation applies to this field? | `df["amount"] * 1.05` |
| Row subset | Which records meet a rule? | `df.loc[df["score"] >= 70]` |
| Whole table | What is the structure? | `df.shape`, `df.info()` |
| Group | How does a metric vary by category? | `df.groupby("model")["latency_ms"].mean()` |

Keep asking:

> **Am I selecting data, changing its representation, or summarizing it?**

These are different operations and should be understandable in the code.

---

# 27. Thinking Like an AI Engineer

### Data Representation

- Is this an identifier, a category, a measurement, or a target?
- Are the column names meaningful?
- Does the dtype reflect the intended meaning?

### Data Inspection

- How many rows and columns do I have?
- Which values are missing?
- Are there duplicates or inconsistent categories?
- Do the values satisfy expected ranges?

### Transformation

- What is the purpose of the new column?
- Could this transformation change the source data unexpectedly?
- Can I express the operation clearly using a vectorized expression?

### Summarization

- Does the average hide variation?
- Are missing records being excluded?
- Does each row represent an independent event, or can it be a retry?

### Maintainability

- Can another engineer understand which records were retained or flagged?
- Are assumptions recorded in code or documentation?
- Could the same transformation be reproduced when new data arrives?

The goal is to go beyond:

> “I can read a CSV and print a few rows.”

Toward:

> **“I can explain what this table contains, assess its basic quality, transform it deliberately, and summarize it without hiding important assumptions.”**

---

# 28. Responsible Use of AI Tools

AI assistants can help with learning Pandas and interpreting data operations. Useful prompts include:

- “Do not write a complete solution. Explain what a Pandas Series is compared with a DataFrame.”
- “Why does this filter require `&` instead of `and`?”
- “Trace what `.loc` selects from this small table.”
- “Explain the difference between `.loc` and `.iloc` using my example.”
- “Suggest data-quality questions I should ask before changing this dataset.”
- “Why might converting an ID column to integer be dangerous?”
- “Explain what each part of this `groupby().agg()` expression returns.”
- “Is this transformation safe, or might it change my original DataFrame?”
- “Give me edge cases for missing values and duplicated IDs without writing the final code.”
- “Explain a Pandas traceback and how I should debug it.”

AI-generated analyses can invent columns, assume incorrect data types, and overlook missing or duplicated records. Verify every suggestion using the actual data and the relevant Pandas documentation.

> **You remain responsible for understanding every operation and explaining why a result is correct.**

---

# 29. Self-Check Questions

Before moving to the Week 6 lab, make sure you can answer:

1. How does Pandas complement NumPy?
2. What is a `Series`?
3. What is a `DataFrame`?
4. What does `df.shape` tell us?
5. What is the difference between `df["score"]` and `df[["score"]]`?
6. What does `df.info()` show?
7. What does `df.describe()` summarize by default?
8. How does `pd.read_csv()` use column headers?
9. Why might an ID such as `0012` need to remain a string?
10. What is the difference between `.loc` and `.iloc`?
11. Does `.loc` include the endpoint of a label-based slice?
12. How do you select rows meeting two conditions?
13. Why should you not use `and` with Boolean Series?
14. What does `.isin()` do?
15. What does `pd.to_numeric(..., errors="coerce")` do with invalid values?
16. How do you count missing values per column?
17. What is the difference between detecting and removing duplicates?
18. How can you inspect records with out-of-range confidence values?
19. What is a vectorized column transformation?
20. What does `.str.strip().str.lower()` do?
21. What does `sort_values()` do?
22. What is the difference between `mean()` and `count()`?
23. What does `groupby()` help you express?
24. How can `value_counts()` reveal unexpected categories?
25. Why should we be careful about a filtered DataFrame and assignments?
26. When might we convert selected DataFrame columns to a NumPy array?
27. Why can an average hide important differences between groups?
28. Why should the reason for excluding records be documented?
29. How does data-quality inspection support later machine-learning work?
30. What separates a reproducible data transformation from an unexplained one-off change?

---

# 30. Looking Ahead

In **Week 7**, we move beyond reading a ready-made table and study **how data arrives**:

- CSV and JSON files
- APIs and HTTP basics
- introductory web scraping
- SQLite and SQL
- ethical data collection
- simple data pipelines

In **Week 8**, we will go deeper into preprocessing, missing-value handling, duplicate records, exploratory data analysis, and visualization.

```text
Week 5 — Numerical arrays and vectorization
   ↓
Week 6 — Labeled tables, manipulation and inspection
   ↓
Week 7 — Ingesting data from multiple sources
   ↓
Week 8 — Preprocessing and exploratory analysis
   ↓
Week 10 — Model workflows and inference
```

---

## Week 6 Key Message

> **The first step toward a useful AI system is understanding the data you have—not just running a model on it.**

Pandas helps us turn raw tabular records into well-understood, intentionally transformed data that other parts of an AI application can use.
