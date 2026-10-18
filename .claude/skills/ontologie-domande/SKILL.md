---
name: ontologie-domande
description: "Come si scrivono le domande di competenza di un'ontologia: le domande a cui deve saper rispondere, ciascuna con la risposta attesa e la fonte. Usala quando scrivi o rivedi ontology/domande.md, o quando ti chiedono a che cosa serve un'ontologia o se è finita."
---

# Le domande di competenza

Un'ontologia si disegna partendo dalle **domande a cui deve saper rispondere**. Sono i suoi test di accettazione: lo
schema contiene ciò che serve a rispondere, e niente di più; l'ontologia è finita quando ogni domanda ha la risposta attesa.

## Com'è fatta una buona domanda

- **Concreta**: «Quali bandi scadono entro 30 giorni?», non «Che cosa sappiamo dei bandi?».
- **Con una risposta che si controlla**: un sì/no, un elenco, un numero.
- **Dal dominio, non dalla tecnica**: le parole del gruppo, mai «quali istanze di gara:Bando…».
- **Con la fonte**: il documento (e la sezione) che rende la domanda sensata e da cui viene la risposta attesa.
- **Una cosa per volta**: due domande in una si dividono.

## Il formato di `ontology/domande.md`

- `## TL;DR`: tre righe e tre punti, scritto per ultimo.
- `## Le domande`: una tabella con **id** (`c1`, `c2`…, non cambiano più), **domanda**, **tipo di risposta** (sì/no,
  elenco, numero), **risposta attesa** (quella vera secondo i documenti, o «da chiarire»), **fonte**.
- `## Fuori dalle domande`: ciò che il dominio contiene ma a cui nessuna domanda chiede di rispondere: non entra nello schema.

Da 5 a 15 domande per una prima versione. Una domanda tolta in una versione successiva tiene il suo id nella sezione
«Tolte», così le query e i rapporti vecchi restano leggibili.
