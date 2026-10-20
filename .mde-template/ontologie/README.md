---
title: Ontologie
---

# Ontologie

## TL;DR

Un metodo di lavoro per MdExplorer: disegnare un'ontologia partendo dalle domande a cui deve rispondere. Tre agenti
preparano domande, schema, regole, esempi e una verifica che risponde a ogni domanda; le persone approvano e decidono.

- Si sceglie in «Crea nuovo progetto» o si aggiunge a un progetto che c'è già; si aggiorna e ci si può staccare.
- Porta solo la sua cartella e la sua skill; i file del gruppo nascono una volta in `ontology/`.
- Con l'esempio, il progetto nasce con un'ontologia della gara di Nordica Crediti già fatta e verificata (dati inventati).

## Che cosa porta

| Che cosa | Dove |
|---|---|
| Il workflow | `workflows/ontologia.md` |
| Le schede dei tre agenti | `agents/` |
| I ruoli: Responsabile del dominio, Ontologo | il documento delle responsabilità del progetto, come ruoli aperti |
| Le istruzioni per gli agenti del progetto | un blocco in `CLAUDE.md`, `AGENTS.md`, `copilot-instructions.md` |
| I file iniziali del gruppo | `ontology/`, copiati una volta da `semi/` |
| La skill delle domande di competenza | `.claude/skills/ontologie-domande/` |
| La teoria | `teoria/`, copiata in `teoria/ontologie/` se la chiedi |

Per scrivere i file usa le skill di MdExplorer `mde-tbox`, `mde-abox` e `mde-shacl`, che si installano quando Fuseki è
acceso per il progetto (Impostazioni del progetto → Fuseki); la verifica usa lo strumento `rdf_check` di MdExplorer.

## Come si comincia

1. Accendi Fuseki per il progetto e riaprilo.
2. Chi risponde di un ruolo lo prende nel documento delle responsabilità (o con «È mio» nella città degli agenti).
3. Compila `ontology/dominio.md`.
4. Il responsabile del dominio lancia «Domande di competenza».
