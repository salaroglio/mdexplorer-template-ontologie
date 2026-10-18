---
title: L'ontologia
---

# L'ontologia

## TL;DR

Questa cartella è l'ontologia del gruppo: il dominio, le domande a cui deve rispondere, lo schema, le regole, gli esempi,
le query e la verifica. Il metodo «Ontologie» l'ha creata una volta; da qui in poi è del gruppo e gli aggiornamenti del
metodo non la toccano.

- Prima di tutto compila [il dominio](dominio.md): che cosa si modella, dove finisce, da quali documenti partire.
- Poi il responsabile del dominio lancia le domande di competenza, nel
  [workflow dell'ontologia](../.mde-template/ontologie/workflows/ontologia.md).
- La verifica non ha bisogno di Fuseki; Fuseki serve per caricare l'ontologia e interrogarla dagli altri strumenti.

## Che cosa c'è

| File o cartella | Che cos'è | Chi lo scrive |
|---|---|---|
| `dominio.md` | il dominio e i documenti da cui partire | il gruppo |
| `domande.md` | le domande di competenza, con la risposta attesa | l'agente analista |
| `TBox/` | lo schema: classi e proprietà | l'agente ontologo |
| `Shapes/` | le regole SHACL | l'agente ontologo |
| `queries/` | una query SPARQL per domanda | l'agente ontologo |
| `ABox/` | gli esempi: le istanze | l'agente ontologo |
| `verifica.md` | il rapporto: ogni domanda con la sua risposta | l'agente verificatore |
