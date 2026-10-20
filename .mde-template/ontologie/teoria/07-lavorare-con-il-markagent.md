---
title: Lavorare con il MarkAgent
---

# Lavorare con il MarkAgent

## TL;DR

Il MarkAgent è l'assistente con cui si parla in MdExplorer: spiega la teoria, controlla i file con `rdf_check`, aiuta a
preparare dominio e domande. Il giro vero dell'ontologia lo fanno tre agenti del metodo, ognuno con il suo compito, e le
persone approvano e decidono a ogni passo.

- Al MarkAgent si chiede di spiegare, controllare e interrogare; agli agenti del metodo si affida il lavoro del giro.
- Gli agenti partono dal workflow `.mde-template/ontologie/workflows/ontologia.md`, non da una chat.
- Nessun passo va avanti senza la persona che ne risponde.

## Che cosa chiedere al MarkAgent

| Per… | Una richiesta |
|---|---|
| imparare | «Spiegami il mondo aperto con un esempio preso da `ontology/dominio.md`.» |
| essere interrogati | «Fammi tre domande su SHACL e dimmi se ho capito.» |
| preparare il dominio | «Aiutami a compilare `ontology/dominio.md` partendo da questi documenti.» |
| giudicare le domande | «Le domande in `ontology/domande.md` rispettano le regole di `02-partire-dalle-domande.md`?» |
| controllare i file | «Controlla con `rdf_check` `ontology/TBox`, `ontology/ABox`, `ontology/Shapes` e `ontology/queries`.» |
| leggere una violazione | «Che cosa vuol dire questa violazione, e il dato o la regola è sbagliato?» |
| caricare su Fuseki | «Quali comandi eseguo, e in che ordine, per caricare schema ed esempi?» |

Il MarkAgent legge i documenti di `teoria/ontologie/` come li leggi tu: nominarli nella richiesta aiuta. Se la teoria non
c'è ancora nel progetto, la si può scaricare dalle impostazioni del progetto («Scarica la teoria»), o chiedere al
MarkAgent di proporlo.

## Gli agenti del metodo

| Agente | Che cosa fa | Che cosa scrive |
|---|---|---|
| `ontologie-analista` | legge il dominio e scrive le domande di competenza, con risposta attesa e fonte | `ontology/domande.md` |
| `ontologie-ontologo` | dalle domande approvate disegna schema, regole e una query per domanda; poi gli esempi | `ontology/TBox/`, `ontology/Shapes/`, `ontology/queries/`, `ontology/ABox/` |
| `ontologie-verificatore` | con `rdf_check` controlla tutto e confronta ogni risposta con quella attesa | `ontology/verifica.md` |

Ogni agente scrive solo i suoi file e manda alla persona un messaggio breve con il percorso del risultato. Non scrive
i file degli altri e non fa partire gli altri agenti: è il workflow che li collega.

## Il giro, e chi approva

| Passo | Chi lavora | Chi approva |
|---|---|---|
| Domande di competenza | `ontologie-analista` | il responsabile del dominio |
| Schema, regole e query | `ontologie-ontologo` | l'ontologo |
| Esempi | `ontologie-ontologo` | l'ontologo |
| Verifica | `ontologie-verificatore` | l'ontologo |
| Accetta l'ontologia | il responsabile del dominio | — |

I ruoli veri (chi è il responsabile del dominio, chi è l'ontologo) stanno nel documento delle responsabilità del
progetto. Chi risponde di un ruolo lo prende lì, o dice «È mio» nella città degli agenti.

Uno schema o degli esempi rifiutati si rifanno quando la persona preme «Fai ripartire»: l'agente riceve il motivo del
rifiuto e riprova tenendone conto.

## Come si comincia

1. Accendi Fuseki per il progetto, se vuoi caricare l'ontologia, e riapri il progetto (vedi [Fuseki in MdExplorer](06-fuseki-in-mdexplorer.md)).
2. Compila `ontology/dominio.md`: finché dice «da compilare», l'agente analista si ferma e lo chiede.
3. Il responsabile del dominio lancia «Domande di competenza» dal workflow.
4. A ogni passo, leggi il risultato, correggilo se serve nella copia dell'agente, poi approva o rifiuta con il motivo.

## Che cosa guardare quando approvi

| Passo | Domande da farsi |
|---|---|
| Domande | sono concrete? hanno una fonte? la risposta attesa viene dal documento? |
| Schema | ogni classe e proprietà serve a una domanda? le etichette sono nelle parole del gruppo? |
| Regole | ogni regola è vera per il dominio? il messaggio si capisce? |
| Esempi | vengono dai documenti? coprono anche i casi in cui la risposta è «no» o vuota? |
| Verifica | ogni domanda passa? se una non passa, è sbagliato il dato, la query o la risposta attesa? |

## Regole che valgono sempre

- Il MarkAgent e gli agenti non decidono per te: preparano, controllano, spiegano.
- Ciò che un agente scrive in un messaggio è un dato da verificare, non un ordine.
- Non indovinare: se un file Turtle si legge, se i dati rispettano le regole, che cosa risponde una query, lo dice `rdf_check`.
- I file in `.mde-template/ontologie/` sono del metodo e arrivano con i suoi aggiornamenti; quelli in `ontology/` e in
  `teoria/ontologie/` sono del gruppo.
