# AI-216: Programming for Artificial Intelligence
## Week 02 Lecture Handout — Python Fundamentals for AI Problem Solving

**Semester:** Fall 2026  
**Week:** 02  
**Theme:** From problem statements to clear Python logic

---

## 1. Why Week 2 Matters

AI engineering starts with programming fundamentals.

Before working with datasets, machine-learning libraries, APIs, or LLMs, you need to be able to take a problem and express it clearly as code.

This week focuses on:

```text
Problem
  ↓
Inputs
  ↓
Variables & Data Types
  ↓
Operators
  ↓
Decisions
  ↓
Repetition
  ↓
Functions
  ↓
Output
```

The goal is not to memorize Python syntax. The goal is to learn how to translate a problem into a sequence of steps that a computer can execute.

---

## 2. Week 2 Learning Outcomes

By the end of this lecture, you should be able to:

1. Use variables and appropriate Python data types to represent information.
2. Apply arithmetic, comparison, and logical operators correctly.
3. Use `if`, `elif`, and `else` to express decision rules.
4. Use `for` and `while` loops to repeat operations.
5. Write simple functions with parameters and return values.
6. Break a small problem into **input → processing → output**.
7. Trace and explain the behavior of a short Python program.

---

## 3. The Problem-Solving Mindset

Before writing code, ask:

1. What information do I have?
2. What output do I need?
3. What calculations are required?
4. What decisions are required?
5. What work needs to repeat?
6. What logic should be placed inside a function?

A simple mental model is:

```text
Input
  ↓
Process
  ↓
Output
```

For larger problems:

```text
Input
  ↓
Validate
  ↓
Transform
  ↓
Decide / Repeat
  ↓
Return Result
  ↓
Output
```

This same thinking appears later in data preprocessing, feature engineering, machine-learning pipelines, API request handling, model inference, and AI application workflows.

### Basic Example

**Problem:** Calculate the sum of two marks.

```python
mark1 = 70
mark2 = 80

total = mark1 + mark2
print(total)
```

### Intermediate Example

**Problem:** Calculate average marks and determine whether a student passed.

```python
marks = [65, 72, 80]
average = sum(marks) / len(marks)

if average >= 50:
    print("Pass")
else:
    print("Fail")
```

### Advanced Example

**Problem:** Evaluate several model scores and summarize whether enough experiments meet a quality threshold.

```python
scores = [0.78, 0.86, 0.91, 0.69, 0.88]
threshold = 0.85

qualified = 0

for score in scores:
    if score >= threshold:
        qualified += 1

percentage = (qualified / len(scores)) * 100
print("Qualified experiments:", qualified)
print("Qualified percentage:", percentage)
```

---

## 4. Variables

A variable gives a name to a value.

### Basic Example

```python
name = "Ali"
age = 20
print(name, age)
```

### Intermediate Example

```python
obtained_marks = 423
total_marks = 500
percentage = (obtained_marks / total_marks) * 100

print("Percentage:", percentage)
```

### Advanced Example

```python
model_name = "baseline_classifier"
accuracy = 0.88
target_accuracy = 0.85
meets_target = accuracy >= target_accuracy

print("Model:", model_name)
print("Accuracy:", accuracy)
print("Meets target:", meets_target)
```

Prefer descriptive names:

```python
monthly_income = 75000
```

over:

```python
x = 75000
```

---

## 5. Core Data Types

Python commonly uses:

- `int`
- `float`
- `str`
- `bool`

### Basic Example

```python
students = 35
temperature = 27.5
course = "AI-216"
is_active = True

print(type(students))
print(type(temperature))
print(type(course))
print(type(is_active))
```

### Intermediate Example

```python
student_name = "Sara"
score = 82
percentage = 82.0
is_passing = score >= 50

print(student_name)
print(score)
print(percentage)
print(is_passing)
```

### Advanced Example

```python
model_name = "fraud_detector"
version = 2
accuracy = 0.934
is_production_ready = accuracy >= 0.90

print(f"{model_name} v{version}")
print("Accuracy:", accuracy)
print("Production ready:", is_production_ready)
```

---

## 6. Type Conversion

`input()` returns text, so conversion is often needed.

### Basic Example

```python
age = int(input("Enter age: "))
print(age + 1)
```

### Intermediate Example

```python
price = float(input("Enter price: "))
quantity = int(input("Enter quantity: "))

total = price * quantity
print("Total:", total)
```

### Advanced Example

```python
accuracy_text = input("Enter model accuracy: ")
accuracy = float(accuracy_text)

meets_target = accuracy >= 0.85

print("Accuracy:", accuracy)
print("Meets target:", meets_target)
```

Invalid conversions can raise errors. Formal exception handling comes in Week 3.

---

## 7. Operators

Operators allow calculations, comparisons, and logical decisions.

### Basic Example — Arithmetic

```python
a = 10
b = 3

print(a + b)
print(a - b)
print(a * b)
print(a / b)
```

### Intermediate Example — Comparison

```python
student_score = 72
passing_score = 50

print(student_score >= passing_score)
print(student_score == passing_score)
```

### Advanced Example — Logical Conditions

```python
attendance = 82
marks = 74
fee_paid = True

is_eligible = attendance >= 75 and marks >= 50 and fee_paid

print("Eligible:", is_eligible)
```

---

## 8. Conditional Statements

Use conditions to express rules.

### Basic Example

```python
marks = 60

if marks >= 50:
    print("Pass")
else:
    print("Fail")
```

### Intermediate Example

```python
marks = 78

if marks >= 85:
    grade = "A"
elif marks >= 70:
    grade = "B"
elif marks >= 50:
    grade = "C"
else:
    grade = "Fail"

print("Grade:", grade)
```

### Advanced Example

```python
attendance = 82
marks = 74
assignment_submitted = True

if attendance >= 75 and marks >= 50 and assignment_submitted:
    result = "Eligible"
elif attendance < 75:
    result = "Not eligible: low attendance"
elif marks < 50:
    result = "Not eligible: low marks"
else:
    result = "Not eligible: assignment missing"

print(result)
```

The order of conditions matters.

---

## 9. Boundary Conditions and Edge Cases

Many bugs happen at boundaries.

### Basic Example

```python
score = 50

if score >= 50:
    print("Pass")
```

### Intermediate Example

```python
usage = 5

if usage <= 5:
    package = "Basic"
elif usage <= 15:
    package = "Standard"
else:
    package = "Premium"

print(package)
```

### Advanced Example

```python
usage = -1

if usage < 0:
    print("Invalid usage")
elif usage <= 5:
    print("Basic")
elif usage <= 15:
    print("Standard")
else:
    print("Premium")
```

Always test:

```text
below boundary
exactly at boundary
above boundary
invalid values
```

---

## 10. `for` Loops

Use a `for` loop to process a sequence.

### Basic Example

```python
for i in range(5):
    print(i)
```

### Intermediate Example

```python
scores = [72, 88, 45, 91, 39]

for score in scores:
    print("Score:", score)
```

### Advanced Example

```python
scores = [72, 88, 45, 91, 39]
passed = 0
failed = 0

for score in scores:
    if score >= 50:
        passed += 1
    else:
        failed += 1

print("Passed:", passed)
print("Failed:", failed)
```

---

## 11. Counters and Accumulators

Counters track frequency; accumulators build totals.

### Basic Example — Counter

```python
count = 0

for value in [1, 2, 3]:
    count += 1

print(count)
```

### Intermediate Example — Accumulator

```python
total = 0

for value in [10, 20, 30]:
    total += value

print(total)
```

### Advanced Example — Both Together

```python
scores = [0.72, 0.88, 0.91, 0.67, 0.86]

total = 0
qualified = 0

for score in scores:
    total += score

    if score >= 0.85:
        qualified += 1

average = total / len(scores)

print("Average:", average)
print("Qualified:", qualified)
```

---

## 12. `range()`

`range()` is useful for controlled repetition.

### Basic Example

```python
for i in range(5):
    print(i)
```

### Intermediate Example

```python
for i in range(1, 6):
    print("Iteration:", i)
```

### Advanced Example

```python
for epoch in range(1, 11):
    if epoch % 2 == 0:
        print("Checkpoint at epoch:", epoch)
```

---

## 13. `while` Loops

A `while` loop repeats while a condition remains true.

### Basic Example

```python
count = 0

while count < 3:
    print(count)
    count += 1
```

### Intermediate Example

```python
attempts = 0
max_attempts = 3

while attempts < max_attempts:
    print("Attempt:", attempts + 1)
    attempts += 1
```

### Advanced Example

```python
balance = 1000
daily_cost = 180
days = 0

while balance >= daily_cost:
    balance -= daily_cost
    days += 1

print("Days affordable:", days)
print("Remaining balance:", balance)
```

---

## 14. `break` and `continue`

These keywords change loop flow.

### Basic Example — `break`

```python
for value in [2, 4, -1, 8]:
    if value < 0:
        break
    print(value)
```

### Intermediate Example — `continue`

```python
for value in [5, -2, 8, -1, 10]:
    if value < 0:
        continue
    print(value)
```

### Advanced Example

```python
scores = [0.82, None, 0.91, 0.88, -1, 0.86]

for score in scores:
    if score is None:
        continue

    if score < 0:
        print("Invalid score encountered")
        break

    print("Valid score:", score)
```

---

## 15. Functions

Functions group reusable logic.

### Basic Example

```python
def greet(name):
    print(f"Hello, {name}")

greet("Ali")
```

### Intermediate Example

```python
def calculate_percentage(obtained, total):
    return (obtained / total) * 100

result = calculate_percentage(423, 500)
print(result)
```

### Advanced Example

```python
def count_scores_above_threshold(scores, threshold):
    count = 0

    for score in scores:
        if score >= threshold:
            count += 1

    return count

scores = [0.78, 0.86, 0.91, 0.69, 0.88]
result = count_scores_above_threshold(scores, 0.85)

print("Qualified:", result)
```

Functions make programs easier to read, reuse, test, change, and organize.

---

## 16. `print()` vs `return`

This distinction is important.

### Basic Example — Print

```python
def show_message():
    print("Hello")
```

### Intermediate Example — Return

```python
def add(a, b):
    return a + b

result = add(10, 20)
print(result)
```

### Advanced Example

```python
def evaluate_score(score, threshold):
    return score >= threshold

result = evaluate_score(0.91, 0.85)

if result:
    print("Model meets target")
else:
    print("Model below target")
```

`print()` displays information. `return` sends a value back to the caller.

---

## 17. Integrated Problem-Solving Example

### Basic

```python
scores = [80, 70, 90]
average = sum(scores) / len(scores)
print(average)
```

### Intermediate

```python
scores = [80, 45, 90, 67]

passed = 0

for score in scores:
    if score >= 50:
        passed += 1

print("Passed:", passed)
```

### Advanced

```python
def summarize_scores(scores, threshold):
    total = 0
    qualified = 0

    for score in scores:
        total += score

        if score >= threshold:
            qualified += 1

    average = total / len(scores)
    percentage = (qualified / len(scores)) * 100

    return average, qualified, percentage


scores = [0.78, 0.86, 0.91, 0.69, 0.88]
average, qualified, percentage = summarize_scores(scores, 0.85)

print("Average:", average)
print("Qualified:", qualified)
print("Qualified percentage:", percentage)
```

This combines variables, operators, loops, conditionals, and functions.

---

## 18. Tracing Code

Tracing helps you understand program state.

### Basic Example

```python
x = 5
x = x + 2
print(x)
```

### Intermediate Example

```python
total = 0

for value in [2, 4, 6]:
    total += value

print(total)
```

### Advanced Example

```python
total = 0
count = 0

for value in [2, 4, 6, 8]:
    if value > 3:
        total += value
        count += 1

average = total / count
print(average)
```

Trace variables after each iteration to understand why the result is correct.

---

## 19. Common Mistakes

### Basic — Assignment vs Comparison

```python
score = 80
score == 80
```

### Intermediate — Wrong condition order

```python
if marks >= 50:
    print("Pass")
elif marks >= 85:
    print("Distinction")
```

### Advanced — Infinite Loop

```python
count = 0

while count < 5:
    print(count)
```

The loop never updates `count`.

---

## 20. Readability Matters

### Basic

```python
score = 72
```

### Intermediate

```python
student_score = 72
passing_score = 50
```

### Advanced

```python
student_score = 72
passing_score = 50
attendance_percentage = 83

is_exam_eligible = (
    student_score >= passing_score
    and attendance_percentage >= 75
)

print(is_exam_eligible)
```

Good code communicates intent.

---

## 21. Responsible Use of AI Tools

Useful prompts:

- "Help me identify the inputs, processing, and outputs in this problem."
- "Do not give me the final code. Help me identify the conditions I need."
- "Trace this loop one iteration at a time."
- "Explain why my condition fails for this boundary value."
- "Review my function names for clarity."
- "Give me three test cases for this logic."

AI tools should support your reasoning, not replace it.

---

## 22. Self-Check Questions

1. What is the difference between `int`, `float`, `str`, and `bool`?
2. What is the difference between `=` and `==`?
3. When would you use `and` instead of `or`?
4. What is the difference between `for` and `while`?
5. What is a counter?
6. What is an accumulator?
7. Why are functions useful?
8. What is the difference between printing a result and returning a result?
9. What does `input()` return by default?
10. How would you break a problem into input, process, and output?

---

## 23. Looking Ahead

In Week 3, we will build on these fundamentals and focus on:

- Functions in more depth
- Modules
- Exceptions
- Debugging
- Object-oriented programming for AI applications

Week 2 is about **expressing logic clearly**.

Week 3 is about **organizing that logic into better software**.

---

## Week 2 Key Message

> **Do not start with Python syntax. Start with the problem, identify the logic, and then express that logic in Python.**
