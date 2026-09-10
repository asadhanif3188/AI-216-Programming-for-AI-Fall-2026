# Lab 01: GitHub Setup & First Push

**Week:** 1  
**Topic:** Git/GitHub Setup, Personal Course Repository, Meaningful Commits & Documentation

---

## 1. Objective

By the end of this lab, you should be able to:

- Configure Git on your computer.
- Create a personal GitHub repository for AI-216 coursework.
- Create a clean repository structure for semester work.
- Add and run your first Python script.
- Track changes using Git.
- Write meaningful commit messages.
- Push your local repository to GitHub.
- Use a `.gitignore` file appropriately.
- Create a simple professional README.
- Explain why version control is important in AI engineering.

---

## 2. Why Git & GitHub Matter in AI Engineering

AI projects change continuously.

You may change:

- Python code
- Data-processing logic
- Model configurations
- Experiments
- API code
- Documentation
- Dependencies

Without version control, it becomes difficult to answer questions such as:

- What changed?
- Who changed it?
- When did it change?
- Why was it changed?
- Can we return to a previous working version?

A basic professional workflow looks like:

```text
Edit Code
   ↓
Check Changes
   ↓
Stage Changes
   ↓
Commit
   ↓
Push
   ↓
GitHub
```

Later in the course, this workflow will grow to include branches, pull requests, testing, APIs, and larger AI projects.

---

## 3. Tools Required

You will need:

- Python
- A code editor / IDE
  - VS Code recommended
  - PyCharm or another approved editor is also acceptable
- Git
- A GitHub account
- A terminal / command prompt

---

## 4. Task 1 — Verify Your Development Environment

### Check Python

Open a terminal and run:

```bash
python --version
```

Depending on your system, you may need:

```bash
python3 --version
```

You should see your installed Python version.

### Check Git

Run:

```bash
git --version
```

You should see the installed Git version.

If either command does not work, complete the required installation before continuing.

---

## 5. Task 2 — Configure Git Identity

Git records the author of every commit.

Configure your name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Use an email address associated with your GitHub account where possible.

Verify the configuration:

```bash
git config --global --list
```

You should be able to locate:

```text
user.name=...
user.email=...
```

---

## 6. Task 3 — Create Your Personal AI-216 Coursework Repository

Create a **new repository in your own GitHub account**.

Recommended repository name:

```text
AI-216-<StudentID>-Fall-2026
```

For example, a student with ID `12345` would use `AI-216-12345-Fall-2026`.

Including your student ID keeps your repository distinct from the official
course repository and from your classmates' repositories, which makes your
work easier to identify during evaluation.

Use the visibility instructed by your instructor.

This is your **personal coursework repository**.  
Do not create or modify the instructor's official course repository.

For this lab, create the repository **without initializing it with a README**, because you will create the files locally.

---

## 7. Task 4 — Create the Initial Repository Structure

Create a local folder with the same name as your repository.

Recommended structure:

```text
AI-216-<StudentID>-Fall-2026/
├── labs/
│   └── week01/
├── assignments/
├── project/
├── .gitignore
└── README.md
```

The repository will grow throughout the semester.

### Purpose of the folders

- `labs/` — weekly laboratory work
- `assignments/` — course assignments
- `project/` — semester project work
- `README.md` — overview of your coursework repository
- `.gitignore` — files Git should not track

Do not add empty placeholder folders that you do not need unless instructed. Git does not track empty directories.

---

## 8. Task 5 — Create Your First Python Script

Inside:

```text
labs/week01/
```

create:

```text
hello_ai216.py
```

The script should:

1. Print your name.
2. Print your student ID.
3. Print the Python version using `sys.version`.
4. Print a short reflection answering:

> **What does Programming for AI mean to me?**

Use 2–3 concise lines for the reflection.

Example structure:

```python
"""Week 1 introductory script for AI-216."""

import sys

print("Name: Your Name")
print("Student ID: Your ID")
print(f"Python Version: {sys.version}")

print("\nWhat Programming for AI means to me:")
print("...")
print("...")
```

Do not copy the reflection from another student or generate text you do not understand.

Run your script and verify that it works before continuing.

---

## 9. Task 6 — Create the Root README

Create:

```text
README.md
```

in the repository root.

Include at least:

```markdown
# AI-216 Programming for AI — Fall 2026

## Student
- Name:
- Student ID:

## About This Repository
This repository contains my coursework for AI-216 Programming for Artificial Intelligence.

## Repository Structure
- labs/
- assignments/
- project/

## Current Progress
- Week 1: Git/GitHub setup and first Python script
```

Keep the README simple and professional.

You will improve it throughout the semester.

---

## 10. Task 7 — Create `.gitignore`

Create a file named:

```text
.gitignore
```

in the repository root.

Include:

```gitignore
# Python cache
__pycache__/
*.pyc

# Virtual environments
.venv/
venv/

# Jupyter
.ipynb_checkpoints/

# VS Code
.vscode/

# Environment variables / secrets
.env
```

### Why `.env` is ignored

Later in the semester, some applications may use API keys or other sensitive configuration.

Files containing secrets should **not** be committed to GitHub.

Never commit:

- Passwords
- API keys
- Access tokens
- Private credentials

---

## 11. Task 8 — Initialize Git

Open the terminal inside your repository folder.

Run:

```bash
git init
```

Check the repository status:

```bash
git status
```

Git should show your new files as untracked.

---

## 12. Task 9 — Create Your First Commit

Stage the files:

```bash
git add .
```

Check what is staged:

```bash
git status
```

Create the first commit:

```bash
git commit -m "Lab01: initialize coursework repository"
```

A commit message should describe what changed.

### Good commit messages

```text
Lab01: initialize coursework repository
Lab01: add Week 1 Python script
Lab01: improve repository documentation
```

### Weak commit messages

```text
update
done
final
work
abc
```

---

## 13. Task 10 — Connect the Repository to GitHub

Copy the repository URL from GitHub.

Then run:

```bash
git remote add origin https://github.com/<username>/AI-216-<StudentID>-Fall-2026.git
git branch -M main
git push -u origin main
```

Replace:

```text
<username>
```

with your GitHub username.

After the push, refresh your GitHub repository in the browser.

Verify that the following files are visible:

```text
README.md
.gitignore
labs/week01/hello_ai216.py
```

---

## 14. Task 11 — Make a Second Meaningful Change

Version control becomes useful when a project changes.

Improve `labs/week01/hello_ai216.py`.

For example:

- Improve labels
- Improve spacing
- Add a clearer module docstring
- Improve your reflection
- Add another useful line of output

Then inspect the changes:

```bash
git status
git diff
```

Stage and commit only the updated script:

```bash
git add labs/week01/hello_ai216.py
git commit -m "Lab01: improve Week 1 Python script"
git push
```

Refresh GitHub and verify that the new version is visible.

---

## 15. Task 12 — Inspect Your Git History

Run:

```bash
git log --oneline
```

You should now see at least two commits.

Example:

```text
a1b2c3d Lab01: improve Week 1 Python script
d4e5f6g Lab01: initialize coursework repository
```

Your exact commit IDs will be different.

Think about the following question:

> If your latest change broke the project, how could Git history help you understand what changed?

---

## 16. Task 13 — Create a Week 1 Lab README

Inside:

```text
labs/week01/
```

create:

```text
README.md
```

Include:

```markdown
# Lab 01 — GitHub Setup & First Push

## What I Completed
- Configured Git
- Created my AI-216 coursework repository
- Added my first Python script
- Created a `.gitignore`
- Pushed my work to GitHub

## Key Git Commands Used
- git init
- git status
- git add
- git commit
- git remote add
- git push
- git diff
- git log --oneline

## What I Learned
- ...
- ...
- ...

## AI Engineering Relevance
Explain briefly how version control supports:
- reproducibility
- traceability
- collaboration
```

Commit and push this file:

```bash
git add labs/week01/README.md
git commit -m "Lab01: add Week 1 documentation"
git push
```

---

## 17. Task 14 — AI-Assisted Learning Log

AI tools may be used for learning unless the instructor prohibits them for a specific task.

For this lab, you may ask an AI assistant questions such as:

- What does `.gitignore` do?
- What is the difference between `git add`, `git commit`, and `git push`?
- Review my README for clarity.
- Explain an error message I received while using Git.

If you use an AI assistant, add this section to:

```text
labs/week01/README.md
```

```markdown
## AI Usage Log

### Tool Used
ChatGPT / Copilot / Claude / Other

### What I Asked
-

### What I Used From the Response
-

### What I Verified or Changed Myself
-
```

You remain responsible for understanding everything you submit.

---

## 18. Optional Challenge — Create Your First Branch

Complete this section only after all required tasks work correctly.

Create a branch:

```bash
git checkout -b week01-readme-improve
```

Make a small improvement to:

```text
labs/week01/README.md
```

Then:

```bash
git add labs/week01/README.md
git commit -m "Lab01: improve Week 1 README"
git checkout main
git merge week01-readme-improve
git push
```

Check the branches:

```bash
git branch
```

Branch-based development will be used more formally later in the course.

---

## 19. Self-Study Learning Resources

Git and GitHub take time to become comfortable with. You are **not expected to memorize every command in this lab**. Use the following resources to review concepts and practice independently.

### Recommended — Start Here

#### 1. GitHub Docs: Getting Started with Git

A beginner-friendly introduction to Git and how it works with GitHub.

**Resource:**  
https://docs.github.com/en/get-started/learning-to-code/getting-started-with-git

Use this when you want to review:

- What Git is
- How Git tracks changes
- Basic local Git workflows
- How Git and GitHub work together

---

#### 2. GitHub Docs: Git Basics

Use this as a reference when you forget a command or concept.

**Resource:**  
https://docs.github.com/en/get-started/git-basics

Useful topics include:

- Setting up Git
- Git configuration
- Remote repositories
- Ignoring files
- Git workflows
- Git cheatsheet

---

#### 3. GitHub Hello World

A short hands-on tutorial covering repositories, branches, commits, pull requests, and merging.

**Resource:**  
https://docs.github.com/en/get-started/using-github/hello-world

This is especially useful before we begin using branches and pull requests more formally.

---

### Recommended — Interactive Practice

#### 4. GitHub Skills

GitHub Skills provides free interactive exercises directly on GitHub.

**Resource:**  
https://skills.github.com/

Recommended beginner topics:

- Introduction to GitHub
- Working with branches
- Pull requests
- Markdown
- GitHub workflows

Use these exercises if you learn better by doing rather than only reading.

---

### Recommended — Deeper Understanding

#### 5. Pro Git Book

The official **Pro Git** book is available online for free.

**Resource:**  
https://git-scm.com/book/en/v2

You do **not** need to read the entire book for AI-216.

For now, focus on the introductory chapters covering:

- What Git is
- Installing Git
- First-time Git setup
- Creating repositories
- Tracking changes
- Viewing commit history
- Working with remotes

Keep this resource for later in the semester as your Git knowledge grows.

---

### Suggested Self-Study Path

If Git/GitHub is completely new to you, follow this order:

```text
1. GitHub: Getting Started with Git
            ↓
2. Repeat this Lab 01 on your own
            ↓
3. GitHub Hello World
            ↓
4. GitHub Skills
            ↓
5. Use Git Basics / Pro Git when you need deeper explanations
```

The best way to learn Git is through **regular use**, not memorization.

Try to develop the habit of using:

```bash
git status
git add
git commit
git push
git log --oneline
```

every week.

---

## 20. Submission Checklist

Before submitting, verify:

- [ ] Python runs correctly.
- [ ] Git is installed and configured.
- [ ] Personal AI-216 GitHub repository exists.
- [ ] Repository name follows the required naming convention.
- [ ] Root `README.md` exists.
- [ ] Root `.gitignore` exists.
- [ ] `.env` is included in `.gitignore`.
- [ ] `labs/week01/hello_ai216.py` exists and runs.
- [ ] `labs/week01/README.md` exists.
- [ ] At least **3 meaningful commits** are visible.
- [ ] All required work is pushed to GitHub.
- [ ] No passwords, API keys, tokens, or credentials are committed.
- [ ] AI Usage Log is included if AI assistance was used.

Suggested commit progression:

```text
Lab01: initialize coursework repository
Lab01: improve Week 1 Python script
Lab01: add Week 1 documentation
```

---

## 21. Expected Learning Outcomes

After completing this lab, you should be able to:

- Configure Git identity.
- Initialize a local Git repository.
- Inspect repository status and file changes.
- Stage and commit changes.
- Write meaningful commit messages.
- Connect a local repository to GitHub.
- Push changes to a remote repository.
- Use `.gitignore` appropriately.
- Maintain basic project documentation.
- Inspect Git history.
- Explain how version control supports reproducibility, traceability, and collaboration in AI engineering.

---

## 22. Key Takeaway

> **Git is not only a submission tool. It is part of the engineering workflow.**

Throughout AI-216, your repository should gradually become evidence of how you think, build, document, and improve AI software.
