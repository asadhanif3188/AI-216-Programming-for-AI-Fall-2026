# AI-216: Programming for Artificial Intelligence
## Lab 03 — Functions, Modules, Exceptions, Debugging & OOP

**Semester:** Fall 2026  
**Week:** 03  
**Topic:** Structuring Python programs for maintainability and reuse

---

## 1. Objective

This lab moves from small working scripts toward **structured Python programs**.

By the end of the lab, you should be able to:

- Break a problem into small reusable functions.
- Design functions with clear inputs and return values.
- Organize related functions into Python modules.
- Import and reuse code from another file.
- Use `try` / `except` to handle predictable runtime errors.
- Read a traceback and debug faulty code systematically.
- Define classes with attributes and methods.
- Decide when a function is sufficient and when a class is more appropriate.
- Refactor procedural code into a more maintainable structure.
- Document and commit your work clearly in GitHub.

No external Python libraries are required.

---

## 2. Before You Start

Make sure your Week 2 work is complete and your repository is working.

You should be able to run:

```bash
python --version
git status
git log --oneline
```

Create the Week 3 folder:

```text
labs/
└── week03/
```

Recommended structure:

```text
labs/week03/
├── task01_functions.py
├── task02_scope.py
├── task03_modules/
│   ├── main.py
│   └── score_utils.py
├── task04_exceptions.py
├── task05_debugging.py
├── task06_oop.py
├── task07_refactor/
│   ├── main.py
│   ├── preprocessing.py
│   └── analyzer.py
└── README.md
```

You may use slightly different filenames if instructed, but your structure should remain clear and professional.

---

## 3. General Coding Expectations

For every task:

1. Read the full problem before coding.
2. Identify the responsibility of each function or class.
3. Use meaningful names.
4. Prefer returning values over printing inside reusable functions.
5. Test normal and edge cases.
6. Read error messages before changing code.
7. Do not submit code you cannot explain.
8. Keep each file focused on a clear purpose.

---

# 4. Task 1 — Function Design & Reuse

## Problem

You are given student assessment scores:

```python
scores = [78, 85, 92, 67, 88]
```

Create the following functions:

```python
def calculate_average(scores):
    ...

def find_highest(scores):
    ...

def count_above_threshold(scores, threshold):
    ...

def classify_average(average):
    ...
```

### Requirements

`calculate_average(scores)`

- Return the average.
- Return `None` if the list is empty.

`find_highest(scores)`

- Return the highest value.
- Return `None` if the list is empty.

`count_above_threshold(scores, threshold)`

- Return how many scores are greater than or equal to the threshold.

`classify_average(average)`

Use:

```text
85 and above → Excellent
70–84.99     → Good
50–69.99     → Satisfactory
Below 50     → Needs Improvement
```

If `average` is `None`, return:

```text
No valid data
```

### Required Output

Your program should call the functions and display a clear summary.

### Concepts

- Parameters
- Return values
- Function composition
- Edge cases
- Single-responsibility thinking

---

# 5. Task 2 — Scope & Hidden State

## Part A — Observe Scope

Run this code:

```python
score = 90

def show_score():
    score = 70
    print("Inside function:", score)

show_score()
print("Outside function:", score)
```

In your Week 3 README, explain:

- Why are the two values different?
- Which variable is local?
- Which variable is global?

## Part B — Refactor Hidden State

Start with:

```python
threshold = 0.85

def is_qualified(score):
    return score >= threshold
```

Refactor it so that the function receives `threshold` explicitly.

Then test it with at least three values.

### Reflection

In one or two sentences, explain why explicit parameters make functions easier to reuse and test.

### Concepts

- Local scope
- Global scope
- Explicit dependencies
- Maintainability

---

# 6. Task 3 — Create and Use Your Own Module

Create:

```text
task03_modules/
├── main.py
└── score_utils.py
```

## `score_utils.py`

Create:

```python
def calculate_average(scores):
    ...

def is_passing(score, passing_score=50):
    ...

def count_above_threshold(scores, threshold):
    ...
```

Add a small demo block:

```python
if __name__ == "__main__":
    ...
```

Use sample values inside the demo.

## `main.py`

Import the functions from `score_utils.py`.

Use:

```python
scores = [72, 88, 45, 91, 67]
```

Your program should display:

- average
- whether the first score is passing
- how many scores are at least 80

### Required Check

Run:

```bash
python score_utils.py
```

Then run:

```bash
python main.py
```

Observe the difference.

### README Question

Explain:

> What is the purpose of `if __name__ == "__main__":`?

### Concepts

- Modules
- Imports
- Reuse across files
- Main guard

---

# 7. Task 4 — Exception Handling

## Problem

Write a small percentage calculator.

The program should ask for:

- obtained marks
- total marks

Create:

```python
def calculate_percentage(obtained, total):
    ...
```

### Validation Rules

The function should reject:

- total marks `<= 0`
- obtained marks `< 0`
- obtained marks greater than total marks

Use `raise ValueError(...)` for invalid logical values.

In the main program, handle expected input errors using `try` / `except`.

### Example Structure

```python
try:
    ...
except ValueError as error:
    ...
else:
    ...
finally:
    ...
```

### Required Test Cases

Test at least:

```text
obtained = 80, total = 100
obtained = -5, total = 100
obtained = 120, total = 100
obtained = 80, total = 0
non-numeric input
```

Record the result of each test in `README.md`.

### Concepts

- Runtime errors
- Validation
- `raise`
- `try`
- `except`
- `else`
- `finally`

---

# 8. Task 5 — Debugging Challenge

The following code contains multiple bugs.

Do **not** rewrite it immediately.

First run it, inspect the output, and debug step by step.

```python
def calculate_average(scores):
    total = 0

    for score in scores:
        total = score

    return total / len(scores)


def classify(average):
    if average >= 50:
        return "Pass"
    elif average >= 85:
        return "Excellent"
    else:
        return "Fail"


scores = [60, 70, 80, 90]

average = calculate_average(scores)
print("Average:", average)
print("Result:", classify(average))
```

## Required Debugging Process

Document:

1. What output did you expect?
2. What output did you actually get?
3. Which function contains the first bug?
4. What caused the bug?
5. What was your fix?
6. What is wrong with the condition order in `classify()`?
7. What is the corrected output?

### Debugging Requirement

Use at least one of:

- temporary `print()` statements
- an IDE debugger / breakpoint
- manual variable tracing

Do not simply ask an AI assistant to rewrite the program.

### Concepts

- Logic errors
- Tracing
- Debugging process
- Condition ordering

---

# 9. Task 6 — Build a Class

## Problem

Create a class:

```python
class ScoreAnalyzer:
    ...
```

The class should store a list of scores.

### Required Attribute

```python
self.scores
```

### Required Methods

```python
def clean(self):
    ...

def average(self):
    ...

def count_above(self, threshold):
    ...

def summary(self):
    ...
```

### Rules

`clean()`

- Keep only values between 0 and 100 inclusive.
- Update the internal `self.scores`.

`average()`

- Return the average.
- Return `None` if there are no valid scores.

`count_above(threshold)`

- Return the number of scores meeting or exceeding the threshold.

`summary()`

Return a dictionary such as:

```python
{
    "count": 4,
    "average": 81.5,
    "highest": 92,
    "lowest": 67
}
```

For an empty dataset, return sensible values.

### Sample Data

```python
raw_scores = [78, -5, 110, 67, 90, 88]
```

### Test

Create an object, clean the scores, then call each method.

### Concepts

- Classes
- Objects
- `__init__`
- `self`
- Attributes
- Methods
- Internal state

---

# 10. Task 7 — Refactor a Procedural Script into Modules + Class

This is the main Week 3 engineering task.

## Starting Script

```python
scores = [78, -5, 92, 110, 67, 88]

cleaned = []

for score in scores:
    if 0 <= score <= 100:
        cleaned.append(score)

average = sum(cleaned) / len(cleaned)

qualified = 0

for score in cleaned:
    if score >= 70:
        qualified += 1

print("Cleaned:", cleaned)
print("Average:", average)
print("Qualified:", qualified)
```

The script works, but everything is mixed together.

Refactor it into:

```text
task07_refactor/
├── main.py
├── preprocessing.py
└── analyzer.py
```

## `preprocessing.py`

Create:

```python
def clean_scores(scores):
    ...
```

Responsibility:

```text
raw scores → valid scores
```

## `analyzer.py`

Create a class:

```python
class ScoreAnalyzer:
    ...
```

It should support:

```python
average()
count_above(threshold)
highest()
lowest()
```

## `main.py`

`main.py` should:

1. Define or receive the raw data.
2. Call `clean_scores(...)`.
3. Create a `ScoreAnalyzer`.
4. Produce a clear summary.

### Expected Flow

```text
Raw Data
   ↓
clean_scores(...)
   ↓
Clean Data
   ↓
ScoreAnalyzer(...)
   ↓
Summary
```

### Engineering Requirement

`main.py` should coordinate the workflow.

It should **not** contain the detailed cleaning or analysis logic.

### Concepts

- Refactoring
- Modules
- Separation of concerns
- Functions vs classes
- Maintainable project structure

---

# 11. Optional Challenge — Mini ML-Like Classifier

Create:

```python
class ThresholdClassifier:
    ...
```

### Required Behavior

```python
model = ThresholdClassifier(threshold=60)
```

Methods:

```python
predict_one(value)
predict(values)
accuracy(values, true_labels)
```

Example:

```python
values = [45, 70, 80, 30]
labels = [False, True, True, False]
```

Expected usage:

```python
print(model.predict(values))
print(model.accuracy(values, labels))
```

### Additional Challenge

Raise a `ValueError` if:

```python
len(values) != len(true_labels)
```

### Why This Matters

This resembles the style used by real machine-learning libraries:

```text
model configuration
+
model state
+
predict(...)
+
evaluate(...)
```

---

# 12. Testing Requirements

For each major task, test more than one case.

Useful categories:

```text
normal input
boundary input
empty input
invalid input
unexpected input
```

For example, for an average function:

```python
[70, 80, 90]
[]
[100]
```

For exception handling:

```text
valid number
negative number
zero
text instead of number
```

For classes:

- object with normal data
- object with invalid data
- object with an empty list

---

# 13. Code Quality Expectations

Your code should demonstrate the Week 3 ideas.

Prefer:

```python
def calculate_average(scores):
    ...
```

over repeated calculation logic.

Prefer:

```text
main.py
score_utils.py
```

over one large file containing unrelated responsibilities.

Prefer:

```python
except ValueError:
```

over:

```python
except:
```

Prefer a class when state and behavior belong together.

Prefer a function when no persistent state is needed.

---

# 14. Git Workflow

Do not make one giant commit at the end.

Suggested progression:

```bash
git add labs/week03/task01_functions.py
git commit -m "Lab03: add reusable score functions"

git add labs/week03/task03_modules/
git commit -m "Lab03: organize score utilities into module"

git add labs/week03/task04_exceptions.py
git commit -m "Lab03: add input validation and exception handling"

git add labs/week03/task06_oop.py
git commit -m "Lab03: add ScoreAnalyzer class"

git add labs/week03/task07_refactor/
git commit -m "Lab03: refactor analysis workflow into modules"

git push
```

Your exact commits may differ, but each commit should represent meaningful progress.

---

# 15. Week 3 README

Create:

```text
labs/week03/README.md
```

Suggested structure:

```markdown
# Lab 03 — Functions, Modules, Exceptions, Debugging & OOP

## Concepts Practiced
- Functions
- Scope
- Modules and imports
- `__name__ == "__main__"`
- Exception handling
- Debugging
- Classes and objects
- Refactoring

## Tasks Completed
1. Function Design
2. Scope
3. Modules
4. Exception Handling
5. Debugging Challenge
6. ScoreAnalyzer Class
7. Refactoring Task

## Debugging Notes
Describe:
- one bug you found
- how you located it
- how you fixed it

## Exception Handling Test Cases
| Case | Input | Expected Result | Actual Result |
| --- | --- | --- | --- |
| Valid | | | |
| Invalid | | | |
| Boundary | | | |

## Design Decisions
Explain:
- Why did you use functions in some places?
- Why did you use a class in Task 6/7?
- What logic belongs in `main.py`?

## What I Found Difficult
-

## What I Learned
-

## AI Engineering Relevance
Explain briefly how modularity, debugging, exceptions, and OOP help build larger AI systems.
```

---

# 16. AI-Assisted Learning

AI tools may be used to support learning unless otherwise instructed.

Useful prompts:

- "Do not rewrite my solution. Help me identify which repeated logic should become functions."
- "Explain this traceback and tell me where to start debugging."
- "Give me test cases for this function, including edge cases."
- "Why is this variable local instead of global?"
- "Explain the purpose of `__name__ == '__main__'` using my module structure."
- "Should this logic be a function or a class? Explain the trade-off."
- "Trace how `self.scores` changes after each method call."
- "Review my module responsibilities without writing code for me."

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

You are still responsible for every submitted line of code.

---

# 17. Self-Study Learning Resources

## Python — Defining Functions

https://docs.python.org/3/tutorial/controlflow.html#defining-functions

Use this for:

- parameters
- return values
- default arguments
- function behavior

---

## Python — Modules

https://docs.python.org/3/tutorial/modules.html

Use this for:

- creating modules
- importing code
- module execution
- `__name__`

---

## Python — Errors and Exceptions

https://docs.python.org/3/tutorial/errors.html

Use this for:

- syntax errors
- exceptions
- `try`
- `except`
- `else`
- `finally`
- raising exceptions

---

## Python — Classes

https://docs.python.org/3/tutorial/classes.html

Use this for:

- classes
- objects
- instance attributes
- methods
- `self`

---

## Python Tutor

https://pythontutor.com/

Useful for visualizing:

- function calls
- local scope
- object state
- method execution

---

## Suggested Self-Study Path

```text
Functions
   ↓
Modules
   ↓
Run/import the same file
   ↓
Exceptions
   ↓
Debug a broken program
   ↓
Classes & objects
   ↓
Refactor one script
```

Do not try to memorize all syntax at once.

The goal is to understand **why the structure exists**.

---

# 18. Submission Checklist

Before submitting, verify:

- [ ] All required work is inside `labs/week03/`.
- [ ] Task 1 uses reusable functions.
- [ ] Scope questions are answered.
- [ ] Task 3 contains at least two Python files.
- [ ] You tested `__name__ == "__main__"`.
- [ ] Task 4 handles expected invalid input.
- [ ] Task 5 includes documented debugging steps.
- [ ] Task 6 contains a working class with attributes and methods.
- [ ] Task 7 separates preprocessing, analysis, and coordination.
- [ ] `labs/week03/README.md` exists.
- [ ] AI Usage Log is included if AI assistance was used.
- [ ] Multiple meaningful Git commits are visible.
- [ ] All work is pushed to GitHub.
- [ ] You can explain why each function/module/class exists.

---

# 19. Expected Learning Outcomes

After completing this lab, you should be able to:

- Refactor repeated logic into functions.
- Explain local and global scope.
- Organize Python code across modules.
- Import and reuse your own functions.
- Handle predictable runtime errors.
- Debug syntax, runtime, and logic problems systematically.
- Create objects with state and behavior.
- Decide when to use functions versus classes.
- Refactor a procedural script into a more maintainable structure.

---

## Lab 03 Key Message

> **Do not measure code quality only by whether it runs. Measure it by whether it can be understood, tested, reused, debugged, and extended.**

Week 3 is where your Python code starts becoming software engineering.
