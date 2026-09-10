# AI-216: Programming for Artificial Intelligence
## Week 1 Lecture Handout — Course Orientation, AI Engineering Roadmap & GitHub
**Semester:** Fall 2026  
**Credits:** 2 (Lecture) + 1 (Lab)

---

## 1. Why this course exists — the AI Engineering mindset

Programming for AI is not only about writing Python syntax or training a machine-learning model.

An AI engineer must be able to take an idea from experimentation to a usable, reproducible, and maintainable system.

Throughout this course, we will gradually work through:

```text
Problem
  ↓
Python
  ↓
Data
  ↓
Model / AI Capability
  ↓
Evaluation
  ↓
Software Project
  ↓
API
  ↓
Container
  ↓
Usable AI Application
  ↓
Iteration
```

The goal of AI-216 is therefore not simply:

> **"I can train a model."**

By the end of the course, you should be moving toward:

> **"I can build an AI application that another person can run, use, understand, and maintain."**

This course will build your programming foundations and connect them to practical AI engineering tasks including data handling, machine learning workflows, software engineering, model serving, containerization, and modern AI applications.

---

## 2. Week 1 Learning Outcomes

By the end of Week 1, you should be able to:

1. Explain the role of an **AI Engineer** and distinguish it from related roles such as Data Scientist, Machine Learning Engineer, and Software Engineer.
2. Describe the major stages involved in building an AI application: programming, data processing, modeling, evaluation, serving, and deployment.
3. Identify common technical skills appearing in AI Engineer job descriptions and map them to topics covered in AI-216.
4. Explain why Git and GitHub are important for professional and reproducible AI development.
5. Set up your course development environment and publish your first course repository to GitHub.

---

## 3. Lecture Agenda — Typical 2-Hour Flow

- **Welcome and course expectations**
- **What "Programming for AI" means**
- **What does an AI Engineer do?**
- **AI-216 course roadmap**
- **Core engineering toolchain**
- **Git and GitHub fundamentals**
- **Career activity: AI Engineer job-posting skill mining**
- **Course expectations for AI-assisted programming**
- **Lab preview**
- **Exit ticket**

---

## 4. AI Engineer Career Context

Artificial Intelligence is a broad field, and several job titles overlap.

### Data Scientist
Typically focuses on:

- Data analysis
- Statistics
- Experimentation
- Machine-learning models
- Communicating insights

### Machine Learning Engineer
Typically focuses on:

- Training and evaluating models
- ML pipelines
- Model optimization
- Production inference
- Scaling ML systems

### Software Engineer
Typically focuses on:

- Application development
- APIs
- Databases
- Testing
- Software architecture
- Deployment

### AI Engineer
An AI Engineer often works across several of these areas.

A modern AI Engineer may need to:

```text
Write Python
    ↓
Work with data
    ↓
Use/train AI models
    ↓
Evaluate outputs
    ↓
Build APIs/applications
    ↓
Deploy systems
    ↓
Monitor and improve them
```

You are not expected to master every area in AI-216.

This course provides the **programming and engineering foundation** on which later AI, ML, deep learning, MLOps, and Generative AI skills can be built.

---

## 5. AI-216 Course Roadmap

The semester will progress approximately as follows:

```text
Python Programming
        ↓
Data Structures
        ↓
NumPy & Pandas
        ↓
Data Ingestion & Preprocessing
        ↓
Machine-Learning Workflows
        ↓
Evaluation & Validation
        ↓
AI Software Engineering
        ↓
Model Serving with APIs
        ↓
Containerization
        ↓
LLM Application Engineering
        ↓
Embeddings & RAG
        ↓
End-to-End AI Application
```

Each topic builds on the previous ones.

The purpose is not to collect isolated tools. The purpose is to understand **where each tool fits in an AI system**.

---

## 6. Core Toolchain

### A) Python

Python will be the primary programming language used throughout AI-216.

You should be able to:

- Install Python
- Run `.py` files
- Install packages
- Read error messages
- Work with project folders
- Use Python from an IDE and terminal

The recommended Python version will be announced by the instructor.

### B) IDE / Development Environment

You may use:

- VS Code
- PyCharm
- Jupyter Notebook
- Another approved development environment

For most software-engineering activities, a code editor such as **VS Code** is recommended.

Jupyter notebooks will still be useful for experimentation and data analysis, but AI engineering is not limited to notebooks.

During the course, you will learn how to move from:

```text
experiment.ipynb
```

toward structured projects such as:

```text
project/
├── src/
├── tests/
├── data/
├── models/
├── requirements.txt
└── README.md
```

### C) Terminal / Command Line

AI engineers frequently use the terminal to:

- Run Python programs
- Install dependencies
- Manage virtual environments
- Use Git
- Start APIs
- Build containers

You do not need to be a command-line expert at the start of the course.

We will gradually build this skill.

### D) Git & GitHub

Git is a **version-control system**.

GitHub is a platform used to host and collaborate on Git repositories.

We will use GitHub throughout the semester for:

- Maintaining course repositories
- Tracking changes
- Writing meaningful commits
- Documentation
- Branches
- Pull requests
- Project milestones
- Professional portfolio development

A professional engineering workflow gradually looks like:

```text
Issue
  ↓
Branch
  ↓
Code
  ↓
Commit
  ↓
Pull Request
  ↓
Review
  ↓
Merge
```

In Week 1, we will begin with repositories, commits, and pushing code.

---

## 7. Supporting Platforms

### Kaggle

Kaggle can be useful for:

- Finding datasets
- Exploring notebooks
- Practicing data analysis
- Participating in ML competitions

Kaggle is a supporting platform rather than the main development environment for this course.

### LinkedIn

LinkedIn will be treated primarily as a **professional and career-development tool**.

Students are encouraged to:

- Maintain a professional profile
- Follow AI and software engineering roles
- Observe current skill requirements
- Document major projects where appropriate

The objective is not social-media activity. The objective is to understand how technical skills connect to career opportunities.

---

## 8. Environment Setup for Week 1

By the end of this week, you should have:

### Python

- [ ] Python installed
- [ ] Python version verified
- [ ] Able to run a `.py` file

Example:

```python
print("Hello, AI-216!")
```

### Development Environment

- [ ] VS Code / PyCharm / another approved IDE installed
- [ ] Able to open a project folder
- [ ] Able to run Python from the editor or terminal

### Git

- [ ] Git installed
- [ ] Git version verified

Example:

```bash
git --version
```

### GitHub

- [ ] GitHub account created
- [ ] Profile name checked
- [ ] Email/configuration completed
- [ ] First repository created

---

## 9. Your AI-216 Coursework Repository

Create a repository for your semester work.

Recommended name:

```text
AI-216-Programming-for-AI-Fall-2026
```

or another naming convention provided by the instructor.

A possible starting structure is:

```text
AI-216-Programming-for-AI-Fall-2026/
├── week01/
│   └── hello_ai216.py
└── README.md
```

As the semester progresses, the repository may evolve into:

```text
AI-216-Programming-for-AI-Fall-2026/
├── labs/
├── assignments/
├── data/
├── projects/
└── README.md
```

Your repository should demonstrate steady progress throughout the semester.

---

## 10. Your First Git Workflow

A basic workflow is:

```bash
git status
git add .
git commit -m "Complete Week 1 setup"
git push
```

A good commit message should describe **what changed**.

Good examples:

```text
Add Week 1 Python setup script
Complete NumPy vectorization exercises
Add preprocessing for missing values
Implement prediction API endpoint
```

Weak examples:

```text
update
work
done
final
abc
```

Meaningful commits help both you and other developers understand project history.

---

## 11. In-Class Activity — AI Engineer Job-Posting Skill Mining

### Duration
Approximately **20–25 minutes**

### Group Size
Work in groups of **2–3 students**.

### Task
Analyze **4–5 real AI / Machine Learning / AI Engineer job postings**.

Search using terms such as:

- AI Engineer
- Machine Learning Engineer
- Junior AI Engineer
- Python AI Developer
- Generative AI Engineer
- Applied AI Engineer

You may use LinkedIn, Indeed, company career pages, or other reputable job platforms.

---

## 12. Skills to Extract

For each posting, identify relevant requirements.

### Programming
Examples:

- Python
- Object-oriented programming
- Data structures
- Clean code

### Data
Examples:

- Pandas
- NumPy
- SQL
- Data preprocessing

### Machine Learning
Examples:

- scikit-learn
- Model training
- Model evaluation
- Feature engineering

### Software Engineering
Examples:

- Git
- Testing
- APIs
- Project structure

### Deployment / Infrastructure
Examples:

- Docker
- Cloud platforms
- CI/CD

### Generative AI
Examples:

- LLM APIs
- Embeddings
- RAG
- Vector databases

### Professional Skills
Examples:

- GitHub portfolio
- Documentation
- Communication
- Teamwork
- Problem solving

---

## 13. Group Output Template

Submit a short note containing:

```markdown
## Job Postings

1. Job title / company / link
2. Job title / company / link
3. Job title / company / link
4. Job title / company / link

## Recurring Skills

### Programming
-

### Data
-

### Machine Learning
-

### Software Engineering
-

### APIs / Backend
-

### Deployment / Cloud
-

### Generative AI / LLMs
-

### Git / Collaboration
-

## Evidence Employers Expect
-

## One Surprising Requirement
-

## Course Mapping

### Covered directly in AI-216
-

### Introduced in AI-216
-

### Outside the current scope of AI-216
-
```

---

## 14. Important Discussion — Do AI Engineers Need to Know Everything?

No.

A job posting may mention:

```text
Python
SQL
PyTorch
Docker
AWS
Kubernetes
FastAPI
Transformers
RAG
Vector databases
CI/CD
Spark
Kafka
MLOps
```

This does **not** mean a beginner must master all of these immediately.

Think of AI engineering as a progression.

AI-216 focuses primarily on building strong foundations in:

```text
Python
+
Data
+
Machine Learning Workflows
+
Software Engineering
+
Git/GitHub
+
APIs
+
Reproducibility
+
Introductory Modern AI Applications
```

Later courses and professional experience will deepen and expand these skills.

---

## 15. AI-Assisted Programming & Professional Expectations

Generative AI and coding assistants are now part of modern software and AI development.

Students may use AI tools for learning and development **unless explicitly prohibited for a particular assessment**.

However:

- You are responsible for every line of code you submit.
- You must understand your solution.
- You must test and verify AI-generated suggestions.
- You should not blindly copy generated code.
- You may be asked to explain your code.
- You may be asked to modify your code.
- You may be asked to debug your code during evaluation.
- Submitting code that you cannot explain is not evidence of learning.
- Assessment-specific AI-use rules always take priority.

The objective is to learn how to use AI tools **responsibly and professionally**, not how to avoid thinking.

---

## 16. Academic Integrity

Students must:

- Submit original work
- Follow university academic-integrity policies
- Properly acknowledge external sources when required
- Follow collaboration rules for each assessment
- Be able to explain submitted work

Direct copying from classmates, repositories, websites, or AI systems without understanding and authorization is not acceptable.

---

## 17. Week 1 Lab Checklist

Complete the following before the next class:

### Development Environment

- [ ] Install Python
- [ ] Install an IDE / code editor
- [ ] Run a Python file locally
- [ ] Install Git
- [ ] Verify Git from the terminal

### GitHub

- [ ] Create or update GitHub account
- [ ] Create your AI-216 coursework repository
- [ ] Add a `week01/` folder
- [ ] Create `hello_ai216.py`
- [ ] Create or update `README.md`
- [ ] Commit your changes with a meaningful message
- [ ] Push your work to GitHub
- [ ] Confirm that your files are visible online

### Career Activity

- [ ] Review AI Engineer job postings
- [ ] Identify recurring technical skills
- [ ] Map those skills to the AI-216 roadmap

### Professional Profile

- [ ] Review your LinkedIn profile
- [ ] Update it where appropriate
- [ ] Keep the wording professional and truthful

---

## 18. Exit Ticket

Before leaving the class, submit a short response on LMS.

Answer:

1. **What is one thing you learned today about the role of an AI Engineer?**
2. **Which part of the AI Engineer roadmap currently feels least familiar to you, and why?**
3. **What is one technical skill you want to improve during AI-216?**
4. **What is one weekly professional habit you will practice?**

Example habits:

- Commit code after completing a meaningful task
- Read error messages before searching for a solution
- Keep README files updated
- Review documentation regularly
- Test code before pushing it

---

## 19. Quick Glossary — Week 1

**AI Engineer**  
A professional who applies programming, AI/ML capabilities, data, and software-engineering practices to build usable AI systems.

**IDE**  
Software used to write, run, and debug code, such as VS Code or PyCharm.

**Terminal / Command Line**  
A text-based interface used to run programs and development tools.

**Git**  
A distributed version-control system used to track changes in source code.

**GitHub**  
A platform for hosting and collaborating on Git repositories.

**Version Control**  
A system for tracking changes to files over time.

**Repository (Repo)**  
A project directory tracked using Git.

**Commit**  
A recorded snapshot of changes in a repository.

**Branch**  
An independent line of development within a Git repository.

**Pull Request**  
A request to review and merge changes from one branch into another.

**Dataset**  
A collection of data used for analysis, machine learning, or AI applications.

**Machine Learning Pipeline**  
A repeatable sequence of data-processing, training, and evaluation steps.

**API**  
An interface that allows one software application to communicate with another.

**Container**  
A portable package containing an application and its dependencies.

**LLM**  
Large Language Model — a model trained on large amounts of text and other data that can perform language-related tasks.

**RAG**  
Retrieval-Augmented Generation — a technique where relevant external information is retrieved and supplied to a generative model when producing an answer.

---

## 20. What to Do Next

Before Week 2:

1. Complete all Week 1 environment setup.
2. Ensure your GitHub repository is accessible and organized.
3. Push your first Python file.
4. Complete the career-skills activity.
5. Review the AI-216 semester roadmap.

Next week we begin:

> **Python Fundamentals for AI — variables, data types, conditions, loops, functions, and problem solving.**

---

## Week 1 Key Message

> **AI engineering is not only about models. It is about building reliable, usable, and maintainable systems around AI capabilities.**

AI-216 begins with Python and GitHub because those are foundational tools for everything that follows.
