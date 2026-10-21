---
title: Ontologies
---

# Ontologies

## TL;DR

A working method for MdExplorer: design an ontology starting from the questions it must answer. Three agents prepare
questions, schema, rules, examples and a check that answers every question; the people approve and decide.

- It is chosen in «Create new project» or added to an existing project; it can be updated and detached.
- It brings only its folder and its skill; the group's files are created once in `ontology/`.
- With the example, the project starts with an ontology of the Nordica Crediti tender, already done and checked (invented data).

## What it brings

| What | Where |
|---|---|
| The workflow | `workflows/ontology.md` |
| The cards of the three agents | `agents/` |
| The roles: Domain owner, Ontologist | the project's responsibilities document, as open roles |
| The instructions for the project's agents | a block in `CLAUDE.md`, `AGENTS.md`, `copilot-instructions.md` |
| The group's starting files | `ontology/`, copied once from `seeds/` |
| The competency-questions skill | `.claude/skills/ontologie-questions/` |
| The theory | `theory/`, copied into `theory/ontologie/` if you ask for it |

To write the files use MdExplorer's skills `mde-tbox`, `mde-abox` and `mde-shacl`, which are installed when Fuseki is
on for the project (Project settings → Fuseki); the check uses MdExplorer's tool `rdf_check`.

Agents, roles and workflows arrive only with the **agent city**: tick it in the «Create new project» showcase, or turn it on later from the project settings. Without it, the method brings its skills, starting files and theory.

## How to start

1. Turn Fuseki on for the project and open it again.
2. Whoever answers for a role takes it in the responsibilities document (or with «It's mine» in the agent city).
3. Fill in `ontology/domain.md`.
4. The domain owner starts «Competency questions».
