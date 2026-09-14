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

### Lecture Agenda — Typical 2-Hour Flow

| Time | Activity |
| --- | --- |
| 0:00 – 0:10 | Problem-solving mindset: input → process → output |
| 0:10 – 0:35 | Variables, data types, lists, type conversion, operators |
| 0:35 – 0:55 | Conditionals, boundary conditions, and edge cases |
| 0:55 – 1:05 | Short break / quick tracing exercise |
| 1:05 – 1:30 | `for` loops, `range()`, counters, accumulators, `while`, `break` / `continue` |
| 1:30 – 1:50 | Functions, `print()` vs `return`, integrated example |
| 1:50 – 2:00 | Common mistakes, self-check, exit ticket |

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

Output:

```text
Qualified experiments: 3
Qualified percentage: 60.0
```

Do not worry if some parts of these examples are new — lists, `sum()`, `len()`, loops, and `+=` are all explained later in this handout.

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

In the intermediate example, `percentage = 82.0` was typed by hand. In real code, you would usually **calculate** it from other values so it can never disagree with them:

```python
score = 82
max_score = 100
percentage = (score / max_score) * 100   # 82.0 — a float, because / always returns a float
```

### f-strings

`f"{model_name} v{version}"` is an **f-string**. The `f` before the quotes lets you place variables (or expressions) inside `{ }`:

```python
name = "Sara"
score = 82
print(f"{name} scored {score} marks")      # Sara scored 82 marks
print(f"Next year: {score + 5}")           # Next year: 87
```

### Lists — Storing Many Values

Many examples in this handout use a **list**: an ordered collection of values written inside square brackets.

```python
scores = [72, 88, 45, 91, 39]

print(type(scores))    # <class 'list'>
print(scores[0])       # 72  — the first item (positions start at 0)
print(len(scores))     # 5   — how many items
print(sum(scores))     # 335 — total of numeric items
print(max(scores))     # 91
print(min(scores))     # 39
```

Lists are covered in depth in Week 4. For now, you only need to create a list, loop over it, and use `len()` and `sum()`.

### `None` — "No Value"

`None` is a special value meaning "nothing here" or "missing". Its type is `NoneType`.

```python
best_score = None          # not known yet
print(best_score)          # None
print(best_score is None)  # True
```

Use `is None` (not `== None`) to check for it. You will see `None` in datasets as missing values, and as the result of functions that do not `return` anything (see Section 16).

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

### More Arithmetic Operators

| Operator | Meaning | Example | Result |
| --- | --- | --- | --- |
| `+` | Addition | `7 + 2` | `9` |
| `-` | Subtraction | `7 - 2` | `5` |
| `*` | Multiplication | `7 * 2` | `14` |
| `/` | Division (always a float) | `7 / 2` | `3.5` |
| `//` | Floor division (whole part) | `7 // 2` | `3` |
| `%` | Modulus (remainder) | `7 % 2` | `1` |
| `**` | Power | `2 ** 3` | `8` |

```python
total_seconds = 135

minutes = total_seconds // 60     # 2
seconds = total_seconds % 60      # 15
print(minutes, "min", seconds, "sec")

epoch = 6
print(epoch % 2 == 0)             # True — an even number has remainder 0 when divided by 2
```

### All Logical Operators — `and`, `or`, `not`

| Operator | True when… |
| --- | --- |
| `and` | **both** sides are true |
| `or` | **at least one** side is true |
| `not` | flips `True` ↔ `False` |

```python
has_scholarship = False
fee_paid = True

can_enroll = fee_paid or has_scholarship   # True — one condition is enough
print("Can enroll:", can_enroll)

is_blocked = False
print("Access allowed:", not is_blocked)  # True
```

Use `and` when **every** rule must hold (eligibility checks). Use `or` when **any one** rule is enough (alternative ways to qualify).

### Floating-Point Results and Formatting

Computers store decimals approximately, so results can look strange:

```python
print(10 / 3)            # 3.3333333333333335
print(0.1 + 0.2)         # 0.30000000000000004
print(0.1 + 0.2 == 0.3)  # False
```

For display, round or format the value:

```python
average = 10 / 3

print(round(average, 2))      # 3.33
print(f"{average:.2f}")       # 3.33  — f-string with 2 decimal places
```

Two practical rules:

- Format floats when **printing** results (`:.2f`).
- Avoid checking floats with `==`; when exact equality matters (for example, money), prefer whole numbers such as rupees or paisa.

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

Python checks an `if / elif / else` chain **from top to bottom** and runs **only the first** branch whose condition is true. In the advanced example, a student with low attendance **and** low marks sees only `"Not eligible: low attendance"`.

If you need to report **every** failed rule, use separate `if` statements instead of `elif`:

```python
attendance = 60
marks = 40
assignment_submitted = False

if attendance < 75:
    print("Low attendance")
if marks < 50:
    print("Low marks")
if not assignment_submitted:
    print("Assignment missing")
```

Output:

```text
Low attendance
Low marks
Assignment missing
```

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

Also watch for **empty input**. Dividing by the number of items fails when there are no items:

```python
scores = []

if len(scores) == 0:
    print("No scores to analyze")
else:
    print("Average:", sum(scores) / len(scores))
```

Without the check, `sum(scores) / len(scores)` raises `ZeroDivisionError`.

Write boundary rules precisely. `"5–15 GB"` is ambiguous; `"> 5 and <= 15"` is not:

```python
if usage > 5 and usage <= 15:
    package = "Standard"

# Python also allows a chained comparison with the same meaning:
if 5 < usage <= 15:
    package = "Standard"
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

Output:

```text
Average: 0.808
Qualified: 3
```

Pattern to remember:

```text
counter:      start at 0 → add 1 when something happens
accumulator:  start at 0 → add the value each time
average:      accumulator / number of items   (check the count is not 0)
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

`range()` can take up to three values:

| Call | Produces | Meaning |
| --- | --- | --- |
| `range(5)` | `0, 1, 2, 3, 4` | stop (not included) |
| `range(1, 6)` | `1, 2, 3, 4, 5` | start, stop |
| `range(0, 10, 2)` | `0, 2, 4, 6, 8` | start, stop, step |
| `range(5, 0, -1)` | `5, 4, 3, 2, 1` | counting down |

```python
for epoch in range(2, 11, 2):
    print("Checkpoint at epoch:", epoch)
```

This prints the same checkpoints as the advanced example, using a step of `2` instead of `%`.

The stop value is **never included** — `range(1, 6)` stops at `5`. This is a common source of off-by-one errors.

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

Output:

```text
Days affordable: 5
Remaining balance: 100
```

### When to Choose `while` Instead of `for`

| Use `for` when… | Use `while` when… |
| --- | --- |
| you know **what** to loop over (a list, a `range`) | you do **not** know in advance how many repetitions are needed |
| the number of repetitions is fixed | the loop should stop when a **condition** changes |

The intermediate example above could also be written with `for attempt in range(3)`. A situation where `while` is the natural choice is repeating until the user types a stop value (a **sentinel**):

```python
total = 0
count = 0

value = input("Enter a score (or 'done' to finish): ")

while value != "done":
    total += float(value)
    count += 1
    value = input("Enter a score (or 'done' to finish): ")

if count > 0:
    print(f"Average of {count} scores: {total / count:.2f}")
else:
    print("No scores entered")
```

Nobody knows beforehand how many scores the user will type, so `while` fits better than `for`.

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

Output:

```text
Valid score: 0.82
Valid score: 0.91
Valid score: 0.88
Invalid score encountered
```

- `None` (a missing value, see Section 5) is **skipped** with `continue`; the loop moves to the next item.
- `-1` is invalid, so `break` **stops the loop completely** — `0.86` is never printed.

| Keyword | Effect |
| --- | --- |
| `continue` | skip the rest of **this** iteration, go to the next item |
| `break` | exit the loop immediately |

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

Parts of a function:

```text
def calculate_percentage(obtained, total):     ← def + name + parameters
    return (obtained / total) * 100            ← body (indented) + return value

result = calculate_percentage(423, 500)        ← call with arguments → result is 84.6
```

- **Parameters** are the names in the definition (`obtained`, `total`).
- **Arguments** are the actual values passed in the call (`423`, `500`).
- Code inside the function does not run until the function is **called**.

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

The basic example only **defines** `show_message`. Nothing appears until it is called:

```python
def show_message():
    print("Hello")

show_message()             # Hello
```

A common mistake is trying to use the result of a function that prints but does not return:

```python
def add_and_print(a, b):
    print(a + b)           # displays 30, but returns nothing

def add_and_return(a, b):
    return a + b           # sends 30 back

result1 = add_and_print(10, 20)
result2 = add_and_return(10, 20)

print("result1:", result1)   # result1: None
print("result2:", result2)   # result2: 30
```

A function without `return` gives back `None`. If a value is needed later — for a calculation, a condition, or a test — **return** it, and let the caller decide whether to print it.

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

Output:

```text
Average: 0.8240000000000001
Qualified: 3
Qualified percentage: 60.0
```

Two things to notice:

**1. Returning several values.** `return average, qualified, percentage` sends back three values together (as a *tuple*). The line

```python
average, qualified, percentage = summarize_scores(scores, 0.85)
```

**unpacks** them into three variables, in the same order.

**2. Cleaner output and a safer function.** The average shows floating-point noise (see Section 7). A slightly improved version formats the output and handles an empty list:

```python
def summarize_scores_safe(scores, threshold):
    if len(scores) == 0:
        return 0, 0, 0

    total = 0
    qualified = 0

    for score in scores:
        total += score

        if score >= threshold:
            qualified += 1

    average = total / len(scores)
    percentage = (qualified / len(scores)) * 100

    return average, qualified, percentage


average, qualified, percentage = summarize_scores_safe([0.78, 0.86, 0.91, 0.69, 0.88], 0.85)

print(f"Average: {average:.2f}")                    # Average: 0.82
print(f"Qualified: {qualified}")                    # Qualified: 3
print(f"Qualified percentage: {percentage:.2f}%")   # Qualified percentage: 60.00%
```

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

### Worked Trace Table — Advanced Example

| Step | `value` | `value > 3`? | `total` | `count` |
| --- | --- | --- | --- | --- |
| start | – | – | 0 | 0 |
| 1 | 2 | False | 0 | 0 |
| 2 | 4 | True | 4 | 1 |
| 3 | 6 | True | 10 | 2 |
| 4 | 8 | True | 18 | 3 |
| after loop | – | – | 18 | 3 |

`average = 18 / 3` → prints `6.0`.

Answers for the other two examples: the basic example prints `7`; the intermediate example prints `12`.

**Try it:** change the list to `[1, 2, 3]` and trace again. No value is greater than `3`, so `count` stays `0` and `total / count` raises `ZeroDivisionError`. How would you guard against that? (Hint: Section 9.)

You can step through any of these programs visually at [Python Tutor](https://pythontutor.com/).

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

### Why These Are Mistakes — and How to Fix Them

**Basic — Assignment vs Comparison**

- `=` **stores** a value: `score = 80`.
- `==` **compares** two values and produces `True` or `False`.

The line `score == 80` on its own compares and then throws the result away — it does nothing useful. The more common real mistake is using `=` where a comparison is needed:

```python
# Wrong — SyntaxError
if score = 80:
    print("Exactly 80")

# Correct
if score == 80:
    print("Exactly 80")
```

**Intermediate — Wrong condition order**

Any mark that is `>= 85` is also `>= 50`, so the first branch always wins and `"Distinction"` can **never** be printed. Put the most specific (strictest) condition first:

```python
marks = 90

# Correct
if marks >= 85:
    print("Distinction")
elif marks >= 50:
    print("Pass")
else:
    print("Fail")
```

**Advanced — Infinite Loop**

> ⚠️ If you run the infinite-loop example, it prints `0` forever. Stop it with **Ctrl + C** in the terminal (or the Stop button in your editor).

Make sure something inside the loop moves the condition toward `False`:

```python
count = 0

while count < 5:
    print(count)
    count += 1      # without this line the loop never ends
```

### More Mistakes to Watch For

**Forgetting to convert `input()`**

```python
age = input("Enter age: ")
print(age + 1)        # TypeError: can only concatenate str (not "int") to str

age = int(input("Enter age: "))
print(age + 1)        # works
```

**Dividing by an empty count**

```python
scores = []
average = sum(scores) / len(scores)   # ZeroDivisionError
```

Check `len(scores) > 0` first (see Section 9).

**Off-by-one with `range()`**

```python
for i in range(1, 5):
    print(i)          # prints 1 to 4, not 1 to 5
```

**Resetting a counter inside the loop**

```python
for score in [60, 70, 80]:
    passed = 0        # wrong: reset on every iteration
    if score >= 50:
        passed += 1

print(passed)         # 1, not 3 — create the counter before the loop
```

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
11. What is the difference between `/`, `//`, and `%`?
12. What is the difference between `break` and `continue`?
13. What does a function return if it has no `return` statement?
14. Why should you test values exactly at a boundary?

<details>
<summary><strong>Answer Key</strong> — try the questions first</summary>

1. `int` stores whole numbers (`35`), `float` stores decimals (`27.5`), `str` stores text (`"AI-216"`), and `bool` stores `True` or `False`.
2. `=` assigns a value to a variable; `==` compares two values and gives `True` or `False`.
3. Use `and` when **all** conditions must be true (e.g., attendance **and** marks **and** fee). Use `or` when **any one** condition is enough (e.g., fee paid **or** scholarship).
4. `for` loops over a known sequence or a fixed number of repetitions; `while` repeats as long as a condition stays true, useful when the number of repetitions is unknown.
5. A variable that starts at `0` and increases by `1` each time something happens (e.g., counting passed students).
6. A variable that starts at `0` and adds a value each time to build a total (e.g., summing scores).
7. They avoid repeating code, give logic a meaningful name, and make programs easier to read, test, reuse, and change.
8. Printing only displays a value on the screen; returning sends the value back to the caller so it can be stored, compared, or used in further calculations.
9. A string (`str`) — even if the user types a number.
10. Identify what information is given (input), which calculations/decisions/repetition are needed (process), and what result must be produced (output).
11. `/` gives a float result (`7 / 2 → 3.5`), `//` gives the whole-number part (`7 // 2 → 3`), and `%` gives the remainder (`7 % 2 → 1`).
12. `continue` skips the rest of the current iteration and moves to the next item; `break` exits the loop entirely.
13. `None`.
14. Most logic bugs happen at boundaries — for example, using `>` instead of `>=` gives the wrong answer only for the exact boundary value.

</details>

---

## Exit Ticket

Before leaving, answer on paper or in your notes:

1. Write the rule "Standard package is above 5 GB up to and including 15 GB" as a Python condition.
2. Predict the output:

   ```python
   count = 0
   for value in [3, 8, 5, 10]:
       if value >= 5:
           count += 1
   print(count)
   ```

3. One thing from today that was clear, and one thing that is still confusing.

---

## Quick Glossary — Week 2

| Term | Meaning |
| --- | --- |
| **Variable** | A name that refers to a value |
| **Data type** | The kind of value: `int`, `float`, `str`, `bool`, `list`, `NoneType` |
| **Type conversion** | Changing a value from one type to another, e.g. `int("5")` |
| **Operator** | A symbol that performs an operation: `+`, `>=`, `and`, … |
| **Condition** | An expression that is `True` or `False` |
| **Boundary case** | A test value exactly at the edge of a rule (e.g. `50` for "pass at 50") |
| **Edge case** | An unusual input such as a negative value or an empty list |
| **Loop / iteration** | Repeating code; one pass through the loop is one iteration |
| **Counter** | A variable that counts how many times something happens |
| **Accumulator** | A variable that builds a running total |
| **Sentinel** | A special value that tells a loop to stop (e.g. `"done"`) |
| **Function** | A named, reusable block of code |
| **Parameter / argument** | Name in the function definition / value passed in the call |
| **Return value** | The value a function sends back to its caller |
| **f-string** | A string starting with `f` that can embed values in `{ }` |
| **Tracing** | Following a program step by step while recording variable values |

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
