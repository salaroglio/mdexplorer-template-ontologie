---
title: Modellare TBox e ABox
---

# Modellare TBox e ABox

## TL;DR

Lo schema (TBox) dichiara le classi e le proprietà che servono alle domande di competenza; gli esempi (ABox) sono le
istanze vere, prese dai documenti, che permettono di rispondere. Si scrivono in Turtle, un file per strato, ognuno
con il suo `.md` compagno, secondo le skill `mde-tbox` e `mde-abox`.

- Una classe è `owl:Class`, una proprietà con un valore è `owl:DatatypeProperty`, una che collega due cose è `owl:ObjectProperty`.
- Lo schema sta in `ontology/TBox/`, le istanze in `ontology/ABox/`, mai mescolati.
- Etichette (`rdfs:label`) e commenti (`rdfs:comment`) non sono un lusso: sono la documentazione dell'ontologia.

## Il formato: Turtle

Turtle scrive le triple come testo. Un punto chiude l'affermazione su un soggetto; un punto e virgola continua con lo
stesso soggetto; `a` vuol dire «è un» (`rdf:type`).

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .

gara:Bando a owl:Class .            # una tripla
gara:NC-2027-014 a gara:Bando ;     # due triple sullo stesso soggetto
    gara:stato "aperto" .
```

## Lo schema (TBox)

L'esempio della gara, ridotto. Ogni elemento c'è perché una domanda lo chiede.

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

gara:Bando       a owl:Class ; rdfs:label "Bando"@it .
gara:Committente a owl:Class ; rdfs:label "Committente"@it .
gara:Criterio    a owl:Class ; rdfs:label "Criterio di valutazione"@it .
gara:Ruolo       a owl:Class ; rdfs:label "Ruolo"@it .

gara:codice a owl:DatatypeProperty ; rdfs:domain gara:Bando ; rdfs:range xsd:string .
gara:stato  a owl:DatatypeProperty ; rdfs:domain gara:Bando ; rdfs:range xsd:string ;
    rdfs:comment "aperto o chiuso"@it .
gara:committente a owl:ObjectProperty ; rdfs:domain gara:Bando ; rdfs:range gara:Committente .

gara:blocca     a owl:DatatypeProperty ; rdfs:domain gara:Criterio ; rdfs:range xsd:boolean .
gara:valutatoDa a owl:ObjectProperty   ; rdfs:domain gara:Criterio ; rdfs:range gara:Ruolo .
```

| Elemento | Che cosa vuol dire | Analogia C# |
|---|---|---|
| `owl:Class` | un tipo di cosa | `class Bando` |
| `owl:DatatypeProperty` | un valore (testo, data, numero) | `public string Stato` |
| `owl:ObjectProperty` | un collegamento a un'altra cosa | `public Committente Committente` |
| `rdfs:domain` / `rdfs:range` | di chi è la proprietà, che cosa contiene | il tipo che la dichiara, il tipo del valore |
| `rdfs:subClassOf` | una classe più specifica | l'ereditarietà |

Attenzione all'analogia: `rdfs:domain` e `rdfs:range` non sono controlli. Non rifiutano un dato sbagliato; dicono che
cosa *si può dedurre* da un dato. I controlli si scrivono in SHACL.

## La gerarchia

Una sottoclasse eredita il significato della superclasse: ogni `gara:BandoEuropeo` è anche un `gara:Bando`.

```turtle
gara:BandoEuropeo a owl:Class ;
    rdfs:subClassOf gara:Bando ;
    rdfs:label "Bando sopra soglia europea"@it .
```

Si aggiunge una sottoclasse quando una domanda distingue davvero i due casi. Altrimenti basta una proprietà.

## Gli esempi (ABox)

Le istanze usano le classi e le proprietà dello schema, e un loro spazio di nomi, per convenzione
`urn:mde:abox:<nome-del-file>#`.

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix :     <urn:mde:abox:gara-nordica#> .

:nordica a gara:Committente ; rdfs:label "Nordica Crediti"@it .

:NC-2027-014 a gara:Bando ;
    gara:codice      "NC-2027-014" ;
    gara:stato       "aperto" ;
    gara:committente :nordica .

:criterio3 a gara:Criterio ;
    gara:blocca     true ;
    gara:valutatoDa :responsabileTecnico .
```

Ogni istanza viene da un documento: la fonte si dice nel `.md` compagno (o con PROV-O, se il gruppo lo vuole). Valori
tipati: numeri senza virgolette, `true`/`false`, date come `"2027-03-15"^^xsd:date`, testi con la lingua (`"…"@it`).

## I file, uno per strato

| File | Che cos'è | Skill |
|---|---|---|
| `ontology/TBox/<strato>.ttl` | classi e proprietà | `mde-tbox` |
| `ontology/TBox/<strato>.md` | descrizione, il `.ttl` incluso, come caricarlo su Fuseki | `mde-tbox` |
| `ontology/ABox/<insieme>.ttl` | le istanze | `mde-abox` |
| `ontology/ABox/<insieme>.md` | che cosa contiene, da quale documento, come caricarlo | `mde-abox` |

I nomi dei file sono in kebab-case (`gara.ttl`, `gara-nordica.ttl`). Le skill si installano nel progetto quando Fuseki
è acceso: vedi [Fuseki in MdExplorer](06-fuseki-in-mdexplorer.md).

## Errori frequenti

- **Istanze nello schema.** Il bando NC-2027-014 non va in `TBox/`: è un dato.
- **Classi senza domanda.** Se nessuna domanda la nomina, la classe non serve ancora.
- **`rdfs:domain` usato come vincolo.** Non rifiuta niente: per «ogni bando ha un committente» serve SHACL.
- **Prefissi diversi fra i file.** L'indirizzo base è quello di `ontology/dominio.md`, uguale ovunque.
- **Rinominare per disambiguare.** Due classi con una proprietà `nome` ciascuna: si usano spazi di nomi diversi
  (skill `mde-tbox`), non `nomeBando` e `nomeCommittente`.
- **Etichette mancanti.** Senza `rdfs:label` nessuno, persona o agente, capisce il grafo.

## Come controllare che il Turtle si legga

Lo strumento `rdf_check` di MdExplorer legge ogni file e dice riga e colonna di un errore, senza bisogno di Fuseki.
Al MarkAgent basta chiedere: «controlla con `rdf_check` `ontology/TBox` e `ontology/ABox`».
