---
title: Fuseki in MdExplorer
---

# Fuseki in MdExplorer

## TL;DR

Apache Jena Fuseki è un triplestore: un server che tiene i grafi RDF e risponde alle query SPARQL. In MdExplorer si
accende per progetto (Impostazioni del progetto → scheda Fuseki), con l'indirizzo del server e un dataset; i file si
caricano con i comandi PowerShell eseguibili dei loro `.md` compagni, un named graph per file.

- Fuseki non serve per scrivere né per verificare l'ontologia: `rdf_check` lavora in memoria.
- Serve per avere l'ontologia del gruppo in un posto solo, interrogabile da persone, agenti e programmi.
- Con Fuseki acceso MdExplorer installa nel progetto le skill `mde-tbox`, `mde-abox` e `mde-shacl`.

## Che cos'è Fuseki

L'analogia .NET: Fuseki sta a un'ontologia come SQL Server sta a un database. I file `.ttl` sono come gli script di
creazione e di popolamento; Fuseki è il server dove vivono i dati e a cui si mandano le query. Un **dataset** è come un
database; un **named graph** è come una tabella, ma per triple: un contenitore con un nome dentro il dataset.

## Accenderlo per il progetto

1. Impostazioni del progetto → scheda **Fuseki** → «Abilita Fuseki (triplestore RDF) per questo progetto».
2. **URL del server**: il Fuseki da usare; il valore iniziale è `http://localhost:3030`.
3. **Nome dataset**: uno per progetto; se resta vuoto, MdExplorer usa il nome del progetto reso adatto a un URL.
4. Username e password solo se il server li chiede (restano sul tuo computer, la password cifrata).
5. «Test connessione»: dice se il server risponde e se il dataset esiste; se non esiste, «Crea dataset» lo crea.
6. Riapri il progetto: è all'apertura che MdExplorer installa le skill `mde-tbox`, `mde-abox` e `mde-shacl`.

Queste impostazioni sono di questo computer, non del gruppo: ognuno accende Fuseki per sé. Il metodo «Ontologie» non lo
accende da solo; se è spento, alla creazione del progetto una nota lo dice.

## Che cosa si carica, e come

Ogni file `.ttl` ha un `.md` compagno con una sezione «Carica su Fuseki»: un comando PowerShell che MdExplorer esegue
dal documento (si compilano due parametri, il dataset e l'indirizzo del server, e si preme Esegui). Il comando manda il
file a Fuseki con una richiesta `PUT` del Graph Store Protocol, nel suo named graph:

| File | Named graph |
|---|---|
| `ontology/TBox/<strato>.ttl` | `urn:mde:tbox:<strato>` |
| `ontology/ABox/<insieme>.ttl` | `urn:mde:abox:<insieme>` |
| `ontology/Shapes/<strato>.shapes.ttl` | `urn:mde:shapes:<strato>` |

Il `PUT` sostituisce tutto il contenuto del named graph: ricaricare un file non crea doppioni e non tocca gli altri.
L'ordine conta: prima lo schema, poi gli esempi. La skill `mde-tbox` prevede anche un `ontology/TBox/load-all.md` che
carica tutti gli strati in una volta.

Il caricamento non è automatico: un file cambiato su disco non arriva da solo su Fuseki, lo si ricarica con il suo
comando. Così si decide quando la versione su Fuseki cambia, per esempio dopo che l'ontologo ha approvato.

## Interrogarlo

- Dai `.md` compagni: la sezione «Verifica» ha una query SPARQL eseguibile che conferma che i dati sono arrivati.
- Dall'interfaccia di Fuseki, all'indirizzo del server: la pagina del dataset ha un editor di query.
- Da un agente o da un programma: l'endpoint `<server>/<dataset>/query` accetta SPARQL via HTTP.

Una query su un named graph preciso usa `FROM <urn:mde:abox:gara-nordica>` o `GRAPH <…> { … }`.

## Che cosa ci si guadagna

| Senza Fuseki | Con Fuseki |
|---|---|
| l'ontologia è un insieme di file nel repository | è anche un grafo vivo, in un posto solo |
| per rispondere si rileggono i file, o si usa `rdf_check` | si interroga con SPARQL, da qualunque strumento |
| ogni agente legge i file a modo suo | gli agenti usano gli stessi fatti, con le stesse query |
| i controlli girano quando li lanci | lo stesso grafo serve da riferimento a tutto il gruppo |

In pratica: le domande di competenza approvate diventano query che chiunque nel gruppo può rieseguire sul grafo
aggiornato, e un nuovo documento si traduce in nuove istanze caricate accanto alle vecchie, controllate con le stesse
regole.

## Che cosa non è

- **Non è il grafo del codice.** MdExplorer ha un suo database per programmi, file e simboli: si interroga con gli
  strumenti `code_*`, mai con SPARQL sul Fuseki del progetto.
- **Non è la verifica.** `rdf_check` controlla file, regole e query senza Fuseki; il giro del metodo usa quello.

## Se qualcosa non va

| Sintomo | Dove guardare |
|---|---|
| «Test connessione» fallisce | il server è acceso? l'URL ha la porta giusta? servono username e password? |
| il dataset non esiste | «Crea dataset» nella stessa scheda |
| le skill `mde-tbox`, `mde-abox`, `mde-shacl` non ci sono | Fuseki è acceso per il progetto? il progetto è stato riaperto dopo? |
| una query non trova niente | il file è stato caricato? il prefisso della query è quello dei dati? |
