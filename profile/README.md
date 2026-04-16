# peirce-lang

A semantic front door to your data.

```bash
npm install -g snf-lens peirce-cli

snf-lens marc ./sample/lew_archer.mrc --out ./lew_archer.duckdb

peirce "WHO.author = 'Macdonald, Ross' AND WHEN.publication_date BETWEEN 1960 AND 1970" \
  --db ./lew_archer.duckdb
```

```
3 results  32ms

  Id                 Label                     Author          Year
  ─────────────────  ────────────────────────  ──────────────  ────
  marc:loc:8002131   Black money               Macdonald, Ross 1966
  marc:loc:10312533  The zebra-striped hearse  Macdonald, Ross 1962
  marc:loc:2640348   The chill                 Macdonald, Ross 1963
```

No server. No SQL. No schema knowledge required. Three commands from a MARC file to a semantic query returning exactly the right results from real Library of Congress records.

---

## What is Peirce?

Peirce is a semantic query language. It works on data organized as meaningful coordinates, so you can query directly by meaning instead of navigating tables and joins.

Data is expressed across six dimensions:

**WHO · WHAT · WHEN · WHERE · WHY · HOW**

Queries combine those dimensions using Boolean logic:

```bash
# AND across dimensions
peirce "WHAT.subject_topic = 'dragons' AND WHEN.publication_date >= '2000'" --db ./corpus.duckdb

# OR within a dimension
peirce "WHAT.genre = 'Fantasy' OR WHAT.genre = 'Science fiction'" --db ./corpus.duckdb

# Explore available fields
peirce "WHO|*" --db ./corpus.duckdb

# Explore values
peirce "WHO|author|*" --db ./corpus.duckdb
```

---

## How it works

SNF (Semantic Normalized Form) is the data model underneath. Every fact becomes a coordinate:

```
DIM|semantic_key|value
```

Example:

```
WHO|author|Macdonald, Ross
WHAT|subject_topic|Private investigators
WHEN|publication_date|1964
```

Peirce evaluates queries as Boolean operations over those coordinates. AND is intersection. OR is union. The system finds the set.

The algebra is the same one that powers Lucene, Druid, and Apache Pinot. The difference is that the coordinates are semantic — they describe meaning, not text tokens.

---

## The algebra in plain sight

The `demo/` folder in `peirce-cli` contains a Roaring Bitmap proof of concept. It builds a miniature SNF substrate entirely in memory using bitmap posting lists and executes Peirce queries against it — showing every posting list, every union, every intersection.

```bash
cd peirce-cli/demo
npm install
node peirce-bitmap-demo.mjs
```

If you work with search infrastructure you'll recognize immediately what you're looking at. The SNF routing algebra is bitmap intersection. Not a metaphor — the actual operation.

---

## What Peirce is (and isn't)

Peirce is:
- A semantic query language
- A way to query by meaning instead of structure
- A front door to meaning-aware data

Peirce is not:
- A database
- A replacement for SQL
- A natural language interface

---

## Packages

| Package | What it does | License |
|---------|-------------|---------|
| [peirce-cli](https://github.com/peirce-lang/peirce-cli) | Query any SNF substrate from the command line | MIT |
| [snf-lens](https://github.com/peirce-lang/snf-lens) | Translate your data into SNF substrates | MIT |

---

## Bring your own data

`snf-lens` translates MARC21 library catalog data today. The lens pattern is open — any domain with structured data can follow the same path. If you build a lens for your domain, consider contributing it back.

Candidates for community lenses: FHIR (healthcare), Open Dental, court records, museum collections, scientific literature.

---

## What's next

`peirce-cli` and `snf-lens` are the open entry point to a larger semantic architecture.

If you run this and want to know what comes next — open an issue or reach out. There's more.

---

## About

Associate's degree — had to choose between finishing school and eating. Started in a law firm mailroom on weekends. 25 years in legal records. Self-taught SQL. The only person in the firm who could query the data. Making under six figures after a quarter century doing work that analysts elsewhere would do.

Built SNF, Peirce, and Reckoner anyway.

Built with metaphor, isomorphism, and AI.

---

## License

`peirce-cli` and `snf-lens` are MIT licensed. Build whatever you want.
