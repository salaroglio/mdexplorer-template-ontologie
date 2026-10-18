---
title: TBox — gara
---

# TBox — gara

## TL;DR

Lo schema della gara: quattro classi (bando, committente, criterio, ruolo) e le proprietà che servono a rispondere alle
cinque domande di competenza di `ontology/domande.md`, niente di più. 36 triple.

- Un bando ha codice, titolo, stato, scadenza delle offerte, se è nel registro e il suo committente.
- Un criterio ha il numero, se blocca la valutazione e chi lo valuta.
- Le regole che i dati devono rispettare stanno in `ontology/Shapes/gara.shapes.ttl`.

## Cosa contiene

| Classe | Proprietà |
|---|---|
| `gara:Bando` | `gara:codice`, `gara:titolo`, `gara:stato`, `gara:scadenzaOfferte`, `gara:nelRegistro`, `gara:committente` |
| `gara:Committente` | — |
| `gara:Criterio` | `gara:numero`, `gara:blocca`, `gara:valutatoDa` |
| `gara:Ruolo` | — |

## Verifica

Strumento `rdf_check` di MdExplorer: vedi `ontology/verifica.md`.
