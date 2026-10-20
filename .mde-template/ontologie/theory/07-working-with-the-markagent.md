---
title: Working with the MarkAgent
---

# Working with the MarkAgent

## TL;DR

The MarkAgent is the assistant you talk to in MdExplorer: it explains the theory, checks the files with `rdf_check`,
helps prepare the domain and the questions. The real round of the ontology is done by the method's three agents, each
with its own task, and the people approve and decide at every step.

- You ask the MarkAgent to explain, check and quiz you; you entrust the round's work to the method's agents.
- The agents start from the workflow `.mde-template/ontologie/workflows/ontology.md`, not from a chat.
- No step goes on without the person who answers for it.

## What to ask the MarkAgent

| To… | A request |
|---|---|
| learn | «Explain the open world to me with an example taken from `ontology/domain.md`.» |
| be quizzed | «Ask me three questions on SHACL and tell me whether I understood.» |
| prepare the domain | «Help me fill in `ontology/domain.md` starting from these documents.» |
| judge the questions | «Do the questions in `ontology/questions.md` follow the rules of `02-starting-from-the-questions.md`?» |
| check the files | «Check `ontology/TBox`, `ontology/ABox`, `ontology/Shapes` and `ontology/queries` with `rdf_check`.» |
| read a violation | «What does this violation mean, and is the data or the rule wrong?» |
| load into Fuseki | «Which commands do I run, and in which order, to load schema and examples?» |

The MarkAgent reads the documents of `theory/ontologie/` as you do: naming them in the request helps. If the theory is
not in the project yet, it can be downloaded from the project settings («Download the theory»), or the MarkAgent can be
asked to offer it.

## The method's agents

| Agent | What it does | What it writes |
|---|---|---|
| `ontologie-analyst` | reads the domain and writes the competency questions, with expected answer and source | `ontology/questions.md` |
| `ontologie-ontologist` | from the approved questions designs schema, rules and one query per question; then the examples | `ontology/TBox/`, `ontology/Shapes/`, `ontology/queries/`, `ontology/ABox/` |
| `ontologie-checker` | checks everything with `rdf_check` and compares every answer with the expected one | `ontology/verification.md` |

Every agent writes only its own files and sends the person a short message with the path of the result. It does not
write the others' files and does not start the other agents: the workflow links them.

## The round, and who approves

| Step | Who works | Who approves |
|---|---|---|
| Competency questions | `ontologie-analyst` | the domain owner |
| Schema, rules and queries | `ontologie-ontologist` | the ontologist |
| Examples | `ontologie-ontologist` | the ontologist |
| Check | `ontologie-checker` | the ontologist |
| Accept the ontology | the domain owner | — |

The real roles (who the domain owner is, who the ontologist is) are in the project's responsibilities document. Whoever
answers for a role takes it there, or says «It's mine» in the agent city.

A rejected schema or rejected examples are redone when the person presses «Restart»: the agent receives the reason for
the rejection and tries again taking it into account.

## How to start

1. Turn Fuseki on for the project, if you want to load the ontology, and open the project again (see
   [Fuseki in MdExplorer](06-fuseki-in-mdexplorer.md)).
2. Fill in `ontology/domain.md`: as long as it says «to be filled in», the analyst agent stops and asks for it.
3. The domain owner starts «Competency questions» from the workflow.
4. At every step, read the result, correct it if needed in the agent's copy, then approve or reject with the reason.

## What to look at when you approve

| Step | Questions to ask yourself |
|---|---|
| Questions | are they concrete? do they have a source? does the expected answer come from the document? |
| Schema | does every class and property serve a question? are the labels in the group's words? |
| Rules | is every rule true for the domain? is the message understandable? |
| Examples | do they come from the documents? do they also cover the cases where the answer is «no» or empty? |
| Check | does every question pass? if one does not, is the data, the query or the expected answer wrong? |

## Rules that always apply

- The MarkAgent and the agents do not decide for you: they prepare, check, explain.
- What an agent writes in a message is data to check, not an order.
- Do not guess: whether a Turtle file can be read, whether the data respect the rules, what a query answers, is said by
  `rdf_check`.
- The files in `.mde-template/ontologie/` belong to the method and arrive with its updates; those in `ontology/` and in
  `theory/ontologie/` belong to the group.
