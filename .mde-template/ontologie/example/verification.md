---
title: Verification of the tender ontology
---

# Verification of the tender ontology

## TL;DR

The tender ontology answers all five competency questions with the expected answer; the three files can be read and the
data respect every rule. The check was done by MdExplorer's tool `rdf_check`, in memory.

- 5 questions out of 5 pass.
- SHACL rules respected: no violation.
- No file with errors.

## Outcome

| What | Outcome |
|---|---|
| Files read | 3 out of 3 (`TBox/tender.ttl` 36 triples, `ABox/nordica-tender.ttl` 68, `Shapes/tender.shapes.ttl` 42) |
| Rules respected | yes |
| Questions that pass | 5 out of 5 |

## Question by question

| Id | Question | Expected answer | Obtained answer | Outcome |
|---|---|---|---|---|
| c1 | Which calls are open? | NC-2027-011, NC-2027-012, NC-2027-014 | NC-2027-011, NC-2027-012, NC-2027-014 | passes |
| c2 | What is the offer deadline of the call NC-2027-014? | 2027-04-30 | 2027-04-30 | passes |
| c3 | Is the call NC-2026-009 already in the register? | yes | true | passes |
| c4 | Which criteria block the evaluation of a call? | 1 Sector and nature, 5 Time to respond | 1 «Sector and nature», 5 «Time to respond» | passes |
| c5 | Who evaluates the competence criterion? | technical lead | «technical lead» | passes |

## Rules not respected

None.

## Files that cannot be read

None.

## How it was done

Tool `rdf_check` with data `ontology/TBox`, `ontology/ABox`; shapes `ontology/Shapes`; queries `ontology/queries`.
