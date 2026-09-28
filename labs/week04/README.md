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
- Recognize the difference between aliasing and copying.
- Choose a data structure based on what the data means.
- Refactor unclear code into cleaner, more maintainable code.
- Organize related data-processing logic across files.
- Document and commit your work clearly in GitHub.

No external Python libraries are required.

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

Write a program that:

1. Prints the first and last score.
2. Prints the middle four values using slicing.
3. Adds a new score of `0.89`.
4. Replaces `0.78` with `0.80`.
5. Counts how many values are greater than or equal to `0.85`.
6. Finds the highest and lowest scores.
7. Prints a sorted version from highest to lowest.

### Requirement

Do not lose the original order when creating the sorted result.

### Hint

Think about the difference between:

```python
list.sort()
```

and:

```python
sorted(list)
```

### Concepts

- Lists
- Indexing
- Slicing
- Mutation
- `append()`
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

### Reflection

Explain why accidental mutation could be dangerous in:

- preprocessing
- experiment tracking
- model evaluation

### Concepts

- Mutability
- Aliasing
- Copying
- Data integrity

---

# 6. Task 3 — Tuples for Fixed Data

## Problem

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
4. Attempts to explain why these values are better represented as tuples than as changing lists.

### Additional Requirement

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

Then unpack the result.

### Concepts

- Tuples
- Immutability
- Tuple unpacking
- Multiple return values

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

# 12. Task 9 — Clean Code Refactoring Challenge

This is the main Week 4 engineering task.

You are given this code:

```python
x = [
    {"i": 1, "l": "spam", "c": 0.94},
    {"i": 2, "l": "ham", "c": 0.72},
    {"i": 3, "l": "unknown", "c": 0.41},
    {"i": 4, "l": "spam", "c": 0.89}
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

The code works, but it is difficult to read.

Refactor it into:

```text
task09_clean_code/
├── main.py
├── preprocessing.py
└── analysis.py
```

---

## `preprocessing.py`

Create:

```python
def filter_by_confidence(predictions, min_confidence):
    ...
```

Responsibility:

```text
all predictions → selected predictions
```

---

## `analysis.py`

Create:

```python
def count_by_label(predictions):
    ...
```

and:

```python
def get_unique_labels(predictions):
    ...
```

Responsibilities:

```text
predictions → label counts
predictions → unique labels
```

---

## `main.py`

Use descriptive data:

```python
predictions = [
    {
        "id": 1,
        "label": "spam",
        "confidence": 0.94
    },
    ...
]
```

Create a named constant:

```python
MIN_CONFIDENCE = 0.80
```

Then:

1. filter predictions
2. count labels
3. collect unique labels
4. create a summary dictionary
5. print a readable report

### Expected Flow

```text
Prediction Records
       ↓
filter_by_confidence(...)
       ↓
Selected Predictions
       ↓
count_by_label(...)
       ↓
get_unique_labels(...)
       ↓
Summary
```

### Clean-Code Requirements

Your refactoring must improve:

- naming
- file organization
- readability
- use of constants
- separation of responsibilities

### README Question

Explain:

> What was wrong with the original code even though it produced correct output?

---

# 13. Optional Challenge — Mini Prediction Analysis

Use:

```python
predictions = [
    {"id": 101, "label": "spam", "confidence": 0.97},
    {"id": 102, "label": "ham", "confidence": 0.83},
    {"id": 103, "label": "spam", "confidence": 0.61},
    {"id": 104, "label": "promotion", "confidence": 0.88},
    {"id": 105, "label": "ham", "confidence": 0.92}
]
```

Build a program that produces:

```python
{
    "total": ...,
    "labels": ...,
    "high_confidence": ...,
    "label_counts": ...,
    "average_confidence": ...
}
```

Use at least:

- one list
- one dictionary
- one set
- one comprehension
- one function

### Additional Challenge

Flag unexpected labels against:

```python
ALLOWED_LABELS = {"spam", "ham"}
```

using set difference.

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
- Data structure selection
- Clean code
- File organization

## Tasks Completed
1. Lists
2. Aliasing vs Copying
3. Tuples
4. Dictionaries
5. Sets
6. Comprehensions
7. `enumerate()` and `zip()`
8. Data Structure Selection
9. Clean Code Refactoring

## Data Structure Decisions
Explain why you chose particular structures in Task 8.

## Aliasing vs Copying
Explain what happened in Task 2.

## Clean-Code Refactoring
Explain:
- what was difficult to read in the original code
- what you changed
- why the refactored version is easier to maintain

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
List copying
  ↓
Tuples
  ↓
Dictionaries
  ↓
Nested structures
  ↓
Sets
  ↓
Comprehensions
  ↓
enumerate() / zip()
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
- [ ] Task 1 demonstrates list operations and non-destructive sorting.
- [ ] Task 2 demonstrates aliasing and copying.
- [ ] Task 3 demonstrates tuples and unpacking.
- [ ] Task 4 uses dictionaries and nested dictionaries.
- [ ] Task 5 uses set operations.
- [ ] Task 6 contains list, dictionary, and set comprehensions.
- [ ] Task 7 uses both `enumerate()` and `zip()`.
- [ ] Task 8 explains data-structure choices.
- [ ] Task 9 is organized into multiple files.
- [ ] Magic values were replaced with meaningful constants where appropriate.
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
- Avoid accidental list aliasing.
- Work with nested data structures.
- Use set operations for validation.
- Write readable comprehensions.
- Use `enumerate()` and `zip()` effectively.
- Select data structures based on meaning and behavior.
- Refactor unclear data-processing code.
- Organize related logic into appropriate files.
- Write cleaner and more maintainable Python programs.

---

## Lab 04 Key Message

> **Choosing the right data structure is part of solving the problem, not just storing the data.**

Week 4 is where your Python programs begin to represent data more intentionally and more professionally.
