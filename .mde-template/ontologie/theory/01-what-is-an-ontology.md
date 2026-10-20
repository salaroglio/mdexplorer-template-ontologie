---
title: What an ontology is
---

# What an ontology is

## TL;DR

An ontology describes a domain as a graph of statements (triples) with global names (IRIs), a schema that says which
kinds of things exist and data that say which things are really there. It is worth it when several people, documents
and programs must answer the same questions with the same words; it is not worth it for a list to keep in order.

- A statement is a triple: subject, predicate, object.
- Every thing has an IRI, a name that holds everywhere, not only in one table.
- You always start from the questions the ontology must answer, not from the classes.

## The triple

Everything, in an ontology, is written as a **triple**: *subject — predicate — object*.

| Subject | Predicate | Object |
|---|---|---|
| the call NC-2027-014 | has as contracting authority | Nordica Crediti |
| the call NC-2027-014 | has as status | "open" |
| Nordica Crediti | is a | Contracting authority |

The object can be another thing (Nordica Crediti) or a value (the text "open", a date, a number). Together the triples
form a **graph**: the things are the nodes, the predicates are the arrows.

The .NET analogy: a triple is like a row of a table `(Id, Property, Value)`, but without tables. There is no table
`Calls` with its columns decided beforehand: there is a set of statements, and another one can be added to a thing
without changing any physical schema.

## IRIs

Every thing and every predicate has an **IRI**, a name in the form of a web address:
`https://example.org/ontology/gara#Bando`. The address does not need to open in a browser: it needs to be **unique**.
Not to write it out every time, a prefix is used: `gara:Bando` stands for `https://example.org/ontology/gara#Bando`.

The prefix and the base address of the group's ontology are in `ontology/domain.md`, in the table «Names», and must be
the same in every file. The analogy: it is the C# namespace, but global. Two groups using the same IRI talk about the
same thing; two using different IRIs talk about different things, even if the name is the same.

## Schema and data: TBox and ABox

| Part | What it says | Example | .NET analogy |
|---|---|---|---|
| TBox | which kinds of things exist and how they connect | there are Calls, every Call has a Contracting authority | C# classes |
| ABox | which things are really there | the call NC-2027-014, of Nordica Crediti | the objects |

The TBox changes rarely; the ABox grows with the work. In the project they live in different folders,
`ontology/TBox/` and `ontology/ABox/`. The document [Modelling TBox and ABox](03-modelling-tbox-and-abox.md) writes
them for real.

## An ontology is not a database schema

| | Database schema | Ontology |
|---|---|---|
| What holds | what is in the tables | what is stated; the rest is unknown |
| Names | local to the database | global (IRIs) |
| Changing the schema | a migration | triples are added |
| Rules on the data | column constraints | SHACL, written apart |

The most important difference is the first one. A database says «if a row is not there, it does not exist». An
ontology says «if a triple is not there, I do not know»: it is the so-called open world. That is why the rules the
data must respect («every call has a contracting authority») are not in the schema but in SHACL: see
[Rules with SHACL](04-rules-with-shacl.md).

## An ontology is not a glossary

A glossary says what a word means, for people. An ontology also says how things connect, in a way a program can query:
«which criteria block the evaluation of an open call, and who evaluates them?» is asked of the graph with a SPARQL
query and has a precise answer. A good glossary is often the first step: its words become the labels (`rdfs:label`)
of the classes.

## When it is worth it

- Several documents talk about the same things with different words, and the group wants one way to name them.
- There are recurring questions that today are answered by rereading the documents by hand.
- Agents and programs must use the same facts: the graph is a common, queryable source.
- The domain's rules must be checked on the data, not only written in a document.

## When it is not worth it

- You only need a list to keep in order: a markdown table is enough.
- Nobody can say which questions it should answer: the questions must be found first.
- The domain changes every week and nobody keeps the data up to date: the graph would soon be old.

## Where to start

Not from the classes. An ontology born from the classes soon becomes a list of words nobody uses. You start from the
**competency questions**: what the ontology must be able to answer. It is the next document,
[Starting from the questions](02-starting-from-the-questions.md).
