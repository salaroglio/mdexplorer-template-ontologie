---
mde_type: workflow
title: L'ontologia, dalle domande all'accettazione
workflow: ontologia.workflow.json
---

# L'ontologia, dalle domande all'accettazione

## TL;DR

Un'ontologia si disegna partendo dalle **domande a cui deve rispondere** (le domande di competenza): sono i suoi test di
accettazione. Da lì nascono lo schema con le sue regole, gli esempi e una verifica che risponde a ogni domanda; il
responsabile del dominio legge il rapporto e decide.

- Il responsabile del dominio lancia le domande di competenza e le approva; l'ontologo avvia lo schema e approva schema,
  esempi e verifica.
- Esempi e verifica partono da soli; la verifica usa lo strumento `rdf_check` di MdExplorer, senza bisogno di Fuseki.
- Uno schema o degli esempi rifiutati si rifanno quando la persona preme «Fai ripartire», senza un limite di volte.

## Il giro

```plantuml(@workflow, ./ontologia.workflow.json)
```

## Chi fa che cosa

| Passo | Chi lavora | Che cosa consegna | Chi approva |
|---|---|---|---|
| Domande di competenza | agente analista | `ontology/domande.md` | il responsabile del dominio |
| Schema, regole e query | agente ontologo | `ontology/TBox/`, `ontology/Shapes/`, `ontology/queries/` | l'ontologo |
| Esempi | agente ontologo | `ontology/ABox/` | l'ontologo |
| Verifica | agente verificatore | `ontology/verifica.md` | l'ontologo |
| Accetta l'ontologia | il responsabile del dominio | la decisione, nel registro dei giri | — |

Le persone vere stanno nel documento delle responsabilità del progetto.

## Parole

| Parola | Che cos'è | Un'analogia |
|---|---|---|
| TBox | lo schema: classi e proprietà | le classi C# |
| ABox | le istanze: i dati veri | gli oggetti |
| SHACL | le regole che i dati devono rispettare | FluentValidation |
| SPARQL | il linguaggio delle query | l'SQL dei grafi |
| Domanda di competenza | una domanda a cui l'ontologia deve saper rispondere | un test di accettazione |
