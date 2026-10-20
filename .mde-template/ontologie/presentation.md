---
document_type: slides
reveal:
  theme: mde-space
---

<!-- .slide: data-background-image="/mde-space/sfondi/orbite.svg" class="mde-sinistra" -->
<img class="mde-icona-grande" src="/mde-space/icone/pianeta.svg" alt="">

# Ontologies

Design an ontology starting from the **questions it must answer**,
and check it before accepting it.

---

## <img class="mde-icona" src="/mde-space/icone/telescopio.svg" alt=""> What it is for

*This method is in Italian: its documents, agents and workflow speak Italian.*

- An ontology born from classes soon becomes a list of words nobody uses
- Here you start from the **competency questions**: what the ontology must be able to say
- Each question becomes a query, and the check makes sure it has its answer

---

## <img class="mde-icona" src="/mde-space/icone/razzo.svg" alt=""> The round

1. **Competency questions** — what it must be able to answer
2. **Schema, rules and queries** — TBox, SHACL, SPARQL
3. **Examples** — real data in the ABox
4. **Check** — every question has its answer, no rule is broken
5. **Acceptance** — the domain owner decides

---

## <img class="mde-icona" src="/mde-space/icone/satellite.svg" alt=""> The words

| Word | What it is | Like… |
|---|---|---|
| TBox | the schema: classes and properties | C# classes |
| ABox | the instances, the real data | the objects |
| SHACL | the rules the data respects | FluentValidation |
| SPARQL | the query language | the SQL of graphs |

---

## <img class="mde-icona" src="/mde-space/icone/astronauta.svg" alt=""> Who does what

| Role | What they do |
|---|---|
| Domain owner | approves the questions and accepts the ontology |
| Ontologist | approves schema, rules, examples and check |

Three agents prepare (analyst, ontologist, checker), **the people approve and decide**.

---

## <img class="mde-icona" src="/mde-space/icone/base.svg" alt=""> What you find in the project

- `ontology/`: the domain to describe and the folders of schema, examples, rules and queries
- three agents and their roles, nobody's yet
- the competency-questions skill; the check uses MdExplorer's `rdf_check`
- **with the example**: the ontology of the Nordica Crediti tender, done and checked (invented data)

---

## <img class="mde-icona" src="/mde-space/icone/stella.svg" alt=""> The theory

If you ask for it, it lands in `teoria/ontologie/`: what an ontology is, starting from the questions,
TBox and ABox, SHACL rules, SPARQL queries, Fuseki in MdExplorer, working with the MarkAgent.

To read alone or with the MarkAgent, and it can also be downloaded later, from the project settings.

---

<!-- .slide: data-background-image="/mde-space/sfondi/luna.svg" class="mde-cielo" -->
## <img class="mde-icona" src="/mde-space/icone/razzo.svg" alt=""> How to start

1. Turn **Fuseki** on for the project (Project settings) and reopen it
2. Each one takes their role («It's mine»)
3. Fill in `ontology/dominio.md`
4. The domain owner starts «Domande di competenza»
