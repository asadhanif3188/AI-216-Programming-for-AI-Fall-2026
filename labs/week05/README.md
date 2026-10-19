# AI-216: Programming for Artificial Intelligence
## Lab 05 — NumPy & Numerical Computing for AI

**Semester:** Fall 2026  
**Week:** 05  
**Topic:** NumPy arrays, shapes, indexing, vectorization, broadcasting, numerical operations, and feature preparation

---

## 1. Objective

In Week 4, you used Python lists, tuples, dictionaries, and sets to organize information. The Week 5 lecture introduces **NumPy** to perform numerical operations on entire collections of values.

This lab takes you from Python-list calculations to **clear, vectorized operations on numerical arrays**. The objective is not to remove every loop; it is to use the representation and numerical operation best suited to the problem.

By the end of this lab, you should be able to:

1. Create and inspect one-dimensional and multidimensional NumPy arrays.
2. Explain how NumPy array operations differ from Python list operations.
3. Read and reason about `shape`, `ndim`, `size`, and `dtype`.
4. Select array values, rows, and columns using indexing and slicing.
5. Distinguish views, aliases, and independent copies.
6. Replace appropriate Python loops with vectorized NumPy expressions.
7. Apply broadcasting correctly and recognize incompatible shapes.
8. Compute row-wise and column-wise numerical summaries using `axis`.
9. Use Boolean masks and `np.where()` for validation and classification.
10. Apply safe numerical transformations, including feature scaling.
11. Use `@` for a simple feature-weight calculation.
12. Build and document a small NumPy-based **sensor feature preparation workflow**.

**Library:** NumPy is required. No Pandas, scikit-learn, or machine-learning framework is needed.

---

## 2. Before You Start

Your Python and Git/GitHub environment from previous weeks should already work.

Verify Python:

```bash
python --version
```

Verify NumPy:

```bash
python -c "import numpy as np; print(np.__version__)"
```

If NumPy is not installed, install it in the Python environment you are using:

```bash
python -m pip install numpy
```

On systems that use `python3` instead of `python`, substitute `python3` in these commands.

### Import Convention

In every Python file that uses NumPy, begin with:

```python
import numpy as np
```

The alias `np` is standard and will be used throughout AI-216.

### Your Week 5 Folder

In your **personal coursework repository**, create:

```text
labs/week05/
├── task01_array_basics.py
├── task02_indexing_shapes.py
├── task03_views_copies.py
├── task04_vectorization.py
├── task05_broadcasting_aggregation.py
├── task06_masks_conditions.py
├── task07_feature_calculations.py
├── task08_sensor_pipeline/
│   ├── main.py
│   └── sensor_utils.py
└── README.md
```

Optional work may be stored as:

```text
labs/week05/optional_benchmark.py
```

Run a file from your repository root, for example:

```bash
python labs/week05/task01_array_basics.py
```

> **Important:** This document is the instructor's lab handout. `labs/week05/README.md` is the documentation **you create** for your own solutions.

---

## 3. General Coding Requirements

For every task:

1. Identify what each array represents: samples, features, pixels, scores, or predictions.
2. State or inspect the **expected shape** before performing an operation.
3. Use descriptive names such as `feature_matrix` or `confidence_scores`.
4. Use NumPy array operations where the task explicitly asks for vectorization.
5. Do not accidentally change source arrays through views or aliases.
6. Use meaningful messages and readable numerical formatting.
7. Test at least one normal case and any relevant edge or boundary case.
8. Explain results rather than only pasting console output.
9. Keep reusable calculations inside functions when appropriate.
10. Be prepared to explain the output and the shapes of intermediate arrays.

Use `np.allclose(...)` when comparing arrays of floating-point results, rather than relying on exact equality. Do not use `np.allclose` as a replacement for checking shapes or business rules.

---

# 4. Task 1 — From Python Lists to NumPy Arrays

**Level:** Basic  
**File:** `task01_array_basics.py`

### Problem

An AI experiment produced scores:

```python
scores_list = [65, 75, 85, 95]
```

Create a NumPy array containing the same values.

### Part A — List Versus Array

Demonstrate and explain the difference between:

```python
scores_list + scores_list
```

and:

```python
scores_array + scores_array
```

Then add `5` to every score using NumPy without writing a loop.

### Part B — Array Information

Print:

- array values
- Python type of the array
- `shape`
- `ndim`
- `size`
- `dtype`

### Part C — Creating Arrays

Create and print:

1. An array containing five zeros using `np.zeros()`.
2. An array containing five ones using `np.ones()`.
3. Even numbers from `2` to `10` inclusive using `np.arange()`.
4. Five equally spaced values from `0` to `1` using `np.linspace()`.

### Thinking Questions

- Why does `+` behave differently for lists and NumPy arrays?
- What does the shape `(4,)` tell you?
- When might a Python list still be preferable?

**Concepts:** `np.array`, array creation, list vs array, attributes, element-wise arithmetic

---

# 5. Task 2 — Shapes, Reshaping, Indexing & Slicing

**Level:** Basic → Intermediate  
**File:** `task02_indexing_shapes.py`

### Problem

Each row below represents one observation. The columns contain:

```text
[temperature_C, humidity_percent, vibration_score]
```

Use:

```python
sensor_features = np.array([
    [20.0, 45.0, 0.30],
    [24.0, 52.0, 0.55],
    [27.0, 61.0, 0.70],
    [30.0, 72.0, 0.85]
])
```

### Part A — Inspect the Matrix

Print its `shape`, `ndim`, `size`, and `dtype`. Explain what each axis represents.

### Part B — Indexing and Slicing

Use NumPy indexing or slicing to select:

1. The first observation.
2. The vibration score of the final observation.
3. **All temperature values** as a one-dimensional array.
4. All humidity values as a **two-dimensional column**.
5. The first three observations.
6. The last two feature columns of all observations.

### Part C — Reshaping

Create:

```python
values = np.arange(1, 13)
```

Reshape it to a `(4, 3)` matrix and print the result. Next, create:

```python
single_feature = np.array([10, 20, 30, 40])
```

Reshape `single_feature` from `(4,)` into `(4, 1)`.

### Shape Checks

Your selected temperature vector should have shape `(4,)`. Your two-dimensional humidity column should have shape `(4, 1)`.

### Thinking Questions

- What does `sensor_features[:, 0]` mean?
- What is the difference between `[:, 1]` and `[:, 1:2]`?
- Why might an ML function require `(4, 1)` rather than `(4,)`?

**Concepts:** shape reasoning, axes, indexing, slicing, `reshape`

---

# 6. Task 3 — Views, Copies & Accidental Mutation

**Level:** Intermediate  
**File:** `task03_views_copies.py`

### Part A — A Slice View

Begin with:

```python
readings = np.array([10, 20, 30, 40, 50])
```

Create a slice containing the first three elements, then change the first value **through the slice**. Print both the slice and `readings`.

### Part B — An Independent Copy

Create a fresh `readings` array using the original values. Select the first three values with a **copy**, modify the copy, and confirm that the original array has not changed.

### Part C — Two-Dimensional Arrays

Create:

```python
matrix = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
```

Compare the behavior of a two-dimensional slice with an independent `.copy()` of the same selection.

### Required Explanation

In your README, explain the differences between:

```python
b = a
b = a[:3]
b = a[:3].copy()
```

**Engineering connection:** In numerical preprocessing, accidental mutation can change raw inputs or earlier results, making experiments difficult to reproduce.

**Concepts:** aliasing, views, copies, mutability

---

# 7. Task 4 — Vectorization & Numerical Data Types

**Level:** Intermediate  
**File:** `task04_vectorization.py`

### Part A — Convert Temperatures Without a Python Loop

Given:

```python
temperatures_c = [0.0, 10.0, 22.5, 30.0, 40.0]
```

Convert the values into Fahrenheit using:

```text
F = (C × 9/5) + 32
```

Produce **two versions**:

1. A Python-list implementation that uses a normal loop.
2. A NumPy version that uses one vectorized expression.

Verify that both versions produce equivalent numerical results.

### Part B — Element-Wise Operations

Given:

```python
base_scores = np.array([0.70, 0.82, 0.91, 0.77])
score_adjustments = np.array([0.03, -0.02, 0.01, 0.05])
```

Calculate adjusted scores using NumPy without writing a loop. Determine their minimum, maximum, and average.

### Part C — `dtype` Matters

Create:

```python
pixels = np.array([0, 64, 128, 255], dtype=np.uint8)
```

1. Print the array's `dtype`.
2. Convert the array to a floating-point type.
3. Scale the values into the range `[0, 1]` by dividing by `255`.
4. Print the scaled array and its `dtype`.

### Thinking Questions

- What does vectorization replace in Part A?
- Why should numerical transformations use an appropriate `dtype`?
- Is a NumPy expression automatically faster in every situation? Why or why not?

**Concepts:** vectorization, array arithmetic, conversion, `dtype`, numerical correctness

---

# 8. Task 5 — Broadcasting, Aggregations & the Meaning of `axis`

**Level:** Intermediate → Advanced  
**File:** `task05_broadcasting_aggregation.py`

### Problem

The following matrix has **four samples and three numerical features**:

```python
features = np.array([
    [10.0, 100.0, 1.0],
    [20.0, 200.0, 2.0],
    [30.0, 300.0, 3.0],
    [40.0, 400.0, 4.0]
])
```

### Part A — Aggregation

Calculate:

1. The mean of all values as one scalar.
2. The mean of each feature column using `axis=0`.
3. The mean of each sample row using `axis=1`.
4. The minimum and maximum of each feature column.

Print the shapes of each result.

### Part B — Broadcasting

1. Subtract the per-column means from the original matrix.
2. Verify that the result retains the same shape.
3. Verify that the centered feature columns each have an average approximately equal to zero.
4. Multiply every column by a different factor using:

```python
factors = np.array([1.0, 0.01, 10.0])
```

Explain why this array broadcasts across the rows.

### Part C — Reason About an Incompatible Shape

Predict what happens if you try to add:

```python
row_offsets = np.array([1.0, 2.0, 3.0, 4.0])
```

to `features` directly.

Then transform `row_offsets` into a `(4, 1)` array and add it to `features`. Show the resulting shape and explain the change.

### Expected Shape Reasoning

```text
features             → (4, 3)
features.mean(axis=0) → (3,)
features.mean(axis=1) → (4,)
row_offsets          → (4,)
reshaped offsets     → (4, 1)
```

**Concepts:** `axis`, `mean`, `min`, `max`, broadcasting, compatible shapes, column-wise operations

---

# 9. Task 6 — Boolean Masks, Validation & `np.where()`

**Level:** Intermediate → Advanced  
**File:** `task06_masks_conditions.py`

### Problem

A service returns the following confidence scores:

```python
confidence_scores = np.array([
    0.94, 0.79, -0.10, 0.88, 1.20, 0.63, 0.81
])
```

Valid confidence scores must be between `0` and `1`, inclusive.

### Part A — Validate

Create a Boolean mask for valid values and select only the valid values.

**Requirement:** Combine NumPy comparisons using `&`, with parentheses around each comparison. Do not use Python's `and` for array comparisons.

Print:

- validity mask
- valid scores
- invalid-value count
- number of valid scores

### Part B — Review Flags

For this exercise, mark a **valid** score for manual review if it is below `0.80`.

Use `np.where()` to create a label array containing:

```text
"review"
"accepted"
```

Do not classify invalid confidence values as accepted; work on the filtered valid scores.

### Part C — Summary

Report:

1. Mean valid confidence.
2. Count marked `accepted`.
3. Count marked `review`.
4. The share of valid scores marked `accepted`.

### Required Boundary Checks

Check exactly `0.0`, `0.80`, and `1.0` as well as a negative value and a value greater than `1`.

**Concepts:** Boolean masking, validation, `&`, `np.where`, vectorized classification, boundary cases

---

# 10. Task 7 — Feature Scaling & Numerical Model Calculations

**Level:** Advanced  
**File:** `task07_feature_calculations.py`

### Part A — Min-Max Scaling

Each row represents a sample; columns represent numerical features:

```python
feature_matrix = np.array([
    [10.0, 100.0, 5.0],
    [20.0, 150.0, 5.0],
    [30.0, 200.0, 5.0],
    [40.0, 250.0, 5.0]
])
```

Scale each column with:

```text
(x - column_min) / (column_max - column_min)
```

**Important:** The third feature is constant. You must handle its zero range without producing `nan` or infinity.

Use column-wise NumPy operations and `np.where()` (or another explicit NumPy-safe approach). Do not loop over individual rows.

### Requirements

- Print column minimums, maximums, and spans.
- Print the scaled feature matrix.
- Confirm that the result has the same shape as the input.
- Confirm that all results are finite (for example, `np.isfinite(...)`).
- Explain what value your chosen method assigns to the constant feature and why.

### Part B — A Simple Weighted Score

Use:

```python
features = np.array([
    [1.0, 2.0],
    [2.0, 3.0],
    [3.0, 4.0]
])
weights = np.array([0.4, 0.6])
bias = 0.5
```

Calculate one weighted score per sample using:

```text
features @ weights + bias
```

Print the result and its shape. Next, explain why `features * weights` is **not the same operation** as `features @ weights`.

> This is a numerical calculation similar to part of a linear model. You are **not training a model** in this lab.

### Engineering Note

When feature scaling becomes part of a machine-learning pipeline in later weeks, scaling parameters must be learned from training data and reused for new observations. Do not recalculate separate scaling parameters for every incoming record.

**Concepts:** feature-wise scaling, zero-range features, broadcasting, finite-value checks, element-wise vs matrix multiplication

---

# 11. Task 8 — Integrated AI-Oriented Sensor Feature Preparation

**Level:** Advanced / Main Engineering Task  
**Folder:** `task08_sensor_pipeline/`

This is the main Week 5 engineering task. It combines the lecture's concepts in a structured numerical-processing workflow.

### Scenario

An industrial monitoring application receives three numerical features:

```text
column 0 → temperature_C
column 1 → humidity_percent
column 2 → vibration_score
```

Some readings are invalid. Before this information can be used for further analysis, the application must validate, summarize, scale, and flag readings.

This is a **rule-based numerical preprocessing exercise**, not a trained machine-learning model.

### Input Dataset

Use:

```python
raw_sensor_data = np.array([
    [21.0, 42.0, 0.30],
    [24.0, 51.0, 0.55],
    [-10.0, 40.0, 0.45],  # invalid temperature
    [26.0, 68.0, 0.80],
    [32.0, 110.0, 0.95],  # invalid humidity
    [28.0, 59.0, 0.72],
    [23.0, 48.0, -0.20],  # invalid vibration
    [30.0, 74.0, 1.00]
], dtype=float)
```

### Valid Ranges

For the purpose of this exercise:

```text
0 <= temperature_C <= 50
0 <= humidity_percent <= 100
0 <= vibration_score <= 1
```

### Program Structure

Create:

```text
task08_sensor_pipeline/
├── main.py
└── sensor_utils.py
```

In `sensor_utils.py`, implement the following functions.

#### A. `clean_sensor_rows(readings)`

**Input:** an `n × 3` NumPy array.  
**Output:** a new NumPy array containing only valid rows.

Requirements:

- Validate all three columns.
- A row is valid only if **all** conditions are satisfied.
- Use NumPy Boolean masks rather than a Python loop over rows.
- Preserve the original column order.
- Do not mutate the original input.

#### B. `summarize_features(readings)`

Return a dictionary containing feature-wise:

```text
mean
minimum
maximum
```

Each value should be a NumPy array of shape `(3,)`.

**Edge case:** For this lab, raise a meaningful `ValueError` if there are no rows to summarize.

#### C. `minmax_scale(readings)`

Return a new scaled array with the **same shape** as the cleaned input.

Requirements:

- Scale by feature column.
- Use NumPy broadcasting.
- Handle any constant feature without division by zero.
- Do not modify the input array.
- Raise `ValueError` when there are no rows to scale.

#### D. `flag_for_review(readings)`

Return a one-dimensional Boolean array where a row is marked `True` if **either** of these rules is satisfied:

```text
temperature_C > 27
OR
vibration_score >= 0.70
```

Use vectorized comparisons and `|`.

### `main.py` Responsibilities

`main.py` should:

1. Define the raw input array.
2. Call `clean_sensor_rows()`.
3. Display the input and cleaned shapes.
4. Call `summarize_features()` on the clean data.
5. Call `minmax_scale()`.
6. Call `flag_for_review()`.
7. Build a readable summary containing:
   - total readings
   - valid readings
   - invalid readings
   - number of rows flagged for review
   - feature-wise means
8. Print the summary and scaled data clearly.

Keep numerical processing in `sensor_utils.py`; `main.py` should coordinate the workflow.

### Required Sanity Checks

For the supplied dataset, your results should satisfy:

```text
Raw data shape     → (8, 3)
Valid data shape   → (5, 3)
Invalid rows       → 3
Review flags       → 3 True values
Scaled data shape  → (5, 3)
```

These checks are included to help you identify logic errors, **not** to replace your explanation of the calculations.

### Engineering Questions

Answer in your README:

- Why does each row represent an observation while each column represents a feature?
- Why should row validation use a single combined Boolean mask?
- How do `axis=0` and broadcasting support feature-wise calculations?
- Which functions should reject an empty dataset, and why?
- Why should the raw array remain unchanged?
- If a new sensor observation arrives later, what preprocessing information would a production ML system need to retain?

**Concepts:** multidimensional arrays, masks, vectorization, `axis`, broadcasting, scaling, NumPy-based modularity, reproducibility

---

# 12. Optional Challenge — Compare Python Loops with NumPy

**File:** `optional_benchmark.py`

Use Python's `time.perf_counter` to compare equivalent numerical operations.

### Suggested Setup

```python
values_list = list(range(200_000))
values_array = np.array(values_list, dtype=float)
```

Compare:

1. A list comprehension that multiplies every value by `2`.
2. A vectorized NumPy multiplication.

Requirements:

- Time the transformation itself rather than printing the values.
- Run each transformation more than once and record the observations.
- Check that both implementations produce equivalent numerical values.
- State whether the creation of the NumPy array was included in your timing.
- Do **not** claim NumPy is universally faster based on one benchmark.

### Reflection

Explain both the readability and performance differences you observed. Timing varies across systems and problem sizes.

---

# 13. Testing and Verification

Strong AI engineering begins with **numerical correctness**.

You should verify more than whether a script runs without errors.

### Recommended Checks

| Situation | What to verify |
| --- | --- |
| Array creation | Shape, dimensions, size, dtype |
| Indexing | Correct values and resulting shape |
| Views/copies | Whether original data changes |
| Vectorization | Result agrees with a small loop-based calculation |
| Broadcasting | Operand shapes and output shape |
| Aggregation | Whether `axis=0` or `axis=1` matches your intention |
| Boolean masks | Boundary values and correct number of selected rows |
| Scaling | Constant feature does not cause `nan` or infinity |
| Weighted scores | The shape of `features @ weights` |
| Integrated workflow | Empty data and zero valid rows are handled deliberately |

Useful debugging checks:

```python
print("Shape:", array.shape)
print("Dtype:", array.dtype)
print("Values:\n", array)
```

Useful numerical checks:

```python
np.allclose(actual, expected)
np.isfinite(values).all()
```

When creating tests, use your own expected values for small arrays that you can calculate by hand.

---

# 14. Code Quality Expectations

### Name the Meaning of Every Axis

Prefer:

```python
sensor_readings
feature_means
scaled_features
```

instead of:

```python
x
a
b
```

### Avoid Magic Values

Prefer:

```python
MIN_CONFIDENCE = 0.80
```

rather than repeating an unexplained number throughout the file.

### Do Not Hide Shape Problems

Do not add random calls to `reshape()` until an error disappears. Understand which axis each dimension represents.

### Respect Original Data

Use `.copy()` when independent data is required. Explain any intentional in-place changes.

### Prefer Readable Vectorization

Vectorization should express the calculation clearly. For complicated control flow, simple functions and loops are still appropriate.

### Keep Responsibilities Separate

The final task should use small, meaningful functions. It does **not** need a class or an elaborate software framework.

---

# 15. Git Workflow

Continue the Git/GitHub habits established earlier in AI-216.

Do not wait until the entire lab is complete before committing.

Suggested progression:

```bash
git add labs/week05/task01_array_basics.py labs/week05/task02_indexing_shapes.py
git commit -m "Lab05: explore NumPy arrays and shapes"

git add labs/week05/task03_views_copies.py labs/week05/task04_vectorization.py
git commit -m "Lab05: implement copies and vectorized calculations"

git add labs/week05/task05_broadcasting_aggregation.py labs/week05/task06_masks_conditions.py
git commit -m "Lab05: add broadcasting and data validation"

git add labs/week05/task07_feature_calculations.py
git commit -m "Lab05: implement scaling and weighted feature scores"

git add labs/week05/task08_sensor_pipeline/
git commit -m "Lab05: build modular sensor feature pipeline"

git add labs/week05/README.md
git commit -m "Lab05: document numerical tests and design decisions"

git push
```

The exact commit grouping can differ; meaningful history matters more than copying these commands exactly.

---

# 16. Your Week 5 README

Create:

```text
labs/week05/README.md
```

Suggested template:

```markdown
# Lab 05 — NumPy & Numerical Computing for AI

## Environment
- Python version:
- NumPy version:

## Concepts Practiced
- NumPy arrays and shapes
- Indexing and slicing
- Views and copies
- Vectorization
- Broadcasting and axis
- Boolean masks and np.where
- Numerical transformations
- Matrix multiplication

## Tasks Completed
1. Array Basics
2. Indexing, Shapes, and Reshaping
3. Views and Copies
4. Vectorization and Dtypes
5. Broadcasting and Aggregation
6. Masks and Conditions
7. Feature Calculations
8. Sensor Feature Preparation

## Shape Reasoning
Explain at least three important intermediate array shapes.

## Testing and Numerical Checks
| Task | Case | Expected | Actual | Pass/Fail |
| --- | --- | --- | --- | --- |
| | | | | |

## View vs Copy
Explain what changed in Task 3 and why.

## Vectorization
Explain one operation you rewrote from a Python loop into a NumPy expression.

## Sensor Pipeline Design
- Function responsibilities:
- Input and output shapes:
- Why invalid rows were removed:
- How constant features are handled:
- What should happen when no valid rows remain:

## Challenges and Debugging
- One problem I encountered:
- How I identified it:
- How I fixed it:

## AI Engineering Relevance
Explain how NumPy supports numerical preprocessing and future ML workflows.
```

If you used an AI assistant, also include the **AI Usage Log** described below.

---

# 17. AI-Assisted Learning

AI tools may be used to **understand, debug, review, or test** your work unless an assessment-specific instruction says otherwise.

Useful prompts:

- "Do not solve the task. Help me identify the shape of each intermediate array."
- "Explain why these shapes cannot broadcast together."
- "Trace `axis=0` and `axis=1` on this matrix with small numbers."
- "Why did modifying this slice also change the original array?"
- "Suggest boundary cases for this NumPy validity mask without writing my final code."
- "Explain the difference between element-wise multiplication and `@`."
- "Review whether my NumPy expression is readable and correct; explain any concern."
- "Give me a small hand-calculable test case for my scaling function."

If AI assistance was used, add this to your `README.md`:

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

You must be able to explain every NumPy expression, including the shapes and operations involved. AI-generated code that you cannot explain is not evidence of learning.

---

# 18. Self-Study Learning Resources

These resources are for independent learning. Start with the first two and use the others when a particular concept causes difficulty.

### 1. NumPy — Absolute Beginners

https://numpy.org/doc/stable/user/absolute_beginners.html

Review how to create arrays, inspect shape and dimensions, index values, and perform introductory numerical operations.

### 2. NumPy — Quickstart Tutorial

https://numpy.org/doc/stable/user/quickstart.html

Focus on array creation, basic operations, indexing, shapes, and array-wise functions.

### 3. NumPy — Broadcasting

https://numpy.org/doc/stable/user/basics.broadcasting.html

Read when you are unsure whether two array shapes can be combined. Work through the examples with pencil and paper before running them.

### 4. NumPy — Indexing

https://numpy.org/doc/stable/user/basics.indexing.html

Use this for selecting elements, rows, columns, Boolean masks, and understanding the shapes of selections.

### 5. NumPy — Copies and Views

https://numpy.org/doc/stable/user/basics.copies.html

Use this to understand why modifying a slice may change the original array.

### 6. NumPy — `np.where()` Reference

https://numpy.org/doc/stable/reference/generated/numpy.where.html

Use this when constructing vectorized, condition-based output arrays.

### Suggested Independent Study Path

```text
Create arrays
    ↓
Inspect shape and dtype
    ↓
Index and slice
    ↓
Compare views and copies
    ↓
Replace a numerical loop with vectorization
    ↓
Understand broadcasting and axis
    ↓
Filter with Boolean masks
    ↓
Prepare a numerical feature matrix
```

The best way to learn NumPy is to predict an array's **shape and output**, run the code, and explain any differences.

---

# 19. Submission Checklist

Before submitting, verify:

- [ ] All required Week 5 work is inside `labs/week05/`.
- [ ] All eight required tasks are completed.
- [ ] Every required script runs using the documented NumPy environment.
- [ ] Task 1 explains list behavior versus array behavior.
- [ ] Task 2 demonstrates both 1D and 2D selections and reshaping.
- [ ] Task 3 demonstrates views, aliases, and independent copies.
- [ ] Task 4 includes loop-based and vectorized numerical transformations.
- [ ] Task 5 explains `axis` and compatible/incompatible broadcasting.
- [ ] Task 6 validates and filters confidence scores using Boolean masks.
- [ ] Task 7 handles a constant feature during scaling and uses `@` correctly.
- [ ] Task 8 contains `main.py` and `sensor_utils.py` with clear responsibilities.
- [ ] Task 8 produces the expected valid-row and review-flag counts.
- [ ] Important intermediate array shapes were checked.
- [ ] Relevant edge cases and numerical results were verified.
- [ ] Original data was not unintentionally mutated.
- [ ] `labs/week05/README.md` documents learning, tests, and design decisions.
- [ ] The AI Usage Log is included if AI assistance was used.
- [ ] Meaningful Git commits are visible and all required work is pushed to GitHub.
- [ ] You can explain why each NumPy operation and `axis` setting is appropriate.

---

# 20. Expected Learning Outcomes

After completing this lab, you should be able to:

- Represent numerical information with NumPy arrays.
- Interpret shapes and dimensions in terms of samples and features.
- Index, slice, and reshape arrays correctly.
- Explain and avoid accidental mutation through array views.
- Replace appropriate numerical loops with vectorized expressions.
- Apply broadcasting and aggregation with correct axis reasoning.
- Build masks and conditional numerical transformations.
- Scale feature columns safely, including constant features.
- Perform a basic weighted feature calculation.
- Organize a small numerical-processing workflow into reusable functions.
- Test, document, and version-control numerical work professionally.

---

## Lab 05 Key Message

> **NumPy is not simply a way to write shorter code. It is a way to represent numerical data and operations clearly, efficiently, and correctly.**

Week 5 builds the numerical foundation needed for **Pandas in Week 6** and reproducible machine-learning workflows later in the semester.
