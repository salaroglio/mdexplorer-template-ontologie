# CHANGELOG

## 2.0.0 — 2026-10-21
- ⚠️ incompatible: the template is in format 2 and needs MdExplorer 2026.10.21.2 or newer. Agents, roles and workflows arrive only with the agent city turned on; the skills live in the template's folder and MDE copies them into .claude/skills/. What to do: update MdExplorer before updating the template; without the city, turn it on from the project settings to get the agents.

## 1.4.0 — 2026-10-20
- The method speaks English too: the branches `en` (structure) and `example` (example), with agents, workflow, skill, starting files and theory in English. The two languages move in lockstep: the same version on every branch.

## 1.3.1 — 2026-10-20
- The presentation has the theory slide, in Italian and in English (the theory was in Italian only then).

## 1.3.0 — 2026-10-20
- The method's theory (`theory`): what an ontology is, starting from the questions, TBox and ABox, SHACL rules, SPARQL queries, Fuseki in MdExplorer, working with the MarkAgent. It is asked for when the project is created or downloaded later. Needs MdExplorer 2026.10.19.14 or newer.

## 1.2.0 — 2026-10-19
- The presentation in MdExplorer's style (theme `mde-space`): space landscapes at the opening and at the end, one icon per title; the English slides also have «How to start». Needs MdExplorer 2026.10.19.11 or newer.

## 1.1.0 — 2026-10-19
- The presentation for the «Create new project» showcase (`slides`), in Italian and in English: what the method is for, the round, the words, who does what. Needs MdExplorer 2026.10.19 or newer.

## 1.0.0 — 2026-10-18
- First version: the design of an ontology from the competency questions to the decision, checked with `rdf_check`.
- Three agents (analyst, ontologist, checker) and two roles (Domain owner, Ontologist).
- The group's starting files in `ontology/`; the skill `ontologie-questions`.
