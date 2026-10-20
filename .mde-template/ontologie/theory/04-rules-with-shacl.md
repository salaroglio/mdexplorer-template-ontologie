---
title: Rules with SHACL
---

# Rules with SHACL

## TL;DR

SHACL writes the rules the data must respect: «every call has exactly one code», «the status is open or closed». The
schema says what exists, SHACL says what is valid; a violation says which piece of data, which property and which rule.
The rules live in `ontology/Shapes/`, following the skill `mde-shacl`.

- A `sh:NodeShape` applies to every instance of a class (`sh:targetClass`).
- Every `sh:property` constrains one property: how many times, of which type, which values.
- Every rule has its `sh:message`, in the group's words: it is what you read when the rule is not respected.

## Why rules are needed

In the document [What an ontology is](01-what-is-an-ontology.md) you saw the open world: if a triple is missing, the
ontology «does not know», it does not say it is an error. To say «something is missing here» you need rules written apart.

The .NET analogy is FluentValidation: the C# classes say what an object looks like, the validators say when an object
is valid. SHACL is to the ontology what a validator is to a DTO.

## An example

The rules of the call and of the criterion, in the tender domain.

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
@prefix sh:   <http://www.w3.org/ns/shacl#> .

gara:BandoShape a sh:NodeShape ;
  sh:targetClass gara:Bando ;
  sh:property [ sh:path gara:codice ; sh:minCount 1 ; sh:maxCount 1 ;
                sh:pattern "^NC-[0-9]{4}-[0-9]{3}$" ;
                sh:message "a call has exactly one code, in the form NC-YYYY-NNN" ] ;
  sh:property [ sh:path gara:stato ; sh:minCount 1 ; sh:maxCount 1 ;
                sh:in ( "aperto" "chiuso" ) ;
                sh:message "the status of a call is open or closed" ] ;
  sh:property [ sh:path gara:committente ; sh:minCount 1 ; sh:class gara:Committente ;
                sh:message "a call has its contracting authority" ] .

gara:CriterioShape a sh:NodeShape ;
  sh:targetClass gara:Criterio ;
  sh:property [ sh:path gara:numero ; sh:minCount 1 ; sh:maxCount 1 ;
                sh:minInclusive 1 ; sh:maxInclusive 5 ;
                sh:message "the criteria are numbered from 1 to 5" ] .
```

(The values `"aperto"` and `"chiuso"` are the example's data, «open» and «closed» in Italian.)

## The most used constraints

| Constraint | What it asks | FluentValidation analogy |
|---|---|---|
| `sh:minCount 1` | the property is there at least once | `NotEmpty()` |
| `sh:maxCount 1` | at most once | a property that is not a list |
| `sh:datatype xsd:date` | the value is a date | the property's type |
| `sh:in ( … )` | the value is one of these | `IsInEnum()` |
| `sh:pattern "…"` | the text matches the regular expression | `Matches()` |
| `sh:minInclusive` / `sh:maxInclusive` | the number is in the range | `InclusiveBetween()` |
| `sh:class gara:Committente` | the link points to a thing of that type | `SetValidator()` on the child |

Cardinality in short: `1..1` is `sh:minCount 1 ; sh:maxCount 1`, `0..1` is just `sh:maxCount 1`, `1..*` is just
`sh:minCount 1`. For more complex rules (either this or that, comparisons between properties) the skill `mde-shacl` has
the patterns.

## How to read a violation

A violation says four things:

| Field | What it is | Example |
|---|---|---|
| `focusNode` | the piece of data that is wrong | `urn:mde:abox:gara-nordica#NC-2027-099` |
| `path` | the property | `gara:stato` |
| `value` | the value found, if there is one | `"sospeso"` |
| `message` | the rule, in the group's words | «the status of a call is open or closed» |

`rdf_check` returns the violations with these fields (and the shape they come from). A report without violations says
`conforms: true`. That is why the `sh:message` must be written well: it is the sentence the group reads when something
is wrong.

## What to do with a violation

1. Look at the source of the data: does the document really say «suspended»?
2. If the document is right, the rule is wrong or incomplete: fix the rule, and maybe a competency question is missing.
3. If the document is wrong or the example was transcribed badly, fix the data.

Never remove a rule only to make the check pass: the rule exists because the group decided it is true.

## Where the files are

| File | What it is |
|---|---|
| `ontology/Shapes/<layer>.shapes.ttl` | the rules of the layer with the same name in `TBox/` |
| `ontology/Shapes/<layer>.shapes.md` | description, the file included, how to load it and validate |

The rules are written by the agent `ontologie-ontologist` together with the schema; the ontologist approves them.

## Frequent mistakes

- Rules without `sh:message`: the violation arrives, but nobody understands why.
- Rules no question justifies: you check something nobody cares about.
- Shapes written as if they were schema: classes and properties are declared in `TBox/`, not here.
