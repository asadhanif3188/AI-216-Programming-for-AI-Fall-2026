# AI-216: Programming for Artificial Intelligence
## Lab 04 — Python Data Structures & Clean Code

**Semester:** Fall 2026  
**Week:** 04  
**Topic:** Lists, tuples, dictionaries, sets, comprehensions, data modeling, file organization, and clean code

---

## 1. Objective

This lab develops your ability to choose and use appropriate Python data structures for AI-oriented programming tasks.

By the end of the lab, you should be able to:

- Use lists to store and process ordered collections.
- Use tuples for fixed groups of related values.
- Use dictionaries for structured records and configuration.
- Use sets for unique values, membership checks, and set operations.
- Write readable list, dictionary, and set comprehensions.
- Use `enumerate()` and `zip()` effectively.
- Recognize the difference between aliasing, shallow copying, and deep copying.
- Use tuples as dictionary keys and explain why lists cannot be keys.
- Sort and count structured records.
- Choose a data structure based on what the data means.
- Refactor unclear code into cleaner, more maintainable code without changing its behavior.
- Extend a data-processing program to handle incomplete and inconsistent records.
- Organize related data-processing logic across files.
- Document and commit your work clearly in GitHub.

No external Python libraries are required. The standard-library modules `copy` and `collections` are allowed.

---

## 2. Before You Start

Make sure your Week 3 work is complete and your repository is working.

You should be able to run:

```bash
python --version
git status
git log --oneline
```

Create the Week 4 folder:

```text
labs/
└── week04/
```

Recommended structure:

```text
labs/week04/
├── task01_lists.py
├── task02_copying.py
├── task03_tuples.py
├── task04_dictionaries.py
├── task05_sets.py
├── task06_comprehensions.py
├── task07_iteration_tools.py
├── task08_data_modeling.py
├── task09_clean_code/
│   ├── main.py
│   ├── preprocessing.py
│   └── analysis.py
├── optional_label_report.py
└── README.md
```

You may use slightly different filenames if instructed, but your folder should remain easy to understand.

---

## 3. General Coding Expectations

For every task:

1. Read the full problem before writing code.
2. Decide which data structure best matches the requirement.
3. Use meaningful variable names.
4. Do not modify data accidentally.
5. Prefer clear code over short code.
6. Test normal and edge cases.
7. Use functions where repeated logic appears.
8. Keep responsibilities separated.
9. Do not submit code you cannot explain.
10. Commit meaningful progress to GitHub.

---

# 4. Task 1 — Working with Lists

## Problem

An AI experiment produced the following accuracy scores:

```python
accuracies = [0.82, 0.91, 0.87, 0.78, 0.93, 0.85]
```

Write a program that performs these steps **in order**:

1. Prints the first and last score.
2. Prints the middle four values using slicing.
3. Adds one new score, `0.89`, using `append()`.
4. Adds a batch of two new scores, `[0.84, 0.90]`, using `extend()`.
5. Replaces `0.78` with `0.80`. Use `index()` to find its position — do not hard-code the position.
6. Prints the updated list.
7. Counts how many values are greater than or equal to `0.85`.
8. Finds the highest and lowest scores.
9. Prints a sorted version from highest to lowest.
10. Prints the list again to show it is still in evaluation order.

### Requirement

Do not lose the original order when creating the sorted result.

Store `0.85` in a named constant rather than repeating it.

### Hint

Think about the difference between:

```python
list.sort()
```

and:

```python
sorted(list)
```

Also try `values.append([0.84, 0.90])` once and print the result. What went wrong, and why does `extend()` fix it?

### Expected Output

Your labels may differ, but the values should match:

```text
First: 0.82
Last: 0.85
Middle four: [0.91, 0.87, 0.78, 0.93]
Updated: [0.82, 0.91, 0.87, 0.8, 0.93, 0.85, 0.89, 0.84, 0.9]
Scores >= 0.85: 6
Highest: 0.93
Lowest: 0.8
Sorted (high to low): [0.93, 0.91, 0.9, 0.89, 0.87, 0.85, 0.84, 0.82, 0.8]
Original order kept: [0.82, 0.91, 0.87, 0.8, 0.93, 0.85, 0.89, 0.84, 0.9]
```

Python prints `0.80` as `0.8` — the value is the same.

### Concepts

- Lists
- Indexing
- Slicing
- Mutation
- `append()` vs `extend()`
- `index()`
- Membership/iteration
- `sorted()`

---

# 5. Task 2 — Aliasing vs Copying

## Part A — Observe the Problem

Run:

```python
original_scores = [70, 80, 90]

processed_scores = original_scores

processed_scores.append(100)

print("Original:", original_scores)
print("Processed:", processed_scores)
```

In your README, explain:

- Why did both variables change?
- Did Python create two separate lists?

---

## Part B — Fix the Problem

Refactor using:

```python
.copy()
```

so that the original list remains unchanged.

### Required Output

Your final version should show:

```text
Original: [70, 80, 90]
Processed: [70, 80, 90, 100]
```

---

## Part C — Shallow Copy vs Deep Copy

Now the data is a **list of dictionaries**:

```python
raw_predictions = [
    {"id": 1, "label": "Spam"},
    {"id": 2, "label": "HAM"}
]
```

1. Make a copy with `.copy()`, change the first record's label to `"spam"` **in the copy**, then print `raw_predictions`.
2. Recreate `raw_predictions`, make a copy with `copy.deepcopy()`, make the same change, and print both lists.

### Required Output

```text
Raw after shallow copy edit: [{'id': 1, 'label': 'spam'}, {'id': 2, 'label': 'HAM'}]
Raw after deep copy edit: [{'id': 1, 'label': 'Spam'}, {'id': 2, 'label': 'HAM'}]
Deep copy: [{'id': 1, 'label': 'spam'}, {'id': 2, 'label': 'HAM'}]
```

In your README, explain why `.copy()` fixed Part B but did **not** protect the raw data in Part C.

### Reflection

Explain why accidental mutation could be dangerous in:

- preprocessing
- experiment tracking
- model evaluation

### Concepts

- Mutability
- Aliasing
- Shallow copy
- Deep copy
- Data integrity

---

# 6. Task 3 — Tuples for Fixed Data

## Part A — Unpacking Fixed Data

A computer-vision application stores:

```python
image_size = (224, 224)
model_result = ("baseline_cnn", 0.91)
```

Write a program that:

1. Unpacks `image_size` into:
   - `width`
   - `height`
2. Unpacks `model_result` into:
   - `model_name`
   - `accuracy`
3. Prints a readable summary.
4. Explains, in a comment or your README, why these values are better represented as tuples than as changing lists.

---

## Part B — Returning a Tuple

You wrote a similar function in Week 3. This time, focus on the fact that the returned value **is a tuple**.

Create a function:

```python
def summarize_scores(scores):
    ...
```

that returns:

```text
minimum, maximum, average
```

as a tuple.

If `scores` is empty, return `None` (the same convention as Week 3).

Then:

1. Call it with `[72, 88, 91, 67]` and unpack the result.
2. Call it with `[]` and print the result.

Why must you check for `None` **before** unpacking?

---

## Part C — Tuples as Dictionary Keys

A model registry maps an input image size to the model that expects it:

```python
input_models = {
    (224, 224): "resnet50",
    (299, 299): "inception_v3",
    (384, 384): "vit_base"
}
```

1. Look up and print the model for a `299 x 299` image.
2. Try creating the same dictionary with a **list** as a key (`[224, 224]`). Catch the error with `try`/`except` and print it.
3. In your README, explain why a tuple can be a dictionary key but a list cannot.

### Expected Output

```text
Image size: 224 x 224
Model: baseline_cnn | Accuracy: 0.91
Minimum: 67 | Maximum: 91 | Average: 79.5
Empty: None
Model for 299x299: inception_v3
TypeError: unhashable type: 'list'
```

### Concepts

- Tuples
- Immutability
- Tuple unpacking
- Multiple return values
- Hashability
- Tuples as dictionary keys

---

# 7. Task 4 — Dictionaries for Structured Records

## Problem

Represent this AI model using a dictionary:

```text
Name: spam_classifier
Version: 2
Accuracy: 0.92
Threshold: 0.80
Status: evaluated
```

Your program should:

1. Create the dictionary.
2. Print the model name.
3. Update the accuracy to `0.94`.
4. Add:

```text
owner = "AI-216 Team"
```

5. Read a missing field using:

```python
.get()
```

with a sensible default.
6. Iterate through all key-value pairs.

---

## Part B — Nested Dictionary

Add nested metrics:

```python
"metrics": {
    "accuracy": 0.94,
    "precision": 0.91,
    "recall": 0.89
}
```

Print:

```text
precision
recall
```

without printing the whole dictionary.

### Concepts

- Dictionaries
- Keys and values
- `get()`
- `items()`
- Nested dictionaries
- Structured metadata

---

# 8. Task 5 — Sets for Unique Labels & Validation

## Problem

You have:

```python
training_labels = [
    "spam",
    "ham",
    "spam",
    "promotion",
    "ham"
]

test_labels = [
    "spam",
    "ham",
    "unknown",
    "promotion"
]
```

Your program should:

1. Convert both collections into sets.
2. Print unique training labels.
3. Print labels present in both datasets.
4. Print labels present in the test set but not the training set.
5. Print labels present in either dataset.

Sets have no fixed order, so print each result with `sorted(...)` to get predictable output.

### Expected Output

```text
Unique training labels: ['ham', 'promotion', 'spam']
In both: ['ham', 'promotion', 'spam']
Only in test: ['unknown']
In either: ['ham', 'promotion', 'spam', 'unknown']
```

### Required Operations

Use:

```python
|
&
-
```

for:

- union
- intersection
- difference

### Engineering Question

In your README, explain:

> Why is a set better than a list for checking unexpected class labels?

Your answer should mention both **clarity** (set operations express the intent directly) and **speed** (hash-based membership checks).

### Optional Experiment

Use the `timeit` example from the lecture (Section 14.4) to compare `in` on a list and a set of 1,000,000 IDs. Record your timings in the README.

### Concepts

- Sets
- Unique values
- Membership
- Union
- Intersection
- Difference
- Data validation

---

# 9. Task 6 — Comprehensions

Complete all three parts.

---

## Part A — List Comprehension

Given:

```python
raw_scores = [78, -5, 92, 110, 67, 85]
```

Create a list containing only valid scores:

```text
0 <= score <= 100
```

Then create a second list containing normalized scores:

```text
score / 100
```

Do this using list comprehensions.

---

## Part B — Dictionary Comprehension

Given:

```python
student_scores = {
    "Ali": 72,
    "Sara": 91,
    "Ahmed": 45,
    "Fatima": 88
}
```

Create:

```python
pass_status
```

where each student maps to:

```python
True
```

or:

```python
False
```

based on:

```text
score >= 50
```

---

## Part C — Set Comprehension

Given:

```python
labels = [
    "Spam",
    "HAM",
    "spam",
    "Ham",
    "UNKNOWN"
]
```

Create a set containing normalized lowercase labels.

### Expected Output

```text
Valid: [78, 92, 67, 85]
Normalized: [0.78, 0.92, 0.67, 0.85]
Pass status: {'Ali': True, 'Sara': True, 'Ahmed': False, 'Fatima': True}
Labels: ['ham', 'spam', 'unknown']
```

(The set in Part C is printed with `sorted(...)`.)

### Readability Requirement

If your comprehension becomes difficult to explain in one sentence, rewrite it as a normal loop.

### Concepts

- List comprehension
- Dictionary comprehension
- Set comprehension
- Filtering
- Transformation
- Readability

---

# 10. Task 7 — `enumerate()` & `zip()`

## Part A — `enumerate()`

Given:

```python
experiments = [
    0.81,
    0.86,
    0.79,
    0.91
]
```

Print:

```text
Experiment 1: 0.81
Experiment 2: 0.86
...
```

using:

```python
enumerate(..., start=1)
```

---

## Part B — `zip()`

Given:

```python
predictions = [True, False, True, True]
actual = [True, False, False, True]
```

Use `zip()` to:

1. Compare each predicted value with the actual value.
2. Count correct predictions.
3. Calculate accuracy.

### Required Output

Print each comparison:

```text
Predicted: True | Actual: True | Correct: True
```

Then print final accuracy.

### Concepts

- `enumerate()`
- `zip()`
- Parallel iteration
- Accuracy calculation

---

# 11. Task 8 — Choose the Right Data Structure

For each scenario below, choose one:

```text
list
tuple
dictionary
set
```

Then write a short explanation in `README.md`.

### Scenario A

Store model names in the exact order they were evaluated.

### Scenario B

Store a fixed image size:

```text
224 x 224
```

### Scenario C

Store:

```text
model name
threshold
version
debug status
```

with meaningful field names.

### Scenario D

Store all unique class labels.

### Scenario E

Store many prediction records such as:

```text
id
label
confidence
```

### Scenario F

Compare allowed labels with labels received from an external source.

---

## Implementation Requirement

Implement at least **three** of the scenarios in Python.

### Concepts

- Data modeling
- Choosing appropriate structures
- Communicating intent through structure

---

# 12. Task 9 — Prediction Analysis: Refactor, Then Extend

This is the main Week 4 engineering task. It has two parts:

- **Part A** — refactor messy code **without changing what it does**.
- **Part B** — extend the clean version to handle a realistic batch of data that includes incomplete and inconsistent records.

---

## Part A — Refactor Without Changing Behavior

You are given this code:

```python
x = [
    {"i": 1, "l": "spam", "c": 0.94},
    {"i": 2, "l": "ham", "c": 0.72},
    {"i": 3, "l": "promotion", "c": 0.41},
    {"i": 4, "l": "spam", "c": 0.89},
    {"i": 5, "l": "ham", "c": 0.97},
    {"i": 6, "l": "promotion", "c": 0.83},
    {"i": 7, "l": "ham", "c": 0.58}
]

y = []

for z in x:
    if z["c"] >= 0.8:
        y.append(z)

d = {}

for z in x:
    if z["l"] not in d:
        d[z["l"]] = 0
    d[z["l"]] += 1

print(y)
print(d)
```

Run it first and save its output. The code works, but it is difficult to read.

Notice what it actually computes:

- `y` — the predictions with confidence at or above `0.8`
- `d` — label counts over **all** predictions, not only the high-confidence ones

Your refactored version must produce the **same results** from the same data.

Refactor it into:

```text
task09_clean_code/
├── main.py
├── preprocessing.py
└── analysis.py
```

### `preprocessing.py`

```python
def filter_by_confidence(predictions, min_confidence):
    ...
```

Responsibility:

```text
all predictions → predictions at or above the threshold
```

### `analysis.py`

```python
def count_by_label(predictions):
    ...


def get_unique_labels(predictions):
    ...
```

Responsibilities:

```text
predictions → label counts   (dictionary)
predictions → unique labels  (set)
```

### `main.py`

Rewrite the data with descriptive keys:

```python
initial_predictions = [
    {"id": 1, "label": "spam", "confidence": 0.94},
    ...
]
```

Create a named constant:

```python
MIN_CONFIDENCE = 0.80
```

Then call the functions and print the results.

### Expected Flow

Filtering and counting are **separate branches** that both start from the full list:

```text
                 Prediction Records
                          │
        ┌─────────────────┼──────────────────┐
        ↓                 ↓                  ↓
filter_by_confidence  count_by_label   get_unique_labels
        ↓                 ↓                  ↓
 Selected predictions   Label counts     Unique labels
        └─────────────────┼──────────────────┘
                          ↓
                       Report
```

### Expected Output (Part A)

```text
Selected IDs: [1, 4, 5, 6]
Label counts: {'spam': 2, 'ham': 3, 'promotion': 2}
Unique labels: ['ham', 'promotion', 'spam']
```

Check these against the output you saved from the original code.

### How to Run

Run from inside the task folder so the imports resolve:

```bash
cd labs/week04/task09_clean_code
python main.py
```

---

## Part B — Extend to a Realistic Batch

A second batch of predictions arrives from another service. It is not as clean:

```python
new_batch = [
    {"id": 8, "label": "Spam", "confidence": 0.91},
    {"id": 9, "label": "ham"},
    {"id": 10, "label": "unknown", "confidence": 0.86},
    {"id": 11, "label": " HAM ", "confidence": 0.66}
]
```

Problems in this batch:

- record `9` has no `confidence`
- records `8` and `11` use inconsistent capitalization and spacing
- record `10` has a label the system does not recognize

Combine both batches into one list, then produce a summary report.

### Additional Constants in `main.py`

```python
ALLOWED_LABELS = {"spam", "ham", "promotion"}
REQUIRED_FIELDS = ("id", "label", "confidence")
TOP_COUNT = 3
```

Think about why `ALLOWED_LABELS` is a set and `REQUIRED_FIELDS` is a tuple.

### Additional Functions in `preprocessing.py`

```python
def split_complete_records(predictions, required_fields):
    ...
```

Returns a tuple `(complete, incomplete)`. A record is complete only if it contains every required field.

**Rule for missing data:** incomplete records are **skipped** from the analysis, and their IDs are **reported** — they are not silently dropped.

```python
def normalize_labels(predictions):
    ...
```

Returns a **new** list in which every label is stripped of surrounding spaces and lowercased.

It must **not** modify the records it receives. (Hint: copy each record with `record.copy()` before changing its label.)

### Additional Functions in `analysis.py`

```python
def find_unexpected_labels(predictions, allowed_labels):
    ...


def average_confidence(predictions):
    ...


def top_predictions(predictions, count):
    ...
```

- `find_unexpected_labels` uses **set difference**.
- `average_confidence` returns `None` for an empty list.
- `top_predictions` uses `sorted(..., key=..., reverse=True)` and a slice.

### Processing Order

```text
initial_predictions + new_batch
        ↓
split_complete_records(...)  →  skipped IDs
        ↓
normalize_labels(...)
        ↓
analysis functions
        ↓
summary dictionary
        ↓
readable report
```

### Summary Dictionary

Build a dictionary with at least these keys before printing anything:

```python
summary = {
    "total_records": ...,
    "valid_records": ...,
    "skipped_ids": ...,
    "labels": ...,
    "label_counts": ...,
    "high_confidence_count": ...,
    "unexpected_labels": ...,
    "average_confidence": ...,
    "top_ids": ...
}
```

Keeping the calculation (building `summary`) separate from the display (printing it) is a clean-code requirement.

### Expected Output (Part B)

Your layout may differ, but the values should match:

```text
=== Prediction Report ===
Total records:          11
Valid records:          10
Skipped (missing data): [9]
Labels:                 ['ham', 'promotion', 'spam', 'unknown']
Label counts:           {'spam': 3, 'ham': 4, 'promotion': 2, 'unknown': 1}
High confidence (>= 0.8): 6
Unexpected labels:      ['unknown']
Average confidence:     0.777
Top 3 by confidence:   [5, 1, 8]
```

Finally, print the label of record `8` in `new_batch`. It must still be `'Spam'` — proof that `normalize_labels` did not modify the raw data.

### Clean-Code Requirements

Your solution must show:

- meaningful names (no `x`, `y`, `z`, `d`)
- named constants instead of magic values
- one clear responsibility per function
- functions that return new data instead of mutating their inputs
- `main.py` that coordinates the workflow but does not contain detailed processing logic

### README Questions

1. What was wrong with the original code even though it produced correct output?
2. How did you confirm that your Part A refactor did not change the behavior?
3. Why are incomplete records reported instead of silently skipped?
4. Which data structure did you use for each part of the summary, and why?

---

# 13. Optional Challenge — Per-Label Confidence Report

Using the cleaned records from Task 9 Part B, build a report that shows, **for each label**:

- how many predictions it has
- its average confidence
- its highest confidence

Example layout:

```text
label       count  avg_conf  max_conf
ham             4     0.732      0.97
promotion       2     0.620      0.83
spam            3     0.913      0.94
unknown         1     0.860      0.86
```

Requirements:

- Group confidences by label into a dictionary of lists.
- Print labels in alphabetical order.
- Implement it **twice**: once with a plain dictionary, and once with `collections.defaultdict(list)`.
- Use `collections.Counter` to find the single most common label.

In your README, compare the two versions: which is clearer, and what does `defaultdict` save you from writing?

This is the same "group by, then aggregate" idea you will use in Week 6 with Pandas.

---

# 14. Testing Requirements

Do not test only the sample values.

For each relevant task, test:

```text
normal input
empty collection
duplicate values
boundary values
unexpected labels
missing dictionary keys
```

Examples:

```python
[]
[50]
[50, 50, 50]
```

Dictionary example:

```python
record = {
    "label": "spam"
}
```

Ask:

> What should happen if `"confidence"` is missing?

In Task 9 Part B the answer is decided for you: **skip the record and report its ID**. For other tasks, decide on a rule, apply it consistently, and write it down in your README. Options include:

```text
skip and report      → split_complete_records(...)
use a safe default   → record.get("confidence", 0.0)
stop with an error   → raise ValueError(...)   (Week 3 exception handling)
```

Specific cases worth testing in Task 9:

```text
filter_by_confidence([], MIN_CONFIDENCE)          → []
average_confidence([])                            → None
record with confidence exactly 0.80               → included (>=)
normalize_labels(...) then check the raw records  → unchanged
```

You do not have to solve every possible production case yet, but you should recognize where edge cases exist.

---

# 15. Code Quality Expectations

Your code should demonstrate Week 4 concepts.

## Use meaningful names

Prefer:

```python
prediction_records
```

over:

```python
x
```

## Avoid magic values

Prefer:

```python
MIN_CONFIDENCE = 0.80
```

over repeating:

```python
0.80
```

throughout the program.

## Keep comprehensions readable

Prefer a normal loop if the comprehension becomes difficult to explain.

## Choose structures intentionally

Do not use a list automatically for every problem.

## Do not mutate your inputs

A function should return new data rather than silently changing the list or dictionaries passed to it.

## Avoid mutable default arguments

Prefer:

```python
def add_label(label, labels=None):
    if labels is None:
        labels = []
    ...
```

over:

```python
def add_label(label, labels=[]):
    ...
```

## Keep output predictable

When printing a set, use `sorted(...)` so the output is the same on every run.

## Do not over-engineer

A simple dictionary may be better than creating a class for a small record.

---

# 16. Git Workflow

Do not wait until the entire lab is complete before committing.

A reasonable progression:

```bash
git add labs/week04/task01_lists.py
git commit -m "Lab04: add list processing exercises"

git add labs/week04/task04_dictionaries.py
git commit -m "Lab04: add structured dictionary exercises"

git add labs/week04/task05_sets.py
git commit -m "Lab04: add set validation exercises"

git add labs/week04/task06_comprehensions.py
git commit -m "Lab04: add collection comprehensions"

git add labs/week04/task09_clean_code/
git commit -m "Lab04: refactor prediction analysis for clean code"

git add labs/week04/task09_clean_code/
git commit -m "Lab04: handle incomplete and inconsistent prediction records"

git push
```

Your exact commit structure may differ.

The important requirement is:

> each commit should represent meaningful progress.

---

# 17. Week 4 README

Create:

```text
labs/week04/README.md
```

Suggested structure:

```markdown
# Lab 04 — Python Data Structures & Clean Code

## Concepts Practiced
- Lists
- Tuples
- Dictionaries
- Sets
- Comprehensions
- `enumerate()`
- `zip()`
- Sorting and counting records
- Data structure selection
- Clean code
- File organization

## Tasks Completed
1. Lists
2. Aliasing vs Copying (including shallow vs deep copy)
3. Tuples (including tuples as dictionary keys)
4. Dictionaries
5. Sets
6. Comprehensions
7. `enumerate()` and `zip()`
8. Data Structure Selection
9. Prediction Analysis — Part A (refactor) and Part B (extend)

## Data Structure Decisions
Explain why you chose particular structures in Task 8.

## Aliasing vs Copying
Explain what happened in Task 2, including why `.copy()` was not enough in Part C.

## Hashability
Explain why a tuple can be a dictionary key but a list cannot (Task 3 Part C).

## Sets vs Lists for Validation
Answer the Task 5 engineering question. Include your timings if you did the optional experiment.

## Clean-Code Refactoring
Explain:
- what was difficult to read in the original code
- what you changed
- how you confirmed the behavior did not change
- why the refactored version is easier to maintain

## Handling Incomplete Records
Explain your Task 9 Part B rules for missing fields and inconsistent labels.

## Edge Cases Tested
Describe at least three useful test cases.

## What I Found Difficult
-

## What I Learned
-

## AI Engineering Relevance
Explain how data structures and clean code affect:
- preprocessing
- configuration
- model outputs
- validation
- maintainability
```

---

# 18. AI-Assisted Learning

AI tools may be used to support learning unless otherwise instructed.

Useful prompts:

- "Do not solve the task. Which data structure best fits this requirement, and why?"
- "Explain why this list changed when I modified another variable."
- "Why did changing a record in my copied list also change the original records?"
- "Help me compare a tuple and list for this scenario."
- "Explain what this nested dictionary represents."
- "Give me three test cases for this dictionary-processing function."
- "Is this comprehension readable, or should I use a loop?"
- "Review my variable names without rewriting the program."
- "Explain whether this code contains magic values."
- "Review my file responsibilities without writing code for me."

If AI assistance was used, add:

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

You remain responsible for understanding every submitted line.

---

# 19. Self-Study Learning Resources

## Python — Data Structures

https://docs.python.org/3/tutorial/datastructures.html

Recommended topics:

- More on Lists
- Tuples and Sequences
- Sets
- Dictionaries
- Looping Techniques

---

## Python — List Comprehensions

https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions

Use this to review:

- transformation
- filtering
- nested expressions

---

## Python — Dictionaries

https://docs.python.org/3/tutorial/datastructures.html#dictionaries

Focus on:

- key-value access
- `items()`
- dictionary construction

---

## Python — Sets

https://docs.python.org/3/tutorial/datastructures.html#sets

Focus on:

- unique values
- membership
- union
- intersection
- difference

---

## Python — Sorting Techniques

https://docs.python.org/3/howto/sorting.html

Focus on:

- `sorted()` vs `list.sort()`
- key functions
- `reverse=True`

---

## Python — `copy` Module

https://docs.python.org/3/library/copy.html

Focus on:

- shallow vs deep copy

---

## Python — `collections` Module

https://docs.python.org/3/library/collections.html

Focus on:

- `Counter`
- `defaultdict`

---

## PEP 8 — Style Guide for Python Code

https://peps.python.org/pep-0008/

You do not need to memorize the full document.

For now, pay attention to:

- naming
- indentation
- whitespace
- readability

---

## Suggested Self-Study Path

```text
Lists
  ↓
List copying (shallow vs deep)
  ↓
Tuples
  ↓
Dictionaries
  ↓
Nested structures
  ↓
Sets and hashability
  ↓
Comprehensions
  ↓
enumerate() / zip()
  ↓
Sorting and counting records
  ↓
Refactor for readability
```

The goal is not to memorize every method.

The goal is to recognize:

> **what structure best represents the problem you are solving.**

---

# 20. Submission Checklist

Before submitting, verify:

- [ ] All required work is inside `labs/week04/`.
- [ ] Task 1 demonstrates `append()`, `extend()`, `index()`, and non-destructive sorting.
- [ ] Task 2 demonstrates aliasing, shallow copying, and deep copying.
- [ ] Task 3 demonstrates tuples, unpacking, the empty-list case, and tuples as dictionary keys.
- [ ] Task 4 uses dictionaries and nested dictionaries.
- [ ] Task 5 uses set operations and prints sets in sorted order.
- [ ] Task 6 contains list, dictionary, and set comprehensions.
- [ ] Task 7 uses both `enumerate()` and `zip()`.
- [ ] Task 8 explains data-structure choices.
- [ ] Task 9 Part A produces the same results as the original code.
- [ ] Task 9 Part B skips and reports incomplete records, normalizes labels without mutating the raw data, and matches the expected report.
- [ ] Task 9 is organized into multiple files and runs with `python main.py` from its folder.
- [ ] Magic values were replaced with meaningful constants where appropriate.
- [ ] No function uses a mutable default argument.
- [ ] `labs/week04/README.md` exists.
- [ ] AI Usage Log is included if AI assistance was used.
- [ ] Multiple meaningful Git commits are visible.
- [ ] All work is pushed to GitHub.
- [ ] You can explain why each chosen data structure is appropriate.

---

# 21. Expected Learning Outcomes

After completing this lab, you should be able to:

- Use lists, tuples, dictionaries, and sets correctly.
- Explain mutable vs immutable structures.
- Avoid accidental aliasing, and choose between shallow and deep copies.
- Explain hashability and use tuples as dictionary keys.
- Work with nested data structures.
- Use set operations for validation, and explain why set lookups are fast.
- Write readable comprehensions.
- Use `enumerate()` and `zip()` effectively.
- Sort and count lists of dictionaries.
- Select data structures based on meaning and behavior.
- Refactor unclear data-processing code without changing its behavior.
- Handle incomplete and inconsistent records deliberately.
- Organize related logic into appropriate files.
- Write cleaner and more maintainable Python programs.

---

## Lab 04 Key Message

> **Choosing the right data structure is part of solving the problem, not just storing the data.**

Week 4 is where your Python programs begin to represent data more intentionally and more professionally.
