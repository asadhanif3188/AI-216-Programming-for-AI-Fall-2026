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

**Concepts:** variables, numeric types, arithmetic, comparisons, `if / elif / else`

---

## 5. Task 2 — Internet Package Advisor

Use these rules:

```text
0–5 GB       → Basic
Above 5–15   → Standard
Above 15     → Premium
```

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

### Option B — Attendance Analysis

Given attendance percentages:

- Count students with attendance `>= 75%`.
- Count students below the requirement.
- Calculate the eligible percentage.

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
