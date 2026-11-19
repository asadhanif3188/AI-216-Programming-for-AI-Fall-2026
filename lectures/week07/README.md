# AI-216: Programming for Artificial Intelligence
## Week 07 Lecture Handout — Data Ingestion: Files, APIs, Web & Databases

**Semester:** Fall 2026  
**Week:** 07  
**Theme:** From data that already exists in a DataFrame to reliable acquisition and storage of data for AI systems

---

## 1. Why Week 7 Matters

In Week 4, we learned to organize values using Python's built-in data structures. In Week 5, NumPy introduced arrays for numerical computation. In Week 6, Pandas helped us **inspect, select, transform, and summarize data already available in a table**.

But where does that data come from in the first place?

In practical AI systems, data arrives in many forms:

- CSV exports from an organization's systems
- JSON documents and API responses
- publicly available web pages (when collection is permitted)
- databases containing records collected over time
- logs and sensor measurements produced continuously

Different sources require different methods of access. Data can be missing, malformed, duplicated, delayed, or restricted by usage rules. Simply obtaining a response is not the same as having trustworthy, usable data.

```text
External Data Sources
  ├── CSV / JSON files
  ├── HTTP APIs
  ├── permitted web pages
  └── databases
          ↓
       Ingest
          ↓
  Inspect & Validate
          ↓
  Represent Consistently
          ↓
         Store
          ↓
  Ready for Pandas / ML
```

> **AI engineering question:** How do we collect data responsibly from different sources, check that it is usable, and store it so future processing can be reproduced?

Week 7 answers this question at an introductory level. **Week 8** will go deeper into preprocessing, missing-value strategies, exploratory data analysis, and visualization.

---

## 2. Week 7 Learning Outcomes

By the end of this lecture, you should be able to:

1. Explain the difference between **data ingestion**, **data validation**, **data transformation**, and **data storage**.
2. Read and write CSV files with Python's `csv` module, including correctly converting field types.
3. Read and write JSON with Python's `json` module and navigate nested records.
4. Describe the request–response model of HTTP and the purpose of common methods and status codes.
5. Retrieve and inspect a public API response using `requests`, a timeout, and basic error handling.
6. Describe how HTML can be inspected with Beautiful Soup and when scraping is inappropriate.
7. Create and query a small SQLite database with `sqlite3` and introductory SQL.
8. Explain why parameterized SQL queries, unique identifiers, and explicit transactions matter.
9. Describe respectful, ethical, and privacy-aware collection practices.
10. Reason through a small **ingest → validate → store** workflow and explain how it connects to AI projects.

---

## 3. The Data Ingestion Mindset

**Ingestion** means bringing data from a source into your program or system. It does not automatically mean the data is correct.

Think in terms of five questions:

1. **Source:** Where did these records originate?
2. **Format:** How are they represented (CSV, JSON, HTML, database rows)?
3. **Contract:** What columns or keys and types are expected?
4. **Failures:** What could be missing, invalid, repeated, or unavailable?
5. **Destination:** Where will valid records be stored and why?

| Stage | Question | Example |
| --- | --- | --- |
| Ingest | Can I obtain records? | Read a CSV file |
| Parse | Can I interpret the format? | Convert JSON text to a Python dictionary |
| Validate | Are required values usable? | Ensure `temperature_c` is numeric |
| Store | Can I retain records? | Insert into SQLite |
| Consume | Can another stage access them? | Read with Pandas |

### 3.1 Basic Example — A Single Value

```python
raw_temperature = "24.5"
temperature_c = float(raw_temperature)
print(temperature_c)
```

A string from an outside system becomes a number we can calculate with.

### 3.2 Intermediate Example — One Structured Record

```python
raw_record = {"sensor_id": "S-01", "temperature_c": "24.5"}

record = {
    "sensor_id": raw_record["sensor_id"],
    "temperature_c": float(raw_record["temperature_c"]),
}
print(record)
```

A source record is converted into a predictable internal structure.

### 3.3 Advanced Example — Many Records with Rejection Tracking

```python
raw_records = [
    {"id": "A1", "temperature_c": "22.1"},
    {"id": "A2", "temperature_c": "bad"},
    {"id": "A3", "temperature_c": "27.0"},
]

accepted = []
rejected = []

for record in raw_records:
    try:
        temperature = float(record["temperature_c"])
    except (ValueError, TypeError, KeyError):
        rejected.append(record["id"])
        continue

    accepted.append({"id": record["id"], "temperature_c": temperature})

print("Accepted:", len(accepted))
print("Rejected IDs:", rejected)
```

**Key idea:** A reliable system makes invalid records *visible*. It does not quietly assume every source value is trustworthy.

---

# 4. CSV: A Common Tabular Data Format

**CSV** stands for *Comma-Separated Values*. A CSV file is plain text containing rows of fields. The first row often provides field names.

```csv
sensor_id,site,temperature_c
S-01,North,22.5
S-02,South,27.1
S-03,North,24.8
```

CSV is common for exports from spreadsheets, sensors, business systems, and public datasets.

Important limitations:

- CSV does not automatically preserve numeric or Boolean types.
- CSV does not inherently define relationships between tables.
- Commas may appear *inside* quoted fields, so splitting a line with `.split(",")` is unreliable.
- Encoding, column names, and missing-field conventions must be agreed upon.

Use Python's built-in **`csv`** module rather than manually parsing delimiters.

---

# 5. Reading CSV Data

### 5.1 Basic Example — Read Rows

```python
import csv
from io import StringIO

csv_text = "sensor_id,temperature_c\nS-01,22.5\nS-02,27.1\n"

reader = csv.reader(StringIO(csv_text))
for row in reader:
    print(row)
```

Output:

```text
['sensor_id', 'temperature_c']
['S-01', '22.5']
['S-02', '27.1']
```

Notice that numeric-looking values are **strings**.

### 5.2 Intermediate Example — Use Column Names

```python
import csv
from io import StringIO

csv_text = "sensor_id,temperature_c\nS-01,22.5\nS-02,27.1\n"

reader = csv.DictReader(StringIO(csv_text))
for row in reader:
    sensor_id = row["sensor_id"]
    temperature = float(row["temperature_c"])
    print(sensor_id, temperature)
```

`DictReader` turns each CSV row into a dictionary keyed by the header names. This avoids relying on column positions.

### 5.3 Advanced Example — Validate Fields Before Using Them

```python
import csv
from io import StringIO

csv_text = """id,temperature_c
A1,22.5
A2,not-available
A3,88.0
A4,19.5
"""

valid = []
issues = []

for line_number, row in enumerate(csv.DictReader(StringIO(csv_text)), start=2):
    try:
        value = float(row["temperature_c"])
    except (TypeError, ValueError, KeyError):
        issues.append((line_number, "non-numeric temperature"))
        continue

    if not -20 <= value <= 60:
        issues.append((line_number, "temperature outside expected range"))
        continue

    valid.append({"id": row["id"], "temperature_c": value})

print("Valid records:", valid)
print("Issues:", issues)
```

The range is a **sample application rule**, not a universal physical limit. A real system must document its own valid ranges.

---

# 6. Reading and Writing CSV Files on Disk

The previous examples used `StringIO` to keep them self-contained. Real programs usually receive files on disk.

### 6.1 Basic Example — Write a CSV

```python
import csv

with open("sample_measurements.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(["sensor_id", "temperature_c"])
    writer.writerow(["S-01", 22.5])
    writer.writerow(["S-02", 27.1])
```

`newline=""` allows the CSV module to manage line endings correctly.

### 6.2 Intermediate Example — Read a CSV File

```python
import csv

# Run the preceding example first to create this file.
with open("sample_measurements.csv", "r", newline="", encoding="utf-8") as file:
    for row in csv.DictReader(file):
        print(row["sensor_id"], float(row["temperature_c"]))
```

### 6.3 Advanced Example — Export Structured Records

```python
import csv

records = [
    {"sensor_id": "S-01", "temperature_c": 22.5, "valid": True},
    {"sensor_id": "S-02", "temperature_c": 27.1, "valid": True},
]

with open("validated_measurements.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(
        file,
        fieldnames=["sensor_id", "temperature_c", "valid"],
    )
    writer.writeheader()
    writer.writerows(records)
```

**Engineering note:** Convert values to their intended types when reading them back. Writing a Boolean to CSV does not make the stored text a real Python `bool`.

---

# 7. JSON: Structured Data Beyond Rows and Columns

**JSON** stands for *JavaScript Object Notation*. It is commonly used for configuration, API requests/responses, and nested records.

```json
{
  "sensor_id": "S-01",
  "temperature_c": 22.5,
  "tags": ["outdoor", "north"],
  "active": true,
  "notes": null
}
```

Common mappings when JSON is parsed in Python:

| JSON | Python |
| --- | --- |
| Object `{}` | `dict` |
| Array `[]` | `list` |
| String | `str` |
| Number | `int` or `float` |
| `true` / `false` | `True` / `False` |
| `null` | `None` |

JSON is more suitable than CSV for many nested structures, but it is not a relational database and does not automatically validate a record's meaning.

---

# 8. Parsing and Creating JSON

### 8.1 Basic Example — Parse JSON Text

```python
import json

payload = '{"sensor_id": "S-01", "temperature_c": 22.5}'
record = json.loads(payload)

print(record["sensor_id"])
print(type(record))
```

`json.loads()` parses JSON **text**.

### 8.2 Intermediate Example — Navigate Nested Records

```python
import json

payload = """{
  "site": "North",
  "observations": [
    {"sensor_id": "S-01", "value": 22.5},
    {"sensor_id": "S-02", "value": 24.1}
  ]
}"""

data = json.loads(payload)
for record in data["observations"]:
    print(data["site"], record["sensor_id"], record["value"])
```

### 8.3 Advanced Example — Serialize a Structured Summary

```python
import json

summary = {
    "source": "sample_sensors",
    "accepted": 3,
    "rejected": 1,
    "temperature_range": {"min": 18.3, "max": 27.1},
    "warnings": ["one missing reading"],
}

json_text = json.dumps(summary, indent=2)
print(json_text)

with open("ingestion_summary.json", "w", encoding="utf-8") as file:
    json.dump(summary, file, indent=2)
```

- `json.dumps()` returns a JSON **string**.
- `json.dump()` writes JSON to a **file-like object**.
- `json.loads()` reads JSON text.
- `json.load()` reads JSON from a file-like object.

**Tip:** Avoid saving secrets, credentials, or unnecessary personal data in exported JSON.

---

# 9. CSV vs JSON: Choosing the Right Format

| Question | CSV | JSON |
| --- | --- | --- |
| Is the data naturally a flat table? | Often suitable | Possible |
| Does each record contain nested objects/lists? | Awkward | Naturally supported |
| Are data types clearly represented? | Mostly text | Basic JSON types |
| Common in spreadsheet exports? | Yes | Less often |
| Common in web APIs? | Sometimes | Very often |

### 9.1 Basic Example — Flat Table

```python
# CSV-like structure: one row per sensor.
rows = [
    ["S-01", 22.5],
    ["S-02", 27.1],
]
```

### 9.2 Intermediate Example — Named Fields

```python
record = {"sensor_id": "S-01", "temperature_c": 22.5}
```

### 9.3 Advanced Example — Nested Response

```python
response = {
    "source": "weather_service",
    "metadata": {"unit": "celsius", "region": "North"},
    "observations": [
        {"sensor_id": "S-01", "temperature": 22.5},
        {"sensor_id": "S-02", "temperature": 27.1},
    ],
}
```

**Key idea:** Choose a format based on the data's structure and the source contract, not because one format is universally better.

---

# 10. Understanding HTTP and APIs

An **API** (Application Programming Interface) provides a defined way for software systems to exchange requests and responses.

Many data APIs use **HTTP**, the protocol underlying the web.

```text
Python program (client)
       |
       | HTTP request
       | GET /records?limit=10
       v
   API server
       |
       | HTTP response
       | status + headers + body
       v
Python program parses JSON
```

Important parts of a request:

- **URL:** Where the resource is located
- **Method:** What operation is requested
- **Headers:** Metadata, such as expected content type or authorization
- **Query parameters:** Filters, pages, and other options
- **Body:** Data sent with methods such as POST, when needed

| Method | Common purpose | Example |
| --- | --- | --- |
| `GET` | Retrieve a resource | Fetch a list of observations |
| `POST` | Submit/create data | Send a new observation |
| `PUT` | Replace a resource | Replace a configuration object |
| `PATCH` | Partially update a resource | Update one field |
| `DELETE` | Remove a resource | Delete a test record |

For Week 7, **GET-based retrieval** is the central practical skill. The other methods are introduced for context.

---

# 11. HTTP Responses, Status Codes, and JSON

A response commonly contains:

```text
HTTP/1.1 200 OK
Content-Type: application/json

{"id": 1, "title": "sample record"}
```

| Status | Meaning |
| --- | --- |
| `200` | Successful request |
| `201` | Resource created |
| `400` | Bad request |
| `401` | Authentication required / invalid authentication |
| `403` | Request forbidden |
| `404` | Resource not found |
| `429` | Too many requests / rate limit |
| `500` | Server-side failure |
| `503` | Service temporarily unavailable |

**Remember:** A network connection can succeed while the HTTP response indicates failure. We must inspect both transport errors and status codes.

### 11.1 Basic Example — Interpret a Status

```python
status_code = 200
print("Success" if 200 <= status_code < 300 else "Not successful")
```

### 11.2 Intermediate Example — Read an HTTP Error

```python
status_code = 404

if status_code == 404:
    print("The requested resource was not found")
elif status_code == 429:
    print("The service is rate limiting requests")
```

### 11.3 Advanced Example — Separate Network and Data Concerns

```python
def interpret_status(status_code):
    if 200 <= status_code < 300:
        return "success"
    if status_code == 429:
        return "rate_limited"
    if 400 <= status_code < 500:
        return "client_error"
    if 500 <= status_code < 600:
        return "server_error"
    return "other"

print(interpret_status(503))
```

The status classification helps decide whether to inspect input, reduce requests, or investigate service availability. Do not blindly retry every error.

---

# 12. Calling a Public API with `requests`

A common Python library for HTTP is **`requests`**. It is not built into Python.

Install it in the environment being used:

```bash
python -m pip install requests
```

The examples below use the public demo service `https://jsonplaceholder.typicode.com`. It supplies illustrative JSON records. **Live examples need internet access** and rely on the external demo service being available; the lecture's core principles do not depend on that service.

### 12.1 Basic Example — One GET Request

```python
import requests

url = "https://jsonplaceholder.typicode.com/todos/1"
response = requests.get(url, timeout=10)
response.raise_for_status()

record = response.json()
print(record["id"], record["title"])
```

- `timeout=10` prevents a request from waiting indefinitely.
- `raise_for_status()` raises an error for HTTP 4xx and 5xx responses.
- `.json()` attempts to decode the response as JSON.

### 12.2 Intermediate Example — Query Parameters

```python
import requests

url = "https://jsonplaceholder.typicode.com/posts"
response = requests.get(
    url,
    params={"userId": 1},
    timeout=10,
)
response.raise_for_status()

posts = response.json()
print("Records received:", len(posts))
print("First title:", posts[0]["title"] if posts else "No results")
```

`params=` safely constructs URL query parameters. It is preferable to manually concatenating values into a URL.

### 12.3 Advanced Example — Validate the API Shape

```python
import requests


def fetch_todo(todo_id):
    url = f"https://jsonplaceholder.typicode.com/todos/{todo_id}"

    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        payload = response.json()
    except requests.RequestException as error:
        print("HTTP/network problem:", error)
        return None
    except ValueError:
        print("Response was not valid JSON")
        return None

    if not isinstance(payload, dict):
        print("Unexpected response structure")
        return None
    if not isinstance(payload.get("id"), int) or not isinstance(payload.get("title"), str):
        print("Required fields are missing or invalid")
        return None

    return {"id": payload["id"], "title": payload["title"]}


print(fetch_todo(1))
```

**Key idea:** A `200` response does not guarantee that the returned data has the structure or meaning your program expects.

---

# 13. API Failure Modes and Defensive Retrieval

A robust ingestion process distinguishes several failure types:

| Failure | Typical cause | Appropriate first response |
| --- | --- | --- |
| Connection/timeout | Network or service unavailable | Report or handle the failure; retry only when appropriate |
| `401` or `403` | Missing permission | Fix authorization or stop; do not bypass controls |
| `404` | Wrong identifier/path | Inspect endpoint and input |
| `429` | Rate limit | Respect `Retry-After` / documented limits |
| Invalid JSON | Wrong or malformed body | Report parse error |
| Missing key | Response schema changed | Validate contract; avoid guessing |
| Empty result | Query returned no records | Treat as a possible valid outcome |

### 13.1 Basic Example — Protect a Required Key

```python
record = {"id": 7}
print(record.get("title", "<missing title>"))
```

### 13.2 Intermediate Example — Validate an Empty Response

```python
records = []

if not records:
    print("No records returned; nothing to process")
else:
    print("Records:", len(records))
```

### 13.3 Advanced Example — Make Failure Explicit

```python
import requests


def fetch_records(url):
    try:
        response = requests.get(url, timeout=(3, 10))
        response.raise_for_status()
        data = response.json()
    except (requests.RequestException, ValueError) as error:
        raise RuntimeError(f"Unable to retrieve usable data: {error}") from error

    if not isinstance(data, list):
        raise ValueError("Expected the API to return a JSON list")

    return data

# Example call (requires network):
# records = fetch_records("https://jsonplaceholder.typicode.com/posts")
```

Whether you **return a failure result**, **raise an exception**, or **retry** depends on the application. The important principle is that failures should not silently become fake or misleading data.

---

# 14. API Pagination: One Response May Not Be All the Data

Many APIs return results in pages. The pagination contract differs across services: some use `page`, some use `offset` and `limit`, and others return a cursor or a `next` URL.

### 14.1 Basic Example — Page Parameters

```python
params = {"page": 1, "limit": 20}
print(params)
```

### 14.2 Intermediate Example — Build a Set of Page Requests

```python
for page in range(1, 4):
    params = {"page": page, "limit": 20}
    print("Request parameters:", params)
```

### 14.3 Advanced Example — Process Pages from a Controlled Fixture

```python
# Simplified example of a paginated API; no network calls needed.
mock_pages = {
    1: [{"id": 1}, {"id": 2}],
    2: [{"id": 3}],
    3: [],
}

collected = []
for page_number in range(1, 10):
    batch = mock_pages.get(page_number, [])
    if not batch:
        break
    collected.extend(batch)

print("Total records:", len(collected))
```

**Engineering note:** Do not assume all APIs use an empty page as the stopping condition. Follow the provider's documented pagination scheme and request limits.

---
# 15. What Is Web Scraping?

**Web scraping** means extracting information from a web page's HTML. It is different from calling a documented API:

```text
API retrieval                     Web-page extraction
---------------------------       ---------------------------
Request a documented endpoint     Request a web page
Receive a structured response     Receive HTML
Parse JSON or CSV                 Locate HTML elements
Use a documented data contract    Depend on page markup
```

If an appropriate, authorized API or downloadable dataset exists, it is often preferable because the structure and terms of access are clearer.

A simplified HTML page might contain:

```html
<html>
  <body>
    <h1>Sensor catalog</h1>
    <ul>
      <li class="sensor">S-01</li>
      <li class="sensor">S-02</li>
    </ul>
  </body>
</html>
```

`BeautifulSoup` helps navigate and select elements in HTML. Install it with:

```bash
python -m pip install beautifulsoup4
```

It is not included in the Python standard library.

---

# 16. Extracting Data from HTML

The first two examples use local, controlled HTML text so we can focus on the parsing logic. The third shows a single request to a public example page; it requires network access and assumes the page remains available.

### 16.1 Basic Example — Extract a Heading

```python
from bs4 import BeautifulSoup

html = "<html><body><h1>Sensor catalog</h1></body></html>"
soup = BeautifulSoup(html, "html.parser")

heading = soup.find("h1")
print(heading.get_text(strip=True) if heading else "No heading")
```

`find()` returns the first matching element (or `None`).

### 16.2 Intermediate Example — Extract Multiple Records

```python
from bs4 import BeautifulSoup

html = """
<ul>
  <li class="sensor" data-id="S-01">North</li>
  <li class="sensor" data-id="S-02">South</li>
</ul>
"""

soup = BeautifulSoup(html, "html.parser")
records = []

for element in soup.select("li.sensor"):
    records.append({
        "sensor_id": element.get("data-id"),
        "site": element.get_text(strip=True),
    })

print(records)
```

`select()` accepts a **CSS selector**. This example extracts both text and an HTML attribute.

### 16.3 Advanced Example — Fetch and Parse One Permitted Example Page

```python
import requests
from bs4 import BeautifulSoup

url = "https://example.com/"

try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
except requests.RequestException as error:
    print("Could not fetch the demonstration page:", error)
else:
    soup = BeautifulSoup(response.text, "html.parser")
    heading = soup.select_one("h1")
    print("Page heading:", heading.get_text(strip=True) if heading else "Not found")
```

**Key idea:** HTML is presentation-oriented. A heading or CSS class can change without notice. A scraper must handle missing elements and respect the site's access rules.

---

# 17. A Web Page Is Not Necessarily a Data API

Some pages load data through JavaScript after the original HTML is delivered. A simple `requests.get()` call retrieves the HTTP response; it does **not** execute the page's JavaScript.

This means the content visible in a browser may not be present in the HTML retrieved with `requests`.

### 17.1 Basic Example — Missing Element

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup("<html><body></body></html>", "html.parser")
heading = soup.select_one("h1")

print("Heading present?", heading is not None)
```

### 17.2 Intermediate Example — Extracting a Link

```python
from bs4 import BeautifulSoup

html = '<a href="/datasets/sensors.csv">Download sensor data</a>'
soup = BeautifulSoup(html, "html.parser")

link = soup.select_one("a[href]")
if link is not None:
    print("Text:", link.get_text(strip=True))
    print("Relative URL:", link.get("href"))
```

### 17.3 Advanced Example — Normalize a Relative URL

```python
from urllib.parse import urljoin

page_url = "https://example.com/catalog/index.html"
relative_href = "../datasets/sensors.csv"

absolute_url = urljoin(page_url, relative_href)
print(absolute_url)
```

**Important:** Constructing a URL does not establish that you are permitted to download it. Check the source's documented terms, access restrictions, and data permissions first.

---

# 18. Ethical, Legal, and Responsible Data Collection

A technically possible download is not automatically an appropriate one.

Data collection should respect:

- source terms of service, licences, and stated access requirements
- `robots.txt` and other published crawl guidance for automated access
- rate limits and server capacity
- privacy, consent, confidentiality, and data minimization
- copyrights, ownership, and attribution requirements
- local law and your institution's policies
- security practices, including safe handling of API keys

**Do not** bypass authentication, CAPTCHAs, rate limits, or access restrictions. Do not collect private or sensitive data without a legitimate basis and the required permissions. An unrestricted URL is not proof of permission to reuse all its contents.

`robots.txt` is useful crawl guidance; it is **not** a substitute for legal permission, licence terms, or privacy obligations.

### 18.1 Basic Example — Ask Before You Collect

For a page containing publicly visible names and email addresses, ask:

```text
Do I need these personal fields?
Do I have permission to collect or reuse them?
Can a less sensitive source solve the problem?
```

If the answer is unclear, do not collect the fields.

### 18.2 Intermediate Example — Minimize Data

```python
api_record = {
    "public_id": "P-104",
    "temperature_c": 23.4,
    "operator_email": "private@example.org",
}

# Only preserve the fields required for numerical analysis.
safe_record = {
    "public_id": api_record["public_id"],
    "temperature_c": api_record["temperature_c"],
}

print(safe_record)
```

This is **data minimization**, not a guarantee of anonymity. Identifiers can still be linkable to people in some contexts.

### 18.3 Advanced Example — Capture Source Provenance

```python
source_manifest = {
    "source_name": "public_sensor_demo",
    "source_type": "authorized_file_export",
    "source_reference": "institutional-demo-v1",
    "collected_for": "classroom numerical analysis",
    "contains_personal_data": False,
    "usage_conditions_reviewed": True,
}

print(source_manifest)
```

A manifest can record **where data came from**, **why it was collected**, and **under what conditions**. The values must reflect real checks; writing `True` does not mean permission was actually obtained.

---

# 19. SQLite: A Small Relational Database

A file can store rows, but a database can store, search, update, and organize records with explicit structure.

**SQLite** is a lightweight relational database engine. It can store a database in one local file and is accessible through Python's built-in `sqlite3` module.

A relational table has named columns and rows:

```text
observations
+----+--------+---------------+
| id | site   | temperature_c |
+----+--------+---------------+
|  1 | North  | 22.5          |
|  2 | South  | 27.1          |
+----+--------+---------------+
```

A schema specifies columns and their intended data types and constraints.

| SQL concept | Meaning |
| --- | --- |
| Table | Named collection of related records |
| Row | One record |
| Column | One field in a record |
| Primary key | Unique identifier for a row |
| Constraint | Rule checked when storing data |
| Query | Request to read or change data |
| Transaction | Group of operations committed or rolled back together |

SQLite is excellent for a small local learning project. It is not a replacement for every multi-user production database.

---

# 20. Creating and Inserting SQLite Records

### 20.1 Basic Example — Create an In-Memory Table

```python
import sqlite3

connection = sqlite3.connect(":memory:")
try:
    connection.execute("CREATE TABLE sensors (id TEXT, temperature_c REAL)")
    connection.execute(
        "INSERT INTO sensors (id, temperature_c) VALUES (?, ?)",
        ("S-01", 22.5),
    )
    connection.commit()

    rows = connection.execute("SELECT id, temperature_c FROM sensors").fetchall()
    print(rows)
finally:
    connection.close()
```

`":memory:"` stores the database in memory for the duration of the connection. The data disappears after the connection closes.

### 20.2 Intermediate Example — Store Multiple Records

```python
import sqlite3

records = [
    ("S-01", "North", 22.5),
    ("S-02", "South", 27.1),
    ("S-03", "North", 24.8),
]

connection = sqlite3.connect(":memory:")
try:
    connection.execute("""
        CREATE TABLE observations (
            sensor_id TEXT PRIMARY KEY,
            site TEXT NOT NULL,
            temperature_c REAL NOT NULL
        )
    """)
    connection.executemany(
        "INSERT INTO observations (sensor_id, site, temperature_c) VALUES (?, ?, ?)",
        records,
    )
    connection.commit()
    print("Rows:", connection.execute("SELECT COUNT(*) FROM observations").fetchone()[0])
finally:
    connection.close()
```

A primary key prevents duplicate IDs in this *example* schema. In a real time-series system, several observations may belong to the same sensor; you would normally use a distinct **observation ID** or a suitable composite key instead.

### 20.3 Advanced Example — Persistent File, Constraints, and Transactions

```python
import sqlite3
from pathlib import Path

Path("data").mkdir(exist_ok=True)
connection = sqlite3.connect("data/ai216_sensors.db")

try:
    with connection:  # commit on success, rollback on exception
        connection.execute("""
            CREATE TABLE IF NOT EXISTS readings (
                reading_id TEXT PRIMARY KEY,
                sensor_id TEXT NOT NULL,
                temperature_c REAL NOT NULL
                    CHECK (temperature_c BETWEEN -20 AND 60)
            )
        """)
        connection.execute(
            "INSERT OR IGNORE INTO readings VALUES (?, ?, ?)",
            ("R-001", "S-01", 22.5),
        )

    print(connection.execute("SELECT COUNT(*) FROM readings").fetchone()[0])
finally:
    connection.close()
```

`INSERT OR IGNORE` makes this demonstration safe to rerun with the same ID, but it also suppresses some constraint errors. In a production pipeline, choose an explicit duplicate/constraint policy and monitor rejected writes rather than masking unexpected data problems.

**Subtle point:** `with connection:` manages the transaction but does **not** automatically close the SQLite connection; the `finally` block closes it.

---

# 21. Reading Data with SQL

SQL stands for **Structured Query Language**. We use SQL to retrieve and summarize data stored in relational tables.

Common statements and clauses:

```sql
SELECT ... FROM ...
WHERE ...
ORDER BY ...
LIMIT ...
GROUP BY ...
```

The following examples use a self-contained in-memory database.

### 21.1 Basic Example — Select Rows

```python
import sqlite3

conn = sqlite3.connect(":memory:")
try:
    conn.execute("CREATE TABLE scores (model TEXT, accuracy REAL)")
    conn.executemany("INSERT INTO scores VALUES (?, ?)", [
        ("baseline", 0.81), ("tree", 0.89), ("linear", 0.85),
    ])

    for row in conn.execute("SELECT model, accuracy FROM scores"):
        print(row)
finally:
    conn.close()
```

### 21.2 Intermediate Example — Filter and Sort

```python
import sqlite3

conn = sqlite3.connect(":memory:")
try:
    conn.execute("CREATE TABLE scores (model TEXT, accuracy REAL)")
    conn.executemany("INSERT INTO scores VALUES (?, ?)", [
        ("baseline", 0.81), ("tree", 0.89), ("linear", 0.85),
    ])

    query = """
        SELECT model, accuracy
        FROM scores
        WHERE accuracy >= ?
        ORDER BY accuracy DESC
        LIMIT 2
    """
    print(conn.execute(query, (0.85,)).fetchall())
finally:
    conn.close()
```

The `?` placeholder separates SQL code from an input value.

### 21.3 Advanced Example — Group and Summarize

```python
import sqlite3

conn = sqlite3.connect(":memory:")
try:
    conn.execute("CREATE TABLE experiments (model_type TEXT, accuracy REAL)")
    conn.executemany("INSERT INTO experiments VALUES (?, ?)", [
        ("tree", 0.87), ("linear", 0.81),
        ("tree", 0.91), ("linear", 0.85),
    ])

    result = conn.execute("""
        SELECT model_type, COUNT(*) AS runs, AVG(accuracy) AS mean_accuracy
        FROM experiments
        GROUP BY model_type
        ORDER BY model_type
    """).fetchall()

    for model_type, runs, mean_accuracy in result:
        print(model_type, runs, round(mean_accuracy, 3))
finally:
    conn.close()
```

**Connection to Week 6:** `GROUP BY` answers questions similar to `pandas.DataFrame.groupby()`, but the data is summarized in the database rather than first being loaded into a DataFrame.

---

# 22. Parameterized Queries and SQL Safety

Never assemble SQL by inserting arbitrary user-controlled text directly into the SQL statement.

Unsafe pattern (**do not use**):

```python
# query = f"SELECT * FROM users WHERE name = '{user_input}'"
```

This can alter the intended query and create a **SQL injection** vulnerability.

Use parameters for **values**:

### 22.1 Basic Example — One Parameter

```python
query = "SELECT * FROM scores WHERE accuracy >= ?"
parameters = (0.85,)
```

### 22.2 Intermediate Example — Multiple Parameters

```python
query = "SELECT * FROM observations WHERE site = ? AND temperature_c >= ?"
parameters = ("North", 20.0)
```

### 22.3 Advanced Example — Safe Lookup Function

```python
import sqlite3


def find_observations(connection, site, minimum_temperature):
    query = """
        SELECT sensor_id, temperature_c
        FROM observations
        WHERE site = ? AND temperature_c >= ?
        ORDER BY temperature_c DESC
    """
    return connection.execute(query, (site, minimum_temperature)).fetchall()


connection = sqlite3.connect(":memory:")
try:
    connection.execute("""
        CREATE TABLE observations (
            sensor_id TEXT, site TEXT, temperature_c REAL
        )
    """)
    connection.executemany("INSERT INTO observations VALUES (?, ?, ?)", [
        ("S-01", "North", 22.5),
        ("S-02", "South", 27.1),
        ("S-03", "North", 24.8),
    ])
    print(find_observations(connection, "North", 23.0))
finally:
    connection.close()
```

**Important limitation:** Placeholders work for **values**, not arbitrary table or column names. If SQL identifiers need to vary, select them from a small allowlist rather than accepting arbitrary user input.

---

# 23. Connecting SQLite and Pandas

SQLite can store local data, and Pandas can later inspect it as a DataFrame.

### 23.1 Basic Example — SQL Returns Tuples

```python
rows = [("S-01", 22.5), ("S-02", 27.1)]
print(rows[0][1])
```

### 23.2 Intermediate Example — Convert to a DataFrame

```python
import pandas as pd

rows = [("S-01", 22.5), ("S-02", 27.1)]
df = pd.DataFrame(rows, columns=["sensor_id", "temperature_c"])
print(df)
```

### 23.3 Advanced Example — Read SQL Directly into Pandas

```python
import sqlite3
import pandas as pd

connection = sqlite3.connect(":memory:")
try:
    connection.execute("CREATE TABLE readings (site TEXT, temperature_c REAL)")
    connection.executemany("INSERT INTO readings VALUES (?, ?)", [
        ("North", 22.5), ("South", 27.1), ("North", 24.8),
    ])

    df = pd.read_sql_query(
        "SELECT site, temperature_c FROM readings",
        connection,
    )
    print(df.groupby("site")["temperature_c"].mean())
finally:
    connection.close()
```

**Key idea:** Data ingestion and data manipulation are connected, but separate engineering responsibilities. Week 7 introduces storage; Week 6 showed how to work with the resulting tabular data.

---

# 24. Designing a Simple Ingest → Validate → Store Pipeline

A **data pipeline** is an organized sequence of steps that moves data from a source toward a useful destination.

```text
SOURCE
   ↓
INGEST      Obtain raw records
   ↓
PARSE       Read CSV / JSON / API payload
   ↓
VALIDATE    Required fields, types, reasonable ranges
   ↓
STORE       Keep accepted records in SQLite / file
   ↓
REPORT      Record counts, failures, source identity
```

The roles should be kept distinct. A pipeline needs a predictable input and output contract for each stage.

### 24.1 Basic Example — One Step

```python
def parse_temperature(raw_value):
    return float(raw_value)

print(parse_temperature("25.3"))
```

### 24.2 Intermediate Example — Compose Two Steps

```python
def parse_temperature(raw_value):
    value = float(raw_value)
    if not -20 <= value <= 60:
        raise ValueError("temperature outside expected range")
    return value


def prepare_record(raw_record):
    return {
        "id": raw_record["id"],
        "temperature_c": parse_temperature(raw_record["temperature_c"]),
    }

print(prepare_record({"id": "R-01", "temperature_c": "25.3"}))
```

### 24.3 Advanced Example — Track Pipeline Outcomes

```python
def validate_record(raw):
    try:
        temperature = float(raw["temperature_c"])
        if not -20 <= temperature <= 60:
            raise ValueError("out of range")
        return {"id": raw["id"], "temperature_c": temperature}
    except (KeyError, ValueError, TypeError):
        return None


def process_batch(records):
    accepted = []
    rejected_ids = []

    for raw in records:
        result = validate_record(raw)
        if result is None:
            rejected_ids.append(raw.get("id", "unknown"))
        else:
            accepted.append(result)

    return accepted, rejected_ids


raw = [
    {"id": "A1", "temperature_c": "22.5"},
    {"id": "A2", "temperature_c": "missing"},
]
accepted, rejected = process_batch(raw)
print("Accepted:", accepted)
print("Rejected:", rejected)
```

**Engineering note:** In a real pipeline, keep enough information to investigate rejected data without unnecessarily preserving sensitive content.

---
# 25. Integrated AI-Oriented Example — Sensor Data Ingestion

Imagine an AI application that will eventually monitor a group of environmental sensors. The model itself is not our concern yet. First, the engineering team must **ingest source records, validate required measurements, and retain them in a queryable database**.

This example uses a **small, controlled CSV fixture** rather than a live service. That keeps the result reproducible and allows us to focus on engineering decisions. In Lab 07, the same ideas can be applied to a different source and dataset.

## 25.1 The Source and Its Contract

Our source claims to contain:

| Field | Expected meaning | Requirement |
| --- | --- | --- |
| `reading_id` | Unique source observation | Nonempty string |
| `site` | Sensor site | Nonempty string |
| `temperature_c` | Temperature in Celsius | Number between -20 and 60 (sample rule) |
| `humidity_percent` | Relative humidity | Number from 0 to 100 |

We intentionally include two invalid records:

```csv
reading_id,site,temperature_c,humidity_percent
R-001,North,22.0,48
R-002,South,34.5,92
R-003,North,not-a-number,51
R-004,West,71,20
R-005,South,27.1,65
R-006,North,18.3,55
```

## 25.2 A Complete, Runnable Conceptual Workflow

```python
import csv
import sqlite3
from io import StringIO
from pathlib import Path

SOURCE_CSV = """reading_id,site,temperature_c,humidity_percent
R-001,North,22.0,48
R-002,South,34.5,92
R-003,North,not-a-number,51
R-004,West,71,20
R-005,South,27.1,65
R-006,North,18.3,55
"""

DATABASE_PATH = Path("data/sensor_readings.db")
SOURCE_NAME = "classroom_csv_v1"


def ingest_csv(csv_text):
    """Parse CSV text into dictionaries; values are still strings."""
    return list(csv.DictReader(StringIO(csv_text)))


def validate_reading(raw):
    """Return a typed record, or raise ValueError with a useful reason."""
    reading_id = (raw.get("reading_id") or "").strip()
    site = (raw.get("site") or "").strip()
    if not reading_id or not site:
        raise ValueError("reading_id and site are required")

    try:
        temperature = float(raw["temperature_c"])
        humidity = float(raw["humidity_percent"])
    except (KeyError, TypeError, ValueError) as error:
        raise ValueError("measurement is missing or not numeric") from error

    if not -20 <= temperature <= 60:
        raise ValueError("temperature outside expected range")
    if not 0 <= humidity <= 100:
        raise ValueError("humidity outside expected range")

    return (reading_id, site, temperature, humidity, SOURCE_NAME)


def store_readings(path, records):
    """Store accepted readings; skip existing reading IDs on reruns."""
    path.parent.mkdir(parents=True, exist_ok=True)
    connection = sqlite3.connect(path)
    try:
        with connection:
            connection.execute("""
                CREATE TABLE IF NOT EXISTS readings (
                    reading_id TEXT PRIMARY KEY,
                    site TEXT NOT NULL,
                    temperature_c REAL NOT NULL,
                    humidity_percent REAL NOT NULL,
                    source_name TEXT NOT NULL
                )
            """)
            before = connection.total_changes
            connection.executemany("""
                INSERT OR IGNORE INTO readings
                (reading_id, site, temperature_c, humidity_percent, source_name)
                VALUES (?, ?, ?, ?, ?)
            """, records)
            inserted_this_run = connection.total_changes - before
        return inserted_this_run
    finally:
        connection.close()


def summarize_stored(path):
    connection = sqlite3.connect(path)
    try:
        return connection.execute("""
            SELECT site, COUNT(*) AS n, ROUND(AVG(temperature_c), 2)
            FROM readings
            GROUP BY site
            ORDER BY site
        """).fetchall()
    finally:
        connection.close()


raw_records = ingest_csv(SOURCE_CSV)
accepted = []
rejected = []

for row in raw_records:
    try:
        accepted.append(validate_reading(row))
    except ValueError as error:
        rejected.append((row.get("reading_id", "unknown"), str(error)))

inserted = store_readings(DATABASE_PATH, accepted)

print("Source records:", len(raw_records))
print("Validated:", len(accepted))
print("Rejected:", rejected)
print("Newly stored this run:", inserted)
print("Stored site summaries:", summarize_stored(DATABASE_PATH))
```

On a fresh database, the conceptual result is:

```text
Source records: 6
Validated: 4
Rejected: [('R-003', 'measurement is missing or not numeric'), ('R-004', 'temperature outside expected range')]
Newly stored this run: 4
Stored site summaries: [('North', 2, 20.15), ('South', 2, 30.8)]
```

The exact number newly inserted may differ when the script is rerun against a pre-existing database. The database retains the valid records from earlier executions.

## 25.3 Why the Responsibilities Are Separate

```text
SOURCE_CSV
   ↓
ingest_csv()           Parse source text
   ↓
validate_reading()     Check expected fields and types
   ↓
accepted / rejected    Make quality outcomes visible
   ↓
store_readings()       Persist valid records in SQLite
   ↓
summarize_stored()     Check stored data is usable
```

This is **basic ingestion validation**, not full data preprocessing. We are not imputing missing values, engineering sophisticated features, visualizing distributions, or training an ML model.

## 25.4 What This Example Demonstrates

- CSV source values must be parsed into appropriate Python types.
- Validation rules should be explicit and explainable.
- Invalid records should be counted and investigated.
- A source-specific ID can help prevent repeat insertion of the same record.
- SQL enables quick sanity checks after storage.
- Each function has one clear responsibility.
- Storing data reliably creates a foundation for later Pandas and ML workflows.

### Important Production Limitations

This is a classroom illustration. A production pipeline would also consider source permissions, schema changes, structured logging, failure recovery, atomicity across stages, monitoring, retention policies, security, and whether an existing record should be **updated** rather than skipped.

---

# 26. Reliability and Repeatability in Data Ingestion

Good data ingestion is not only about one successful run. We also need to reason about what happens on the **second run**, after a source changes, or when only part of a batch is valid.

## 26.1 Basic Example — Count Inputs and Outputs

```python
raw_count = 10
accepted_count = 8
rejected_count = 2

assert accepted_count + rejected_count == raw_count
print("Batch accounted for")
```

## 26.2 Intermediate Example — Detect Schema Changes

```python
expected_columns = {"reading_id", "site", "temperature_c"}
received_columns = {"reading_id", "site", "temperature"}

missing = expected_columns - received_columns
if missing:
    print("Source schema changed; missing:", sorted(missing))
```

## 26.3 Advanced Example — Idempotent Insertion Concept

```python
import sqlite3

conn = sqlite3.connect(":memory:")
try:
    conn.execute("CREATE TABLE readings (reading_id TEXT PRIMARY KEY, value REAL)")
    batch = [("R-1", 10.0), ("R-2", 20.0)]

    for _ in range(2):
        conn.executemany(
            "INSERT OR IGNORE INTO readings (reading_id, value) VALUES (?, ?)",
            batch,
        )

    print(conn.execute("SELECT COUNT(*) FROM readings").fetchone()[0])
finally:
    conn.close()
```

Output:

```text
2
```

**Idempotence** means that repeating an operation does not produce unintended additional effects. Here, repeating ingestion of the *same IDs* does not create extra rows. The appropriate duplicate policy depends on the meaning of the identifiers and whether source records can change.

---

# 27. Organizing an Ingestion Project

Week 3 introduced functions and modules. Week 4 introduced file organization and clean code. Week 7 gives these ideas a concrete application.

### 27.1 Basic Example — One Short Script

For a tiny demonstration:

```text
ingest_demo.py
```

One file is enough if the code is genuinely small.

### 27.2 Intermediate Example — Separate Source and Storage

```text
project/
├── main.py
├── ingestion.py       # obtain and parse source records
└── storage.py         # SQLite schema and queries
```

`main.py` coordinates the work instead of hiding every detail inside one large function.

### 27.3 Advanced Example — A Growing Data Pipeline

```text
project/
├── main.py
├── data/
│   ├── raw/              # source fixtures or permitted downloads
│   └── processed/        # validated exports
├── src/
│   ├── sources.py        # CSV, JSON, or API access
│   ├── validation.py     # explicit record checks
│   └── storage.py        # persistence
└── README.md             # how to run and what assumptions apply
```

This is an example, not a rule that every student must build this many files. Use structure to clarify real responsibilities, not to add unnecessary complexity.

**Secrets note:** A production API key must not be hardcoded into source code or pushed to GitHub. We will develop configuration and environment-variable practices more formally in Week 12.

---

# 28. Common Mistakes and Engineering Pitfalls

## 28.1 Assuming CSV Values Have Numeric Types

```python
raw = {"temperature_c": "22.5"}
# raw["temperature_c"] + 1  # TypeError
value = float(raw["temperature_c"])
```

## 28.2 Parsing CSV with `split(",")`

Quoted fields may contain commas. Use `csv.reader()` or `csv.DictReader()` instead.

## 28.3 Confusing `json.load()` and `json.loads()`

- `load(file)` reads a file-like object.
- `loads(text)` parses a string.

## 28.4 Assuming HTTP `200` Means the Data Is Valid

Check the structure, required fields, expected types, and permitted source usage.

## 28.5 Omitting Timeouts

A network request can hang longer than expected. Use a documented timeout and decide how failures are handled.

## 28.6 Ignoring Pagination

The first API response may contain only a subset of the available records.

## 28.7 Treating Every Web Page as Scrapeable

Do not bypass restrictions or ignore licensing, privacy, terms, or rate limits. Prefer permitted, documented data access.

## 28.8 Assuming HTML Layout Is Stable

A selector that works today can break when a page changes. Check for missing elements.

## 28.9 Building SQL by String Concatenation

Use parameter placeholders for values rather than embedding untrusted text into SQL.

## 28.10 Ignoring Database Constraints

A duplicate identifier or invalid range can cause a write failure. Define a deliberate policy and examine stored results.

## 28.11 Silently Discarding Source Errors

Dropping every malformed record without counts or reasons hides quality problems.

## 28.12 Confusing Ingestion with Full Preprocessing

Week 7 focuses on acquiring, validating basic contracts, and storing data. More extensive cleaning, missing-value strategies, and EDA are central topics in Week 8.

---

# 29. A Practical Mental Model for Week 7

Before using a new source, identify:

| Question | What to look for |
| --- | --- |
| Where is the data? | File path, API endpoint, page, or database |
| May I access and reuse it? | Permission, terms, licence, privacy |
| What is the format? | CSV, JSON, HTML, SQL rows |
| How do I parse it? | `csv`, `json`, `requests`, Beautiful Soup, `sqlite3` |
| What schema do I expect? | Required keys, columns, and types |
| What can fail? | Network, missing fields, wrong types, invalid ranges |
| How will I know what failed? | Rejected-record counts, error messages, basic logs |
| Where will it be stored? | CSV/JSON, SQLite, or a later database |
| Can the process be repeated safely? | IDs, duplicate rules, documented behavior |
| Can I explain its provenance? | Source identity, collection purpose, usage rights |

The central shift is from:

> “I got the data into Python.”

to:

> **“I can explain where the data came from, what assumptions I checked, what failed, and how it is stored.”**

---

# 30. Thinking Like an AI Engineer

### Source Selection

- Is there a documented API or downloadable dataset?
- Am I allowed to access and reuse these records?
- Does the data contain sensitive information I do not need?

### Parsing and Validation

- Do external strings need type conversion?
- What fields or columns are required?
- What does an empty or malformed response mean?
- Have I made invalid-record handling explicit?

### Reliability

- What happens if the network fails?
- Does the source paginate results?
- What happens if the same batch is processed twice?
- How will I detect that the source's schema has changed?

### Storage

- Which fields require a unique identifier?
- Are queries parameterized?
- What belongs in a database versus a file?
- Can a later Pandas workflow read the stored records?

### Maintainability

- Do functions have understandable responsibilities?
- Is the source provenance documented?
- Can another engineer reproduce the ingestion process?

---

# 31. Responsible Use of AI Tools

AI assistants can help explain source formats, debugging messages, and pipeline decisions. They should **not** replace checking the actual data contract or source permissions.

Useful prompts include:

- “Do not write the full solution. Help me identify the required columns and types in this CSV specification.”
- “What is the difference between `csv.reader()` and `csv.DictReader()` for these records?”
- “Explain how this JSON response maps to Python dictionaries and lists.”
- “What does this HTTP status code mean, and what should I investigate first?”
- “Why is my `requests.get()` call timing out? Suggest a debugging checklist, not a bypass.”
- “Explain why this Beautiful Soup selector returns `None`.”
- “Help me understand this SQLite constraint error.”
- “What is the risk of embedding user input directly in this SQL query?”
- “Suggest boundary cases for my record-validation function.”
- “Review my ingest → validate → store design for separation of concerns.”
- “What provenance and permissions should I verify before collecting data from this source?”

Remember:

- Verify generated explanations against actual documentation and code behavior.
- Do not paste API keys, private records, or credentials into AI tools.
- You are responsible for understanding every line of your own program.

---

# 32. Self-Check Questions

Before moving to Lab 07, make sure you can answer:

1. What is data ingestion, and how is it different from preprocessing?
2. Why should we validate records immediately after reading them?
3. What is CSV best suited for?
4. Why is `line.split(",")` an unsafe general CSV parser?
5. What does `csv.DictReader()` return for each row?
6. Why are CSV numbers often strings when read by Python?
7. Why do we use `newline=""` when writing CSV with Python's `csv` module?
8. What kinds of structures can JSON represent that CSV handles poorly?
9. What is the difference between `json.load()` and `json.loads()`?
10. What Python values correspond to JSON `true`, `false`, and `null`?
11. What is an API?
12. What is the role of HTTP in data retrieval?
13. How does GET differ from POST conceptually?
14. What do HTTP status codes 200, 404, 429, and 500 indicate?
15. Why should an HTTP client specify a timeout?
16. What does `response.raise_for_status()` do?
17. Why is an HTTP `200` not a guarantee of a valid record?
18. What is pagination, and why can ignoring it lead to incomplete datasets?
19. How does HTML scraping differ from using an API?
20. What does a CSS selector do in Beautiful Soup?
21. Why might content visible in a browser not appear in the HTML returned by `requests`?
22. Why must data-collection permissions be checked before scraping?
23. What does `robots.txt` communicate, and what does it *not* authorize?
24. What is a relational table?
25. What is a primary key?
26. How does `sqlite3.connect(":memory:")` differ from a file-backed database?
27. What do `SELECT`, `WHERE`, and `GROUP BY` do?
28. Why should we use SQL placeholders for values?
29. Why do database transactions matter?
30. What is an idempotent ingestion operation?
31. Why should a pipeline report both accepted and rejected records?
32. What information should a source-provenance record contain?
33. How do SQLite and Pandas complement each other?
34. What work remains for Week 8 after data is ingested and stored?

---

# 33. Looking Ahead

In **Week 8**, the focus moves from data acquisition to **Data Preprocessing & Exploratory Data Analysis (EDA)**:

- missing values and strategies for handling them
- duplicate records and data-quality decisions
- categorical data
- introductory normalization and scaling concepts
- exploratory data analysis
- visualization and descriptive statistics
- reproducibility

```text
Week 5: Numerical computation (NumPy)
        ↓
Week 6: Tabular data manipulation (Pandas)
        ↓
Week 7: Data ingestion, validation and storage
        ↓
Week 8: Deeper preprocessing and EDA
        ↓
Weeks 10–11: ML workflows and evaluation
```

Week 7 teaches you how to **bring data into a system**. Week 8 will teach you how to **understand and prepare that data for analytical and ML use**.

---

## Week 7 Key Message

> **An AI system is only as trustworthy as the process that brings data into it.**

Files, APIs, permitted web sources, and databases are different interfaces to the same engineering responsibility: acquire data responsibly, understand its structure, validate assumptions, handle failures, and store usable records reproducibly.
