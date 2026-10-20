---
title: The theory of ontologies
---

# The theory of ontologies

## TL;DR

These documents teach the method «Ontologies»: what an ontology is, how to start from the questions it must answer,
how to write schema, rules, examples and queries, and what changes when the ontology lives on Fuseki. They are read in
order, alone or guided by the MarkAgent.

- They are documents to learn from: the real work is in `ontology/`, and the method's agents prepare it.
- Every document uses the same example, a call for tenders, and analogies with .NET when they help.
- The group can correct and enrich them: since they were copied here, they belong to the group.

## The reading order

| # | Document | What it teaches |
|---|---|---|
| 1 | [What an ontology is](01-what-is-an-ontology.md) | triples, IRIs, when it is worth it and when not |
| 2 | [Starting from the questions](02-starting-from-the-questions.md) | competency questions as acceptance tests |
| 3 | [Modelling TBox and ABox](03-modelling-tbox-and-abox.md) | classes, properties, instances, in Turtle |
| 4 | [Rules with SHACL](04-rules-with-shacl.md) | the constraints the data must respect |
| 5 | [Querying with SPARQL](05-querying-with-sparql.md) | one query per question, and the check |
| 6 | [Fuseki in MdExplorer](06-fuseki-in-mdexplorer.md) | turning Fuseki on, loading, querying |
| 7 | [Working with the MarkAgent](07-working-with-the-markagent.md) | what to ask, who does what |

Whoever is in a hurry reads 1, 2 and 7: they are enough to understand the round and to start it. The others are for
whoever approves schema, rules and examples, that is for whoever must judge whether the agents' work is right.

## The words, in a table

| Word | What it is | A .NET analogy |
|---|---|---|
| TBox | the schema: classes and properties | C# classes |
| ABox | the instances: the real data | the objects |
| SHACL | the rules the data must respect | FluentValidation |
| SPARQL | the query language | the SQL of graphs |
| Competency question | a question the ontology must be able to answer | an acceptance test |
| Turtle | the text format in which triples are written | the JSON of graphs |

## How to use them with the MarkAgent

The MarkAgent reads these files as you do. A few requests that work:

- «Read `theory/ontologie/02-starting-from-the-questions.md` and help me write three competency questions for my domain.»
- «Explain the difference between TBox and ABox with an example taken from my documents.»
- «Check with `rdf_check` whether `ontology/TBox` and `ontology/ABox` can be read, and explain the errors to me.»
- «Quiz me: ask me three questions on SHACL and tell me whether I understood.»

The MarkAgent explains and checks; the real round of the ontology is done by the method's agents, and the people
approve. See [Working with the MarkAgent](07-working-with-the-markagent.md).

## Where things are in the project

| Folder or file | What it contains |
|---|---|
| `ontology/domain.md` | the domain, its boundaries, the documents to start from |
| `ontology/questions.md` | the competency questions |
| `ontology/TBox/` | the schema |
| `ontology/Shapes/` | the SHACL rules |
| `ontology/queries/` | one SPARQL query per question |
| `ontology/ABox/` | the examples |
| `ontology/verification.md` | the verification report |
| `theory/ontologie/` | these documents |
