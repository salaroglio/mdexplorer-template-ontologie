---
title: Modelling TBox and ABox
---

# Modelling TBox and ABox

## TL;DR

The schema (TBox) declares the classes and properties the competency questions need; the examples (ABox) are the real
instances, taken from the documents, that make it possible to answer. They are written in Turtle, one file per layer,
each with its companion `.md`, following the skills `mde-tbox` and `mde-abox`.

- A class is `owl:Class`, a property with a value is `owl:DatatypeProperty`, one that links two things is `owl:ObjectProperty`.
- The schema lives in `ontology/TBox/`, the instances in `ontology/ABox/`, never mixed.
- Labels (`rdfs:label`) and comments (`rdfs:comment`) are not a luxury: they are the ontology's documentation.

## The format: Turtle

Turtle writes triples as text. A full stop closes the statement on a subject; a semicolon continues with the same
subject; `a` means «is a» (`rdf:type`).

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .

gara:Bando a owl:Class .            # one triple
gara:NC-2027-014 a gara:Bando ;     # two triples on the same subject
    gara:stato "aperto" .
```

The names of the example (`gara:Bando`, «call for tenders»; `gara:stato`, «status») come from an Italian domain: the
IRIs stay as they are, and the labels say in English what they mean.

## The schema (TBox)

The tender example, reduced. Every element is there because a question asks for it.

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

gara:Bando       a owl:Class ; rdfs:label "Call for tenders"@en .
gara:Committente a owl:Class ; rdfs:label "Contracting authority"@en .
gara:Criterio    a owl:Class ; rdfs:label "Evaluation criterion"@en .
gara:Ruolo       a owl:Class ; rdfs:label "Role"@en .

gara:codice a owl:DatatypeProperty ; rdfs:domain gara:Bando ; rdfs:range xsd:string .
gara:stato  a owl:DatatypeProperty ; rdfs:domain gara:Bando ; rdfs:range xsd:string ;
    rdfs:comment "open or closed"@en .
gara:committente a owl:ObjectProperty ; rdfs:domain gara:Bando ; rdfs:range gara:Committente .

gara:blocca     a owl:DatatypeProperty ; rdfs:domain gara:Criterio ; rdfs:range xsd:boolean .
gara:valutatoDa a owl:ObjectProperty   ; rdfs:domain gara:Criterio ; rdfs:range gara:Ruolo .
```

| Element | What it means | C# analogy |
|---|---|---|
| `owl:Class` | a kind of thing | `class Call` |
| `owl:DatatypeProperty` | a value (text, date, number) | `public string Status` |
| `owl:ObjectProperty` | a link to another thing | `public Authority Authority` |
| `rdfs:domain` / `rdfs:range` | whose property it is, what it contains | the type that declares it, the type of the value |
| `rdfs:subClassOf` | a more specific class | inheritance |

Careful with the analogy: `rdfs:domain` and `rdfs:range` are not checks. They do not reject wrong data; they say what
*can be inferred* from a piece of data. The checks are written in SHACL.

## The hierarchy

A subclass inherits the meaning of the superclass: every `gara:BandoEuropeo` is also a `gara:Bando`.

```turtle
gara:BandoEuropeo a owl:Class ;
    rdfs:subClassOf gara:Bando ;
    rdfs:label "Call above the European threshold"@en .
```

A subclass is added when a question really tells the two cases apart. Otherwise a property is enough.

## The examples (ABox)

The instances use the classes and properties of the schema, and a namespace of their own, by convention
`urn:mde:abox:<file-name>#`.

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix :     <urn:mde:abox:gara-nordica#> .

:nordica a gara:Committente ; rdfs:label "Nordica Crediti"@en .

:NC-2027-014 a gara:Bando ;
    gara:codice      "NC-2027-014" ;
    gara:stato       "aperto" ;
    gara:committente :nordica .

:criterio3 a gara:Criterio ;
    gara:blocca     true ;
    gara:valutatoDa :responsabileTecnico .
```

Every instance comes from a document: the source is stated in the companion `.md` (or with PROV-O, if the group wants
it). Typed values: numbers without quotes, `true`/`false`, dates as `"2027-03-15"^^xsd:date`, texts with their language
(`"…"@en`).

## The files, one per layer

| File | What it is | Skill |
|---|---|---|
| `ontology/TBox/<layer>.ttl` | classes and properties | `mde-tbox` |
| `ontology/TBox/<layer>.md` | description, the `.ttl` included, how to load it on Fuseki | `mde-tbox` |
| `ontology/ABox/<set>.ttl` | the instances | `mde-abox` |
| `ontology/ABox/<set>.md` | what it contains, from which document, how to load it | `mde-abox` |

File names are in kebab-case (`gara.ttl`, `gara-nordica.ttl`). The skills are installed in the project when Fuseki is
on: see [Fuseki in MdExplorer](06-fuseki-in-mdexplorer.md).

## Frequent mistakes

- **Instances in the schema.** The call NC-2027-014 does not go into `TBox/`: it is data.
- **Classes without a question.** If no question names it, the class is not needed yet.
- **`rdfs:domain` used as a constraint.** It rejects nothing: «every call has a contracting authority» needs SHACL.
- **Different prefixes across files.** The base address is the one of `ontology/domain.md`, the same everywhere.
- **Renaming to disambiguate.** Two classes with a property `name` each: use different namespaces (skill `mde-tbox`),
  not `callName` and `authorityName`.
- **Missing labels.** Without `rdfs:label` nobody, person or agent, understands the graph.

## How to check that the Turtle can be read

MdExplorer's tool `rdf_check` reads every file and gives the line and column of an error, with no need for Fuseki. Just
ask the MarkAgent: «check `ontology/TBox` and `ontology/ABox` with `rdf_check`».
