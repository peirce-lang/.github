# peirce-lang

Semantic routing for meaning-aware data. Boolean queries over coordinates.

SNF (Semantic Normalized Form) is a data model that organizes facts by what
kind of thing they are — WHO, WHAT, WHEN, WHERE, WHY, HOW — and lets you
query across them without knowing the underlying schema or writing joins.

Peirce is the query language for SNF. Named after Charles Sanders Peirce,
whose triadic sign relation maps directly onto the SNF record structure.

---

## Repositories

| Repo | Language | What it is |
|---|---|---|
| [lens-tool](https://github.com/peirce-lang/lens-tool) | JavaScript | UI wizard + CLI for mapping CSVs into SNF and querying them. Start here. |
| [snf-peirce](https://github.com/peirce-lang/snf-peirce) | Python | Python runtime for SNF — lens authoring, data compilation, and Peirce queries. For data practitioners and Jupyter workflows. |
| [snf-lens](https://github.com/peirce-lang/snf-lens) | JavaScript | Lens authoring library — the core JS implementation of the lens format. |
| [peirce-cli](https://github.com/peirce-lang/peirce-cli) | JavaScript | Command-line interface for running Peirce queries against a compiled substrate. |

---

## The Peirce parser

The Peirce grammar is defined in the [language specification](https://github.com/peirce-lang/peirce-cli).
The canonical reference implementation is the JavaScript parser in
`lens-tool` (`peirce_parser.cjs`). It is MIT licensed.

Other language implementations (Python: `snf-peirce/parser.py`) exist as
conformant ports. A conformant implementation must produce identical
constraint objects for identical input. Cross-language conformance is
verified by `test_conformance.py` in the Python package — 37 tests
covering the full grammar.

**If your implementation diverges from the JS reference parser on valid
input, your implementation is non-conformant.**

### Operator precedence

AND binds more tightly than OR. `A AND B OR C AND D` parses as
`(A AND B) OR (C AND D)`.

Parentheses are supported to override this:

```
(WHO.role = "attorney" AND WHERE.office = "Seattle") OR (WHO.role = "partner" AND WHERE.office = "New York")
```

---

## Quick example

```
WHO.artist = "Miles Davis" AND WHEN.released BETWEEN "1955" AND "1965"
```

This query runs identically against any SNF-compliant substrate —
CSV, DuckDB, PostgreSQL, SQL Server, or Apache Pinot. The substrate
adapter absorbs all translation. From the query's perspective, there
is only the semantic coordinate space.

---

## License

SNF specification and Peirce language specification: MIT.
Peirce reference parser: MIT.
See individual repositories for component-level licensing.
