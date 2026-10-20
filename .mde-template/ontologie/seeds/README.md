---
title: The ontology
---

# The ontology

## TL;DR

This folder is the group's ontology: the domain, the questions it must answer, the schema, the rules, the examples, the
queries and the check. The method «Ontologies» created it once; from here on it belongs to the group and the method's
updates do not touch it.

- First of all fill in [the domain](domain.md): what is modelled, where it ends, which documents to start from.
- Then the domain owner starts the competency questions, in the
  [ontology workflow](../.mde-template/ontologie/workflows/ontology.md).
- The check does not need Fuseki; Fuseki is for loading the ontology and querying it from the other tools.

## What is here

| File or folder | What it is | Who writes it |
|---|---|---|
| `domain.md` | the domain and the documents to start from | the group |
| `questions.md` | the competency questions, with the expected answer | the analyst agent |
| `TBox/` | the schema: classes and properties | the ontologist agent |
| `Shapes/` | the SHACL rules | the ontologist agent |
| `queries/` | one SPARQL query per question | the ontologist agent |
| `ABox/` | the examples: the instances | the ontologist agent |
| `verification.md` | the report: every question with its answer | the checker agent |
