# ProjectML

**An open modelling language for turning what stakeholders actually said into a traceable requirement
baseline.**

> **Status: phase 1, nothing published yet.** The metamodel is being drafted. `spec/` does not exist. The
> decisions this repository is built on are in
> [`docs/2026-08-26-kernel-brainstorm.md`](docs/2026-08-26-kernel-brainstorm.md), which is the founding
> record rather than a specification.

## What it is

A project starts with someone saying something — an email, a call note, a site visit, a tech rider, a
regulation, a designer's own decision note. ProjectML models the chain from that sentence to the
requirements it obliges, the decisions taken along the way, and what is still open, so that every line of a
finished requirement set can be walked back to the words it came from, and every stated request can be shown
to have produced something or to have been deliberately left alone.

The cycle ends in a **baseline**: a dated, identified cut of the requirements in force, which is what the
people building the thing design against.

## What it is not

ProjectML does not do schedule, tasks, dependencies, resources, budget or milestones. It covers the
**evidence-and-decision half** of a project — a decision log, an issue log and a requirements register, with
traceability holding them together.

It also does not describe designs. What satisfies a requirement is the business of a **design language**
attached beneath it: SysML v2, UML, [EventML](https://github.com/szantob/EventML), or one not yet written.
Each attaches on the same terms, through a binding that declares how its elements meet a requirement.
ProjectML is not a competitor to those languages — it is the front end they decline to have, and it ends
exactly where they begin.

## What ships

**Prose and diagrams. Nothing else.** No notation, no schema, no validator, no scripts, no requirement
templates, no domain vocabulary. Those belong to an *implementation*: a self-contained package carrying a
notation, a filled set of requirement definitions and a rule-set, which lives in its own repository.
[`CLAUDE.md`](CLAUDE.md) §1 draws the line and explains how to tell which side of it a sentence is on.

## Standards this builds on

| Standard | What ProjectML takes from it |
|---|---|
| ISO/IEC/IEEE 29148 | Requirements engineering vocabulary — *stakeholder need*, *requirements baseline* |
| ISO/IEC/IEEE 42010 | *Architecture Decision* and *Architecture Rationale*, and the rule that a project states its own criterion for which decisions it records |
| SysML v2 | The `def`/`usage` split, `requirement`, and `satisfy` — including which end of the edge carries it |

## Licence

MIT. See [`LICENSE`](LICENSE).
