# peirce-lang

A query language for data that has meaning.

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

## What this is

SNF (Semantic Normalized Form) is a data model and Boolean routing algebra. It organizes facts along six dimensions — **WHO WHAT WHEN WHERE WHY HOW** — and treats queries as set operations over those coordinates. AND narrows. OR widens. The system finds the set.

The algebra is the same one that powers Lucene, Druid, and Apache Pinot. The difference is that the coordinates are semantic — they describe meaning, not text tokens.

Peirce is the query language for that coordinate space. Named after Charles Sanders Peirce, whose triadic sign relation maps directly onto the SNF record structure.

---

## Packages

| Package | What it does | License |
|---------|-------------|---------|
| [peirce-cli](https://github.com/peirce-lang/peirce-cli) | Query any SNF substrate from the command line | MIT |
| [snf-lens](https://github.com/peirce-lang/snf-lens) | Translate your data into SNF substrates | MIT |

---

## Quick demo

The `snf-lens` package includes a sample dataset — 20 Lew Archer novels by Ross Macdonald from the Library of Congress. Run the three commands above and you have a working semantic substrate in under a minute.

Then try discovery — explore what's in the data without knowing the schema:

```bash
peirce "*" --db ./lew_archer.duckdb          # what dimensions have data?
peirce "WHO|*" --db ./lew_archer.duckdb      # what fields are in WHO?
peirce "WHO|author|*" --db ./lew_archer.duckdb  # what authors are there?
```

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

## Bring your own data

`snf-lens` translates MARC21 library catalog data today. The lens pattern is open — any domain with structured data can follow the same path. If you build a lens for your domain, consider contributing it back.

Candidates for community lenses: FHIR (healthcare), Open Dental, court records, museum collections, scientific literature.

---

## The larger stack

`peirce-cli` and `snf-lens` are the open entry point. For teams and production use, Reckoner is the full workbench — visual query builder, standing queries, data quality diagnostics, multi-substrate federation. The substrate format is identical. A file you build with `snf-lens` loads directly into Reckoner.

---

## About

Associate's degree — had to choose between finishing school and eating. Started in a law firm mailroom on weekends. 25 years in legal records. Self-taught SQL. The only person in the firm who could query the data. Making under six figures after a quarter century doing work that analysts elsewhere would do.

Built SNF, Peirce, and Reckoner anyway.

Built with metaphor, isomorphism, and AI.

---

## License

`peirce-cli` and `snf-lens` are MIT licensed. Build whatever you want.
