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
3. Distinguish aliasing, shallow copies, and deep copies.
4. Use tuples when data should remain fixed.
5. Use dictionaries to represent key-value relationships and structured records.
6. Use sets to represent unique values and perform membership/set operations.
7. Explain why sets and dictionaries give fast membership checks, and what "hashable" means.
8. Write readable list, dictionary, and set comprehensions.
9. Iterate effectively using `enumerate()`, `zip()`, and dictionary methods.
10. Sort and count structured records using `key=`, `dict.get()`, and `collections.Counter`.
11. Choose an appropriate data structure for a given problem.
12. Organize Python files and data-related code clearly.
13. Apply practical clean-code principles to data-oriented Python programs.

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

## 6.4 `append()` vs `extend()`

`append()` adds **one item** — even if that item is itself a list.

`extend()` adds **each item** from another collection.

```python
values = [10, 20]
values.append([30, 40])
print(values)

values = [10, 20]
values.extend([30, 40])
print(values)
```

Output:

```text
[10, 20, [30, 40]]
[10, 20, 30, 40]
```

Use `extend()` when you want to merge a batch of new values into an existing list.

## 6.5 `insert()`, `pop()`, and `index()`

```python
models = ["baseline", "knn"]

models.insert(1, "tree")
print(models)

last_model = models.pop()
print(last_model, models)

print(models.index("tree"))
```

Output:

```text
['baseline', 'tree', 'knn']
knn ['baseline', 'tree']
1
```

- `insert(position, item)` adds an item at a specific position.
- `pop()` removes **and returns** the last item (or the item at a given position).
- `index(item)` returns the position of the first matching item, and raises `ValueError` if the item is missing.

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
alias = original

alias.append(40)

print("Original:", original)
print("Alias:", alias)
```

Both variables refer to the same list.

> Avoid naming a variable `copy` — that name belongs to Python's `copy` module, which you will use in Section 7.4.

## 7.3 Advanced Example — Independent Copy

```python
raw_scores = [70, 80, 90]
processed_scores = raw_scores.copy()

processed_scores.append(100)

print("Raw:", raw_scores)
print("Processed:", processed_scores)
```

These three forms all create a new, independent list:

```python
processed_scores = raw_scores.copy()
processed_scores = list(raw_scores)
processed_scores = raw_scores[:]
```

Accidental mutation can cause subtle bugs in data-processing pipelines.

Ask:

> **Am I modifying the original data, or should I create a new version?**

## 7.4 Shallow Copy vs Deep Copy

`.copy()` creates a **shallow copy**: a new outer list, but the items inside are still shared.

For a list of numbers this does not matter, because numbers cannot be changed in place.

For a **list of dictionaries** — the most common pattern this week — it matters a lot:

```python
raw_predictions = [
    {"id": 1, "label": "Spam"},
    {"id": 2, "label": "HAM"}
]

cleaned = raw_predictions.copy()
cleaned[0]["label"] = "spam"

print(raw_predictions)
```

Output:

```text
[{'id': 1, 'label': 'spam'}, {'id': 2, 'label': 'HAM'}]
```

The raw data changed, because both lists contain **the same dictionary objects**.

A **deep copy** duplicates the nested objects too:

```python
import copy

raw_predictions = [
    {"id": 1, "label": "Spam"},
    {"id": 2, "label": "HAM"}
]

cleaned = copy.deepcopy(raw_predictions)
cleaned[0]["label"] = "spam"

print(raw_predictions)
print(cleaned)
```

Output:

```text
[{'id': 1, 'label': 'Spam'}, {'id': 2, 'label': 'HAM'}]
[{'id': 1, 'label': 'spam'}, {'id': 2, 'label': 'HAM'}]
```

```text
alias          → same list, same items
shallow copy   → new list, same items
deep copy      → new list, new items
```

In practice, you often do not need `deepcopy()`. A clean alternative is to **build new records** instead of editing old ones (see Section 27.4).

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

You saw this function in Week 3. What was new then was returning several values; what is new now is noticing that those values travel together as **a tuple**.

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

`return minimum, maximum, average` builds a tuple, and the assignment unpacks it:

```python
result = summarize(scores)
print(type(result))
```

```text
<class 'tuple'>
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

## 10.4 Dictionaries Remember Insertion Order

Since Python 3.7, a dictionary keeps keys in the order they were **inserted**:

```python
config = {
    "threshold": 0.80,
    "debug": False,
    "model": "spam_classifier"
}

print(list(config))
```

```text
['threshold', 'debug', 'model']
```

This makes printed reports predictable. Sets, by contrast, do **not** preserve order (Section 13).

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

## 12.4 Sorting Records with `key=`

`sorted()` cannot guess how to compare two dictionaries.

Use `key=` to tell it **which value to sort by**. The key is a function that receives one record and returns the value to compare:

```python
predictions = [
    {"id": 201, "label": "ham", "confidence": 0.72},
    {"id": 202, "label": "spam", "confidence": 0.94},
    {"id": 203, "label": "unknown", "confidence": 0.41},
    {"id": 204, "label": "spam", "confidence": 0.89}
]


def get_confidence(prediction):
    return prediction["confidence"]


ranked = sorted(
    predictions,
    key=get_confidence,
    reverse=True
)

for result in ranked:
    print(result["id"], result["confidence"])
```

Output:

```text
202 0.94
204 0.89
201 0.72
203 0.41
```

Note that we pass `get_confidence` — the function itself — **not** `get_confidence()`.

Taking the top results is then just a slice:

```python
top_two = ranked[:2]
```

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

Because sets are unordered, the printed order may differ from run to run. When you need predictable output (for reports or grading), print `sorted(categories)`.

## 13.4 What Can Go Inside a Set or Be a Dictionary Key?

Set elements and dictionary keys must be **hashable**.

In practice, for this course:

```text
hashable      → int, float, str, bool, tuple (of hashable values)
not hashable  → list, dict, set
```

Python uses a value's **hash** — a number computed from the value — to find it quickly. If a value could change after being stored, its hash would change too, and Python could no longer find it. That is why mutable types are not allowed.

This is another reason tuples matter. A tuple can be a dictionary key; a list cannot:

```python
input_models = {
    (224, 224): "resnet50",
    (299, 299): "inception_v3"
}

print(input_models[(224, 224)])
```

```text
resnet50
```

```python
input_models = {
    [224, 224]: "resnet50"
}
```

```text
TypeError: unhashable type: 'list'
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

## 14.4 Why Membership Checks Are Fast in Sets and Dictionaries

To answer `value in some_list`, Python checks items **one by one** until it finds a match. With a million items, that can mean a million comparisons.

A set (or dictionary) uses the value's hash to jump **directly** to where the value would be stored. The time barely changes as the collection grows.

```text
value in list  → checks items one by one   → slower as data grows   (O(n))
value in set   → jumps to the location      → roughly constant time  (O(1))
value in dict  → same as set, checks keys   → roughly constant time  (O(1))
```

You can measure this with the standard `timeit` module:

```python
import timeit

ids_list = list(range(1_000_000))
ids_set = set(ids_list)

list_time = timeit.timeit(
    "999_999 in ids_list",
    globals=globals(),
    number=100
)

set_time = timeit.timeit(
    "999_999 in ids_set",
    globals=globals(),
    number=100
)

print(f"List lookup: {list_time:.4f} seconds")
print(f"Set lookup:  {set_time:.6f} seconds")
```

Typical output (exact numbers vary by machine):

```text
List lookup: 0.7587 seconds
Set lookup:  0.000009 seconds
```

For a handful of labels the difference is invisible. For validating millions of records against a list of allowed IDs, it is the difference between seconds and hours.

> **If the main question is "is this value in the collection?", use a set.**

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
| Fast "is it in there?" checks | `set` (or `dict` keys) | Hash-based lookup stays fast as data grows |
| Composite lookup key, e.g. `(width, height)` | `tuple` as a `dict` key | Tuples are hashable; lists are not |
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

## 27.4 Do Not Mutate Your Inputs

A function that quietly changes the data passed into it is hard to trust.

Surprising:

```python
def normalize_labels(records):
    for record in records:
        record["label"] = record["label"].lower()
    return records
```

After calling it, the caller's **raw** data has also changed — the aliasing problem from Section 7, hidden inside a function.

Predictable:

```python
def normalize_labels(records):
    normalized = []

    for record in records:
        cleaned = record.copy()
        cleaned["label"] = cleaned["label"].lower()
        normalized.append(cleaned)

    return normalized
```

`record.copy()` creates a new dictionary for each record, so the original records stay untouched. (This is safe here because the values inside each record are strings and numbers, not nested lists or dictionaries.)

A useful rule:

> **Functions should return new data rather than silently modifying the data they receive — unless modifying it is the function's stated purpose.**

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

### Importing from `src/`

When modules live inside a folder, `main.py` imports them through the folder name:

```python
from src.preprocessing import clean_records
from src.analysis import calculate_summary
```

Run the program from the **project folder** (the one containing `main.py`):

```bash
cd project
python main.py
```

If you run it from somewhere else, Python may not find `src` and will raise `ModuleNotFoundError`.

In the simpler layout from Section 28.2, all files sit next to `main.py`, so a plain `from preprocessing import clean_records` works — again, as long as you run `python main.py` from that folder.

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

The same counting logic is shorter with `dict.get()` and a default of `0`:

```python
label_counts = {}

for prediction in raw_predictions:
    label = prediction["label"]
    label_counts[label] = label_counts.get(label, 0) + 1
```

Counting is so common that the standard library provides a ready-made tool, `collections.Counter`:

```python
from collections import Counter

labels = [prediction["label"] for prediction in raw_predictions]
label_counts = Counter(labels)

print(label_counts)
print(label_counts.most_common(1))
```

```text
Counter({'spam': 2, 'ham': 1, 'unknown': 1})
[('spam', 2)]
```

A related tool, `collections.defaultdict`, is useful for **grouping** — for example, collecting all confidence values per label:

```python
from collections import defaultdict

confidences_by_label = defaultdict(list)

for prediction in raw_predictions:
    confidences_by_label[prediction["label"]].append(prediction["confidence"])

print(dict(confidences_by_label))
```

```text
{'spam': [0.94, 0.89], 'ham': [0.72], 'unknown': [0.41]}
```

Learn the plain-dictionary version first so you understand what these tools do. You will see the same idea again in Week 6 as Pandas' `value_counts()` and `groupby()`.

## 29.4 Step 4 — Build a Summary

```python
summary = {
    "total_predictions": len(raw_predictions),
    "unique_labels": sorted(unique_labels),
    "high_confidence_count": len(high_confidence),
    "label_counts": label_counts
}

print(summary)
```

`sorted(unique_labels)` turns the set into an alphabetical list, so the report prints in the same order every time.

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

Dictionaries keep insertion order; sets do not. If you need a set's contents in a stable order, use `sorted(the_set)`.

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
alias = original
```

does not create an independent list.

And `.copy()` on a list of dictionaries still shares the dictionaries inside it (Section 7.4).

### Using a Mutable Default Argument

Risky:

```python
def add_label(label, labels=[]):
    labels.append(label)
    return labels


print(add_label("spam"))
print(add_label("ham"))
```

```text
['spam']
['spam', 'ham']
```

The default list is created **once**, when the function is defined, and then shared by every call. The second call "remembers" the first.

Safer:

```python
def add_label(label, labels=None):
    if labels is None:
        labels = []

    labels.append(label)
    return labels
```

```text
['spam']
['ham']
```

Use `None` as the default for lists, dictionaries, and sets, and create the new collection inside the function.

### Using a List as a Dictionary Key or Set Element

```python
{[224, 224]: "resnet50"}
```

raises `TypeError: unhashable type: 'list'`. Use a tuple instead (Section 13.4).

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
- Am I checking membership — especially against a large collection?
- Am I comparing groups?

### Comprehensions

- Does the compact form remain easy to read?
- Am I creating a new collection from an existing one?

### Clean Code

- Can another programmer understand what this structure represents?
- Are names meaningful?
- Are rules expressed through named constants?
- Is data-processing logic split into understandable steps?
- Do my functions return new data instead of silently changing their inputs?

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
4. Why can `alias = original` create unexpected behavior?
5. Why does `.copy()` on a list of dictionaries not fully protect the original data?
6. When is a tuple more appropriate than a list?
7. What is tuple unpacking?
8. What problem does a dictionary solve better than a list?
9. What is the difference between `record["key"]` and `record.get("key")`?
10. Why are sets useful for unique labels?
11. What do union, intersection, and difference mean?
12. Why is `value in some_set` faster than `value in some_list` for large collections?
13. Why can a tuple be a dictionary key, but a list cannot?
14. What is a list comprehension?
15. When should you avoid a comprehension?
16. What does `enumerate()` provide?
17. What does `zip()` do?
18. How do you sort a list of dictionaries by one of their fields?
19. Which data structure would you use for model configuration, and why?
20. Why are meaningful variable names part of clean code?
21. What is a magic value?
22. Why is `def f(items=[])` risky?
23. Why should a function avoid modifying the data passed into it?
24. Why should file organization reflect responsibilities?
25. Why might a list of dictionaries be useful for dataset-like records?
26. How does choosing the right data structure improve maintainability?

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
