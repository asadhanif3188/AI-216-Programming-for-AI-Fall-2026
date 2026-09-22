# AI-216: Programming for Artificial Intelligence
## Week 03 Lecture Handout — Functions, Modules, Exceptions, Debugging & Object-Oriented Programming

**Semester:** Fall 2026  
**Week:** 03  
**Theme:** From working scripts to structured, reusable AI software

---

## 1. Why Week 3 Matters

In Week 2, you learned how to express logic using:

- variables
- operators
- conditions
- loops
- simple functions

That is enough to solve small problems.

But real AI applications quickly become larger:

```text
load data
clean data
validate inputs
transform values
train/use a model
evaluate outputs
serve predictions
```

If all of that logic is written in one large script, the code becomes difficult to:

- understand
- debug
- test
- reuse
- extend
- collaborate on

Week 3 introduces the next step:

```text
Working Script
      ↓
Functions
      ↓
Modules
      ↓
Exception Handling
      ↓
Debugging
      ↓
Classes & Objects
      ↓
Structured AI Software
```

The goal is to start thinking like an engineer who organizes code into **small, understandable, reusable components**.

---

## 2. Week 3 Learning Outcomes

By the end of this lecture, you should be able to:

1. Design functions with clear inputs, outputs, and responsibilities.
2. Explain the difference between local and global scope.
3. Organize Python code into reusable modules.
4. Import and use functions from another Python file.
5. Handle predictable runtime errors using `try`, `except`, `else`, and `finally`.
6. Use a systematic debugging process to locate and fix programming errors.
7. Define classes and create objects with attributes and methods.
8. Explain why modularity and OOP are useful in AI applications.

---

## 3. From Procedural Code to Structured Code

A beginner program often looks like this:

```python
scores = [72, 88, 91, 67]

total = 0
for score in scores:
    total += score

average = total / len(scores)

if average >= 70:
    print("Good performance")
else:
    print("Needs improvement")
```

This works.

But imagine that the same program later needs to:

- clean invalid scores
- calculate multiple statistics
- read data from files
- serve results through an API

A better design separates responsibilities:

```python
def clean_scores(scores):
    ...

def calculate_average(scores):
    ...

def classify_performance(average):
    ...
```

Structured programming asks:

> **What are the separate responsibilities in this program?**

That question is central to AI engineering.

---

# 4. Functions Revisited

A function is a named block of reusable logic.

A good function should usually have:

```text
clear purpose
+ clear inputs
+ clear output
+ one main responsibility
```

---

## 4.1 Basic Example — Simple Reusable Function

```python
def greet(name):
    return f"Welcome, {name}"

message = greet("Ali")
print(message)
```

### Key Idea

The function:

- receives one input
- performs one task
- returns one output

---

## 4.2 Intermediate Example — Data Transformation

```python
def normalize_score(score):
    return score / 100

scores = [78, 85, 92]

for score in scores:
    normalized = normalize_score(score)
    print(normalized)
```

### Key Idea

The same transformation can be applied consistently to many values.

---

## 4.3 Advanced Example — Small Processing Pipeline

```python
def clean_scores(scores):
    cleaned = []

    for score in scores:
        if 0 <= score <= 100:
            cleaned.append(score)

    return cleaned


def calculate_average(scores):
    if len(scores) == 0:
        return None

    return sum(scores) / len(scores)


def classify_average(average):
    if average is None:
        return "No valid data"

    if average >= 85:
        return "Excellent"
    elif average >= 70:
        return "Good"
    elif average >= 50:
        return "Satisfactory"
    else:
        return "Needs improvement"


raw_scores = [78, -5, 110, 67, 90]

valid_scores = clean_scores(raw_scores)
average = calculate_average(valid_scores)
result = classify_average(average)

print("Valid scores:", valid_scores)
print("Average:", average)
print("Result:", result)
```

### Key Idea

Real applications are often built by **combining small functions** rather than writing one large function.

---

# 5. Function Parameters and Return Values

Parameters define what information a function needs.

`return` sends a result back to the caller.

---

## 5.1 Basic Example

```python
def add(a, b):
    return a + b

result = add(10, 5)
print(result)
```

---

## 5.2 Intermediate Example — Default Parameter

```python
def is_passing(score, passing_score=50):
    return score >= passing_score

print(is_passing(72))
print(is_passing(72, 80))
```

The default value is used only when the caller does not provide one.

---

## 5.3 Advanced Example — Multiple Returned Values

```python
def summarize_scores(scores):
    minimum = min(scores)
    maximum = max(scores)
    average = sum(scores) / len(scores)

    return minimum, maximum, average


scores = [72, 88, 91, 67]

minimum, maximum, average = summarize_scores(scores)

print("Minimum:", minimum)
print("Maximum:", maximum)
print("Average:", average)
```

### Key Idea

A function can return multiple related results.

---

# 6. Function Scope

Variables created inside a function normally belong to that function.

This is called **local scope**.

---

## 6.1 Basic Example — Local Variable

```python
def calculate_total():
    total = 100 + 50
    return total

print(calculate_total())
```

`total` exists inside the function.

---

## 6.2 Intermediate Example — Same Name, Different Scope

```python
score = 90

def show_score():
    score = 70
    print("Inside:", score)

show_score()
print("Outside:", score)
```

Output:

```text
Inside: 70
Outside: 90
```

The two variables are different.

---

## 6.3 Advanced Example — Avoid Unnecessary Global State

Less maintainable:

```python
threshold = 0.85

def is_qualified(score):
    return score >= threshold
```

More explicit:

```python
def is_qualified(score, threshold):
    return score >= threshold

print(is_qualified(0.91, 0.85))
```

### Why Prefer Explicit Inputs?

Explicit inputs make a function:

- easier to understand
- easier to test
- easier to reuse
- less dependent on hidden state

This becomes important in larger AI systems.

---

# 7. Designing Good Functions

A useful principle is:

> **One function, one clear responsibility.**

Avoid:

```python
def process_everything():
    # read input
    # clean data
    # calculate statistics
    # classify results
    # print report
    # save output
    ...
```

Prefer:

```python
def load_data():
    ...

def clean_data(data):
    ...

def calculate_statistics(data):
    ...

def classify_result(stats):
    ...

def display_report(result):
    ...
```

This makes code easier to modify and debug.

---

# 8. Modules

A **module** is a Python file containing reusable code.

Instead of keeping everything in one file:

```text
main.py
```

you can organize code like:

```text
project/
├── main.py
├── preprocessing.py
└── metrics.py
```

---

## 8.1 Basic Example — Importing a Built-In Module

```python
import math

value = math.sqrt(81)
print(value)
```

Python's standard library contains many useful modules.

---

## 8.2 Intermediate Example — Importing Specific Functions

```python
from math import sqrt, ceil

print(sqrt(49))
print(ceil(4.2))
```

This imports only the required names.

---

## 8.3 Advanced Example — Creating Your Own Module

Create:

```text
score_utils.py
```

```python
def calculate_average(scores):
    return sum(scores) / len(scores)


def is_passing(score, passing_score=50):
    return score >= passing_score
```

Then create:

```text
main.py
```

```python
from score_utils import calculate_average, is_passing

scores = [72, 88, 45, 91]

average = calculate_average(scores)

print("Average:", average)
print("First score passing:", is_passing(scores[0]))
```

### Key Idea

Modules separate related responsibilities into different files.

---

# 9. Import Styles

Python supports several import styles.

### Import the Module

```python
import math

print(math.sqrt(25))
```

### Import a Specific Name

```python
from math import sqrt

print(sqrt(25))
```

### Import with an Alias

```python
import math as m

print(m.sqrt(25))
```

Aliases become especially common later:

```python
import numpy as np
import pandas as pd
```

Use aliases that are standard and recognizable.

---

# 10. `__name__ == "__main__"`

A Python file can be:

- run directly
- imported as a module

This pattern helps distinguish those two cases.

---

## 10.1 Basic Example

```python
def greet():
    print("Hello from the module")


if __name__ == "__main__":
    greet()
```

---

## 10.2 Intermediate Example

```python
def calculate_average(scores):
    return sum(scores) / len(scores)


if __name__ == "__main__":
    test_scores = [70, 80, 90]
    print(calculate_average(test_scores))
```

The test code runs only when the file is executed directly.

---

## 10.3 Advanced Example

`score_utils.py`:

```python
def calculate_average(scores):
    if len(scores) == 0:
        return None

    return sum(scores) / len(scores)


def demo():
    sample = [75, 80, 95]
    print("Demo average:", calculate_average(sample))


if __name__ == "__main__":
    demo()
```

`main.py`:

```python
from score_utils import calculate_average

scores = [60, 75, 92]
print(calculate_average(scores))
```

When imported, `demo()` does not run automatically.

---

# 11. Errors Are Part of Programming

Programs fail for different reasons.

A useful distinction is:

```text
Syntax Error
Runtime Error
Logic Error
```

---

## 11.1 Basic Example — Syntax Error

```python
if score >= 50
    print("Pass")
```

The colon is missing.

Python cannot parse the code.

---

## 11.2 Intermediate Example — Runtime Error

```python
value = int("abc")
```

The code is syntactically valid, but fails while running.

---

## 11.3 Advanced Example — Logic Error

```python
scores = [80, 90, 100]
average = sum(scores) / 2

print(average)
```

The program runs, but the formula is wrong.

Logic errors are often the hardest because Python may not report any error.

---

# 12. Exception Handling

Exceptions are errors that occur while a program is running.

Use `try` and `except` when you expect that an operation may fail.

---

## 12.1 Basic Example

```python
try:
    age = int(input("Enter age: "))
    print("Age:", age)
except ValueError:
    print("Please enter a valid whole number.")
```

---

## 12.2 Intermediate Example — Multiple Exceptions

```python
try:
    total = float(input("Enter total marks: "))
    obtained = float(input("Enter obtained marks: "))

    percentage = (obtained / total) * 100
    print("Percentage:", percentage)

except ValueError:
    print("Marks must be numeric.")

except ZeroDivisionError:
    print("Total marks cannot be zero.")
```

---

## 12.3 Advanced Example — Validation + Exception Handling

```python
def calculate_percentage(obtained, total):
    if total <= 0:
        raise ValueError("Total marks must be greater than zero.")

    if obtained < 0:
        raise ValueError("Obtained marks cannot be negative.")

    return (obtained / total) * 100


try:
    obtained = float(input("Obtained marks: "))
    total = float(input("Total marks: "))

    percentage = calculate_percentage(obtained, total)
    print(f"Percentage: {percentage:.2f}%")

except ValueError as error:
    print("Invalid input:", error)
```

### Key Idea

Good programs do not only calculate results.

They also protect themselves against invalid states.

---

# 13. `else` and `finally`

Exception handling can also include:

```python
else
finally
```

---

## 13.1 Basic Example — `else`

```python
try:
    value = int("25")
except ValueError:
    print("Conversion failed")
else:
    print("Converted value:", value)
```

`else` runs when no exception occurs.

---

## 13.2 Intermediate Example — `finally`

```python
try:
    value = int(input("Enter a number: "))
    print("Value:", value)
except ValueError:
    print("Invalid number")
finally:
    print("Program finished this operation")
```

`finally` runs whether an exception occurs or not.

---

## 13.3 Advanced Example — Resource-Oriented Thinking

```python
def process_request(value):
    try:
        number = float(value)
        result = 100 / number

    except ValueError:
        return "Input must be numeric"

    except ZeroDivisionError:
        return "Input cannot be zero"

    else:
        return f"Result: {result:.2f}"

    finally:
        print("Request processed")


print(process_request("5"))
```

The `finally` block can be useful later when working with resources such as files, database connections, or network operations.

---

# 14. Avoid Catching Everything

This is possible:

```python
try:
    ...
except:
    print("Something went wrong")
```

But it is usually too broad.

Prefer specific exceptions:

```python
except ValueError:
    ...

except ZeroDivisionError:
    ...
```

Specific handling makes debugging easier because you know what kind of failure occurred.

---

# 15. Debugging

Debugging means finding and fixing the cause of incorrect program behavior.

A useful process is:

```text
Observe
  ↓
Reproduce
  ↓
Localize
  ↓
Inspect
  ↓
Fix
  ↓
Test Again
```

Do not randomly change code until the error disappears.

---

# 16. Debugging with `print()`

Simple print statements are often enough to understand a problem.

---

## 16.1 Basic Example

```python
score = 72

print("score =", score)

if score >= 50:
    print("Pass")
```

---

## 16.2 Intermediate Example — Inspect a Loop

```python
scores = [70, 40, 90]
passed = 0

for score in scores:
    print("Current score:", score)

    if score >= 50:
        passed += 1

    print("Passed so far:", passed)

print("Final passed count:", passed)
```

---

## 16.3 Advanced Example — Find a Logic Bug

Buggy code:

```python
def calculate_average(scores):
    total = 0

    for score in scores:
        total = score

    return total / len(scores)
```

Add tracing:

```python
def calculate_average(scores):
    total = 0

    for score in scores:
        print("Before:", total)
        print("Adding:", score)

        total = score

        print("After:", total)

    return total / len(scores)
```

The debug output reveals that `total` is being replaced rather than accumulated.

Fix:

```python
total += score
```

---

# 17. Reading Tracebacks

When Python raises an exception, it usually provides a traceback.

Example:

```text
Traceback (most recent call last):
  File "main.py", line 8, in <module>
    result = calculate_average([])
  File "main.py", line 4, in calculate_average
    return sum(scores) / len(scores)
ZeroDivisionError: division by zero
```

Read tracebacks from the bottom upward:

```text
Exception type
    ↓
Error message
    ↓
Line where it happened
    ↓
Function call path
```

The traceback is information, not just failure.

---

# 18. Debugging with an IDE

Modern IDEs such as VS Code and PyCharm support debuggers.

A debugger lets you:

- set breakpoints
- pause execution
- inspect variables
- step through code line by line
- observe program state

Conceptually:

```text
Run
 ↓
Breakpoint
 ↓
Inspect Variables
 ↓
Step Over / Step Into
 ↓
Understand State Change
```

You do not need advanced debugger skills yet, but you should know that this is a professional alternative to adding many `print()` statements.

---

# 19. Object-Oriented Programming

Object-Oriented Programming groups:

```text
data + behavior
```

together.

The main concepts for this week are:

```text
Class       → blueprint
Object      → instance created from a class
Attribute   → data stored in an object
Method      → behavior defined on an object
```

---

# 20. Classes and Objects

## 20.1 Basic Example

```python
class Student:
    pass


student1 = Student()

print(student1)
```

The class defines a new type.

`student1` is an object created from that class.

---

## 20.2 Intermediate Example — Attributes

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks


student1 = Student("Ali", 78)
student2 = Student("Sara", 85)

print(student1.name, student1.marks)
print(student2.name, student2.marks)
```

Each object has its own state.

---

## 20.3 Advanced Example — Attributes + Methods

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def is_passing(self):
        return self.marks >= 50

    def summary(self):
        return f"{self.name}: {self.marks} marks"


student1 = Student("Ali", 45)
student2 = Student("Sara", 88)

print(student1.summary())
print("Passing:", student1.is_passing())

print(student2.summary())
print("Passing:", student2.is_passing())
```

### Key Idea

An object holds both:

```text
state
+
behavior related to that state
```

---

# 21. Understanding `self`

`self` refers to the current object.

Consider:

```python
class Student:
    def __init__(self, name):
        self.name = name
```

When you write:

```python
student1 = Student("Ali")
student2 = Student("Sara")
```

Python keeps separate values:

```text
student1.name → Ali
student2.name → Sara
```

---

## 21.1 Basic Example

```python
class Counter:
    def __init__(self):
        self.value = 0
```

---

## 21.2 Intermediate Example

```python
class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1
```

---

## 21.3 Advanced Example

```python
class Counter:
    def __init__(self, start=0):
        self.value = start

    def increment(self, amount=1):
        self.value += amount

    def reset(self):
        self.value = 0


counter = Counter(10)

counter.increment()
counter.increment(5)

print(counter.value)

counter.reset()
print(counter.value)
```

Methods can change object state through `self`.

---

# 22. OOP in an AI Context

Many AI libraries use objects because models have:

- configuration
- learned state
- methods

Conceptually:

```text
Model Object
├── configuration
├── learned parameters
├── fit(...)
├── predict(...)
└── evaluate(...)
```

---

## 22.1 Basic Example — Threshold Model

```python
class ThresholdModel:
    def __init__(self, threshold):
        self.threshold = threshold

    def predict(self, value):
        return value >= self.threshold


model = ThresholdModel(50)

print(model.predict(40))
print(model.predict(75))
```

---

## 22.2 Intermediate Example — Batch Prediction

```python
class ThresholdModel:
    def __init__(self, threshold):
        self.threshold = threshold

    def predict(self, values):
        predictions = []

        for value in values:
            predictions.append(value >= self.threshold)

        return predictions


model = ThresholdModel(60)

data = [45, 70, 80, 30]
predictions = model.predict(data)

print(predictions)
```

---

## 22.3 Advanced Example — Model State + Evaluation

```python
class ThresholdClassifier:
    def __init__(self, threshold):
        self.threshold = threshold

    def predict_one(self, value):
        return value >= self.threshold

    def predict(self, values):
        predictions = []

        for value in values:
            predictions.append(self.predict_one(value))

        return predictions

    def accuracy(self, values, true_labels):
        predictions = self.predict(values)

        correct = 0

        for predicted, actual in zip(predictions, true_labels):
            if predicted == actual:
                correct += 1

        return correct / len(true_labels)


model = ThresholdClassifier(60)

values = [45, 70, 80, 30]
labels = [False, True, True, False]

print("Predictions:", model.predict(values))
print("Accuracy:", model.accuracy(values, labels))
```

This is still a simple rule-based classifier, but its interface resembles real ML libraries.

---

# 23. Functions vs Classes

Not every problem needs a class.

Use a function when:

```text
input → operation → output
```

is enough.

Example:

```python
def normalize(value):
    return value / 100
```

A class is useful when you need to keep **state and related behavior together**.

Example:

```python
class ThresholdModel:
    def __init__(self, threshold):
        self.threshold = threshold

    def predict(self, value):
        return value >= self.threshold
```

A useful question is:

> **Does this logic need to remember state between operations?**

If not, a function may be simpler.

---

# 24. Refactoring: From Script to Modules and Objects

Suppose you begin with:

```python
scores = [72, 88, 91, 67]

cleaned = []
for score in scores:
    if 0 <= score <= 100:
        cleaned.append(score)

average = sum(cleaned) / len(cleaned)

print(average)
```

A first refactor uses functions:

```python
def clean_scores(scores):
    return [score for score in scores if 0 <= score <= 100]


def calculate_average(scores):
    return sum(scores) / len(scores)
```

A larger project may separate files:

```text
project/
├── main.py
├── preprocessing.py
└── metrics.py
```

And stateful behavior may become an object:

```python
class ScoreAnalyzer:
    def __init__(self, scores):
        self.scores = scores

    def clean(self):
        self.scores = [
            score
            for score in self.scores
            if 0 <= score <= 100
        ]

    def average(self):
        return sum(self.scores) / len(self.scores)
```

Refactoring means improving structure **without changing the intended behavior**.

---

# 25. Common Mistakes

### Function Does Too Much

```python
def process_everything():
    ...
```

Break large responsibilities into smaller functions.

---

### Printing Instead of Returning

Less reusable:

```python
def calculate_total(a, b):
    print(a + b)
```

More reusable:

```python
def calculate_total(a, b):
    return a + b
```

---

### Mutable Global State

Avoid making functions depend on hidden global variables when explicit parameters are clearer.

---

### Circular or Confusing Imports

Keep modules focused and dependencies simple.

---

### Catching Every Exception

Avoid:

```python
except:
    ...
```

Prefer handling known exception types.

---

### Ignoring the Traceback

Read the final exception and line number before changing code.

---

### Forgetting `self`

Incorrect:

```python
class Student:
    def __init__(name):
        self.name = name
```

Correct:

```python
class Student:
    def __init__(self, name):
        self.name = name
```

---

### Creating Classes for Everything

A class is not automatically better than a function.

Use the simplest structure that clearly solves the problem.

---

# 26. Thinking Like an AI Engineer

Before writing a larger program, ask:

### Functions

- What repeated logic exists?
- What should the function receive?
- What should it return?
- Does it have one clear responsibility?

### Modules

- Which functions belong together?
- Which code should live in a separate file?
- What should `main.py` coordinate?

### Exceptions

- What failures are predictable?
- Which invalid inputs should be rejected?
- Which exception type should be handled?

### Debugging

- Can I reproduce the problem?
- Which line or function is responsible?
- What are the variable values at that point?

### OOP

- What entity has state?
- What attributes belong to it?
- What behavior belongs with that state?
- Do I actually need a class?

This is the shift from:

> "How do I make this code run?"

to:

> **"How do I design this code so another engineer can understand, test, reuse, and extend it?"**

---

# 27. Responsible Use of AI Tools

AI assistants can be useful for learning structured programming.

Useful prompts include:

- "Do not rewrite my code. Help me identify which parts should become functions."
- "What should the inputs and return value of this function be?"
- "Explain the difference between a module and a class using my example."
- "Help me interpret this Python traceback."
- "Give me three test cases that could expose bugs in this function."
- "Explain why this exception occurs without giving me the final code."
- "Is a class necessary here, or would functions be simpler? Explain why."
- "Trace how `self` changes in this object after each method call."

Use AI to improve your reasoning.

Do not use it as a replacement for understanding the program.

---

# 28. Self-Check Questions

Before moving to the lab, make sure you can answer:

1. Why are small functions easier to maintain than one large function?
2. What is the difference between a parameter and an argument?
3. What is the difference between `print()` and `return`?
4. What is local scope?
5. What is a Python module?
6. What is the difference between `import module` and `from module import function`?
7. Why is `if __name__ == "__main__":` useful?
8. What is the difference between a syntax error, runtime error, and logic error?
9. Why should you catch specific exceptions?
10. What information does a traceback provide?
11. What is a class?
12. What is an object?
13. What is an attribute?
14. What is a method?
15. What does `self` refer to?
16. When might a function be better than a class?
17. Why do machine-learning libraries often represent models as objects?

---

# 29. Looking Ahead

In Week 4, we will focus on:

- lists
- tuples
- dictionaries
- sets
- comprehensions
- choosing appropriate data structures
- file organization
- clean-code practices

Week 2 taught you how to **express logic**.

Week 3 teaches you how to **structure that logic**.

Week 4 will teach you how to **organize the data that logic works on**.

---

## Week 3 Key Message

> **Working code solves today's problem. Structured code makes tomorrow's changes manageable.**

Functions, modules, exception handling, debugging, and OOP are not separate Python topics. Together, they form the foundation for building maintainable AI applications.
