# AI-216: Programming for Artificial Intelligence
## Week 04 Lecture Handout — Python Data Structures & Clean Code

**Semester:** Fall 2026  
**Week:** 04  
**Theme:** Organizing data clearly so AI programs remain readable, reliable, and maintainable

---

## 1. Why Week 4 Matters

In Week 2, you learned how to express logic.

In Week 3, you learned how to structure that logic using functions, modules, exceptions, debugging, and classes.

Week 4 focuses on the other half of most programs:

> **How should we organize the data that our logic works on?**

AI applications constantly work with collections of information:

```text
sensor readings
student records
feature values
labels
configuration settings
API responses
model outputs
unique categories
```

Choosing the wrong data structure can make code difficult to understand.

Choosing the right one can make the solution almost explain itself.

This week focuses on:

```text
Many Values
    ↓
Lists
    ↓
Tuples
    ↓
Dictionaries
    ↓
Sets
    ↓
Comprehensions
    ↓
Choosing the Right Structure
    ↓
Clean Code
    ↓
Better Organized AI Programs
```

The goal is not to memorize methods.

The goal is to learn how to represent data in a way that makes your program **clear, correct, and easy to extend**.

---

## 2. Week 4 Learning Outcomes

By the end of this lecture, you should be able to:

1. Use lists to store and process ordered collections of values.
2. Explain the difference between mutable and immutable data structures.
3. Use tuples when data should remain fixed.
4. Use dictionaries to represent key-value relationships and structured records.
5. Use sets to represent unique values and perform membership/set operations.
6. Write readable list, dictionary, and set comprehensions.
7. Iterate effectively using `enumerate()`, `zip()`, and dictionary methods.
8. Choose an appropriate data structure for a given problem.
9. Organize Python files and data-related code clearly.
10. Apply practical clean-code principles to data-oriented Python programs.

---

## 3. From Individual Values to Collections

A single variable stores one value:

```python
score = 85
```

But real programs usually work with many related values:

```python
scores = [72, 85, 91, 67, 88]
```

Or structured records:

```python
student = {
    "name": "Ali",
    "score": 85,
    "passed": True
}
```

Or unique categories:

```python
labels = {"spam", "ham", "unknown"}
```

The data structure you choose should answer questions such as:

- Does order matter?
- Can values repeat?
- Should the data change?
- Do I need named fields?
- Do I need membership checks?
- Am I representing one record or many records?

### Basic Example

```python
scores = [72, 85, 91]
print(scores)
```

### Intermediate Example

```python
student = {
    "name": "Sara",
    "score": 91
}

print(student["name"])
```

### Advanced Example

```python
predictions = [
    {"label": "spam", "confidence": 0.94},
    {"label": "ham", "confidence": 0.73},
    {"label": "spam", "confidence": 0.88}
]

labels = {
    prediction["label"]
    for prediction in predictions
}

print(labels)
```

---

# 4. Lists

A **list** stores an ordered collection of items.

Lists are:

- ordered
- mutable
- able to contain duplicate values
- able to contain different Python types

Syntax:

```python
values = [10, 20, 30]
```

## 4.1 Basic Example — Store Multiple Values

```python
scores = [72, 85, 91, 67]

print(scores)
print(scores[0])
print(scores[-1])
```

## 4.2 Intermediate Example — Update and Extend a List

```python
scores = [72, 85, 91]

scores.append(88)
scores[0] = 75

print(scores)
```

## 4.3 Advanced Example — Process Model Evaluation Scores

```python
evaluation_scores = [0.81, 0.88, 0.76, 0.91, 0.85]

qualified_scores = []

for score in evaluation_scores:
    if score >= 0.85:
        qualified_scores.append(score)

average = sum(evaluation_scores) / len(evaluation_scores)

print("All scores:", evaluation_scores)
print("Qualified:", qualified_scores)
print(f"Average: {average:.3f}")
```

Lists are useful for batches of predictions, evaluation scores, feature values, experiment results, and sequences of records.

---

# 5. List Indexing, Slicing & Common Operations

## 5.1 Basic Example — Indexing

```python
models = ["baseline", "tree", "knn"]

print(models[0])
print(models[1])
print(models[-1])
```

## 5.2 Intermediate Example — Slicing

```python
scores = [60, 70, 80, 90, 100]

print(scores[1:4])
print(scores[:3])
print(scores[::2])
```

Slicing uses:

```text
start : stop : step
```

The `stop` position is not included.

## 5.3 Advanced Example — Window of Recent Predictions

```python
predictions = [
    "spam",
    "ham",
    "ham",
    "spam",
    "ham",
    "unknown",
    "ham"
]

recent_predictions = predictions[-3:]

print("Recent predictions:", recent_predictions)

if "unknown" in recent_predictions:
    print("Review recent outputs manually.")
```

---

# 6. Useful List Methods

Common methods include:

```python
append()
extend()
insert()
remove()
pop()
sort()
reverse()
count()
index()
```

## 6.1 Basic Example

```python
values = [10, 20]

values.append(30)

print(values)
```

## 6.2 Intermediate Example

```python
labels = ["cat", "dog", "cat", "bird"]

print(labels.count("cat"))

labels.remove("bird")
labels.append("rabbit")

print(labels)
```

## 6.3 Advanced Example — Maintaining Experiment Results

```python
accuracies = [0.82, 0.91, 0.87, 0.78]

accuracies.sort(reverse=True)

best = accuracies[0]
worst = accuracies[-1]

print("Sorted:", accuracies)
print("Best:", best)
print("Worst:", worst)
```

`list.sort()` changes the original list.

If you want a new sorted list:

```python
sorted_accuracies = sorted(accuracies)
```

---

# 7. Mutability, Aliasing & Copying Lists

Lists are mutable.

That means multiple variables can accidentally refer to the same list.

## 7.1 Basic Example — Mutation

```python
scores = [70, 80, 90]

scores[0] = 75

print(scores)
```

## 7.2 Intermediate Example — Aliasing

```python
original = [10, 20, 30]
copy = original

copy.append(40)

print("Original:", original)
print("Copy:", copy)
```

Both variables refer to the same list.

## 7.3 Advanced Example — Independent Copy

```python
raw_scores = [70, 80, 90]
processed_scores = raw_scores.copy()

processed_scores.append(100)

print("Raw:", raw_scores)
print("Processed:", processed_scores)
```

Accidental mutation can cause subtle bugs in data-processing pipelines.

Ask:

> **Am I modifying the original data, or should I create a new version?**

---

# 8. Tuples

A **tuple** is an ordered collection like a list, but it is immutable.

Syntax:

```python
point = (10, 20)
```

Tuples are useful when values conceptually belong together and should not change.

## 8.1 Basic Example

```python
coordinate = (33.7, 73.1)

print(coordinate[0])
print(coordinate[1])
```

## 8.2 Intermediate Example — Tuple Unpacking

```python
model_result = ("baseline", 0.88)

model_name, accuracy = model_result

print("Model:", model_name)
print("Accuracy:", accuracy)
```

## 8.3 Advanced Example — Returning Multiple Values

```python
def summarize(values):
    minimum = min(values)
    maximum = max(values)
    average = sum(values) / len(values)

    return minimum, maximum, average


scores = [72, 88, 91, 67]

minimum, maximum, average = summarize(scores)

print("Minimum:", minimum)
print("Maximum:", maximum)
print("Average:", average)
```

---

# 9. Lists vs Tuples

Use a list when the collection is expected to change.

Use a tuple when the values should behave as a fixed group.

### Basic Example

```python
training_scores = [0.81, 0.84, 0.87]
training_scores.append(0.90)
```

### Intermediate Example

```python
image_size = (224, 224)
width, height = image_size
```

### Advanced Example

```python
model_results = [
    ("baseline", 0.81),
    ("tree", 0.87),
    ("knn", 0.84)
]

for model_name, accuracy in model_results:
    print(model_name, accuracy)
```

A useful question is:

> **Should this collection change after creation?**

---

# 10. Dictionaries

A **dictionary** stores key-value pairs.

Syntax:

```python
student = {
    "name": "Ali",
    "score": 85
}
```

Dictionaries are ideal when fields have names.

## 10.1 Basic Example — One Structured Record

```python
student = {
    "name": "Ali",
    "score": 85,
    "passed": True
}

print(student["name"])
print(student["score"])
```

## 10.2 Intermediate Example — Updating and Reading Safely

```python
model = {
    "name": "baseline",
    "accuracy": 0.82,
    "version": 1
}

model["accuracy"] = 0.86
model["status"] = "evaluated"

owner = model.get("owner", "unknown")

print(model)
print("Owner:", owner)
```

## 10.3 Advanced Example — AI Model Metadata

```python
model_metadata = {
    "name": "spam_classifier",
    "version": 3,
    "metrics": {
        "accuracy": 0.93,
        "precision": 0.91,
        "recall": 0.89
    },
    "features": [
        "message_length",
        "contains_link",
        "keyword_score"
    ]
}

print("Model:", model_metadata["name"])
print("Accuracy:", model_metadata["metrics"]["accuracy"])
print("Features:", model_metadata["features"])
```

Dictionaries are commonly used for configuration, structured records, API payloads, JSON-like data, model metadata, and metrics.

---

# 11. Dictionary Operations

Useful dictionary methods include:

```python
keys()
values()
items()
get()
update()
pop()
```

## 11.1 Basic Example — Keys and Values

```python
student = {
    "name": "Sara",
    "score": 91
}

print(student.keys())
print(student.values())
```

## 11.2 Intermediate Example — Iterating Key-Value Pairs

```python
metrics = {
    "accuracy": 0.91,
    "precision": 0.88,
    "recall": 0.86
}

for metric, value in metrics.items():
    print(metric, value)
```

## 11.3 Advanced Example — Update Configuration

```python
config = {
    "threshold": 0.80,
    "max_requests": 100,
    "debug": False
}

new_settings = {
    "threshold": 0.85,
    "debug": True
}

config.update(new_settings)

print(config)
```

---

# 12. Nested Data Structures

Real data often combines several structures.

A common pattern is:

```text
list of dictionaries
```

Each dictionary represents one record.

## 12.1 Basic Example

```python
students = [
    {"name": "Ali", "score": 78},
    {"name": "Sara", "score": 91}
]

print(students[0]["name"])
```

## 12.2 Intermediate Example — Filter Records

```python
students = [
    {"name": "Ali", "score": 78},
    {"name": "Sara", "score": 91},
    {"name": "Ahmed", "score": 42}
]

passed_students = []

for student in students:
    if student["score"] >= 50:
        passed_students.append(student)

print(passed_students)
```

## 12.3 Advanced Example — Structured AI Predictions

```python
predictions = [
    {
        "id": 101,
        "prediction": "spam",
        "confidence": 0.96
    },
    {
        "id": 102,
        "prediction": "ham",
        "confidence": 0.73
    },
    {
        "id": 103,
        "prediction": "spam",
        "confidence": 0.61
    }
]

high_confidence = []

for result in predictions:
    if result["confidence"] >= 0.80:
        high_confidence.append(result)

for result in high_confidence:
    print(
        result["id"],
        result["prediction"],
        result["confidence"]
    )
```

This representation is similar to data exchanged through APIs.

---

# 13. Sets

A **set** stores unique values.

Sets are:

- unordered
- mutable
- unable to contain duplicates

Syntax:

```python
categories = {"cat", "dog", "bird"}
```

## 13.1 Basic Example — Remove Duplicates

```python
labels = ["spam", "ham", "spam", "ham", "unknown"]

unique_labels = set(labels)

print(unique_labels)
```

## 13.2 Intermediate Example — Membership

```python
allowed_roles = {"admin", "analyst", "engineer"}

role = "engineer"

if role in allowed_roles:
    print("Access role recognized")
```

## 13.3 Advanced Example — Unique Categories Across Data

```python
records = [
    {"id": 1, "category": "finance"},
    {"id": 2, "category": "health"},
    {"id": 3, "category": "finance"},
    {"id": 4, "category": "education"}
]

categories = set()

for record in records:
    categories.add(record["category"])

print("Unique categories:", categories)
```

---

# 14. Set Operations

Important operations include:

```text
union
intersection
difference
```

## 14.1 Basic Example — Union

```python
dataset_a = {"cat", "dog"}
dataset_b = {"dog", "bird"}

all_labels = dataset_a | dataset_b

print(all_labels)
```

## 14.2 Intermediate Example — Intersection

```python
training_labels = {"cat", "dog", "bird"}
test_labels = {"dog", "bird", "rabbit"}

common = training_labels & test_labels

print("Common labels:", common)
```

## 14.3 Advanced Example — Detect Unexpected Labels

```python
known_labels = {"spam", "ham"}
incoming_labels = {"spam", "ham", "unknown", "promotion"}

unexpected = incoming_labels - known_labels

print("Unexpected labels:", unexpected)

if unexpected:
    print("Data validation required.")
```

Set operations can express validation logic very clearly.

---

# 15. Comprehensions

A comprehension creates a new collection using compact syntax.

Common forms:

```text
list comprehension
dictionary comprehension
set comprehension
```

A comprehension should improve readability.

If it becomes difficult to read, use a normal loop.

---

# 16. List Comprehensions

## 16.1 Basic Example

```python
squares = [
    value ** 2
    for value in range(5)
]

print(squares)
```

## 16.2 Intermediate Example — Filter Values

```python
scores = [72, 45, 88, 39, 91]

passed = [
    score
    for score in scores
    if score >= 50
]

print(passed)
```

## 16.3 Advanced Example — Transform Valid Data

```python
raw_scores = [78, -5, 92, 110, 67]

normalized = [
    score / 100
    for score in raw_scores
    if 0 <= score <= 100
]

print(normalized)
```

This performs filtering and transformation in one expression.

---

# 17. Dictionary Comprehensions

## 17.1 Basic Example

```python
squares = {
    value: value ** 2
    for value in range(5)
}

print(squares)
```

## 17.2 Intermediate Example

```python
scores = {
    "Ali": 72,
    "Sara": 91,
    "Ahmed": 45
}

pass_status = {
    name: score >= 50
    for name, score in scores.items()
}

print(pass_status)
```

## 17.3 Advanced Example — Normalize Metrics

```python
metrics = {
    "accuracy": 91,
    "precision": 88,
    "recall": 86
}

normalized_metrics = {
    name: value / 100
    for name, value in metrics.items()
}

print(normalized_metrics)
```

---

# 18. Set Comprehensions

## 18.1 Basic Example

```python
values = [1, 2, 2, 3, 3, 4]

unique_squares = {
    value ** 2
    for value in values
}

print(unique_squares)
```

## 18.2 Intermediate Example

```python
labels = ["Spam", "HAM", "spam", "Ham"]

normalized_labels = {
    label.lower()
    for label in labels
}

print(normalized_labels)
```

## 18.3 Advanced Example — Extract Unique Valid Categories

```python
records = [
    {"category": "Finance", "valid": True},
    {"category": "Health", "valid": True},
    {"category": "Finance", "valid": False},
    {"category": "Education", "valid": True}
]

valid_categories = {
    record["category"].lower()
    for record in records
    if record["valid"]
}

print(valid_categories)
```

---

# 19. When Not to Use a Comprehension

Compact code is not automatically clean code.

A complex comprehension may be harder to understand:

```python
result = [
    x * 2 if x > 0 else 0
    for x in values
    if x is not None
]
```

A normal loop may be clearer:

```python
result = []

for value in values:
    if value is None:
        continue

    if value > 0:
        result.append(value * 2)
    else:
        result.append(0)
```

A useful rule:

> **Prefer the form that makes the intent easiest to understand.**

---

# 20. Iterating More Effectively

Python provides tools that make iteration clearer.

---

# 21. `enumerate()`

Use `enumerate()` when you need both the item and its position.

## 21.1 Basic Example

```python
models = ["baseline", "tree", "knn"]

for index, model in enumerate(models):
    print(index, model)
```

## 21.2 Intermediate Example

```python
scores = [0.82, 0.90, 0.76]

for index, score in enumerate(scores, start=1):
    print(f"Experiment {index}: {score}")
```

## 21.3 Advanced Example — Track Invalid Rows

```python
values = [85, -3, 91, 120, 77]

for row_number, value in enumerate(values, start=1):
    if not 0 <= value <= 100:
        print(
            f"Invalid value at row {row_number}: {value}"
        )
```

---

# 22. `zip()`

Use `zip()` to iterate over related sequences together.

## 22.1 Basic Example

```python
names = ["Ali", "Sara"]
scores = [78, 91]

for name, score in zip(names, scores):
    print(name, score)
```

## 22.2 Intermediate Example

```python
predictions = [True, False, True]
actual = [True, True, True]

for predicted, expected in zip(predictions, actual):
    print(predicted, expected)
```

## 22.3 Advanced Example — Calculate Accuracy

```python
predictions = [True, False, True, True]
actual = [True, False, False, True]

correct = 0

for predicted, expected in zip(predictions, actual):
    if predicted == expected:
        correct += 1

accuracy = correct / len(actual)

print("Accuracy:", accuracy)
```

You will see this pattern again when evaluating machine-learning models.

---

# 23. Choosing the Right Data Structure

A useful comparison:

| Need | Good Choice | Why |
| --- | --- | --- |
| Ordered sequence that changes | `list` | Mutable and ordered |
| Fixed ordered group | `tuple` | Immutable and ordered |
| Named fields / key-value mapping | `dict` | Values accessed by meaningful keys |
| Unique values | `set` | Automatically removes duplicates |
| Many structured records | list of dictionaries | One dictionary per record |

## 23.1 Basic Scenario — Ordered Scores

```python
scores = [72, 85, 91]
```

Use a list because order matters and new values may be added.

## 23.2 Intermediate Scenario — Model Configuration

```python
config = {
    "model_name": "spam_classifier",
    "threshold": 0.85,
    "debug": False
}
```

Use a dictionary because field names communicate meaning.

## 23.3 Advanced Scenario — Dataset Validation

```python
allowed_labels = {"spam", "ham"}
incoming = {"spam", "ham", "promotion"}

unexpected = incoming - allowed_labels

print(unexpected)
```

Use sets because the problem is about unique groups and difference.

---

# 24. Clean Code

Clean code communicates its purpose clearly.

It should be:

- readable
- predictable
- maintainable
- testable
- easier to change

---

# 25. Meaningful Names

## 25.1 Basic Example

Less clear:

```python
x = [70, 80, 90]
```

Better:

```python
student_scores = [70, 80, 90]
```

## 25.2 Intermediate Example

Less clear:

```python
d = {
    "a": 0.91,
    "p": 0.88
}
```

Better:

```python
evaluation_metrics = {
    "accuracy": 0.91,
    "precision": 0.88
}
```

## 25.3 Advanced Example

Less clear:

```python
def f(x, t):
    return [v for v in x if v >= t]
```

Better:

```python
def filter_scores_above_threshold(scores, threshold):
    return [
        score
        for score in scores
        if score >= threshold
    ]
```

---

# 26. Avoid Magic Values

A **magic value** is an unexplained value embedded directly in logic.

## 26.1 Basic Example

Less clear:

```python
if score >= 50:
    print("Pass")
```

Better:

```python
PASSING_SCORE = 50

if score >= PASSING_SCORE:
    print("Pass")
```

## 26.2 Intermediate Example

```python
MIN_CONFIDENCE = 0.80

if prediction["confidence"] >= MIN_CONFIDENCE:
    print("Accept prediction")
```

## 26.3 Advanced Example — Configuration Dictionary

```python
CONFIG = {
    "passing_score": 50,
    "min_confidence": 0.80,
    "max_invalid_records": 5
}

if score >= CONFIG["passing_score"]:
    print("Pass")
```

Later in the course, configuration will move into more maintainable forms.

---

# 27. Keep Logic Readable

## 27.1 Basic Example

```python
if score >= 50:
    print("Pass")
```

## 27.2 Intermediate Example — Early `continue`

More nested:

```python
for score in scores:
    if score is not None:
        if score >= 0:
            print(score)
```

Clearer:

```python
for score in scores:
    if score is None:
        continue

    if score < 0:
        continue

    print(score)
```

## 27.3 Advanced Example — Separate Responsibilities

Instead of:

```python
def process(records):
    # clean
    # validate
    # calculate
    # print
    ...
```

Prefer:

```python
def clean_records(records):
    ...

def validate_records(records):
    ...

def calculate_summary(records):
    ...

def display_summary(summary):
    ...
```

Week 3 introduced this principle.

Week 4 applies it to data-oriented code.

---

# 28. File Organization

As programs grow, files should reflect responsibilities.

Do not create a separate file for every tiny function.

Organize code around meaningful concerns.

## 28.1 Basic Example — One Small Script

```text
score_analysis.py
```

For a tiny program, one file may be enough.

## 28.2 Intermediate Example — Separate Data Logic

```text
project/
├── main.py
├── preprocessing.py
└── analysis.py
```

Possible responsibilities:

```text
preprocessing.py → cleaning and validation
analysis.py      → calculations and summaries
main.py          → coordinate the workflow
```

## 28.3 Advanced Example — Growing AI Project

```text
project/
├── main.py
├── data/
│   └── sample.csv
├── src/
│   ├── preprocessing.py
│   ├── validation.py
│   └── analysis.py
└── README.md
```

Conceptually:

```text
Data
 ↓
preprocessing.py
 ↓
validation.py
 ↓
analysis.py
 ↓
main.py / application
```

You do not need this structure for every small exercise.

Use structure when it makes responsibilities clearer.

---

# 29. Integrated Data-Oriented Example

Suppose an AI service produces prediction records:

```python
raw_predictions = [
    {
        "id": 1,
        "label": "spam",
        "confidence": 0.94
    },
    {
        "id": 2,
        "label": "ham",
        "confidence": 0.72
    },
    {
        "id": 3,
        "label": "unknown",
        "confidence": 0.41
    },
    {
        "id": 4,
        "label": "spam",
        "confidence": 0.89
    }
]
```

We want to:

1. identify unique labels
2. keep high-confidence predictions
3. count predictions by label
4. create a summary

## 29.1 Step 1 — Unique Labels with a Set

```python
unique_labels = {
    prediction["label"]
    for prediction in raw_predictions
}

print(unique_labels)
```

## 29.2 Step 2 — Filter with a List Comprehension

```python
MIN_CONFIDENCE = 0.80

high_confidence = [
    prediction
    for prediction in raw_predictions
    if prediction["confidence"] >= MIN_CONFIDENCE
]

print(high_confidence)
```

## 29.3 Step 3 — Count with a Dictionary

```python
label_counts = {}

for prediction in raw_predictions:
    label = prediction["label"]

    if label not in label_counts:
        label_counts[label] = 0

    label_counts[label] += 1

print(label_counts)
```

## 29.4 Step 4 — Build a Summary

```python
summary = {
    "total_predictions": len(raw_predictions),
    "unique_labels": unique_labels,
    "high_confidence_count": len(high_confidence),
    "label_counts": label_counts
}

print(summary)
```

Different structures solve different parts of the problem:

```text
list       → ordered prediction records
dictionary → one structured prediction
set        → unique labels
dictionary → counts and summary
```

This is the core Week 4 skill:

> **Choose structures based on the meaning of the data.**

---

# 30. Common Mistakes

### Using a List When Keys Would Be Clearer

Less clear:

```python
student = ["Ali", 85, True]
```

Better:

```python
student = {
    "name": "Ali",
    "score": 85,
    "passed": True
}
```

### Expecting a Set to Preserve Meaningful Order

Do not use a set when sequence order matters.

### Modifying a List While Iterating Over It

Risky:

```python
values = [1, -2, 3, -4]

for value in values:
    if value < 0:
        values.remove(value)
```

Safer:

```python
values = [
    value
    for value in values
    if value >= 0
]
```

### Accessing Missing Dictionary Keys

This may fail:

```python
print(record["confidence"])
```

When a default makes sense:

```python
confidence = record.get("confidence", 0.0)
```

### Overusing Comprehensions

Do not compress complex business logic into one unreadable expression.

### Confusing Copy with Alias

```python
copy = original
```

does not create an independent list.

### Choosing a Class When a Dictionary Is Enough

For simple data:

```python
prediction = {
    "label": "spam",
    "confidence": 0.92
}
```

may be perfectly appropriate.

---

# 31. Thinking Like an AI Engineer

When choosing a data structure, ask:

### Lists

- Does order matter?
- Can values repeat?
- Will the collection change?

### Tuples

- Is this a fixed group of related values?
- Should accidental mutation be prevented?

### Dictionaries

- Do values need meaningful names?
- Am I representing a record, configuration, or mapping?

### Sets

- Do I care about unique values?
- Am I checking membership?
- Am I comparing groups?

### Comprehensions

- Does the compact form remain easy to read?
- Am I creating a new collection from an existing one?

### Clean Code

- Can another programmer understand what this structure represents?
- Are names meaningful?
- Are rules expressed through named constants?
- Is data-processing logic split into understandable steps?

The engineering question is not:

> "Which Python structure can hold this data?"

It is:

> **"Which structure best communicates what this data means and how it will be used?"**

---

# 32. Responsible Use of AI Tools

AI assistants can help you reason about data structures.

Useful prompts include:

- "Do not write the code. Which Python data structure best fits this requirement, and why?"
- "Would a list, tuple, dictionary, or set communicate this data more clearly?"
- "Explain whether this comprehension is readable or should be rewritten as a loop."
- "Show me how the values in this nested dictionary are organized without solving my assignment."
- "Why did changing this copied list also change the original?"
- "Give me three edge cases for this list-processing function."
- "Review these variable names for clarity without rewriting the program."
- "Is this file structure too complex for the size of this program? Explain."

Use AI to improve your reasoning.

Do not use it as a substitute for understanding the data model.

---

# 33. Self-Check Questions

Before moving to the lab, make sure you can answer:

1. What makes a list mutable?
2. What is the difference between indexing and slicing?
3. What is the difference between `append()` and `extend()`?
4. Why can `copy = original` create unexpected behavior?
5. When is a tuple more appropriate than a list?
6. What is tuple unpacking?
7. What problem does a dictionary solve better than a list?
8. What is the difference between `record["key"]` and `record.get("key")`?
9. Why are sets useful for unique labels?
10. What do union, intersection, and difference mean?
11. What is a list comprehension?
12. When should you avoid a comprehension?
13. What does `enumerate()` provide?
14. What does `zip()` do?
15. Which data structure would you use for model configuration, and why?
16. Why are meaningful variable names part of clean code?
17. What is a magic value?
18. Why should file organization reflect responsibilities?
19. Why might a list of dictionaries be useful for dataset-like records?
20. How does choosing the right data structure improve maintainability?

---

# 34. Looking Ahead

In Week 5, we move from general Python collections to **NumPy and numerical computing**.

You will learn:

- NumPy arrays
- lists vs arrays
- vectorization
- indexing and slicing
- numerical operations
- why vectorized computation matters in AI

Week 3 taught you how to **structure logic**.

Week 4 teaches you how to **structure data**.

Week 5 will show how specialized numerical data structures can make AI computation faster and more expressive.

---

## Week 4 Key Message

> **Good programs do not only contain the right data — they represent that data in the right structure.**

Lists, tuples, dictionaries, sets, comprehensions, and clean-code practices are not isolated Python features. Together, they help you build AI programs that are easier to understand, validate, extend, and maintain.
