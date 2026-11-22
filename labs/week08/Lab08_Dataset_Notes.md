# Week 08 Study-Space Observation Data — Dataset Notes

This CSV is a **synthetic teaching fixture**, created for AI-216 Lab 08. It is **not** collected from real students, rooms, devices, people, or campus buildings. The numbers are illustrative, not operational measurements. Use the supplied source unchanged and describe it as synthetic in your README.

## Scenario

A hypothetical campus monitors the number of occupied seats, study-space categories, noise levels, and room temperatures at several locations and times. Students should audit the observations, document cleaning decisions, and explore patterns. No model training is expected.

## Schema

| Column | Meaning | Notes |
| --- | --- | --- |
| `observation_id` | Logical observation identifier | Expected unique; keep as text |
| `space_id` | Hypothetical study-space identifier | A space appears repeatedly at multiple times; repeated space IDs are **not** duplicates |
| `space_type` | Type of space | Valid canonical labels: `quiet`, `group`, `computer`. Inputs use different spellings/cases/spaces; approved aliases in handout |
| `observed_date` | Observation date | ISO YYYY-MM-DD string |
| `hour` | Hour of day | Integer, 0–23 |
| `occupied_seats` | Observed occupied seats | Integer; from 0 to seat_capacity |
| `seat_capacity` | Space capacity | Positive integer |
| `noise_db` | Illustrative noise reading | Optional; valid if missing or within 20–100 |
| `temperature_c` | Illustrative temperature reading | Optional; valid if missing or within 15–40 |

`N/A` and blank numeric measurement cells represent missing measurements. For task validation, missing `noise_db` and `temperature_c` values may be retained; missing `occupied_seats` is **not** acceptable because occupancy is required to answer the main question. An unparseable non-missing numeric token should be flagged rather than silently interpreted as an ordinary missing measurement.

## Source-level sanity checks (for graders and students)

- Raw CSV: **46 rows**, **9 columns**.
- Exactly repeated complete row: **1** (the additional `OBS-010`).
- Additional ID collision with inconsistent values: **1** (the additional `OBS-012`).
- After exact-row deduplication and deterministic keep-first duplicate-ID handling: **44 distinct observation IDs**.
- Invalid unique observations that violate the stated rules: **8** (`OBS-037` through `OBS-044`).
- Valid unique observations: **36**.
- Missing optional `noise_db` among valid observations: **4**.
- Missing optional `temperature_c` among valid observations: **3**.
- `OBS-022` includes a **high but allowable noise reading of 94 dB**. `OBS-027` includes a **high but allowable temperature of 37 °C**; do not discard these only because they look unusual.

The counts above apply to the exact policies described in the instructor handout: **first occurrence wins** for an ID collision, full-row duplicates and key conflicts are logged separately, and invalid rows are excluded from the valid subset. If you design a *different* policy for exploration, preserve and document the difference rather than silently changing expected checks.

## Data ethics and reproducibility

- Preserve `data/study_space_observations.csv` unchanged.
- Never call these measurements real or infer anything about individual students.
- Put generated reports and plots in `outputs/`.
- Explain rejected records, missing-value treatment, and plot limitations in your own words.
