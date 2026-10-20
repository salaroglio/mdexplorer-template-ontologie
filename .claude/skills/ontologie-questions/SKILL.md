---
name: ontologie-questions
description: "How to write an ontology's competency questions: the questions it must be able to answer, each with its expected answer and its source. Use it when you write or review ontology/questions.md, or when you are asked what an ontology is for or whether it is finished."
---

# The competency questions

An ontology is designed starting from the **questions it must be able to answer**. They are its acceptance tests: the
schema contains what is needed to answer them, and nothing more; the ontology is finished when every question has its
expected answer.

## What a good question looks like

- **Concrete**: «Which calls for tenders close within 30 days?», not «What do we know about calls for tenders?».
- **With an answer that can be checked**: a yes/no, a list, a number.
- **From the domain, not from the technique**: the group's words, never «which instances of tender:Call…».
- **With its source**: the document (and the section) that makes the question meaningful and that gives the expected answer.
- **One thing at a time**: two questions in one are split.

## The format of `ontology/questions.md`

- `## TL;DR`: three lines and three points, written last.
- `## The questions`: a table with **id** (`c1`, `c2`…, they never change), **question**, **type of answer** (yes/no,
  list, number), **expected answer** (the true one according to the documents, or «to be clarified»), **source**.
- `## Outside the questions`: what the domain contains but no question asks to answer: it does not go into the schema.

From 5 to 15 questions for a first version. A question removed in a later version keeps its id in the section
«Removed», so old queries and reports stay readable.
