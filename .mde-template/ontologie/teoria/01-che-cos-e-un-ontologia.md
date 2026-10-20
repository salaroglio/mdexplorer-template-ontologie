---
title: Che cos'è un'ontologia
---

# Che cos'è un'ontologia

## TL;DR

Un'ontologia descrive un dominio come un grafo di affermazioni (triple) con nomi globali (IRI), uno schema che dice
quali tipi di cose esistono e i dati che dicono quali cose ci sono davvero. Conviene quando più persone, documenti e
programmi devono rispondere alle stesse domande con le stesse parole; non conviene per un elenco da tenere in ordine.

- Un'affermazione è una tripla: soggetto, predicato, oggetto.
- Ogni cosa ha un IRI, un nome che vale ovunque, non solo in una tabella.
- Si comincia sempre dalle domande a cui l'ontologia deve rispondere, non dalle classi.

## La tripla

Tutto, in un'ontologia, è scritto come **tripla**: *soggetto — predicato — oggetto*.

| Soggetto | Predicato | Oggetto |
|---|---|---|
| il bando NC-2027-014 | ha come committente | Nordica Crediti |
| il bando NC-2027-014 | ha come stato | "aperto" |
| Nordica Crediti | è un | Committente |

L'oggetto può essere un'altra cosa (Nordica Crediti) o un valore (il testo "aperto", una data, un numero). Le triple
insieme formano un **grafo**: le cose sono i nodi, i predicati sono le frecce.

L'analogia .NET: una tripla è come una riga di una tabella `(Id, Proprietà, Valore)`, ma senza tabelle. Non c'è una
tabella `Bandi` con le sue colonne decise prima: c'è un insieme di affermazioni, e a una cosa se ne può aggiungere
un'altra senza cambiare nessuno schema fisico.

## Gli IRI

Ogni cosa e ogni predicato hanno un **IRI**, un nome nella forma di un indirizzo web:
`https://example.org/ontology/gara#Bando`. Non serve che l'indirizzo si apra nel browser: serve che sia **unico**. Per
non riscriverlo ogni volta si usa un prefisso: `gara:Bando` vale `https://example.org/ontology/gara#Bando`.

Il prefisso e l'indirizzo base dell'ontologia del gruppo stanno in `ontology/dominio.md`, nella tabella «Nomi», e
devono essere uguali in tutti i file. L'analogia: è il namespace C#, ma globale. Due gruppi che usano lo stesso IRI
parlano della stessa cosa; due che usano IRI diversi parlano di cose diverse, anche se il nome è uguale.

## Schema e dati: TBox e ABox

| Parte | Che cosa dice | Esempio | Analogia .NET |
|---|---|---|---|
| TBox | quali tipi di cose esistono e come si collegano | esistono i Bandi, ogni Bando ha un Committente | le classi C# |
| ABox | quali cose ci sono davvero | il bando NC-2027-014, di Nordica Crediti | gli oggetti |

La TBox cambia di rado; la ABox cresce con il lavoro. Nel progetto stanno in cartelle diverse, `ontology/TBox/` e
`ontology/ABox/`. Il documento [Modellare TBox e ABox](03-modellare-tbox-e-abox.md) le scrive davvero.

## Un'ontologia non è uno schema di database

| | Schema di database | Ontologia |
|---|---|---|
| Che cosa vale | ciò che sta nelle tabelle | ciò che è affermato; il resto non si sa |
| Nomi | locali al database | globali (IRI) |
| Cambiare lo schema | migrazione | si aggiungono triple |
| Regole sui dati | vincoli delle colonne | SHACL, scritte a parte |

La differenza più importante è la prima. Un database dice «se una riga non c'è, non esiste». Un'ontologia dice «se
una tripla non c'è, non lo so»: è il cosiddetto mondo aperto. Per questo le regole che i dati devono rispettare
(«ogni bando ha un committente») non stanno nello schema, ma in SHACL: vedi [Le regole con SHACL](04-regole-con-shacl.md).

## Un'ontologia non è un glossario

Un glossario dice che cosa vuol dire una parola, per le persone. Un'ontologia dice anche come le cose si collegano,
in un modo che un programma può interrogare: «quali criteri bloccano la valutazione di un bando aperto, e chi li
valuta?» si chiede al grafo con una query SPARQL e ha una risposta precisa. Un buon glossario è spesso il primo passo:
le sue parole diventano le etichette (`rdfs:label`) delle classi.

## Quando conviene

- Più documenti parlano delle stesse cose con parole diverse, e il gruppo vuole un solo modo di nominarle.
- Ci sono domande ricorrenti a cui oggi si risponde rileggendo i documenti a mano.
- Agenti e programmi devono usare gli stessi fatti: il grafo è una fonte comune, interrogabile.
- Le regole del dominio vanno controllate sui dati, non solo scritte in un documento.

## Quando non conviene

- Serve solo un elenco da tenere in ordine: basta una tabella in markdown.
- Nessuno sa dire a quali domande dovrebbe rispondere: prima vanno trovate le domande.
- Il dominio cambia ogni settimana e nessuno tiene aggiornati i dati: il grafo diventerebbe vecchio presto.

## Da dove si parte

Non dalle classi. Un'ontologia nata dalle classi diventa presto un elenco di parole che nessuno usa. Si parte dalle
**domande di competenza**: ciò che l'ontologia deve saper rispondere. È il prossimo documento,
[Partire dalle domande](02-partire-dalle-domande.md).
