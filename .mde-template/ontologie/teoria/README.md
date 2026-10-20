---
title: La teoria delle ontologie
---

# La teoria delle ontologie

## TL;DR

Questi documenti insegnano il metodo «Ontologie»: che cos'è un'ontologia, come si parte dalle domande a cui deve
rispondere, come si scrivono schema, regole, esempi e query, e che cosa cambia quando l'ontologia sta su Fuseki.
Si leggono in ordine, da soli o facendosi guidare dal MarkAgent.

- Sono documenti per imparare: il lavoro vero sta in `ontology/`, e lo preparano gli agenti del metodo.
- Ogni documento usa lo stesso esempio, una gara d'appalto, e le analogie con .NET quando aiutano.
- Il gruppo li può correggere e arricchire: da quando sono stati copiati qui sono del gruppo.

## L'ordine di lettura

| # | Documento | Che cosa insegna |
|---|---|---|
| 1 | [Che cos'è un'ontologia](01-che-cos-e-un-ontologia.md) | triple, IRI, quando conviene e quando no |
| 2 | [Partire dalle domande](02-partire-dalle-domande.md) | le domande di competenza come test di accettazione |
| 3 | [Modellare TBox e ABox](03-modellare-tbox-e-abox.md) | classi, proprietà, istanze, in Turtle |
| 4 | [Le regole con SHACL](04-regole-con-shacl.md) | i vincoli che i dati devono rispettare |
| 5 | [Interrogare con SPARQL](05-interrogare-con-sparql.md) | una query per domanda, e la verifica |
| 6 | [Fuseki in MdExplorer](06-fuseki-in-mdexplorer.md) | accendere Fuseki, caricare, interrogare |
| 7 | [Lavorare con il MarkAgent](07-lavorare-con-il-markagent.md) | che cosa chiedere, chi fa che cosa |

Chi ha fretta legge 1, 2 e 7: bastano per capire il giro e farlo partire. Gli altri servono a chi approva schema,
regole ed esempi, cioè a chi deve giudicare se il lavoro degli agenti è giusto.

## Le parole, in una tabella

| Parola | Che cos'è | Un'analogia .NET |
|---|---|---|
| TBox | lo schema: classi e proprietà | le classi C# |
| ABox | le istanze: i dati veri | gli oggetti |
| SHACL | le regole che i dati devono rispettare | FluentValidation |
| SPARQL | il linguaggio delle query | l'SQL dei grafi |
| Domanda di competenza | una domanda a cui l'ontologia deve saper rispondere | un test di accettazione |
| Turtle | il formato di testo in cui si scrivono le triple | il JSON dei grafi |

## Come usarli con il MarkAgent

Il MarkAgent legge questi file come li leggi tu. Qualche richiesta che funziona:

- «Leggi `teoria/ontologie/02-partire-dalle-domande.md` e aiutami a scrivere tre domande di competenza per il mio dominio.»
- «Spiegami la differenza fra TBox e ABox con un esempio preso dai miei documenti.»
- «Controlla con `rdf_check` se `ontology/TBox` e `ontology/ABox` si leggono, e spiegami gli errori.»
- «Interrogami: fammi tre domande su SHACL e dimmi se ho capito.»

Il MarkAgent spiega e controlla; il giro vero dell'ontologia lo fanno gli agenti del metodo, e le persone approvano.
Vedi [Lavorare con il MarkAgent](07-lavorare-con-il-markagent.md).

## Dove sta che cosa nel progetto

| Cartella o file | Che cosa contiene |
|---|---|
| `ontology/dominio.md` | il dominio, i suoi confini, i documenti da cui partire |
| `ontology/domande.md` | le domande di competenza |
| `ontology/TBox/` | lo schema |
| `ontology/Shapes/` | le regole SHACL |
| `ontology/queries/` | una query SPARQL per domanda |
| `ontology/ABox/` | gli esempi |
| `ontology/verifica.md` | il rapporto di verifica |
| `teoria/ontologie/` | questi documenti |
