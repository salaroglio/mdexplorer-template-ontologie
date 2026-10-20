---
title: TBox — tender
---

# TBox — tender

## TL;DR

The tender schema: four classes (call, contracting authority, criterion, role) and the properties needed to answer the
five competency questions of `ontology/questions.md`, nothing more. 36 triples.

- A call has code, title, status, offer deadline, whether it is in the register and its contracting authority.
- A criterion has its number, whether it blocks the evaluation and who evaluates it.
- The rules the data must respect are in `ontology/Shapes/tender.shapes.ttl`.

## What it contains

| Class | Properties |
|---|---|
| `gara:Bando` (call) | `gara:codice`, `gara:titolo`, `gara:stato`, `gara:scadenzaOfferte`, `gara:nelRegistro`, `gara:committente` |
| `gara:Committente` (contracting authority) | — |
| `gara:Criterio` (criterion) | `gara:numero`, `gara:blocca`, `gara:valutatoDa` |
| `gara:Ruolo` (role) | — |

The IRIs come from an Italian domain and stay as they are; the labels say in English what they mean.

## Check

MdExplorer's tool `rdf_check`: see `ontology/verification.md`.
