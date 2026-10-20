---
title: Le regole con SHACL
---

# Le regole con SHACL

## TL;DR

SHACL scrive le regole che i dati devono rispettare: «ogni bando ha un codice solo», «lo stato è aperto o chiuso».
Lo schema dice che cosa esiste, SHACL dice che cosa è valido; una violazione dice quale dato, quale proprietà e quale
regola. Le regole stanno in `ontology/Shapes/`, secondo la skill `mde-shacl`.

- Una `sh:NodeShape` si applica a tutte le istanze di una classe (`sh:targetClass`).
- Ogni `sh:property` vincola una proprietà: quante volte, di che tipo, quali valori.
- Ogni regola ha il suo `sh:message`, nelle parole del gruppo: è ciò che si legge quando non è rispettata.

## Perché servono le regole

Nel documento [Che cos'è un'ontologia](01-che-cos-e-un-ontologia.md) si è visto il mondo aperto: se una tripla manca,
l'ontologia «non lo sa», non dice che è un errore. Per dire «qui manca qualcosa» servono regole scritte a parte.

L'analogia .NET è FluentValidation: le classi C# dicono com'è fatto un oggetto, i validator dicono quando un oggetto è
valido. SHACL sta all'ontologia come un validator sta a un DTO.

## Un esempio

Le regole del bando e del criterio, nel dominio della gara.

```turtle
@prefix gara: <https://example.org/ontology/gara#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .
@prefix sh:   <http://www.w3.org/ns/shacl#> .

gara:BandoShape a sh:NodeShape ;
  sh:targetClass gara:Bando ;
  sh:property [ sh:path gara:codice ; sh:minCount 1 ; sh:maxCount 1 ;
                sh:pattern "^NC-[0-9]{4}-[0-9]{3}$" ;
                sh:message "un bando ha un codice solo, nella forma NC-AAAA-NNN" ] ;
  sh:property [ sh:path gara:stato ; sh:minCount 1 ; sh:maxCount 1 ;
                sh:in ( "aperto" "chiuso" ) ;
                sh:message "lo stato di un bando è aperto o chiuso" ] ;
  sh:property [ sh:path gara:committente ; sh:minCount 1 ; sh:class gara:Committente ;
                sh:message "un bando ha il suo committente" ] .

gara:CriterioShape a sh:NodeShape ;
  sh:targetClass gara:Criterio ;
  sh:property [ sh:path gara:numero ; sh:minCount 1 ; sh:maxCount 1 ;
                sh:minInclusive 1 ; sh:maxInclusive 5 ;
                sh:message "i criteri sono numerati da 1 a 5" ] .
```

## I vincoli più usati

| Vincolo | Che cosa chiede | Analogia FluentValidation |
|---|---|---|
| `sh:minCount 1` | la proprietà c'è almeno una volta | `NotEmpty()` |
| `sh:maxCount 1` | al massimo una volta | una proprietà non lista |
| `sh:datatype xsd:date` | il valore è una data | il tipo della proprietà |
| `sh:in ( … )` | il valore è uno di questi | `IsInEnum()` |
| `sh:pattern "…"` | il testo rispetta l'espressione regolare | `Matches()` |
| `sh:minInclusive` / `sh:maxInclusive` | il numero sta nell'intervallo | `InclusiveBetween()` |
| `sh:class gara:Committente` | il collegamento punta a una cosa di quel tipo | `SetValidator()` sul figlio |

Cardinalità in breve: `1..1` è `sh:minCount 1 ; sh:maxCount 1`, `0..1` è solo `sh:maxCount 1`, `1..*` è solo
`sh:minCount 1`. Per regole più complesse (o questo o quello, confronti fra proprietà) la skill `mde-shacl` ha gli schemi.

## Come si legge una violazione

Una violazione dice quattro cose:

| Campo | Che cos'è | Esempio |
|---|---|---|
| `focusNode` | il dato che non va | `urn:mde:abox:gara-nordica#NC-2027-099` |
| `path` | la proprietà | `gara:stato` |
| `value` | il valore trovato, se c'è | `"sospeso"` |
| `message` | la regola, nelle parole del gruppo | «lo stato di un bando è aperto o chiuso» |

`rdf_check` restituisce le violazioni con questi campi (e la shape da cui vengono). Un rapporto senza violazioni dice
`conforms: true`. Per questo il `sh:message` va scritto bene: è la frase che il gruppo legge quando qualcosa non va.

## Che cosa si fa con una violazione

1. Si guarda la fonte del dato: il documento dice davvero «sospeso»?
2. Se il documento ha ragione, la regola è sbagliata o incompleta: si corregge la regola, e magari manca una domanda di competenza.
3. Se il documento ha torto o l'esempio è stato trascritto male, si corregge il dato.

Mai togliere una regola solo per far passare la verifica: la regola esiste perché il gruppo ha deciso che è vera.

## Dove stanno i file

| File | Che cos'è |
|---|---|
| `ontology/Shapes/<strato>.shapes.ttl` | le regole dello strato con lo stesso nome in `TBox/` |
| `ontology/Shapes/<strato>.shapes.md` | descrizione, il file incluso, come caricarlo e validare |

Le regole le scrive l'agente `ontologie-ontologo` insieme allo schema; le approva l'ontologo.

## Errori frequenti

- Regole senza `sh:message`: la violazione arriva, ma nessuno capisce perché.
- Regole che nessuna domanda giustifica: si controlla qualcosa che non interessa.
- Shape scritte come se fossero schema: classi e proprietà si dichiarano in `TBox/`, non qui.
