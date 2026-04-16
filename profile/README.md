# peirce-lang

A semantic front door to your data.

SNF (Semantic Normalized Form) organizes facts by what kind of thing they are —
**WHO · WHAT · WHEN · WHERE · WHY · HOW** — and lets you query across them
without knowing the underlying schema or writing joins.

Peirce is the query language for SNF. Queries are Boolean operations over
semantic coordinates. AND is intersection. OR is union. The system finds the set.

---

## Get started

```bash
pip install snf-peirce
```

```python
import pandas as pd
from snf_peirce import suggest, compile_data, query

df       = pd.read_csv("my_collection.csv")
draft    = suggest(df)          # proposes dimension and semantic key for each field
# confirm or override any mapping, then:
lens     = draft.to_lens(lens_id="my_lens_v1", authority="me")
compiled = compile_data(df, lens)

query(compiled, 'WHO.artist = "Miles Davis" AND WHEN.released BETWEEN "1955" AND "1965"')
```

No server. No SQL. No schema knowledge required.

---

## How meaning gets assigned

When you ingest a new dataset, `snf-peirce` examines the data structure and
suggests which dimension and semantic key each field belongs to. You confirm
or override every mapping — nothing is assigned without your approval. The
meaning decision is always yours. The tool handles the encoding. No ML, no NLP.

The lens is a human-authored map from your fields to semantic coordinates,
captured once and reusable forever.

If you prefer a visual interface for the mapping step, `lens-tool` provides
a browser-based UI that lets you see the hub-and-spoke structure as you work.
The lens it produces is directly compatible with `snf-peirce`.

---
## Who this is for

A lot of people are locked out of their own data. Not because the data doesn't
exist — it does. Not because the questions aren't valid — they are. Because
the tools that answer those questions require syntax knowledge, database access,
or technical infrastructure that most people don't have and shouldn't need to
learn just to ask a question.

SNF and Peirce are for anyone on the wrong side of that wall.

- The hobbyist who exported their Discogs collection, Scryfall card list,
  Letterboxd history, or IMDB watchlist and wants to actually query it
- The domain expert who understands the data but doesn't control the database
  and can only ask what the application allows
- The citizen journalist or activist working with public records, exports, or
  scraped datasets who needs answers, not a SQL tutorial
- Anyone with messy, heterogeneous data from multiple sources who thinks in
  questions, not table joins

Map your data once. Ask questions in plain terms forever.

---
## How it works

Every fact becomes a coordinate:

```
DIM | semantic_key     | value

WHO  | author           | Macdonald, Ross
WHAT | subject_topic    | Private investigators
WHEN | publication_date | 1964
```

Peirce evaluates queries as Boolean operations over those coordinates.
The algebra is the same one that powers Lucene, Druid, and Apache Pinot.
The difference is that the coordinates are semantic — they describe meaning,
not text tokens.

---

## Query syntax

```python
# AND across dimensions — narrows results
query(compiled, 'WHO.artist = "Miles Davis" AND WHEN.released = "1959"')

# OR within a dimension — widens results
query(compiled, 'WHO.artist = "Miles Davis" OR WHO.artist = "John Coltrane"')

# Range
query(compiled, 'WHEN.released BETWEEN "1955" AND "1965"')

# Explore available fields
query(compiled, 'WHO|*')

# Explore values
query(compiled, 'WHO|artist|*')
```

The same query string runs identically against any SNF-compliant substrate —
CSV, DuckDB, PostgreSQL, SQL Server, or Apache Pinot. The substrate adapter
absorbs all translation.

---

## The algebra in plain sight

The `demo/` folder in `peirce-cli` contains a Roaring Bitmap proof of concept.
It builds a miniature SNF substrate entirely in memory using bitmap posting lists
and executes Peirce queries against it — showing every posting list, every union,
every intersection.

```bash
cd peirce-cli/demo
npm install
node peirce-bitmap-demo.mjs
```

If you work with search infrastructure you'll recognize immediately what you're
looking at. The SNF routing algebra is bitmap intersection. Not a metaphor —
the actual operation.

---

## Performance

Tested on a home machine — AMD Ryzen 9, 32GB RAM, consumer NVMe, Docker
container, default settings throughout. No tuning. No provisioned infrastructure.

| Query | Substrate | Dataset | Result |
|---|---|---|---|
| 5-constraint, correct dimension order | Apache Pinot | 8.9M court opinions | 42ms |
| 5-constraint, trap order (same query) | Apache Pinot | 8.9M court opinions | timeout (4.7s, aborted) |
| 3-constraint intersection | PostgreSQL | 1M documents | 281ms |

The timeout result is the point. A 5-dimensional query with the least selective
constraint evaluated first did not degrade — it failed to return at all. The
identical query in correct dimension order completed in 42ms. This is not a
performance difference. It is a categorical difference in whether the query
completes.

All results: single machine, single Docker container, no tuning, no independent
replication. Absolute millisecond numbers will improve on real infrastructure.
The ordering ratios are structural.

---

## Repositories

| Repo | What it is |
|---|---|
| [snf-peirce](https://github.com/peirce-lang/snf-peirce) | Python runtime — lens authoring, data compilation, Peirce queries. Primary interface. |
| [lens-tool](https://github.com/peirce-lang/lens-tool) | Browser UI for visual lens mapping. Optional — useful if you prefer to see the hub-and-spoke structure while authoring. |
| [snf-lens](https://github.com/peirce-lang/snf-lens) | Core JS lens authoring library. |
| [peirce-cli](https://github.com/peirce-lang/peirce-cli) | CLI for running Peirce queries. Contains the bitmap demo and the canonical reference parser. |

---

## The Peirce parser

The canonical reference implementation is the JavaScript parser in
`peirce-cli` (`peirce_parser.cjs`). It is MIT licensed.

The Python implementation in `snf-peirce` is a conformant port. Cross-language
conformance is verified by 37 tests covering the full grammar — same input
always produces identical constraint objects in both languages.

**Operator precedence:** AND binds more tightly than OR.
`A AND B OR C AND D` parses as `(A AND B) OR (C AND D)`.
Parentheses override this in the usual way.

---

## About

Associate's degree — had to choose between finishing school and eating.

Started in a law firm mailroom on weekends. 25 years in legal records

Self-taught SQL. The only person in the firm who could query the data.

Built SNF, Peirce, and Reckoner anyway.

Built with metaphor, isomorphism, and AI.

---

## License

SNF specification, Peirce language specification, and reference parser: MIT.
See individual repositories for component-level licensing.
