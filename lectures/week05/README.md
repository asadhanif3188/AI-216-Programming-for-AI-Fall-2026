# AI-216: Programming for Artificial Intelligence
## Week 05 Lecture Handout — NumPy & Numerical Computing

**Semester:** Fall 2026  
**Week:** 05  
**Theme:** From Python collections to efficient numerical computation for AI

---

## 1. Why Week 5 Matters

In Week 4, you learned to represent data with **lists, tuples, dictionaries, and sets**. Those structures are essential for general-purpose Python programming. But AI applications frequently work with large collections of **numbers**:

- sensor readings and measurements
- pixel intensities in images
- numerical features describing customers or transactions
- confidence scores from a model
- matrices of features, weights, and predictions

We could process these values using Python lists and loops. For many numerical tasks, however, we want a representation that makes calculations across entire collections straightforward.

**NumPy** (Numerical Python) provides the `ndarray`: a multidimensional array designed for numerical computation.

```text
Python Fundamentals
       ↓
Lists and Loops
       ↓
NumPy Arrays
       ↓
Vectorized Operations
       ↓
Indexing, Slicing & Broadcasting
       ↓
Numerical Summaries and Transformations
       ↓
Feature Matrices for AI/ML
```

> **AI engineering question:** How can we express numerical work over many observations clearly, correctly, and efficiently?

NumPy is one important answer. It will also provide a foundation for the Pandas and scikit-learn workflows introduced later in the course.

---

## 2. Week 5 Learning Outcomes

By the end of this lecture, you should be able to:

1. Explain what NumPy is and when an array is more suitable than a Python list.
2. Create one-dimensional and multidimensional NumPy arrays.
3. Inspect an array's `shape`, `ndim`, `size`, and `dtype`.
4. Use NumPy indexing and slicing to select values, rows, and columns.
5. Explain the difference between a view and a copy.
6. Perform element-wise numerical operations without writing an explicit Python loop.
7. Explain broadcasting and recognize incompatible shapes.
8. Compute numerical summaries with functions such as `sum()`, `mean()`, `min()`, and `max()`.
9. Filter and transform data with Boolean masks and `np.where()`.
10. Explain vectorization and why it is frequently useful in AI workloads.
11. Describe how a two-dimensional array represents a machine-learning feature matrix.

---

## 3. The NumPy Mindset: Operate on Collections, Not One Number at a Time

NumPy changes how we *express* numerical problems. Instead of describing every loop iteration, we often express an operation that applies to the entire array.

For the examples below, import NumPy first:

```python
import numpy as np
```

`np` is the widely used conventional alias. The examples assume this import has already been run.

### 3.1 Basic Example — One Number Versus Many Numbers

```python
score = 80
print(score + 5)

scores = np.array([80, 85, 90])
print(scores + 5)
```

Output:

```text
85
[85 90 95]
```

One expression applies to each element in the array.

### 3.2 Intermediate Example — A Repeated Transformation

```python
raw_scores = np.array([65, 75, 85, 95])
normalized_scores = raw_scores / 100

print(normalized_scores)
```

Output:

```text
[0.65 0.75 0.85 0.95]
```

The operation describes **what transformation is required**, not how many times to repeat it.

### 3.3 Advanced Example — A Matrix of AI Features

```python
# Each row is one observation; each column is one feature.
features = np.array([
    [20, 150],
    [24, 200],
    [22, 180]
], dtype=float)

feature_means = features.mean(axis=0)
centered_features = features - feature_means

print("Means:", feature_means)
print("Centered shape:", centered_features.shape)
print(centered_features)
```

The means are computed **per column** and then subtracted from every row. We will unpack `axis` and broadcasting later.

**Key idea:** A NumPy expression often describes a transformation over a vector or matrix.

---

# 4. What Is a NumPy Array?

The core NumPy object is `numpy.ndarray`, normally created using `np.array()` or an array-creation function.

An `ndarray` stores elements of a particular data type in a multidimensional structure. Unlike a normal Python list, it is designed for efficient numerical operations.

Useful attributes:

| Attribute | Meaning | Example |
| --- | --- | --- |
| `ndim` | Number of dimensions (axes) | `2` |
| `shape` | Length along each dimension | `(3, 2)` |
| `size` | Total number of elements | `6` |
| `dtype` | Element data type | `float64` (platform-dependent display possible) |

## 4.1 Basic Example — One-Dimensional Array

```python
scores = np.array([70, 80, 90])

print(scores)
print(type(scores))
print(scores.shape)
print(scores.ndim)
```

A one-dimensional array with three elements has shape `(3,)`.

## 4.2 Intermediate Example — Two-Dimensional Array

```python
marks = np.array([
    [70, 80, 90],
    [65, 75, 85]
])

print("Dimensions:", marks.ndim)
print("Shape:", marks.shape)
print("Elements:", marks.size)
```

Output:

```text
Dimensions: 2
Shape: (2, 3)
Elements: 6
```

Think of the shape as **(rows, columns)** for this two-dimensional example.

## 4.3 Advanced Example — Images as Multidimensional Arrays

```python
# A tiny RGB image: height=2, width=3, channels=3.
image = np.zeros((2, 3, 3), dtype=np.uint8)

image[0, 0] = [255, 0, 0]  # red pixel

print("Dimensions:", image.ndim)
print("Shape:", image.shape)
print("Number of values:", image.size)
```

Output:

```text
Dimensions: 3
Shape: (2, 3, 3)
Number of values: 18
```

**AI connection:** Arrays can represent feature vectors, tables of numerical features, images, and many other AI inputs.

---

# 5. Creating Arrays

NumPy has several array-creation functions:

- `np.array(...)` — convert provided values into an array
- `np.zeros(...)` — fill with zeros
- `np.ones(...)` — fill with ones
- `np.full(...)` — fill with a chosen value
- `np.arange(...)` — create a regularly spaced range
- `np.linspace(...)` — choose a specified number of evenly spaced values

## 5.1 Basic Example — From a Python List

```python
python_scores = [72, 85, 91]
scores = np.array(python_scores)

print(scores)
```

## 5.2 Intermediate Example — Initialized Matrices

```python
zeros = np.zeros((2, 3))
ones = np.ones((2, 3))
thresholds = np.full((2, 3), 0.80)

print("Zeros:\n", zeros)
print("Ones:\n", ones)
print("Thresholds:\n", thresholds)
```

This is useful when preparing a collection of values before calculations.

## 5.3 Advanced Example — Numerical Sampling Points

```python
steps = np.arange(0, 10, 2)
probability_grid = np.linspace(0, 1, 6)

print("Steps:", steps)
print("Probability grid:", probability_grid)
```

Output:

```text
Steps: [0 2 4 6 8]
Probability grid: [0.  0.2 0.4 0.6 0.8 1. ]
```

`np.arange(start, stop, step)` excludes `stop`. `np.linspace(start, stop, num)` normally includes the endpoint; for decimal sample points, `linspace` is often clearer.

---

# 6. Python Lists Versus NumPy Arrays

Both can contain many values, but their behavior is not the same.

| Question | Python `list` | NumPy `ndarray` |
| --- | --- | --- |
| Can store different kinds of values? | Yes | Typically a common `dtype` |
| Designed for numerical vector operations? | Not directly | Yes |
| `+` between two collections | Concatenates | Adds element-wise (when compatible) |
| Multiply by an integer | Repeats list | Multiplies element-wise |
| Two-dimensional numerical calculations | Nested structures and loops | Built-in multidimensional operations |

## 6.1 Basic Example — `+` Means Something Different

```python
python_a = [1, 2, 3]
python_b = [4, 5, 6]
print(python_a + python_b)

numpy_a = np.array([1, 2, 3])
numpy_b = np.array([4, 5, 6])
print(numpy_a + numpy_b)
```

Output:

```text
[1, 2, 3, 4, 5, 6]
[5 7 9]
```

## 6.2 Intermediate Example — List Loop Versus Array Expression

```python
scores = [60, 70, 80]

with_loop = []
for score in scores:
    with_loop.append(score + 5)

with_numpy = np.array(scores) + 5

print(with_loop)
print(with_numpy)
```

Both compute the same values, but the array expression is simpler for this numeric operation.

## 6.3 Advanced Example — Numerical Feature Matrix

```python
# Rows: samples. Columns: height_cm and weight_kg.
samples = np.array([
    [170, 70],
    [165, 60],
    [180, 85]
], dtype=float)

heights_m = samples[:, 0] / 100
weights_kg = samples[:, 1]
bmi = weights_kg / (heights_m ** 2)

print("BMI:", np.round(bmi, 2))
```

This applies a calculation to **every row** without manually looping through the observations.

**Important:** NumPy is not a replacement for every Python collection. Dictionaries and lists remain valuable when records are heterogeneous or when named fields are more important than numerical operations.

---

# 7. Data Types (`dtype`) and Numerical Precision

An array normally has one element type. NumPy may convert input values into a compatible type when creating an array.

Examples include `int32`, `int64`, `float32`, `float64`, `bool`, and `uint8`. Exact defaults can depend on the machine and NumPy version.

## 7.1 Basic Example — Inspect a Data Type

```python
values = np.array([1, 2, 3])

print(values.dtype)
```

## 7.2 Intermediate Example — Convert a Data Type

```python
scores = np.array([70, 80, 90])
normalized = scores.astype(float) / 100

print(normalized)
print(normalized.dtype)
```

`astype()` produces an array with the requested type; it does not change `scores` here.

## 7.3 Advanced Example — Image Pixel Type

```python
pixels = np.array([0, 127, 255], dtype=np.uint8)

# Convert to floating point before normalizing.
normalized_pixels = pixels.astype(np.float32) / 255.0

print(normalized_pixels)
print(normalized_pixels.dtype)
```

**Important pitfalls:** Integer arrays can overflow when the type cannot represent a result. For example, unsigned 8-bit values only represent 0 through 255. Choose types deliberately, especially when combining pixel values or doing arithmetic.

---

# 8. Array Indexing and Slicing

Indexing selects individual positions. Slicing selects ranges.

NumPy uses familiar Python slicing syntax, but with a separate index or slice for each axis.

## 8.1 Basic Example — One-Dimensional Indexing

```python
scores = np.array([72, 85, 91, 67, 88])

print(scores[0])
print(scores[-1])
print(scores[1:4])
```

## 8.2 Intermediate Example — Rows and Columns

```python
features = np.array([
    [10, 100],
    [20, 200],
    [30, 300]
])

print("First row:", features[0, :])
print("Second column:", features[:, 1])
print("One value:", features[1, 0])
```

`features[:, 1]` means **all rows, column 1**.

## 8.3 Advanced Example — Select a Feature Submatrix

```python
# Four samples, three features per sample.
features = np.array([
    [1.0, 12.0, 100.0],
    [2.0, 14.0, 110.0],
    [3.0, 16.0, 120.0],
    [4.0, 18.0, 130.0]
])

first_three_samples = features[:3, :]
selected_features = first_three_samples[:, [0, 2]]

print("Shape:", selected_features.shape)
print(selected_features)
```

The result contains three rows and two selected columns, so its shape is `(3, 2)`.

---

# 9. Views, Copies, and Accidental Mutation

Like lists, NumPy arrays can be shared between variables. In addition, **basic slicing often returns a view**: a new array object that refers to the same underlying data.

This is important for correctness, not only performance.

## 9.1 Basic Example — Aliasing

```python
original = np.array([10, 20, 30])
alias = original

alias[0] = 99
print(original)
```

Output:

```text
[99 20 30]
```

## 9.2 Intermediate Example — Slice View

```python
original = np.array([10, 20, 30, 40])
view = original[1:3]

view[0] = 999
print(original)
```

Output:

```text
[ 10 999  30  40]
```

## 9.3 Advanced Example — Independent Copy

```python
raw_data = np.array([
    [10.0, 100.0],
    [20.0, 200.0]
])

processed_data = raw_data.copy()
processed_data[:, 0] /= 10

print("Raw:\n", raw_data)
print("Processed:\n", processed_data)
```

**Key distinctions:**

- `alias = array` shares the same array object.
- `view = array[1:3]` typically shares underlying data.
- `copy = array.copy()` creates independent underlying data.
- Boolean and integer-array *advanced indexing* generally creates a copy, not a view.

Ask: **Should this transformation modify the original array?**

---

# 10. Shapes, Dimensions, and Reshaping

In AI, an array's shape is often as important as its values.

A common convention is:

```text
rows    = samples
columns = features
```

A feature matrix containing four observations and three features has shape `(4, 3)`.

## 10.1 Basic Example — Inspect Shape

```python
values = np.array([10, 20, 30, 40, 50, 60])

print(values.shape)
print(values.ndim)
```

## 10.2 Intermediate Example — Reshape a Vector

```python
values = np.arange(1, 7)
matrix = values.reshape(2, 3)

print(matrix)
print(matrix.shape)
```

Output:

```text
[[1 2 3]
 [4 5 6]]
(2, 3)
```

## 10.3 Advanced Example — Add a Feature Axis

```python
# Five samples, one numerical feature per sample.
ages = np.array([18, 20, 22, 24, 26])

feature_matrix = ages.reshape(-1, 1)

print("Original:", ages.shape)
print("For ML:", feature_matrix.shape)
print(feature_matrix)
```

Output shapes:

```text
Original: (5,)
For ML: (5, 1)
```

`-1` asks NumPy to infer the corresponding dimension. The number of elements must remain consistent; reshaping cannot invent or discard values.

---

# 11. Vectorization: Numerical Operations without Explicit Python Loops

**Vectorization** means expressing operations on arrays rather than writing a Python loop for each element. Many NumPy operations execute their low-level iteration in compiled code.

This can improve readability and often improves performance for sufficiently large, suitable numerical workloads. It is not a guarantee that every array expression is faster in every situation.

## 11.1 Basic Example — Element-Wise Arithmetic

```python
scores = np.array([70, 80, 90])

print(scores + 5)
print(scores * 2)
print(scores / 100)
```

## 11.2 Intermediate Example — Combine Two Arrays

```python
quiz_scores = np.array([70, 80, 90])
assignment_scores = np.array([85, 75, 95])

final_scores = 0.4 * quiz_scores + 0.6 * assignment_scores

print(final_scores)
```

Each position in `final_scores` combines the corresponding quiz and assignment score.

## 11.3 Advanced Example — Multiple Features at Once

```python
features = np.array([
    [10.0, 100.0],
    [20.0, 200.0],
    [30.0, 300.0]
])

# Convert the first feature from tens to units and
# the second feature from hundreds to units.
scales = np.array([10.0, 100.0])
scaled_features = features / scales

print(scaled_features)
```

Output:

```text
[[1. 1.]
 [2. 2.]
 [3. 3.]]
```

This uses **broadcasting**, introduced next.

---

# 12. Broadcasting

Broadcasting allows certain operations between arrays of different shapes by conceptually expanding dimensions where compatible.

A useful rule: compare shapes **from right to left**. Dimensions are compatible if they are equal, or if one of them is `1`. A missing leading dimension can act like `1`.

Broadcasting does not require creating a fully repeated copy of the smaller operand in the usual implementation.

## 12.1 Basic Example — Array and Scalar

```python
scores = np.array([70, 80, 90])

print(scores + 10)
```

The scalar `10` applies to every element.

## 12.2 Intermediate Example — Matrix and Row Vector

```python
features = np.array([
    [10.0, 100.0],
    [20.0, 200.0],
    [30.0, 300.0]
])

corrections = np.array([1.0, 10.0])
corrected = features - corrections

print(corrected)
```

Shapes:

```text
features:    (3, 2)
corrections: (2,)
result:      (3, 2)
```

## 12.3 Advanced Example — Center Each Feature Column

```python
features = np.array([
    [2.0, 20.0],
    [4.0, 30.0],
    [6.0, 40.0]
])

column_means = features.mean(axis=0)
centered = features - column_means

print("Means:", column_means)
print("Centered:\n", centered)
```

Output:

```text
Means: [ 4. 30.]
Centered:
 [[ -2. -10.]
 [  0.   0.]
 [  2.  10.]]
```

**Important:** Shapes `(3, 2)` and `(3,)` are not compatible for direct element-wise operations here. Shape `(2,)` is compatible with `(3, 2)` for per-column operations.

---

# 13. Numerical Aggregations and the Meaning of `axis`

Aggregations calculate summaries such as total, average, minimum, maximum, and standard deviation.

Common operations:

```python
np.sum(...)
np.mean(...)
np.min(...)
np.max(...)
np.std(...)
```

**Axis intuition for 2D arrays:**

- `axis=0`: reduce across rows, yielding one result per column
- `axis=1`: reduce across columns, yielding one result per row

## 13.1 Basic Example — One Array Summary

```python
scores = np.array([70, 80, 90])

print("Total:", np.sum(scores))
print("Mean:", np.mean(scores))
print("Minimum:", np.min(scores))
print("Maximum:", np.max(scores))
```

## 13.2 Intermediate Example — Column and Row Means

```python
scores = np.array([
    [70, 80, 90],
    [60, 75, 85]
])

print("Per-column mean:", scores.mean(axis=0))
print("Per-row mean:", scores.mean(axis=1))
```

Output:

```text
Per-column mean: [65.  77.5 87.5]
Per-row mean: [80.         73.33333333]
```

## 13.3 Advanced Example — Feature-Wise Summary

```python
features = np.array([
    [18.0, 50.0],
    [20.0, 60.0],
    [22.0, 70.0]
])

summary = {
    "mean": features.mean(axis=0),
    "min": features.min(axis=0),
    "max": features.max(axis=0),
    "std": features.std(axis=0)
}

for name, values in summary.items():
    print(name, np.round(values, 2))
```

The dictionary adds readable names; NumPy handles the numerical calculations.

---

# 14. Boolean Masks and Array Filtering

A comparison with a NumPy array usually produces a **Boolean array** of the same shape.

Masks can then select values or rows.

## 14.1 Basic Example — One Condition

```python
scores = np.array([70, 45, 88, 39, 91])

mask = scores >= 50
print(mask)
print(scores[mask])
```

## 14.2 Intermediate Example — Multiple Conditions

```python
sensor_readings = np.array([12, -3, 45, 108, 70])

valid_mask = (sensor_readings >= 0) & (sensor_readings <= 100)
valid_readings = sensor_readings[valid_mask]

print(valid_readings)
```

Use `&` for element-wise AND, `|` for element-wise OR, and `~` for element-wise NOT. **Parenthesize each comparison.** Python's plain `and` / `or` are not suitable for combining Boolean arrays.

## 14.3 Advanced Example — Filter Matrix Rows

```python
# Columns: age, monthly_spending
customers = np.array([
    [19, 5000],
    [35, 8000],
    [17, 3000],
    [42, 12000]
])

adult_mask = customers[:, 0] >= 18
adult_customers = customers[adult_mask]

print(adult_customers)
print("Selected shape:", adult_customers.shape)
```

Here the Boolean mask has one entry **per row**.

---

# 15. Vectorized Conditional Transformations with `np.where()`

`np.where(condition, value_if_true, value_if_false)` chooses values element-wise based on a condition.

## 15.1 Basic Example — Pass/Fail Labels

```python
scores = np.array([72, 45, 88])
labels = np.where(scores >= 50, "pass", "fail")

print(labels)
```

## 15.2 Intermediate Example — Apply a Minimum Value

```python
values = np.array([-3, 5, -1, 9])
non_negative = np.where(values < 0, 0, values)

print(non_negative)
```

NumPy also provides `np.clip(values, minimum, maximum)` when you need to constrain a numeric range.

## 15.3 Advanced Example — Confidence-Based Review Flag

```python
confidence = np.array([0.95, 0.62, 0.88, 0.40])
threshold = 0.80

review_status = np.where(
    confidence >= threshold,
    "auto-review-passed",
    "manual-review-needed"
)

for index, status in enumerate(review_status, start=1):
    print(f"Prediction {index}: {status}")
```

**AI connection:** This illustrates post-processing of model outputs. A real system would also need well-justified decision thresholds and evaluation.

---

# 16. Numerical Transformations and Feature Scaling

AI models often benefit from numerical features expressed on comparable scales. Here we study the NumPy operations, **not a full machine-learning preprocessing strategy** (that comes later in the course).

Two simple transformations:

```text
Min-max scaling: (x - min) / (max - min)
Mean centering:  x - mean
```

## 16.1 Basic Example — Divide by a Known Maximum

```python
scores = np.array([60, 80, 100], dtype=float)
normalized = scores / 100

print(normalized)
```

## 16.2 Intermediate Example — Mean Centering

```python
values = np.array([10.0, 20.0, 30.0])
centered = values - values.mean()

print(centered)
print("Centered mean:", centered.mean())
```

## 16.3 Advanced Example — Column-Wise Min-Max Scaling

```python
# Rows are samples; columns are numerical features.
features = np.array([
    [18.0, 100.0],
    [20.0, 150.0],
    [22.0, 200.0]
])

minimums = features.min(axis=0)
maximums = features.max(axis=0)
spans = maximums - minimums

# Use a safe divisor when a feature is constant.
safe_spans = np.where(spans == 0, 1, spans)
scaled_features = (features - minimums) / safe_spans

print(scaled_features)
```

Output:

```text
[[0.  0. ]
 [0.5 0.5]
 [1.  1. ]]
```

**Engineering caution:** In a real ML workflow, scaling parameters should be *fitted using training data* and reused for validation/test data; recalculating them separately can cause misleading results or data leakage. We revisit this in Weeks 10–11.

---

# 17. Element-Wise Multiplication Versus Matrix Multiplication

NumPy supports both element-wise multiplication and matrix multiplication. They mean different things.

- `a * b`: multiply matching elements (with broadcasting when compatible)
- `a @ b`: matrix multiplication / dot-product style operation

## 17.1 Basic Example — Element-Wise Multiplication

```python
weights = np.array([2, 3, 4])
values = np.array([10, 20, 30])

print(weights * values)
```

Output:

```text
[ 20  60 120]
```

## 17.2 Intermediate Example — Dot Product

```python
features = np.array([2.0, 3.0, 4.0])
weights = np.array([0.5, 1.0, 2.0])

score = features @ weights
print(score)
```

Output:

```text
12.0
```

This combines corresponding feature/weight products into one number.

## 17.3 Advanced Example — Scores for Several Samples

```python
# Three samples, two features each.
features = np.array([
    [1.0, 2.0],
    [2.0, 3.0],
    [3.0, 4.0]
])

weights = np.array([0.4, 0.6])
bias = 0.5

scores = features @ weights + bias

print("Output shape:", scores.shape)
print("Scores:", scores)
```

This resembles part of a linear model's numerical calculation. We are not training a model yet; we are examining the operations that many models use.

---

# 18. Why Vectorized Computation Matters in AI

A large number of AI tasks repeatedly apply simple mathematical operations to large collections of values. Vectorization often replaces many Python-level loop steps with an array operation implemented in optimized native code.

Benefits can include:

- clear expression of numerical intent
- less manual indexing
- efficient array-wise computation
- compatibility with the broader scientific Python ecosystem

But good engineering requires **measurement**, not assumptions. For small inputs or code that creates excessive temporary arrays, vectorization might not provide an advantage.

## 18.1 Basic Example — Compare Two Styles

```python
scores = [60, 70, 80]

loop_result = []
for score in scores:
    loop_result.append(score / 100)

array_result = np.array(scores) / 100

print(loop_result)
print(array_result)
```

## 18.2 Intermediate Example — Batch Numerical Rule

```python
confidences = np.array([0.91, 0.55, 0.88, 0.76])
accepted = confidences >= 0.80

print("Accepted:", accepted)
print("Count accepted:", np.sum(accepted))
```

Booleans act as `1` or `0` when summed, making `np.sum(accepted)` a count.

## 18.3 Advanced Example — Reasonable Performance Comparison

```python
from time import perf_counter

values_list = list(range(200_000))
values_array = np.array(values_list, dtype=float)

start = perf_counter()
python_result = [value * 2 for value in values_list]
python_time = perf_counter() - start

start = perf_counter()
numpy_result = values_array * 2
numpy_time = perf_counter() - start

print("Same values:", np.allclose(python_result, numpy_result))
print(f"Python list time: {python_time:.6f}s")
print(f"NumPy array time: {numpy_time:.6f}s")
```

This measures the transformation itself; it does not include building the NumPy array. Results vary by machine, workload, and how often work is repeated. For more rigorous benchmarks, repeat measurements using appropriate tooling.

**Key idea:** Vectorization matters because it changes both the **clarity of the calculation** and often its **execution characteristics**.

---

# 19. Integrated AI-Oriented Example — Sensor Feature Preparation

Let's connect array creation, shape, indexing, masks, column-wise operations, broadcasting, and numerical summarization.

### Problem

A sensor collects **temperature** and **humidity** readings. Some rows are invalid. We want to:

1. represent the readings as a numerical matrix
2. keep only valid rows
3. summarize each feature
4. scale the valid readings
5. flag readings with unusually high temperature

This is a numerical-data example, **not an actual trained AI model**.

## 19.1 Start with Raw Data

```python
import numpy as np

# Rows: observations. Columns: [temperature_C, humidity_percent]
raw_readings = np.array([
    [22.0, 55.0],
    [25.0, 60.0],
    [-80.0, 50.0],   # invalid for this illustrative scenario
    [28.0, 65.0],
    [32.0, 110.0],   # invalid humidity
    [30.0, 70.0]
])

print("Shape:", raw_readings.shape)
```

Output:

```text
Shape: (6, 2)
```

## 19.2 Select Valid Rows

For this example, assume the acceptable measurement ranges are 0–50°C and 0–100% humidity.

```python
temperature = raw_readings[:, 0]
humidity = raw_readings[:, 1]

valid_rows = (
    (temperature >= 0) & (temperature <= 50)
    & (humidity >= 0) & (humidity <= 100)
)

clean_readings = raw_readings[valid_rows]

print("Clean shape:", clean_readings.shape)
print(clean_readings)
```

Output:

```text
Clean shape: (4, 2)
[[22. 55.]
 [25. 60.]
 [28. 65.]
 [30. 70.]]
```

## 19.3 Summarize Features

```python
feature_means = clean_readings.mean(axis=0)
feature_mins = clean_readings.min(axis=0)
feature_maxs = clean_readings.max(axis=0)

print("Means:", feature_means)
print("Minimums:", feature_mins)
print("Maximums:", feature_maxs)
```

Output:

```text
Means: [26.25 62.5 ]
Minimums: [22. 55.]
Maximums: [30. 70.]
```

## 19.4 Scale the Numerical Features

```python
spans = feature_maxs - feature_mins
safe_spans = np.where(spans == 0, 1, spans)
scaled_readings = (clean_readings - feature_mins) / safe_spans

print("Scaled:\n", scaled_readings)
```

The resulting array has the **same shape** as the cleaned data.

## 19.5 Create a Simple Rule-Based Flag

```python
HIGH_TEMPERATURE_C = 27.0

high_temperature = clean_readings[:, 0] > HIGH_TEMPERATURE_C

print("High-temperature flags:", high_temperature)
print("Total flagged:", np.sum(high_temperature))
```

Output:

```text
High-temperature flags: [False False  True  True]
Total flagged: 2
```

### What This Example Demonstrates

```text
Raw 2D Array
     ↓
Column Selection
     ↓
Boolean Validation Mask
     ↓
Clean 2D Array
     ↓
Column-Wise Aggregation
     ↓
Broadcasted Scaling
     ↓
Vectorized Rule-Based Output
```

This is a useful precursor to the more formal preprocessing and machine-learning workflows later in AI-216.

---

# 20. Common Mistakes and Engineering Pitfalls

## 20.1 Expecting Array Operations to Behave Like Lists

```python
print([1, 2] + [3, 4])
print(np.array([1, 2]) + np.array([3, 4]))
```

List concatenation and array addition are different operations.

## 20.2 Confusing `shape` and `size`

```python
values = np.zeros((3, 4))

print(values.shape)  # (3, 4)
print(values.size)   # 12
```

## 20.3 Using Python `and` with Boolean Arrays

Incorrect:

```python
# invalid = (scores >= 0) and (scores <= 100)
```

Correct:

```python
valid = (scores >= 0) & (scores <= 100)
```

## 20.4 Ignoring Incompatible Shapes

```python
matrix = np.ones((3, 2))
column_values = np.array([1, 2, 3])

# matrix + column_values  # ValueError: incompatible shapes
```

For one value per **row**, use a column vector:

```python
print(matrix + column_values.reshape(3, 1))
```

## 20.5 Confusing a View with a Copy

```python
array = np.array([10, 20, 30])
part = array[:2]
part[0] = 99

print(array)  # The original has changed.
```

Use `.copy()` when independent data is required.

## 20.6 Dividing by Zero in Scaling

If a feature is constant, its `max - min` is zero. Handle this case deliberately rather than assuming every feature has variation.

## 20.7 Ignoring Empty Arrays

Functions such as `np.mean()` on an empty array can produce warnings and `nan` results. Validate the number of observations before aggregating.

## 20.8 Confusing Element-Wise Multiplication with `@`

```python
values * weights  # element-wise
values @ weights  # dot product for compatible 1D inputs
```

## 20.9 Assuming NumPy Arrays Automatically Make Everything Faster

Performance depends on array sizes, operation types, temporary allocations, and data-conversion costs. Measure when performance matters.

---

# 21. NumPy Shape Reasoning — A Practical Mental Model

Before writing an array operation, ask three questions:

1. **What does each axis represent?**
2. **What shape does this operation expect?**
3. **What shape will it return?**

Consider:

```python
features = np.array([
    [18.0, 150.0],
    [20.0, 175.0],
    [22.0, 200.0]
])
```

| Expression | Shape | Meaning |
| --- | --- | --- |
| `features` | `(3, 2)` | 3 samples, 2 features |
| `features[0]` | `(2,)` | First sample |
| `features[:, 0]` | `(3,)` | First feature across samples |
| `features[:, :1]` | `(3, 1)` | First feature as a 2D column |
| `features.mean(axis=0)` | `(2,)` | One mean per feature |
| `features.mean(axis=1)` | `(3,)` | One mean per sample |

The difference between `(3,)` and `(3, 1)` may look small, but it often determines whether later numerical code behaves correctly.

---

# 22. Thinking Like an AI Engineer

When processing numerical data, ask:

### Data Representation

- Is this data truly numerical, or would a dictionary/list of records communicate it better?
- How many observations and features are present?
- What does each array axis represent?

### Numerical Correctness

- What is the expected `dtype`?
- Are there invalid values, empty arrays, or constant features?
- Could mutation change the original data unexpectedly?

### Vectorization

- Can the operation be expressed clearly over the whole array?
- Is broadcasting doing what I intended?
- Would a normal loop be clearer for complex branching logic?

### Engineering Quality

- Are transformations documented?
- Can the expected shape be explained?
- Can another developer reproduce the calculation?
- Have I verified the output with a small, understandable example?

The engineering goal is not simply to eliminate every `for` loop.

> **The goal is to use a numerical representation and operation that are correct, clear, and appropriate for the problem.**

---

# 23. Responsible Use of AI Tools

AI assistants can help you reason about arrays, but you must remain able to explain the calculation.

Useful learning prompts:

- "Explain the difference between a Python list and a NumPy array using one small numerical example."
- "Do not solve my exercise. Help me determine the shape of each intermediate array."
- "Show why these two shapes cannot broadcast together."
- "Explain why changing a sliced array also changed the original."
- "Help me trace `axis=0` and `axis=1` on this 2D matrix."
- "Give me three edge cases for this NumPy data-cleaning function."
- "Review whether this vectorized expression is readable and correct; explain the reasoning."
- "Explain what numerical calculation `features @ weights` represents."

You are responsible for testing and explaining AI-generated suggestions. An answer that produces code without understanding does not demonstrate programming skill.

---

# 24. Self-Check Questions

1. What problem does NumPy solve that a normal Python list does not solve as conveniently?
2. What is an `ndarray`?
3. What is the difference between `shape`, `ndim`, `size`, and `dtype`?
4. What shape does a one-dimensional array of five values have?
5. How can a list be converted into a NumPy array?
6. What is the difference between `np.arange()` and `np.linspace()`?
7. How does `+` behave differently on two lists and two NumPy arrays?
8. What is the difference between element-wise operations and matrix multiplication?
9. How do you select the first column of a 2D array?
10. What does `array[:, 1]` mean?
11. What is the difference between an array view and an array copy?
12. What happens when you use `.reshape(-1, 1)` on a one-dimensional array?
13. What is vectorization?
14. What is broadcasting?
15. Why can `(3, 2)` and `(2,)` broadcast together?
16. What is the difference between `axis=0` and `axis=1` for a 2D aggregation?
17. How can a Boolean mask select valid values?
18. Why must array comparisons be combined with `&` instead of `and`?
19. What does `np.where()` do?
20. What is a possible failure when min-max scaling a constant feature?
21. What shape is commonly used to represent samples and features in ML?
22. Why is it important to retain training-derived scaling parameters for later inference?
23. Why should performance claims be measured rather than assumed?
24. When might ordinary Python data structures still be preferable to NumPy?

---

# 25. Looking Ahead

In Week 6, we move from general numerical arrays to **Pandas & Data Manipulation**:

- Pandas `Series` and `DataFrame`
- reading datasets
- selecting and filtering records
- transformations and aggregations
- inspecting data quality

Week 4 taught you **how to choose Python data structures**.

Week 5 teaches you **how to calculate efficiently with numerical arrays**.

Week 6 will teach you **how to work with labeled, tabular datasets**.

---

## Week 5 Key Message

> **NumPy lets us express numerical computations over arrays instead of manually managing every value.**

The most important skills are understanding **array shapes, types, indexing, vectorized operations, broadcasting, and numerical correctness**. These skills form part of the foundation for data analysis and machine-learning systems.
