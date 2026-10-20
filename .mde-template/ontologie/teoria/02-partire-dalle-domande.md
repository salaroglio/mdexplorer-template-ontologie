---
title: Partire dalle domande
---

# Partire dalle domande

## TL;DR

Un'ontologia si disegna partendo dalle domande a cui deve saper rispondere: le domande di competenza. Sono i suoi test
di accettazione: lo schema contiene ciò che serve a rispondere, e niente di più; l'ontologia è finita quando ogni
domanda ha la risposta attesa.

- Una buona domanda è concreta, ha una risposta che si controlla, usa le parole del dominio e cita la fonte.
- Le domande stanno in `ontology/domande.md`, con un id che non cambia più (`c1`, `c2`…).
- Le scrive l'agente analista, le approva il responsabile del dominio.

## Perché le domande vengono prima

Pensa a un progetto .NET scritto partendo dai test di accettazione: prima si scrive che cosa il programma deve fare,
poi il codice che lo fa, e il programma è finito quando i test passano. Le domande di competenza fanno la stessa cosa
per un'ontologia.

Senza domande succedono due cose. Si modella troppo: classi che nessuno interroga, proprietà copiate da un documento
solo perché c'erano. E non si sa mai quando si è finito: manca il criterio per dire «basta così».

## Com'è fatta una buona domanda

| Regola | Sì | No |
|---|---|---|
| concreta | «Quali bandi sono aperti?» | «Che cosa sappiamo dei bandi?» |
| con una risposta che si controlla | sì/no, un elenco, un numero | «spiegami i bandi» |
| dalle parole del dominio | «Chi valuta il criterio 3?» | «Quali istanze di gara:Ruolo…» |
| con la fonte | il documento e la sezione da cui viene | nessuna fonte |
| una cosa per volta | due domande separate | «quali bandi sono aperti e chi li valuta?» |

La **risposta attesa** è quella vera secondo i documenti. Se i documenti non la danno, la risposta attesa è «da
chiarire»: non si inventa. Una domanda «da chiarire» è un'informazione preziosa, perché dice che cosa manca.

## Il formato di `ontology/domande.md`

Il formato lo dice la skill `ontologie-domande` del metodo. In breve:

- `## TL;DR`: tre righe e tre punti, scritto per ultimo.
- `## Le domande`: una tabella con id, domanda, tipo di risposta, risposta attesa, fonte.
- `## Fuori dalle domande`: ciò che il dominio contiene ma a cui nessuna domanda chiede di rispondere. Non entra
  nello schema.

Un esempio di tabella, sul dominio di una gara:

| id | domanda | risposta attesa | fonte |
|---|---|---|---|
| c1 | Quali bandi sono aperti? | NC-2027-014 | `bandi.md`, sezione «Aperti» |
| c3 | Il bando NC-2027-014 è nel registro? | sì | `registro-bandi.md` |
| c4 | Quali criteri bloccano la valutazione? | 1, 3 | capitolato, sezione 7 |

(La colonna «tipo di risposta» qui è omessa per stare larghi; nel file c'è.)

Da 5 a 15 domande per una prima versione. Gli id non cambiano più: una domanda tolta resta nella sezione «Tolte» con il
suo id, così le query e i rapporti vecchi restano leggibili.

## Da domanda a schema

Ogni domanda dice che cosa lo schema deve contenere. Si legge una domanda e ci si chiede: quali cose nomina? quali
collegamenti? quali valori?

| Domanda | Cose (classi) | Collegamenti e valori (proprietà) |
|---|---|---|
| c1 Quali bandi sono aperti? | Bando | stato, codice |
| c4 Quali criteri bloccano la valutazione? | Criterio | blocca, numero |
| c5 Chi valuta il criterio 3? | Criterio, Ruolo | valutatoDa |

Quello che non compare in nessuna domanda non entra nello schema. Se poi serve, si aggiunge prima la domanda.

## Da domanda a verifica

Ogni domanda diventa una query SPARQL in `ontology/queries/`, con l'id nel nome del file (`c1-bandi-aperti.rq`). La
verifica esegue tutte le query sugli esempi e confronta ogni risposta con quella attesa: è il momento in cui i test di
accettazione girano. Vedi [Interrogare con SPARQL](05-interrogare-con-sparql.md).

## Chi fa che cosa

| Passo | Chi |
|---|---|
| compilare `ontology/dominio.md` | il gruppo |
| scrivere le domande | l'agente `ontologie-analista`, dopo che il responsabile del dominio lancia il giro |
| approvarle o farle rifare | il responsabile del dominio |

Il responsabile può correggere le domande nella copia dell'agente prima di approvarle. Una domanda approvata è un
impegno: lo schema e la verifica si misurano su quella.

## Errori frequenti

- Domande scritte pensando già allo schema («quali istanze hanno la proprietà stato?»): vanno riscritte con le parole del gruppo.
- Troppe domande nella prima versione: meglio dieci buone che quaranta vaghe.
- Una risposta attesa presa a memoria invece che dal documento: senza fonte non si può verificare.
