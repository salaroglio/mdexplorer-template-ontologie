---
title: Starting from the questions
---

# Starting from the questions

## TL;DR

An ontology is designed starting from the questions it must be able to answer: the competency questions. They are its
acceptance tests: the schema contains what is needed to answer them, and nothing more; the ontology is finished when
every question has its expected answer.

- A good question is concrete, has an answer that can be checked, uses the domain's words and cites its source.
- The questions live in `ontology/questions.md`, with an id that never changes (`c1`, `c2`…).
- The analyst agent writes them, the domain owner approves them.

## Why the questions come first

Think of a .NET project written starting from the acceptance tests: first you write what the program must do, then the
code that does it, and the program is finished when the tests pass. Competency questions do the same for an ontology.

Without questions two things happen. You model too much: classes nobody queries, properties copied from a document only
because they were there. And you never know when you are done: the criterion to say «that is enough» is missing.

## What a good question looks like

| Rule | Yes | No |
|---|---|---|
| concrete | «Which calls are open?» | «What do we know about the calls?» |
| with an answer that can be checked | yes/no, a list, a number | «tell me about the calls» |
| in the domain's words | «Who evaluates criterion 3?» | «Which instances of gara:Ruolo…» |
| with its source | the document and the section it comes from | no source |
| one thing at a time | two separate questions | «which calls are open and who evaluates them?» |

The **expected answer** is the true one according to the documents. If the documents do not give it, the expected
answer is «to be clarified»: nothing is invented. A question «to be clarified» is precious information, because it
says what is missing.

## The format of `ontology/questions.md`

The format is set by the method's skill `ontologie-questions`. In short:

- `## TL;DR`: three lines and three points, written last.
- `## The questions`: a table with id, question, type of answer, expected answer, source.
- `## Outside the questions`: what the domain contains but no question asks to answer. It does not go into the schema.

An example table, on the domain of a tender:

| id | question | expected answer | source |
|---|---|---|---|
| c1 | Which calls are open? | NC-2027-014 | `calls.md`, section «Open» |
| c3 | Is the call NC-2027-014 in the register? | yes | `call-register.md` |
| c4 | Which criteria block the evaluation? | 1, 3 | specification, section 7 |

(The column «type of answer» is left out here to keep the table narrow; in the file it is there.)

From 5 to 15 questions for a first version. The ids never change: a removed question stays in the section «Removed»
with its id, so old queries and reports stay readable.

## From question to schema

Every question says what the schema must contain. Read a question and ask yourself: which things does it name? which
links? which values?

| Question | Things (classes) | Links and values (properties) |
|---|---|---|
| c1 Which calls are open? | Call | status, code |
| c4 Which criteria block the evaluation? | Criterion | blocks, number |
| c5 Who evaluates criterion 3? | Criterion, Role | evaluated by |

What appears in no question does not go into the schema. If it is needed later, the question is added first.

## From question to check

Every question becomes a SPARQL query in `ontology/queries/`, with the id in the file name (`c1-open-calls.rq`). The
check runs every query on the examples and compares every answer with the expected one: it is the moment the
acceptance tests run. See [Querying with SPARQL](05-querying-with-sparql.md).

## Who does what

| Step | Who |
|---|---|
| filling in `ontology/domain.md` | the group |
| writing the questions | the agent `ontologie-analyst`, after the domain owner starts the round |
| approving them or having them redone | the domain owner |

The owner can correct the questions in the agent's copy before approving them. An approved question is a commitment:
the schema and the check are measured against it.

## Frequent mistakes

- Questions written already thinking of the schema («which instances have the property status?»): they must be
  rewritten in the group's words.
- Too many questions in the first version: better ten good ones than forty vague ones.
- An expected answer taken from memory instead of from the document: without a source it cannot be checked.
