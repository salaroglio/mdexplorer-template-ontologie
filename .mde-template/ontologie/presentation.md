---
document_type: slides
---

# Ontologies

Design an ontology starting from the **questions it must answer**,
and check it before accepting it.

*This method is in Italian: its documents, agents and workflow speak Italian.*

---

## What it is for

- An ontology born from classes soon becomes a list of words nobody uses
- Here you start from the **competency questions**: what the ontology must be able to say
- Each question becomes a query, and the check makes sure it has its answer

---

## The round

1. **Competency questions** — what it must be able to answer
2. **Schema, rules and queries** — TBox, SHACL, SPARQL
3. **Examples** — real data in the ABox
4. **Check** — every question has its answer, no rule is broken
5. **Acceptance** — the domain owner decides

---

## The words

| Word | What it is | Like… |
|---|---|---|
| TBox | the schema: classes and properties | C# classes |
| ABox | the instances, the real data | the objects |
| SHACL | the rules the data respects | FluentValidation |
| SPARQL | the query language | the SQL of graphs |

---

## Who does what

| Role | What they do |
|---|---|
| Domain owner | approves the questions and accepts the ontology |
| Ontologist | approves schema, rules, examples and check |

Three agents prepare (analyst, ontologist, checker), **the people approve and decide**.

---

## What you find in the project

- `ontology/`: the domain to describe and the folders of schema, examples, rules and queries
- three agents and their roles, nobody's yet
- the competency-questions skill; the check uses MdExplorer's `rdf_check`
- **with the example**: the ontology of the Nordica Crediti tender, done and checked (invented data)
