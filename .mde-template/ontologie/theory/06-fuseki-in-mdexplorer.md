---
title: Fuseki in MdExplorer
---

# Fuseki in MdExplorer

## TL;DR

Apache Jena Fuseki is a triplestore: a server that holds RDF graphs and answers SPARQL queries. In MdExplorer it is
turned on per project (Project settings → Fuseki tab), with the server's address and a dataset; the files are loaded
with the runnable PowerShell commands of their companion `.md` files, one named graph per file.

- Fuseki is needed neither to write nor to check the ontology: `rdf_check` works in memory.
- It is needed to have the group's ontology in one place, queryable by people, agents and programs.
- With Fuseki on, MdExplorer installs the skills `mde-tbox`, `mde-abox` and `mde-shacl` in the project.

## What Fuseki is

The .NET analogy: Fuseki is to an ontology what SQL Server is to a database. The `.ttl` files are like the creation and
population scripts; Fuseki is the server where the data live and where queries are sent. A **dataset** is like a
database; a **named graph** is like a table, but for triples: a container with a name inside the dataset.

## Turning it on for the project

1. Project settings → **Fuseki** tab → «Enable Fuseki (RDF triplestore) for this project».
2. **Server URL**: the Fuseki to use; the initial value is `http://localhost:3030`.
3. **Dataset name**: one per project; if left empty, MdExplorer uses the project's name made fit for a URL.
4. Username and password only if the server asks for them (they stay on your computer, the password encrypted).
5. «Test connection»: says whether the server answers and whether the dataset exists; if it does not, «Create dataset»
   creates it.
6. Open the project again: it is when the project opens that MdExplorer installs the skills `mde-tbox`, `mde-abox` and
   `mde-shacl`.

These settings belong to this computer, not to the group: everyone turns Fuseki on for themselves. The method
«Ontologies» does not turn it on by itself; if it is off, a note says so when the project is created.

## What is loaded, and how

Every `.ttl` file has a companion `.md` with a section «Load into Fuseki»: a PowerShell command that MdExplorer runs from
the document (you fill in two parameters, the dataset and the server's address, and press Run). The command sends the
file to Fuseki with a `PUT` request of the Graph Store Protocol, into its named graph:

| File | Named graph |
|---|---|
| `ontology/TBox/<layer>.ttl` | `urn:mde:tbox:<layer>` |
| `ontology/ABox/<set>.ttl` | `urn:mde:abox:<set>` |
| `ontology/Shapes/<layer>.shapes.ttl` | `urn:mde:shapes:<layer>` |

The `PUT` replaces the whole content of the named graph: loading a file again creates no duplicates and does not touch
the others. The order matters: first the schema, then the examples. The skill `mde-tbox` also provides an
`ontology/TBox/load-all.md` that loads every layer at once.

Loading is not automatic: a file changed on disk does not reach Fuseki by itself, you load it again with its command.
This way you decide when the version on Fuseki changes, for example after the ontologist has approved.

## Querying it

- From the companion `.md` files: the section «Check» has a runnable SPARQL query that confirms the data arrived.
- From Fuseki's interface, at the server's address: the dataset's page has a query editor.
- From an agent or a program: the endpoint `<server>/<dataset>/query` accepts SPARQL over HTTP.

A query on a specific named graph uses `FROM <urn:mde:abox:gara-nordica>` or `GRAPH <…> { … }`.

## What you gain

| Without Fuseki | With Fuseki |
|---|---|
| the ontology is a set of files in the repository | it is also a live graph, in one place |
| to answer, you reread the files or use `rdf_check` | you query it with SPARQL, from any tool |
| every agent reads the files its own way | the agents use the same facts, with the same queries |
| the checks run when you start them | the same graph is the reference for the whole group |

In practice: the approved competency questions become queries anyone in the group can run again on the updated graph,
and a new document turns into new instances loaded next to the old ones, checked with the same rules.

## What it is not

- **It is not the code graph.** MdExplorer has its own database for programs, files and symbols: it is queried with the
  `code_*` tools, never with SPARQL on the project's Fuseki.
- **It is not the check.** `rdf_check` checks files, rules and queries without Fuseki; the method's round uses that.

## If something goes wrong

| Symptom | Where to look |
|---|---|
| «Test connection» fails | is the server on? does the URL have the right port? are username and password needed? |
| the dataset does not exist | «Create dataset» in the same tab |
| the skills `mde-tbox`, `mde-abox`, `mde-shacl` are not there | is Fuseki on for the project? was the project opened again afterwards? |
| a query finds nothing | was the file loaded? is the query's prefix the data's one? |
