# AI-216: Programming for Artificial Intelligence
## Lab 07 — Data Ingestion: Files, APIs, Web & Databases

**Semester:** Fall 2026  
**Week:** 07  
**Topic:** CSV, JSON, HTTP APIs, introductory web scraping, SQLite/SQL, ethical data collection, and reproducible ingestion pipelines

---

## 1. Objective

In Week 6, you learned to use **Pandas** to inspect and manipulate tables. But real AI applications do not begin with an ideal DataFrame. The data must first arrive from somewhere: a CSV export, a JSON response, an HTTP API, an HTML page, or a database.

The engineering challenge this week is:

```text
Source → Read/Fetch → Parse → Validate → Store → Verify
```

This lab helps you build those steps using manageable Python programs. You will also encounter **invalid source values, duplicate identifiers, missing fields, network failures, and database constraints**. Getting the happy path to run is only part of the job.

By the end of Lab 07, you should be able to:

1. Read and write CSV files using `csv.DictReader` and `csv.DictWriter`.
2. Parse and serialize structured JSON, including nested records.
3. Call a public HTTP API using a timeout and handle unsuccessful responses.
4. Validate the shape and types of data returned by an API.
5. Extract structured information from a controlled HTML document with Beautiful Soup.
6. Explain essential legal, ethical, privacy, and rate-limit considerations in data collection.
7. Create and query a SQLite table with Python and parameterized SQL.
8. Apply basic validation rules, preserve useful error information, and track rejected records.
9. Build a small **ingest → validate → store** pipeline that is safe to rerun.
10. Document your data sources, design decisions, tests, and Git commits.

**Scope:** This is a data-ingestion lab, **not** a machine-learning training or advanced preprocessing assignment. Deeper missing-value treatment, visualization, and EDA are covered in Week 8.

---

## 2. Before You Start

Use your Python/Git environment from earlier labs. Check that Python and Git work:

```bash
python --version
git status
```

Install the small number of external libraries used here:

```bash
python -m pip install requests beautifulsoup4 pandas
```

- `csv`, `json`, `sqlite3`, `pathlib`, and `datetime` are part of Python's standard library.
- `requests` is for HTTP calls.
- `beautifulsoup4` is for parsing HTML.
- `pandas` was introduced in Week 6 and is used here to inspect SQL query results.
- If your environment uses `python3`, substitute it for `python`.

### Provided Lab Files

The lab package contains these **synthetic and offline-ready** files:

```text
data/
├── station_counts.csv
├── station_updates.json
├── api_repository_fixture.json
├── station_notice.html
└── README.md
```

Read `data/README.md` for provenance and the data contract. The bike-station records are deliberately synthetic; **do not describe them as real observations**. The API fixture is not a live API response.

### Suggested Student Repository Structure

Place your work in your personal coursework repository:

```text
labs/week07/
├── data/                              # instructor-supplied fixtures
├── task01_csv.py
├── task02_json.py
├── task03_api.py
├── task04_html.py
├── task05_sqlite.py
├── task06_sql_reports.py
├── task07_validation.py
├── task08_pipeline/
│   ├── main.py
│   ├── sources.py
│   ├── quality.py
│   └── storage.py
├── outputs/                           # generated locally
└── README.md                          # your solution documentation
```

The **instructor's lab handout** is also named `README.md` in the course repository; in your personal solutions repository, `labs/week07/README.md` should describe *your implementation, tests, and findings*. Keep the instructor-provided source files unchanged so results remain reproducible.

Run a task from the root of your personal repository, for example:

```bash
python labs/week07/task01_csv.py
```

Use paths derived from `__file__` (with `pathlib.Path`) where possible, so your scripts do not depend on which directory the terminal starts in.

---

## 3. The Week 7 Scenario: Campus Bike Availability

A smart-campus team receives periodic snapshots of bike parking stations. In a later AI application, the snapshots could support demand forecasting or alerting, but this lab focuses only on **reliable collection and storage**.

The provided flat CSV and nested JSON describe the same kind of record:

| Field | Meaning | Required validation |
| --- | --- | --- |
| `record_id` | Source record identifier | Nonempty; unique after combining sources |
| `station_id` | Station identifier | Must be `S01`, `S02`, or `S03` |
| `observed_at` | Time of observation | Valid ISO 8601 timestamp with offset |
| `bikes_available` | Available bikes | Integer, at least `0` |
| `dock_capacity` | Total bike docks | Positive integer; availability must not exceed it |

For example, a valid record could be:

```python
{
    "record_id": "R001",
    "station_id": "S01",
    "observed_at": "2026-11-01T08:00:00+00:00",
    "bikes_available": 5,
    "dock_capacity": 10
}
```

**Important distinction:** `csv.DictReader` produces text values, whereas JSON numbers may already be Python integers. Both sources must end up with the *same validated representation* before storage.

Other fixtures serve distinct exercises: the mock HTML describes **station notices**, and the offline API fixture describes a **software repository**. Do not combine those unrelated records into the bike-counts table.

---

## 4. General Coding Requirements

For every required task:

1. Work out the expected input and output before writing code.
2. Use descriptive names, small functions, and clear responsibilities, as practiced in Weeks 3–4.
3. Distinguish raw source data from parsed/validated data.
4. Do not silently replace invalid data or invent missing measurements.
5. Use `try`/`except` for anticipated errors and report useful information.
6. Keep raw fixtures unchanged; place generated results in `outputs/`.
7. Test at least the normal case and one relevant failure or edge case.
8. Avoid hard-coded machine-specific absolute paths.
9. Follow the data provider's access rules; do not collect restricted or personal information.
10. Be able to explain every function, query, and library call in your submission.

You should **not** submit code copied directly from the Week 7 lecture's integrated sensor example. The dataset, checks, and resulting database in this lab are different.

---

# 5. Task 1 — Reading and Writing CSV Files (Basic)

**File:** `task01_csv.py`  
**Source:** `data/station_counts.csv`

### Part A — Read a CSV Export

Use Python's `csv` module to:

1. Open the supplied CSV with `encoding="utf-8"` and `newline=""`.
2. Use `csv.DictReader` to read rows as dictionaries.
3. Print the column names, total number of raw rows, and the first two records.
4. Explain why fields such as `"5"` and `"10"` need conversion before numerical comparisons.

Helpful starting point (not a complete solution):

```python
import csv

with open(csv_path, newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)
    # Inspect reader.fieldnames and then iterate over rows.
```

### Part B — Type Conversion and Rejection Awareness

1. Try converting `bikes_available` and `dock_capacity` to integers.
2. Do not allow an invalid text value to crash the whole batch.
3. Count how many rows cannot be converted to the required integer types.
4. Differentiate **conversion failure** from a value that converts but violates a business rule.

### Part C — Write a Small CSV Report

Export a **preview of the first five raw source records** to `outputs/csv_preview.csv` using `csv.DictWriter`. Keep the same header order as the source.

**Thinking questions:** Why is `split(",")` not a reliable general CSV parser? What is the difference between *reading* a CSV field and *validating* it?

**Evidence:** Include the first two rows of your preview and the conversion-failure count in your README.

---

# 6. Task 2 — Reading Nested JSON (Basic → Intermediate)

**File:** `task02_json.py`  
**Source:** `data/station_updates.json`

### Part A — Explore the JSON Envelope

Use `json.load()` to inspect the top-level dictionary:

```text
source_name

generated_at

records
```

1. Print `source_name`.
2. Print how many entries are inside `records`.
3. Show the first record and inspect the Python types of its numeric fields.
4. Compare a JSON number with a CSV numeric-looking string.

### Part B — Validate the Structure

Before iterating:

- confirm the top-level value is a dictionary;
- confirm `records` exists and is a list;
- check that each record is a dictionary;
- report a missing or invalid required field instead of accessing it blindly.

### Part C — Serialize Your Own Summary

Create `outputs/json_preview_summary.json` with at least:

```text
source_name
record_count
field_names_in_first_record
```

Use `json.dump(..., indent=2)`. Do not copy the entire input file into your output.

**Thinking questions:** What is the difference between `json.load()` and `json.loads()`? Why can the same source schema require different conversion logic in CSV and JSON?

---

# 7. Task 3 — Calling a Public HTTP API (Intermediate)

**File:** `task03_api.py`  
**Offline fallback:** `data/api_repository_fixture.json`

### Scenario

A data-oriented application occasionally needs basic metadata about its open-source dependencies. Explore a public GitHub repository metadata endpoint:

```text
https://api.github.com/repos/python/cpython
```

This is a **public, read-only** endpoint. Availability and rate limits are external conditions; do not assume the request will always work. No API token is required for this classroom example, and no personal account data is needed.

### Part A — HTTP Basics

1. Issue one GET request using `requests` and an explicit timeout (such as 10 seconds).
2. Inspect the HTTP status code and final response URL.
3. Call `raise_for_status()` before processing a successful response.
4. Parse the response as JSON and verify it is a dictionary.
5. Extract fields such as `full_name`, `language`, `stargazers_count`, and `forks_count` **only after checking the expected types**.

Starter call (complete the validation yourself):

```python
import requests

response = requests.get(
    "https://api.github.com/repos/python/cpython",
    timeout=10,
)
```

### Part B — Failure Handling and Offline Reproducibility

Handle at least:

- a timeout or connection failure;
- non-success status such as 403, 404, 429, or 5xx;
- invalid JSON or unexpected JSON structure;
- a required field that is missing or of the wrong type.

If the remote service cannot be reached, **load the supplied local fixture** and perform the same shape/type validation. Label the outcome as `offline_fixture` rather than `live_api`. You must never report the fixture's numbers as current GitHub statistics.

### Part C — Output and Explanation

Create `outputs/api_summary.json` containing:

```text
source_mode              # live_api or offline_fixture
source_reference         # URL or fixture path
repository_name
language
stars
forks
```

Include in the README which mode you used, what happened with the request, and why a successful HTTP status is not sufficient proof of valid data.

**Bonus:** Record a timestamp of *your retrieval attempt* separately from the repository's own `updated_at` field (if present); they mean different things.

---

# 8. Task 4 — Introductory and Ethical HTML Extraction (Intermediate)

**File:** `task04_html.py`  
**Source:** `data/station_notice.html`

### Part A — Parse a Controlled HTML Page

Use Beautiful Soup with the built-in `html.parser`:

1. Read the local HTML file.
2. Print the page title or main heading.
3. Select all `article.station-card` elements.
4. For each card, extract the `data-station-id`, station heading, and status text.
5. Store complete records as a list of dictionaries.

### Part B — Handle Missing Elements

One card has a deliberately missing status. Your scraper must not crash.

- Report cards with missing required fields separately.
- Do not assume `select_one()` always returns an element.
- Export complete station notices to `outputs/station_notices.csv`.

### Part C — Explain the Ethics

In `README.md`, answer:

1. Why does this lab scrape a **local controlled fixture** instead of arbitrary live websites?
2. What should you check before scraping a public website? Discuss terms, `robots.txt` guidance, permissions, request load, and privacy.
3. Why might a static `requests`/Beautiful Soup scraper fail on a page rendered with JavaScript?

**Required boundary:** Do not scrape login-protected pages, personal profiles, or websites that prohibit the activity. The local fixture is sufficient for completing the lab.

---

# 9. Task 5 — SQLite Tables, Constraints & Parameterized SQL (Intermediate)

**File:** `task05_sqlite.py`

### Part A — Create a Table

Use Python's built-in `sqlite3` module. For this introductory task, you may use an in-memory database (`:memory:`).

Design a table called `station_snapshots` with at least:

```text
record_id            TEXT PRIMARY KEY
station_id           TEXT NOT NULL
observed_at          TEXT NOT NULL
bikes_available      INTEGER NOT NULL
dock_capacity        INTEGER NOT NULL
source_name          TEXT NOT NULL
```

Add relevant `CHECK` constraints to reject impossible counts and capacities. Consider whether `bikes_available` can be zero and whether it can exceed `dock_capacity`.

### Part B — Insert and Query

1. Insert **three valid, typed records** from the supplied fixtures, using parameter placeholders (`?`).
2. Query all rows with `SELECT`.
3. Query one station using a parameterized `WHERE station_id = ?`.
4. Try a duplicate `record_id` and inspect how the primary key protects the table.
5. Try a record with `bikes_available > dock_capacity` and explain the constraint failure.
6. Use a context manager or explicit `commit()`, and close the connection.

### Part C — SQL Safety

In your README, explain why this is unsafe:

```python
# Do not execute queries constructed this way:
query = f"SELECT * FROM station_snapshots WHERE station_id = '{user_input}'"
```

Explain why `?` parameter binding is preferable. **Do not attempt to build a harmful query.**

---

# 10. Task 6 — SQL Summaries and Pandas Interoperability (Intermediate → Advanced)

**File:** `task06_sql_reports.py`

Use a small SQLite table populated with valid bike-station records. You may reuse your Task 5 insertion logic or initialize a fresh temporary database.

### Part A — Filter and Sort

Write SQL queries to:

1. Select records for a chosen station ID.
2. Sort observations by `observed_at` (the supplied ISO timestamp strings include the same UTC offset).
3. Select records where `bikes_available` is below a threshold, using a bound parameter.

### Part B — Aggregate by Station

Write a query that returns:

```text
station_id
record_count
average_bikes_available
```

using `COUNT(*)`, `AVG(...)`, `GROUP BY`, and `ORDER BY`. Round values for reporting, but preserve the original stored integers.

### Part C — Read Results into Pandas

Use the Week 6 knowledge of `pd.read_sql_query()` to:

- load your grouped SQL result into a DataFrame;
- inspect its columns, shape, and data types;
- print a readable table.

**Thinking question:** What work did SQL do, and what work did Pandas do? Why might each be useful in an AI data workflow?

---

# 11. Task 7 — Validation, Duplicates & Rejection Tracking (Advanced)

**File:** `task07_validation.py`

### The Problem

You have **two source formats** with the same logical fields, but not every source record is valid. Treat validation as a separate responsibility from reading files or inserting database rows.

Implement functions with responsibilities similar to:

```python
def validate_record(raw_record):
    """Return a normalized typed record or raise ValueError."""
    ...


def process_batch(source_records, seen_ids):
    """Classify valid, invalid, and duplicate records."""
    ...
```

You may choose your own signatures, but explain what each function receives and returns.

### Validation Contract

For each record:

1. Confirm the required keys exist and have meaningful values.
2. Strip whitespace around text identifiers.
3. Convert numeric fields to integers where appropriate; **reject** unparseable values.
4. Reject `dock_capacity <= 0`.
5. Reject `bikes_available < 0` or `bikes_available > dock_capacity`.
6. Verify that `observed_at` is a valid timestamp; use `datetime.fromisoformat()` and require a time-zone offset.
7. Require `station_id` in `{"S01", "S02", "S03"}`.
8. Treat a repeated `record_id` as a duplicate, not a new observation.

### Source Ordering and Conflict Policy

Process the CSV records **first**, then the JSON `records` list, preserving each source's original order.

For this lab, retain the **first valid occurrence** of each `record_id`. If a later record uses the same ID but different values, flag it as a **duplicate/conflict**, rather than silently replacing existing data. Count invalid records separately from duplicates. (Production systems may need a different conflict-resolution policy.)

### Rejection Tracking

A rejected-record entry should contain at least:

```text
source_name
record_id (if available)
reason
```

Possible reason categories: `missing_field`, `invalid_integer`, `invalid_timestamp`, `invalid_capacity`, `count_out_of_range`, `unknown_station`, `duplicate_record_id`.

Do not create a generic “something went wrong” entry for every failure. Useful reasons make debugging, source correction, and grading easier.

**README requirement:** Describe one invalid numeric value, one invalid range, and one duplicate ID from the fixtures without modifying the fixtures.

---

# 12. Task 8 — Integrated Ingest → Validate → Store Pipeline (Advanced)

This is the main Week 7 engineering task. Build a small, repeatable program that consumes **both** supplied bike-station sources and stores one canonical record per accepted ID.

### Target Structure

```text
task08_pipeline/
├── main.py
├── sources.py
├── quality.py
└── storage.py
```

Suggested responsibilities:

| Module | Main responsibility |
| --- | --- |
| `sources.py` | Read CSV and JSON; label each record with its source |
| `quality.py` | Normalize types, validate business rules, classify rejected/duplicate records |
| `storage.py` | Create SQLite table, insert accepted rows safely, query counts and grouped statistics |
| `main.py` | Coordinate the stages, print results, and write generated reports |

**Design principle:** `main.py` should coordinate the workflow; it should not contain all of the file parsing, validation, and SQL logic.

### Inputs

```text
data/station_counts.csv
data/station_updates.json
```

### Required Processing Sequence

```text
CSV source ─┐
            ├─→ Parse → Normalize → Validate → Deduplicate
JSON source ┘                           │
                                      ├─ rejected + reasons
                                      └─ accepted
                                           ↓
                                 Parameterized SQLite insert
                                           ↓
                                SQL count and station summary
                                           ↓
                                  Output files + README
```

### Outputs

Create these files automatically under `labs/week07/outputs/`:

1. `station_snapshots.db` — SQLite table storing accepted records.
2. `rejected_records.csv` — rejected rows and/or duplicate IDs with explicit reasons.
3. `station_summary.csv` — from a **SQL query over stored records**, not a separate hand-calculated report.
4. `ingestion_summary.json` — a simple source/audit summary, including input count, validation rejects, within-batch duplicates, accepted candidates, rows newly inserted, and total stored rows.

For the summary JSON, label counters precisely: `accepted_candidates` (valid unique records in the current combined input) is **not** always equal to `new_rows_inserted` (new rows stored in the database in this run).

### Expected Sanity Checks

On a fresh, empty SQLite database, with source ordering and rules exactly as specified:

| Metric | Expected value |
| --- | ---: |
| Raw CSV records | 14 |
| Raw JSON records | 7 |
| Total raw records | 21 |
| Invalid records (validation failures) | 5 |
| Duplicate IDs within combined input | 1 |
| Valid unique records / accepted candidates | 15 |
| Rows stored after first run | 15 |
| Station IDs represented | 3 |

You must explain *why* those numbers follow from the source data. Do not hard-code these totals in your implementation.

### Safe Rerun Requirement

Run the program twice **without deleting the SQLite file between runs**.

- On the first run with a fresh database, the program should insert 15 new records.
- On the next run, it should not grow beyond 15 rows or create duplicate records.
- It is normal for `new_rows_inserted` to be `0` on the second run, while `accepted_candidates` remains `15`.
- If a record conflicts with an existing database primary key, do not use `INSERT OR REPLACE` to silently overwrite it. Explain how your program handles this situation.

### Engineering Questions (Answer in README)

1. How did you reconcile CSV strings with JSON numeric values?
2. Which data validation rules are *format checks* and which are *business rules*?
3. What happens if one file is missing or malformed?
4. Why are rejected records retained in an audit file?
5. How does the database primary key help with repeatability?
6. What do you do if the same record ID has different values in the two sources?
7. How do you know the station summary came from stored records rather than unvalidated input?
8. Why do ingestion reliability and provenance matter before any ML model is trained?

---

# 13. Optional Challenge — A Source Manifest and One Additional Audit

Choose **one** extension (do not add unnecessary complexity to the required solution):

**Option A — Source manifest:** Write `outputs/source_manifest.json` with the two fixture paths, source types, dataset version/name, run timestamp, and a note that both sources are synthetic. Do not claim you verified permissions that you did not verify.

**Option B — Stronger conflict handling:** When the same `record_id` appears more than once, compare the values and distinguish an *exact repeat* from a *conflicting duplicate*. Still preserve the first valid occurrence.

**Option C — Small consistency check:** Calculate `station_count` from the database using SQL and compare it with the count in the exported station summary. Report whether they match.

**No ML models or dashboards are required.** The focus remains ingestion quality.

---

# 14. Testing and Verification

Do not test only the default run. Test failure cases deliberately **without changing the instructor-supplied fixtures**: create small in-memory dictionaries or temporary test files.

| Scenario | What to verify |
| --- | --- |
| Empty CSV (header only) | No crash; appropriate zero-row summary |
| Invalid integer such as `"n/a"` | Rejected and clearly explained |
| Negative bike count | Rejected |
| Available bikes greater than capacity | Rejected |
| Capacity equal to zero | Rejected |
| Missing timestamp | Rejected |
| Unknown station ID | Rejected |
| Duplicate record ID | Only one canonical record stored |
| Valid count at `0` | Accepted |
| Valid count equal to capacity | Accepted |
| Missing JSON `records` list | Helpful shape/format error |
| Broken API connection | Controlled error or documented offline fallback |
| HTML card missing status | Skipped or explicitly flagged, not a crash |
| Pipeline run twice | Row count unchanged on second run |

If a file is missing or malformed, **do not silently treat it as an empty successful source**. Report the failure; a failed run should not masquerade as a fully successful collection.

Record at least **five concrete test cases** in your README, including observed output and whether it met expectations.

---

# 15. Code Quality and Engineering Expectations

### Make Data Contracts Visible

Document what each field means and which values are allowed. Avoid unexplained literals for stations, columns, and constraints.

### Separate Responsibilities

Use functions for parsing, validation, storage, and reporting. Avoid building a large procedural script with all operations mixed together.

### Respect Input Data

Do not edit the original CSV or JSON to make tests pass. Reject invalid data with a reason instead of silently changing values.

### Handle Files and Connections Safely

Use context managers where practical:

```python
with open(path, newline="", encoding="utf-8") as file:
    ...
```

Commit database writes using a transaction and close the SQLite connection even after an error.

### Query Safely

Use `?` placeholders for values in SQL. Avoid building SQL strings by concatenating user input.

### Keep the Pipeline Reproducible

Use stable input paths, document dependencies, and keep a simple way to run the pipeline. Include a minimal `.gitignore` for generated `.db` files and Python cache directories. Do **not** commit credentials, access tokens, or personal data.

### Avoid Over-Engineering

You do not need asynchronous requests, containerization, an ORM, a web server, ML training, or a large framework. Small, testable Python modules are sufficient.

---

# 16. Git & GitHub Workflow

Make incremental, meaningful commits. Example sequence from your repository root:

```bash
git add labs/week07/task01_csv.py labs/week07/task02_json.py
git commit -m "Lab07: parse CSV and JSON sources"

git add labs/week07/task03_api.py labs/week07/task04_html.py
git commit -m "Lab07: handle API and HTML sources"

git add labs/week07/task05_sqlite.py labs/week07/task06_sql_reports.py
git commit -m "Lab07: create SQLite queries and summaries"

git add labs/week07/task07_validation.py labs/week07/task08_pipeline/
git commit -m "Lab07: implement validated ingestion pipeline"

git add labs/week07/README.md
git commit -m "Lab07: document tests and pipeline decisions"

git push
```

These are examples; your filenames and exact commit grouping may vary. Commit instructor-provided small fixture files if they belong to your coursework repository, but keep generated SQLite databases and any secrets out of Git.

A useful `.gitignore` entry:

```gitignore
__pycache__/
*.pyc
labs/week07/outputs/*.db
```

---

# 17. Your Week 7 README

Create `labs/week07/README.md` in **your personal solutions repository**, summarizing your work. A suggested outline:

```markdown
# Lab 07 — Data Ingestion: Files, APIs, Web & Databases

## Environment and Setup
- Python version
- Dependencies and installation
- How to run the individual tasks and final pipeline

## Data Sources and Provenance
- CSV fixture and JSON fixture: synthetic classroom data
- HTML fixture: local controlled page
- API: live mode or labeled offline fallback

## Tasks Completed
- CSV, JSON, HTTP API, HTML, SQLite, SQL, validation, integrated pipeline

## Ingestion Contract
- Required fields, expected types, range rules, source ordering

## API and HTML Observations
- HTTP result or fallback reason
- Missing HTML field behavior
- Ethical data collection considerations

## Validation and Rejection Audit
- Invalid data examples and reason categories
- Duplicate/conflict policy

## SQL and Database Design
- Primary key and constraints
- Parameterized queries
- How the grouped report was produced

## Testing and Reruns
| Test | Expected | Actual | Pass/Fail |
| --- | --- | --- | --- |
| First clean run | | | |
| Second run | | | |
| Invalid count | | | |
| Missing timestamp | | | |
| Other case | | | |

## Design Decisions and Debugging
- One challenge and how you resolved it

## AI Engineering Relevance
- Why validation and provenance matter before models

## AI Usage Log
- Tool, prompt, what was used, and what was independently checked
```

Include *your actual measurements and observations* rather than copying expected numbers without running the program.

---

# 18. Responsible AI-Assisted Learning

AI tools may support learning and debugging, but they must not replace your own reasoning or become a source of unverified outputs.

Useful prompts include:

- “Do not write the complete solution. Explain why CSV numeric values arrive as strings.”
- “Help me design the input/output contract for a record-validation function.”
- “Give me three edge cases for a timestamp parser without coding it for me.”
- “Explain this HTTP status or traceback and where I should debug first.”
- “Help me reason about why SQL placeholders are safer than string concatenation.”
- “Review my source/validation/storage module boundaries without rewriting my project.”
- “Why did a second pipeline run attempt to insert the same records?”
- “Explain how to test whether rejected records are being counted correctly.”

If you used AI, document it:

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

You are responsible for understanding and defending every line of your submitted code.

---

# 19. Self-Study Learning Resources

Use these resources to clarify concepts as you work; you do not need to read every reference from start to finish.

### 1. Python — CSV File Reading and Writing

https://docs.python.org/3/library/csv.html

Focus on `DictReader`, `DictWriter`, `newline=""`, and the fact that CSV values are commonly parsed as strings.

### 2. Python — JSON Module

https://docs.python.org/3/library/json.html

Review `load()`, `loads()`, `dump()`, and nested dictionaries/lists.

### 3. Requests — Quickstart

https://requests.readthedocs.io/en/latest/user/quickstart/

Focus on GET requests, timeouts, status checks, and JSON parsing.

### 4. GitHub REST API — Repository Endpoint

https://docs.github.com/en/rest/repos/repos#get-a-repository

Use this for understanding the example public repository metadata endpoint. The API has rate limits and may be unavailable; the supplied local fixture is enough to complete the validation portion.

### 5. Beautiful Soup — Documentation

https://www.crummy.com/software/BeautifulSoup/bs4/doc/

Focus on `select()`, `select_one()`, attributes, and `.get_text(strip=True)`.

### 6. Python — SQLite

https://docs.python.org/3/library/sqlite3.html

Focus on connections, transactions, parameter substitution, `execute()`, and `fetchall()`.

### 7. SQLite — SQL Syntax

https://www.sqlite.org/lang_select.html

Review `WHERE`, `GROUP BY`, `ORDER BY`, and aggregate queries.

### 8. Pandas — SQL Reading

https://pandas.pydata.org/docs/reference/api/pandas.read_sql_query.html

Connect SQL queries back to what you learned about DataFrames in Week 6.

### Suggested Independent Learning Path

```text
Understand CSV headers and types
       ↓
Read nested JSON
       ↓
Handle HTTP failures and validate responses
       ↓
Extract from controlled HTML
       ↓
Create/query SQLite tables
       ↓
Validate mixed-source records
       ↓
Build an ingest → validate → store pipeline
       ↓
Verify reruns and document provenance
```

---

# 20. Submission Checklist

Before submitting, confirm:

- [ ] Work is inside `labs/week07/` in your personal repository.
- [ ] Required instructor fixtures remain unchanged.
- [ ] CSV and JSON tasks report record counts and data types correctly.
- [ ] API task handles timeouts/HTTP errors and labels offline fallback accurately.
- [ ] HTML task extracts local station cards and handles missing status.
- [ ] SQLite task uses a primary key, constraints, and parameterized inserts/queries.
- [ ] SQL task demonstrates filtering, grouping, and Pandas interoperability.
- [ ] Record-validation task distinguishes invalid data from duplicates.
- [ ] Final pipeline combines both CSV and JSON sources in the specified order.
- [ ] Rejected records include meaningful reasons.
- [ ] Database has 15 canonical rows after a fresh successful run.
- [ ] Running the pipeline a second time does not increase that total.
- [ ] Station summary comes from a SQL query over stored records.
- [ ] Generated database and secrets are not committed.
- [ ] Your README explains provenance, design choices, tests, and observations.
- [ ] AI Usage Log is included if AI assistance was used.
- [ ] Your Git history contains multiple meaningful commits.
- [ ] Work is pushed and you can explain your implementation in a brief technical discussion.

---

# 21. Expected Learning Outcomes

After completing this lab, you should be able to:

- distinguish CSV, JSON, API, HTML, and database sources;
- build reliable reading/parsing functions for structured data;
- handle predictable API/network failures;
- extract data from controlled HTML responsibly;
- create SQLite tables and write parameterized SQL;
- validate and deduplicate multi-source data without silent data loss;
- make a small ingestion pipeline repeatable and auditable;
- document data contracts, provenance, tests, and design decisions.

---

## Lab 07 Key Message

> **Good AI systems start with trustworthy data movement.** An ingestion pipeline should not merely move records; it should explain what came in, what was rejected, what was stored, and what will happen on the next run.
