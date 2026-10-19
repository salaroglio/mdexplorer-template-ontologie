---
document_type: slides
reveal:
  theme: mde-space
---

<!-- .slide: data-background-image="/mde-space/sfondi/orbite.svg" class="mde-sinistra" -->
<img class="mde-icona-grande" src="/mde-space/icone/pianeta.svg" alt="">

# Ontologie

Disegnare un'ontologia partendo dalle **domande a cui deve rispondere**,
e verificarla prima di accettarla.

---

## <img class="mde-icona" src="/mde-space/icone/telescopio.svg" alt=""> A che cosa serve

- Un'ontologia nata dalle classi diventa presto un elenco di parole che nessuno usa
- Qui si parte dalle **domande di competenza**: ciò che l'ontologia deve saper dire
- Ogni domanda diventa una query, e la verifica controlla che abbia la sua risposta

---

## <img class="mde-icona" src="/mde-space/icone/razzo.svg" alt=""> Il giro

1. **Domande di competenza** — che cosa deve saper rispondere
2. **Schema, regole e query** — TBox, SHACL, SPARQL
3. **Esempi** — dati veri nella ABox
4. **Verifica** — ogni domanda ha la sua risposta, nessuna regola è violata
5. **Accettazione** — il responsabile del dominio decide

---

## <img class="mde-icona" src="/mde-space/icone/satellite.svg" alt=""> Le parole

| Parola | Che cos'è | Come… |
|---|---|---|
| TBox | lo schema: classi e proprietà | le classi C# |
| ABox | le istanze, i dati veri | gli oggetti |
| SHACL | le regole che i dati rispettano | FluentValidation |
| SPARQL | il linguaggio delle query | l'SQL dei grafi |

---

## <img class="mde-icona" src="/mde-space/icone/astronauta.svg" alt=""> Chi fa che cosa

| Ruolo | Che cosa fa |
|---|---|
| Responsabile del dominio | approva le domande e accetta l'ontologia |
| Ontologo | approva schema, regole, esempi e verifica |

Tre agenti preparano (analista, ontologo, verificatore), **le persone approvano e decidono**.

---

## <img class="mde-icona" src="/mde-space/icone/base.svg" alt=""> Che cosa trovi nel progetto

- `ontology/`: il dominio da descrivere e le cartelle di schema, esempi, regole e query
- tre agenti e i loro ruoli, ancora senza nome
- la skill delle domande di competenza; la verifica usa `rdf_check` di MdExplorer
- **con l'esempio**: l'ontologia della gara di Nordica Crediti, già fatta e verificata (dati inventati)

---

<!-- .slide: data-background-image="/mde-space/sfondi/luna.svg" class="mde-cielo" -->
## <img class="mde-icona" src="/mde-space/icone/razzo.svg" alt=""> Come si comincia

1. Accendi **Fuseki** per il progetto (Impostazioni del progetto) e riaprilo
2. Ognuno prende il suo ruolo («È mio»)
3. Compila `ontology/dominio.md`
4. Il responsabile del dominio lancia «Domande di competenza»
