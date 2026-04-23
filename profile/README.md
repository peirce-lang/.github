# Reckoner

**Reckoner is the intermediary step between finding a dataset, narrowing it down, and sending it where it needs to go next.**

For people who are experts in a domain but not in data — the lawyer, the cataloger, the researcher, the archivist, the collector. People who know exactly what they're looking for but currently have to ask someone else to write the query, wait, iterate, and still not quite get what they meant.

That middle step — narrowing a dataset to the exact set you mean, understanding why it contains what it contains, and handing it cleanly to whatever comes next — currently happens in Excel, or across four emails, or not at all.

Reckoner is that step. I sometimes think of it as a finishing school for data.

---

## What it does

You describe what you want across the dimensions that matter to your work — **WHO, WHAT, WHEN, WHERE, WHY, HOW**. Reckoner finds the exact set of records that match and shows you exactly why each one is in the set.

**A record collector querying a Discogs collection:**

```
WHO   artist   Elvis Presley ×
WHERE label    RCA Victor ×
```

Results: 24 records. Each result card shows exactly which coordinates matched. You didn't write SQL. You didn't export anything. You described what you wanted.

**The same surface, on legal billing data:**

```
WHO   attorney      Smith ×
WHAT  matter_type   advisory ×  OR  litigation ×
WHEN  year          2022 ×  —  2024 ×
```

Results: 47 matters. Same constraint surface. Same trace. Same export. Only the data changed.

The query surface doesn't know what substrate it's on. It just knows coordinates.

---

## The thing that makes it different

When you open a field, Reckoner shows you what's actually in your data — not what you wish were there.

*"The War And Treaty"* and *"The War and Treaty"* are two different values. One capital letter. Both count 1. Every collection has this. Most tools hide it. Reckoner surfaces it without you asking, groups the variants together, and lets you add all of them as OR conditions in one click.

This is what happens when a tool understands the *structure* of your data rather than just letting you query it.

---

## Named sets and set operations

A query result isn't ephemeral. You name it, save it as a `.rset.json` file, and reload it later. The file carries the full query, the substrate it ran against, the entity IDs, and a timestamp.

With two named sets you get set operations:

```
TS2018_2023        8 entities    (Taylor Swift, released 2018–2023)
TaylorThrough2020  5 entities    (Taylor Swift, released before 2021)

Diff        →  what's unique to each
Intersect   →  what they share
Union       →  everything across both
```

No SQL. No EXCEPT clauses. No pivot tables. Just two named sets and an operation.

Every export includes the full query record — what you asked, when, against which dataset. Not just a spreadsheet. A methodology artifact.

---

## How data gets in

Data enters through **SNF Model Builder** — a wizard that maps your source data (CSV, Excel, a database export) to the six-dimension coordinate structure that Reckoner queries. You map fields to dimensions once. After that the data is queryable.

The coordinate structure — **SNF (Semantic Normalized Form)** — is the underlying protocol. It works on PostgreSQL, DuckDB, SQL Server, and Apache Pinot. The same query surface runs on all of them. You don't migrate your data — you add a semantic layer on top of where it already lives.

Export any system to CSV. Translate to coordinates once. Query it on any substrate, forever. The source system schema becomes irrelevant. Vendor lock-in dissolves.

---

## The stack

```
Your data (CSV, database, API export)
        ↓
SNF Model Builder    maps fields → coordinates, handles messy data before ingest
        ↓
SNF substrate        coordinate store on PostgreSQL, DuckDB, SQL Server, or Pinot
        ↓
Reckoner             the workbench — constraints, sets, inspection, export
```

Each layer has a defined contract. Reckoner doesn't know what database you're on. The substrate is just the thing that answers "which entity IDs have this coordinate." Everything else is above it.

---

## Reckoner doesn't replace your other tools. It prepares your data for them.

Your BI tool, your spreadsheet, your reporting pipeline, your document management system — they're good at what they do. What none of them were designed to do is decide which records should be in scope in the first place.

That decision currently happens in Excel, or in someone's head, or across four emails between a domain expert and a data engineer trying to understand what they actually meant.

Reckoner is that step — and only that step.

Define the exact set that matters. Understand why it contains what it contains. Then hand it — cleanly, with full provenance — to whatever comes next. Your BI tool gets exactly the records it should be visualizing. Your export goes to OpenRefine with scope already defined. Your reporting pipeline gets a bounded, auditable working set instead of a full dump.

Everything downstream works better when the set going in is exactly right.

```
Reckoner defines the set
        ↓
Excel / Power BI / OpenRefine / your pipeline   does what it's good at
```

This is not a replacement for the tools you already have and trust.
It's the missing step before them that nobody built properly until now.

---

## Who it's for

**Data stewards and catalogers** who spend invisible time fixing quality problems their tools don't surface. Reckoner shows you where the data is inconsistent and lets you define quality rules as queries.

**Legal professionals** working document management systems, billing data, or case records. Multi-dimensional queries against matter data without rewriting SQL every time you change one condition.

**Researchers and archivists** who need repeatable, auditable working sets. Every saved set carries its full query provenance. Every export is a methodology record.

**Library technologists** building semantic query access over catalog data, digitized collections, or institutional repositories at any scale.

**Collectors and hobbyists** who have outgrown spreadsheets. Records, books, comics, board games, films, stamps — whatever the collection is. If it has structure and you've ever wanted to ask a multi-dimensional question about it without exporting to Excel, Reckoner is for you. No enterprise context required.

**Anyone** who has ever had to explain to a data engineer what they actually meant.

---

## See it running

[![Reckoner Demo](https://img.youtube.com/vi/mD-NVPNWL-o/maxresdefault.jpg)](https://youtu.be/mD-NVPNWL-o)

A record collection, a data quality catch, a diff between two named sets, and a substrate switch. 5 minutes. No setup required to watch.

---

## Licensing

| Component | License |
|---|---|
| SNF specification | MIT — fully open, no restrictions |
| Peirce query language | MIT — fully open, no restrictions |
| Peirce reference parser (`snf-peirce`) | MIT |
| Portolan planner | AGPL v3 |
| Reckoner workbench | AGPL v3 |

**Libraries, archives, museums, universities, and academic researchers: free, permanently.**

**Commercial use:** Enterprise licenses available for organizations that need commercial terms, private modifications, or SLA-backed support.

For enterprise licensing: [contact to be added before public release]

---

## Get started

```bash
pip install snf-peirce
```

→ [`snf-peirce` documentation](snf-peirce/README.md) — Python library, query syntax, Jupyter workflow

→ [SNF specification](SNF_README.md) — the coordinate model and routing algebra

→ [Peirce query language](docs/peirce.md) — full query syntax reference

→ [Running Reckoner](docs/reckoner.md) — the visual workbench, setup guide

---

*Resolve meaning once. Project everywhere.*
