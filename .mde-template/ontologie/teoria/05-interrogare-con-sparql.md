---
title: Interrogare con SPARQL
---

# Interrogare con SPARQL

## TL;DR

SPARQL è il linguaggio con cui si fanno domande al grafo: si scrive uno schema di triple con delle variabili, e il
grafo restituisce i valori che lo rendono vero. Ogni domanda di competenza diventa una query in `ontology/queries/`,
e la verifica le esegue tutte con lo strumento `rdf_check`, in memoria, senza Fuseki.

- `SELECT` restituisce una tabella di valori, `ASK` restituisce sì o no.
- Un file `.rq` per domanda, con l'id della domanda nel nome: `c1-bandi-aperti.rq`.
- La verifica confronta ogni risposta con quella attesa in `ontology/domande.md`.

## L'idea in una riga

SQL interroga tabelle, SPARQL interroga triple. Nella clausola `WHERE` si scrivono triple come in Turtle, ma con delle
variabili (`?b`, `?codice`): il risultato sono tutte le combinazioni di valori per cui quelle triple esistono.

## SELECT: un elenco

La domanda c1, «Quali bandi sono aperti?».

```sparql
# c1 — Quali bandi sono aperti?
PREFIX gara: <https://example.org/ontology/gara#>
SELECT ?codice WHERE {
  ?b a gara:Bando ;
     gara:stato "aperto" ;
     gara:codice ?codice .
}
ORDER BY ?codice
```

Si legge così: «trova ogni `?b` che è un Bando, ha stato "aperto" e ha un codice `?codice`; restituisci i codici».
L'analogia SQL: `SELECT codice FROM Bandi WHERE stato = 'aperto' ORDER BY codice`, ma senza tabella `Bandi`.

## SELECT con un collegamento

La domanda c4, «Quali criteri bloccano la valutazione?», con l'etichetta del criterio.

```sparql
# c4 — Quali criteri bloccano la valutazione di un bando?
PREFIX gara: <https://example.org/ontology/gara#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?numero ?criterio WHERE {
  ?c a gara:Criterio ;
     gara:blocca true ;
     gara:numero ?numero ;
     rdfs:label ?criterio .
}
ORDER BY ?numero
```

Un collegamento fra due cose si segue scrivendo la tripla che le unisce: `?c gara:valutatoDa ?r . ?r rdfs:label ?ruolo .`
è il JOIN di SPARQL.

## ASK: sì o no

La domanda c3, «Il bando NC-2027-014 è nel registro?».

```sparql
# c3 — Il bando NC-2027-014 è nel registro dei bandi?
PREFIX gara: <https://example.org/ontology/gara#>
ASK {
  ?b a gara:Bando ;
     gara:codice "NC-2027-014" ;
     gara:nelRegistro true .
}
```

`ASK` risponde `true` se esiste almeno un modo di rendere vere le triple, `false` altrimenti. Attenzione al mondo
aperto: `false` vuol dire «non risulta», non «è falso». Se la risposta attesa è «no», la domanda va pensata bene.

## Altri pezzi utili

| Pezzo | A che cosa serve | Esempio |
|---|---|---|
| `FILTER` | una condizione sui valori | `FILTER(?scadenza <= "2027-04-30"^^xsd:date)` |
| `OPTIONAL` | una tripla che può mancare | `OPTIONAL { ?b gara:scadenzaOfferte ?s }` |
| `COUNT` | contare | `SELECT (COUNT(?b) AS ?quanti)` |
| `ORDER BY` | ordinare | `ORDER BY ?codice` |

## Una query per domanda

| Regola | Perché |
|---|---|
| un file `.rq` per domanda, in `ontology/queries/` | la verifica sa quale domanda sta controllando |
| nome con l'id: `c1-bandi-aperti.rq` | l'id non cambia mai, il nome resta leggibile |
| prima riga: `# c1 — la domanda` | chi apre il file sa che cosa chiede |
| solo le parole dello schema | una query che usa una proprietà non dichiarata è un errore dello schema o della query |

Le query le scrive l'agente `ontologie-ontologo` insieme allo schema: sono il modo più rapido per accorgersi che allo
schema manca qualcosa. Se una domanda non si riesce a scrivere come query, lo schema non basta a rispondere.

## La verifica con `rdf_check`

`rdf_check` è uno strumento MCP di MdExplorer (gruppo `agents`). Legge i file di dati (schema ed esempi), valida i dati
con le regole SHACL ed esegue ogni query, tutto in memoria. Per ogni query restituisce:

| Campo | Che cos'è |
|---|---|
| `kind` | `ask`, `select` o `graph` |
| `answer` | per un `ASK`: vero o falso |
| `variables`, `rows`, `rowCount` | per un `SELECT`: le colonne, le prime 50 righe, quante sono |
| `error` | la query non si legge, e perché |

Se un file di dati non si legge, le regole e le query non girano: si corregge prima il file. `rdf_check` non sa se una
risposta è quella giusta: il confronto con la risposta attesa lo fa l'agente `ontologie-verificatore`, che scrive
`ontology/verifica.md`, domanda per domanda: risposta attesa, ottenuta, passa o no.

Una chiamata tipica: dati `ontology/TBox` e `ontology/ABox`, regole `ontology/Shapes`, query `ontology/queries`.

## Errori frequenti

- Un prefisso nella query diverso da quello dei dati: la query non trova niente, senza errori.
- Un testo cercato senza la lingua quando nei dati ce l'ha: `"aperto"` e `"aperto"@it` sono valori diversi.
- Una query che risponde «giusto» per caso perché gli esempi sono pochi: gli esempi devono coprire anche i casi in cui la risposta è vuota o «no».
