# AI-216: Programming for Artificial Intelligence
## Lab 02 — Python Fundamentals for Problem Solving

**Semester:** Fall 2026  
**Week:** 02  
**Topic:** Variables, operators, conditionals, loops, functions, and problem decomposition

---

## 1. Objective

This lab develops the Python problem-solving skills introduced in the Week 2 lecture.

By the end of the lab, you should be able to:

- Represent information using variables and appropriate data types.
- Use arithmetic, comparison, and logical operators.
- Implement decisions with `if`, `elif`, and `else`.
- Process multiple values using loops.
- Use counters and accumulators.
- Write simple functions using parameters and return values.
- Break a problem into **input → processing → output**.
- Test your logic using normal and boundary cases.
- Commit and document your work in your AI-216 GitHub repository.

No external Python libraries are required.

---

## 2. Before You Start

Make sure your Week 1 setup is working:

```bash
python --version
git --version
git status
```

Create:

```text
labs/week02/
```

Recommended structure:

```text
labs/week02/
├── task01_expense_tracker.py
├── task02_package_advisor.py
├── task03_temperature_monitor.py
├── task04_score_analysis.py
├── task05_eligibility.py
├── task06_functions.py
└── README.md
```

Also add the files for Task 7 and the optional challenge:

```text
labs/week02/
├── task07_decomposition.py        # Task 7 — whichever option you choose
└── optional_menu.py               # Optional Challenge (if attempted)
```

Run each file from the repository root, for example:

```bash
python labs/week02/task01_expense_tracker.py
```

> **Note:** This `README.md` is the instructor's lab handout. The `labs/week02/README.md` you create (Section 15) lives in **your own** coursework repository.

---

## 3. General Coding Requirements

For every task:

1. Read the problem before writing code.
2. Identify the **input**.
3. Identify the **processing / rules**.
4. Identify the **output**.
5. Use meaningful variable names.
6. Run and test your program.
7. Do not submit code you cannot explain.

Where appropriate, test:

- A normal case
- A boundary case
- An unusual case

Output formatting:

- Display floats and percentages with **2 decimal places**, e.g. `f"{average:.2f}"` or `f"{percentage:.2f}%"`.
- Label every printed value so the output can be understood without reading the code.

---

## 4. Task 1 — Daily Expense Tracker

A student wants to track daily spending.

Store:

- Food expense
- Transport expense
- Other expense
- Daily budget

Your program should:

1. Calculate total expense.
2. Calculate remaining budget.
3. Determine whether the student is:
   - Within budget
   - Exactly at budget
   - Over budget
4. Display the result clearly.

At the top of your file, add:

```python
# Input:
# Processing:
# Output:
```

**Clarifications:**

- Store the values directly in variables (no `input()` needed for this task).
- Use **whole-number** amounts (e.g., PKR `450`), not decimals. Floats such as `0.1 + 0.2` are not exactly `0.3`, which can break the "exactly at budget" check.
- To test the three outcomes, change the values and re-run the program. Record each case in your `README.md`.

Example values:

```python
food_expense = 450
transport_expense = 200
other_expense = 150
daily_budget = 1000
```

Expected output for these values (your wording may differ):

```text
Total expense: 800
Remaining budget: 200
Status: Within budget
```

Test at least:

| Food | Transport | Other | Budget | Expected status |
| --- | --- | --- | --- | --- |
| 450 | 200 | 150 | 1000 | Within budget (remaining 200) |
| 500 | 300 | 200 | 1000 | Exactly at budget (remaining 0) |
| 600 | 300 | 250 | 1000 | Over budget (remaining -150) |

**Concepts:** variables, numeric types, arithmetic, comparisons, `if / elif / else`

---

## 5. Task 2 — Internet Package Advisor

Use these rules:

```text
0–5 GB       → Basic
Above 5–15   → Standard
Above 15     → Premium
```

Precise rules (use these for your conditions):

| Package | Rule |
| --- | --- |
| Invalid | `usage < 0` |
| Basic | `0 <= usage <= 5` |
| Standard | `5 < usage <= 15` |
| Premium | `usage > 15` |

Your program should:

1. Ask the user for data usage in GB.
2. Convert the input to a numeric type.
3. Reject negative values with a clear message.
4. Recommend the package.

Test at least:

```text
0
5
5.1
15
15.1
-1
```

Record the results in `README.md`.

Expected results:

| Input (GB) | Expected output |
| --- | --- |
| `0` | Basic |
| `5` | Basic |
| `5.1` | Standard |
| `15` | Standard |
| `15.1` | Premium |
| `-1` | Invalid — usage cannot be negative |

Use `float()` (not `int()`) for the conversion, because `5.1` is a valid input.

> **Out of scope this week:** non-numeric input such as `abc` will crash `float()` with a `ValueError`. You do not need to handle that yet — exception handling is covered in Week 3.

**Concepts:** `input()`, type conversion, conditionals, boundary testing

---

## 6. Task 3 — Temperature Monitoring

Use:

```python
temperatures = [21.5, 29.0, 32.5, 18.0, 35.2, 27.8, 14.0]
```

Classify each reading:

```text
Below Normal → below 15°C
Normal       → 15°C to 30°C inclusive
High         → above 30°C
```

Your program should:

1. Loop through all readings.
2. Print the category for each reading.
3. Count each category.
4. Print a final summary.

Precise rules:

| Category | Rule |
| --- | --- |
| Below Normal | `temperature < 15` |
| Normal | `15 <= temperature <= 30` |
| High | `temperature > 30` |

Expected summary (check your output against this):

```text
Below Normal: 1
Normal: 4
High: 2
```

After it works, add `15.0` and `30.0` to the list and confirm both are counted as **Normal**.

**Concepts:** lists, `for` loops, counters, conditions

---

## 7. Task 4 — Model Score Analysis

Use:

```python
scores = [0.72, 0.81, 0.88, 0.91, 0.67, 0.86, 0.79]
threshold = 0.85
```

Your program should:

1. Count scores meeting or exceeding the threshold.
2. Count scores below the threshold.
3. Calculate the average score.
4. Calculate the percentage meeting the target.
5. Print a clear summary.

Do not use NumPy or Pandas.

Add a comment answering:

> Why might this logic later be placed inside a reusable function?

Print the average and percentage with 2 decimal places. Expected summary:

```text
Meeting target (>= 0.85): 3
Below target: 4
Average score: 0.81
Percentage meeting target: 42.86%
```

Think about it: what would happen if `scores` were an empty list? Add a check so the program prints a clear message instead of crashing with `ZeroDivisionError`.

**Concepts:** loops, counters, accumulators, arithmetic, conditions

---

## 8. Task 5 — Applicant Eligibility

A training program accepts applicants if:

- Age is at least 18.
- Programming score is at least 60.
- Prerequisite course is completed.

For one applicant:

1. Store the information.
2. Check all conditions.
3. Print whether the applicant is eligible.
4. If not eligible, identify which requirement(s) were not met.

Example:

```python
age = 19
programming_score = 72
prerequisite_completed = True
```

Test at least three applicants.

**How to test:** change the three values and re-run the program for each applicant, then record the results in your `README.md`. (In Task 6 you will see how functions make repeated checks like this easier.)

Use at least these applicants:

| Applicant | Age | Programming score | Prerequisite completed | Expected result |
| --- | --- | --- | --- | --- |
| 1 | 19 | 72 | `True` | Eligible |
| 2 | 18 | 60 | `True` | Eligible (both values exactly at the boundary) |
| 3 | 17 | 55 | `False` | Not eligible — age, programming score, **and** prerequisite not met |

**Hint:** an `if / elif` chain stops at the **first** true condition, so it can report only one failed requirement. To list **every** requirement that was not met, use separate `if` statements:

```python
if age < 18:
    print("- Age requirement not met")
if programming_score < 60:
    print("- Programming score requirement not met")
if not prerequisite_completed:
    print("- Prerequisite course not completed")
```

**Concepts:** booleans, `and`, multiple conditions

---

## 9. Task 6 — Refactor Logic into Functions

Create:

```python
def calculate_percentage(obtained, total):
    ...

def is_passing(score, passing_score):
    ...

def count_values_above_threshold(values, threshold):
    ...
```

Requirements:

- `calculate_percentage` returns a percentage.
- `is_passing` returns `True` or `False`.
- `count_values_above_threshold` returns a count.
- Do not print inside these functions.
- Call each function with sample values and print the returned result.

Clarifications:

- `is_passing` should return `True` when `score >= passing_score` (a score exactly at the passing mark passes).
- `count_values_above_threshold` should count values that **meet or exceed** the threshold (`>=`), matching Task 4.
- Optional: make `calculate_percentage` return `0` when `total` is `0`, instead of crashing.

Example calls and expected results:

```python
print(calculate_percentage(423, 500))        # 84.6
print(is_passing(50, 50))                    # True
print(is_passing(49, 50))                    # False
print(count_values_above_threshold([0.72, 0.81, 0.88, 0.91, 0.67, 0.86, 0.79], 0.85))  # 3
```

**Concepts:** `def`, parameters, return values, reusable logic

---

## 10. Task 7 — Problem Decomposition Challenge

Choose one.

### Option A — Electricity Usage Classification

```text
Low    → below 5 kWh
Normal → 5–10 kWh
High   → above 10 kWh
```

Produce counts and percentages.

Use:

```python
daily_usage_kwh = [3.2, 5.0, 7.5, 10.0, 12.4, 4.9, 10.1]
```

Precise rules: Low `< 5`, Normal `5 <= usage <= 10`, High `> 10`.

Expected summary:

```text
Low: 2 (28.57%)
Normal: 3 (42.86%)
High: 2 (28.57%)
```

### Option B — Attendance Analysis

Given attendance percentages:

- Count students with attendance `>= 75%`.
- Count students below the requirement.
- Calculate the eligible percentage.

Use:

```python
attendance = [82, 74.9, 75, 91, 60, 88, 70]
```

Expected summary:

```text
Eligible (>= 75%): 4
Below requirement: 3
Eligible percentage: 57.14%
```

### Option C — AI Service Request Monitor

Use:

```python
response_times = [250, 420, 180, 900, 310, 1500, 275]
```

Classify:

```text
Fast       → <= 300 ms
Acceptable → 301–700 ms
Slow       → > 700 ms
```

Produce a summary.

Precise rules: Fast `<= 300`, Acceptable `300 < time <= 700`, Slow `> 700`.

Expected summary:

```text
Fast: 3
Acceptable: 2
Slow: 2
```

Whichever option you choose, put your solution in `task07_decomposition.py`.

Before coding, add:

```python
# Problem:
# Inputs:
# Rules:
# Repetition:
# Outputs:
```

---

## 11. Optional Challenge — Menu with a `while` Loop

Create:

```text
1. Check pass/fail
2. Calculate percentage
3. Exit
```

Continue until the user chooses `3`.

Hints:

- Reuse `is_passing` and `calculate_percentage` from Task 6 (copy them into `optional_menu.py`; importing between files is covered in Week 3).
- Keep the menu choice as a string and compare with `"1"`, `"2"`, `"3"` — this avoids converting invalid menu input.
- Print a clear message for any other choice, then show the menu again.

Loop skeleton:

```python
choice = ""

while choice != "3":
    print("1. Check pass/fail")
    print("2. Calculate percentage")
    print("3. Exit")
    choice = input("Choose an option: ")

    # handle "1", "2", "3", and invalid choices here
```

**Concepts:** `while`, input, conditions, function calls

---

## 12. Testing Your Programs

For condition-based programs, test:

```text
below boundary
exactly at boundary
above boundary
```

For example:

```text
49
50
51
```

Formal exception handling comes in Week 3, so focus here on logical validation.

Also check:

- **Unusual values** — negative numbers, zero, and empty lists.
- **Float comparisons** — avoid `==` with decimals (`0.1 + 0.2 == 0.3` is `False`); use whole numbers where exact equality matters.
- **Expected results** — compare your output with the expected summaries given in the tasks. If they differ, trace your loop one iteration at a time (or use [Python Tutor](https://pythontutor.com/)).

A simple way to record test cases in your `README.md`:

| Task | Input | Expected | Actual | Pass? |
| --- | --- | --- | --- | --- |
| 2 | `5` | Basic | Basic | ✅ |
| 2 | `5.1` | Standard | Standard | ✅ |
| 2 | `-1` | Invalid | Invalid | ✅ |

---

## 13. Code Quality Expectations

Prefer:

```python
passing_score = 50
student_score = 72
```

over:

```python
x = 50
y = 72
```

Use meaningful names, consistent indentation, reasonable spacing, and comments only where they add value.

---

## 14. Git Workflow

Do not wait until the entire lab is finished before committing.

Example:

```bash
git status
git add labs/week02/task01_expense_tracker.py
git commit -m "Lab02: add expense tracker logic"

git add labs/week02/task02_package_advisor.py
git commit -m "Lab02: add package recommendation logic"

git add labs/week02/
git commit -m "Lab02: complete Python fundamentals exercises"

git push
```

Commit message guidance:

- Start with the lab and task: `Lab02: ...` (as above), so your history is easy to scan by week.
- Use a short, present-tense description of **what changed**: `Lab02: add boundary checks to package advisor`.
- Avoid vague messages such as `update`, `final`, or `changes`.
- Commit after each task works — at least one commit per task is a good habit.

---

## 15. Week 2 README

Create:

```text
labs/week02/README.md
```

Suggested structure:

```markdown
# Lab 02 — Python Fundamentals for Problem Solving

## Concepts Practiced
- Variables and data types
- Operators
- Conditionals
- Loops
- Functions

## Tasks Completed
1. Daily Expense Tracker
2. Internet Package Advisor
3. Temperature Monitoring
4. Model Score Analysis
5. Applicant Eligibility
6. Functions
7. Problem Decomposition Challenge

## Boundary / Test Cases
Describe at least three useful test cases.

## What I Found Difficult
-

## What I Learned
-

## AI Engineering Relevance
Explain where this type of logic could appear in:
- data processing
- validation
- model evaluation
- AI application code
```

---

## 16. AI-Assisted Learning

You may use an AI assistant to help understand, debug, test, or review your work unless otherwise instructed.

Useful prompts:

- "Do not write the solution. Help me identify the inputs, rules, and outputs."
- "Give me boundary test cases for this condition."
- "Trace my loop one iteration at a time."
- "Explain why my result is incorrect."
- "Review my variable names for readability."
- "Explain the difference between `print` and `return`."

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

---

## 17. Self-Study Learning Resources

### Python Official Tutorial

https://docs.python.org/3/tutorial/

Recommended topics:

- Using Python as a calculator
- First steps toward programming
- `if` statements
- `for` statements
- `range()`
- Defining functions

### Python Built-in Functions

https://docs.python.org/3/library/functions.html

Useful references:

```text
print()
input()
int()
float()
str()
len()
range()
type()
```

### Python Tutor

https://pythontutor.com/

Useful for visualizing:

- Variable values
- Loop iterations
- Function calls

Suggested study routine:

```text
Read one concept
    ↓
Type the example yourself
    ↓
Change the values
    ↓
Predict the output
    ↓
Run it
    ↓
Explain why it happened
```

---

## 18. Submission Checklist

- [ ] All required `.py` files are inside `labs/week02/`.
- [ ] Every required script runs.
- [ ] Variables have meaningful names.
- [ ] Boundary cases were tested where appropriate.
- [ ] Functions return values where required.
- [ ] `labs/week02/README.md` exists.
- [ ] AI Usage Log is included if AI assistance was used.
- [ ] Multiple meaningful Git commits are visible.
- [ ] All work is pushed to GitHub.
- [ ] You can explain every submitted solution.
- [ ] Task 7 and the optional challenge (if attempted) are in `task07_decomposition.py` / `optional_menu.py`.
- [ ] Outputs for fixed-data tasks match the expected summaries.
- [ ] Floats and percentages are displayed with 2 decimal places.

### Submission

- Push your work to your own coursework repository (`AI-216-<StudentID>-Fall-2026`) under `labs/week02/`.
- Deadline: as announced on LMS.
- Work pushed after the deadline may not be evaluated.

<!-- ### Suggested Marking Guide

| Component | Weight |
| --- | --- |
| Tasks 1–5: correct logic and boundary handling | 40% |
| Task 6: functions return values correctly (no printing inside) | 15% |
| Task 7: decomposition comments + correct summary | 15% |
| Testing evidence recorded in `README.md` | 10% |
| Code quality: naming, formatting, readable output | 10% |
| Git history (multiple meaningful commits) and README reflection | 10% |
| Optional challenge | Bonus |

During evaluation, you may be asked to explain or modify any part of your code. -->

---

## 19. Expected Learning Outcomes

After completing this lab, you should be able to:

- Translate small problem statements into Python logic.
- Use variables, data types, and operators appropriately.
- Write conditionals with clear boundary handling.
- Process multiple values using loops.
- Use counters and accumulators.
- Write and call simple functions.
- Test programs using multiple cases.
- Document and commit programming work clearly.

---

## Lab 02 Key Message

> **Strong AI engineering starts with strong programming logic.**

Before using powerful libraries, make sure you can understand and control the basic flow of a program yourself.
