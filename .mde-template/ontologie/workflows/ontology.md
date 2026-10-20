---
mde_type: workflow
title: The ontology, from the questions to the acceptance
workflow: ontology.workflow.json
---

# The ontology, from the questions to the acceptance

## TL;DR

An ontology is designed starting from the **questions it must answer** (the competency questions): they are its
acceptance tests. From them come the schema with its rules, the examples and a check that answers every question; the
domain owner reads the report and decides.

- The domain owner starts the competency questions and approves them; the ontologist starts the schema and approves
  schema, examples and check.
- Examples and check start by themselves; the check uses MdExplorer's tool `rdf_check`, with no need for Fuseki.
- A rejected schema or rejected examples are redone when the person presses «Restart», with no limit on the times.

## The round

```plantuml(@workflow, ./ontology.workflow.json)
```

## Who does what

| Step | Who works | What they deliver | Who approves |
|---|---|---|---|
| Competency questions | analyst agent | `ontology/questions.md` | the domain owner |
| Schema, rules and queries | ontologist agent | `ontology/TBox/`, `ontology/Shapes/`, `ontology/queries/` | the ontologist |
| Examples | ontologist agent | `ontology/ABox/` | the ontologist |
| Check | checker agent | `ontology/verification.md` | the ontologist |
| Accept the ontology | the domain owner | the decision, in the rounds register | — |

The real people are in the project's responsibilities document.

## Words

| Word | What it is | An analogy |
|---|---|---|
| TBox | the schema: classes and properties | C# classes |
| ABox | the instances: the real data | the objects |
| SHACL | the rules the data must respect | FluentValidation |
| SPARQL | the query language | the SQL of graphs |
| Competency question | a question the ontology must be able to answer | an acceptance test |
