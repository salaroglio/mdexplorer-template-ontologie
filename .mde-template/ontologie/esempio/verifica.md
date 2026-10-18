---
title: Verifica dell'ontologia della gara
---

# Verifica dell'ontologia della gara

## TL;DR

L'ontologia della gara risponde a tutte e cinque le domande di competenza con la risposta attesa; i tre file si leggono e
i dati rispettano tutte le regole. Il controllo l'ha fatto lo strumento `rdf_check` di MdExplorer, in memoria.

- 5 domande su 5 passano.
- Regole SHACL rispettate: nessuna violazione.
- Nessun file con errori.

## Esito

| Che cosa | Esito |
|---|---|
| File letti | 3 su 3 (`TBox/gara.ttl` 36 triple, `ABox/gara-nordica.ttl` 68, `Shapes/gara.shapes.ttl` 42) |
| Regole rispettate | sì |
| Domande che passano | 5 su 5 |

## Domanda per domanda

| Id | Domanda | Risposta attesa | Risposta ottenuta | Esito |
|---|---|---|---|---|
| c1 | Quali bandi sono aperti? | NC-2027-011, NC-2027-012, NC-2027-014 | NC-2027-011, NC-2027-012, NC-2027-014 | passa |
| c2 | Qual è la scadenza delle offerte del bando NC-2027-014? | 2027-04-30 | 2027-04-30 | passa |
| c3 | Il bando NC-2026-009 è già nel registro? | sì | vero | passa |
| c4 | Quali criteri bloccano la valutazione di un bando? | 1 Settore e natura, 5 Tempo per rispondere | 1 «Settore e natura», 5 «Tempo per rispondere» | passa |
| c5 | Chi valuta il criterio della competenza? | responsabile tecnico | «responsabile tecnico» | passa |

## Regole non rispettate

Nessuna.

## File che non si leggono

Nessuno.

## Come è stata fatta

Strumento `rdf_check` con data `ontology/TBox`, `ontology/ABox`; shapes `ontology/Shapes`; queries `ontology/queries`.
