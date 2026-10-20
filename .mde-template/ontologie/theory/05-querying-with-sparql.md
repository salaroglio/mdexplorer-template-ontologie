---
title: Querying with SPARQL
---

# Querying with SPARQL

## TL;DR

SPARQL is the language for asking the graph questions: you write a pattern of triples with variables, and the graph
returns the values that make it true. Every competency question becomes a query in `ontology/queries/`, and the check
runs them all with the tool `rdf_check`, in memory, without Fuseki.

- `SELECT` returns a table of values, `ASK` returns yes or no.
- One `.rq` file per question, with the question's id in the name: `c1-open-calls.rq`.
- The check compares every answer with the expected one in `ontology/questions.md`.

## The idea in one line

SQL queries tables, SPARQL queries triples. In the `WHERE` clause you write triples as in Turtle, but with variables
(`?b`, `?code`): the result is every combination of values for which those triples exist.

## SELECT: a list

The question c1, «Which calls are open?».

```sparql
# c1 — Which calls are open?
PREFIX gara: <https://example.org/ontology/gara#>
SELECT ?code WHERE {
  ?b a gara:Bando ;
     gara:stato "aperto" ;
     gara:codice ?code .
}
ORDER BY ?code
```

It reads: «find every `?b` that is a Call, has status "aperto" (open) and has a code `?code`; return the codes». The SQL
analogy: `SELECT code FROM Calls WHERE status = 'open' ORDER BY code`, but without a `Calls` table.

## SELECT with a link

The question c4, «Which criteria block the evaluation?», with the criterion's label.

```sparql
# c4 — Which criteria block the evaluation of a call?
PREFIX gara: <https://example.org/ontology/gara#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?number ?criterion WHERE {
  ?c a gara:Criterio ;
     gara:blocca true ;
     gara:numero ?number ;
     rdfs:label ?criterion .
}
ORDER BY ?number
```

A link between two things is followed by writing the triple that joins them: `?c gara:valutatoDa ?r . ?r rdfs:label ?role .`
is SPARQL's JOIN.

## ASK: yes or no

The question c3, «Is the call NC-2027-014 in the register?».

```sparql
# c3 — Is the call NC-2027-014 in the call register?
PREFIX gara: <https://example.org/ontology/gara#>
ASK {
  ?b a gara:Bando ;
     gara:codice "NC-2027-014" ;
     gara:nelRegistro true .
}
```

`ASK` answers `true` if there is at least one way to make the triples true, `false` otherwise. Mind the open world:
`false` means «not found», not «false». If the expected answer is «no», the question must be thought through.

## Other useful pieces

| Piece | What it is for | Example |
|---|---|---|
| `FILTER` | a condition on the values | `FILTER(?deadline <= "2027-04-30"^^xsd:date)` |
| `OPTIONAL` | a triple that may be missing | `OPTIONAL { ?b gara:scadenzaOfferte ?d }` |
| `COUNT` | counting | `SELECT (COUNT(?b) AS ?howMany)` |
| `ORDER BY` | sorting | `ORDER BY ?code` |

## One query per question

| Rule | Why |
|---|---|
| one `.rq` file per question, in `ontology/queries/` | the check knows which question it is checking |
| a name with the id: `c1-open-calls.rq` | the id never changes, the name stays readable |
| first line: `# c1 — the question` | whoever opens the file knows what it asks |
| only the schema's words | a query using an undeclared property is an error of the schema or of the query |

The queries are written by the agent `ontologie-ontologist` together with the schema: they are the fastest way to
notice that the schema is missing something. If a question cannot be written as a query, the schema is not enough to
answer it.

## The check with `rdf_check`

`rdf_check` is an MCP tool of MdExplorer (group `agents`). It reads the data files (schema and examples), validates the
data with the SHACL rules and runs every query, all in memory. For every query it returns:

| Field | What it is |
|---|---|
| `kind` | `ask`, `select` or `graph` |
| `answer` | for an `ASK`: true or false |
| `variables`, `rows`, `rowCount` | for a `SELECT`: the columns, the first 50 rows, how many there are |
| `error` | the query cannot be read, and why |

If a data file cannot be read, the rules and the queries do not run: fix the file first. `rdf_check` does not know
whether an answer is the right one: the comparison with the expected answer is done by the agent `ontologie-checker`,
which writes `ontology/verification.md`, question by question: expected answer, obtained answer, passes or not.

A typical call: data `ontology/TBox` and `ontology/ABox`, rules `ontology/Shapes`, queries `ontology/queries`.

## Frequent mistakes

- A prefix in the query different from the data's: the query finds nothing, with no error.
- A text searched without its language when the data have one: `"open"` and `"open"@en` are different values.
- A query that answers «right» by chance because there are few examples: the examples must also cover the cases where
  the answer is empty or «no».
