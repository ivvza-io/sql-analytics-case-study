# From Excel-Based Analysis to a Reproducible SQL Analytics Model

Designing a reusable, analytics-ready semantic layer to replace spreadsheet-based workflows and support downstream modeling and engineering decision-making.

---

## Overview

- **Domain:** Production, process, and quality data  
- **Focus:** Data modeling, SQL-based transformations, analytics readiness  
- **Stack:** PostgreSQL, SQL, Python (pandas)  
- **Outcome:** Reproducible, extensible analytics foundation replacing spreadsheet-based workflows  

**Documentation**
- Architecture → [`docs/architecture.md`](docs/architecture.md)

**Notebook**
- Semantic layer consumption example → [`notebooks/01_sql_semantic_layer_consumption.ipynb`](notebooks/01_sql_semantic_layer_consumption.ipynb)

---

## Role Within the Portfolio

This study case establishes the **analytical foundation** for all modeling and decision-support work documented in this portfolio.

The semantic layer designed here is reused directly in:
- chemistry-only modeling (Study Case 2),
- cross-system generalization (Study Case 3),
- variable influence screening (Study Case 4),
- and uncertainty-aware design tools (Study Case 5).

More broadly, the same foundation supported additional analytical and monitoring workflows in practice.  
However, this portfolio intentionally focuses on the subset of analyses related to **chemistry-driven modeling and engineering design decisions**, as these best illustrate the analytical reasoning and trade-offs behind the work.

SC1 is therefore not an isolated data engineering exercise, but a **reusable analytical system** designed to support multiple downstream use cases under consistent data semantics.

---

## Context & Problem

Critical production, process, and quality data were historically captured using Excel files and Google Sheets.

Each production area maintained its own spreadsheets, which were later used for:
- daily production monitoring,
- process variable tracking,
- Statistical Process Control (SPC),
- traceability analysis,
- ad-hoc exploratory analysis.

Over time, this approach became increasingly fragile.

Key issues included:
- More than **10 active workbooks** with **20+ sheets**
- Minimal validation at data entry
- Manual merges across files and formulas
- Performance degradation and file corruption
- Limited reproducibility and traceability

At this stage, the problem was no longer purely technical.  
**The workflow itself had become a bottleneck to reliable analysis.**

---

## Project Goal

The goal of this project was to design and implement a centralized PostgreSQL-based analytics model that would:

- Act as a single source of truth
- Enforce data consistency and validation early
- Support repeatable analytical consumption
- Eliminate spreadsheet-dependent workflows
- Serve as a stable foundation for downstream analytics and modeling

The objective was not simply to store data, but to build a **reusable analytical system**.

---

## Key Challenges

### Data inconsistency
Historical spreadsheet data contained duplicated records, mixed formats, and undocumented implicit rules.

### Data model design
The main challenge was conceptual: designing a structure that was:
- simple enough for daily use,
- flexible enough for multiple analytical perspectives,
- stable enough to evolve over time.

### Workflow transition
Moving from manual Excel-based analysis to a database-centric workflow required a shift in how data was consumed and reasoned about.

---

## Approach & Solution

The solution was implemented as a layered analytical architecture that separates ingestion, validation, semantic modeling, and analytical consumption.

The diagram below summarizes the overall data flow and responsibility boundaries:

```mermaid
flowchart LR
  A[Excel<br/>Google Sheets] --> B[Python pandas<br/>Extract and standardize]
  B --> C[CSV exports<br/>staging-ready]
  C --> D[(PostgreSQL)]
  D --> E[Semantic layer<br/>Analytics-ready views]
  E --> F[Python notebooks<br/>Analysis & modeling]
```
### Database as a Single Source of Truth

A PostgreSQL database was designed to act as the system’s analytical backbone, where:
- ingestion occurs under explicit constraints,
- transformations are centralized,
- and analytical outputs are consistent and traceable.

### Layered data model

The model is organized into:
- **raw** — minimally processed, ingestion-oriented tables  
- **clean** — validated and standardized representations  
- **semantic** — analytics-ready views optimized for consumption  

This separation prevents downstream correction logic from leaking into analysis.

---

## Core Data Modeling Decisions

- **Explicit heat-level grain**  
  All analytical outputs are anchored at heat level, enabling consistent joins across chemistry, process, and quality domains.

- **Separation of core entities and process domains**  
  Stable entities (e.g., heats, machines) are modeled independently from process-specific measurements.

- **Early validation and error prevention**  
  Duplicates, invalid formats, and constraint violations are detected during raw-to-clean transitions.

- **SQL-first transformation strategy**  
  Business logic and structural transformations are implemented in SQL to maximize reuse and transparency.

- **Selective use of Python**  
  Pandas is reserved for transformations that are cumbersome in SQL, such as complex reshaping for analysis.

- **Pragmatic scope control**  
  Only data required for current analytical use cases is modeled, avoiding unnecessary complexity.

---

## Data Ingestion Strategy

Data ingestion is handled via Python and pandas:

1. Read Excel / Sheets
2. Standardize formats and naming
3. Export staging-ready CSV files
4. Load into PostgreSQL raw tables
5. Apply SQL-based validation and transformations

This decouples extraction from business logic, keeping the database authoritative.

---

## Analytical Consumption

Once semantic views are defined, Python is used for analytical consumption:

- generation of heat-level analytical datasets,
- exploratory and statistical analysis,
- reproducible notebooks consuming stable SQL interfaces.

The included notebook demonstrates this interaction pattern rather than advanced analysis.

---

## Results & Impact

The implementation delivered measurable benefits:

- Elimination of spreadsheet dependency
- Increased data reliability through enforced constraints
- Removal of performance bottlenecks
- Reproducible analytical workflows
- Foundation for modeling and decision-support use cases documented in later study cases

---

## Lessons Learned

When a consistent and well-defined data model is established upfront, downstream analytical complexity decreases dramatically.

Data quality, reproducibility, and analytical velocity are direct consequences of disciplined data modeling.

---

## Closing

This study case represents a shift from file-based analysis to a **system-level analytical foundation**.

It is not an end in itself, but a prerequisite for all modeling and engineering decision tools built on top of it.

> By fixing data semantics early, all downstream analyses become comparable, repeatable, and defensible.

---