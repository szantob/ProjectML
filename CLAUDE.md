# CLAUDE.md

Conventions for anyone — human or agent — working in this repository. Read this file first; it is the
starting point for every task, and most tasks are executed by agents with no other context than what is
written here.

## 1. What this repo is, and the one mistake to avoid

ProjectML is a **metamodel**. It defines concepts and types: what a `Source` is, what a `Need` is, what a
`Requirement` is, what a `RequirementDef` is, which edges connect them, what states a value can be in, and
what a design language must declare to attach underneath. It says all of that in **prose and diagrams**.

**It is not a language implementation, and the most common failure in this project is drifting into one.**

| Do not write | Because |
|---|---|
| YAML, JSON, XML, or any file format | Notation belongs to an implementation, not the metamodel. A design language attaching underneath brings its own |
| A schema, grammar, or EBNF | Same reason. There is no concrete syntax here at all |
| A filled-in `RequirementDef` | The metamodel defines what a `RequirementDef` *is* and contains none. **The moment any element goes into one, that is implementation** |
| A worked example in a notation | There is no notation to write it in |
| A validator, script, parser or exporter | Nothing executable ships from this repository |
| A domain vocabulary — audio, software, construction | Vocabulary belongs to whoever owns an implementation |

**The test, when unsure:** could this sentence be true of a project modelled in YAML, in SysML v2 textual
notation, and in a spreadsheet, without change? If yes, it is metamodel. If it assumes one of them, it is
implementation and does not belong here.

Diagrams are welcome and expected — Mermaid, rendered in place on GitHub and diffable in the repository.
An abstract-syntax diagram is not notation; it is how a metamodel is drawn.

**What an implementation is**, so the boundary is visible from both sides: an implementation is a
self-contained package carrying a notation, a filled set of `RequirementDef`s, and a rule-set a project may
vary as it runs. It lives in its own repository — except the SysML v2 binding, which lives here because
nobody outside this project would maintain it. See §2, K17 and K18.

## 2. Where this came from

The metamodel is being lifted out of [EventML](https://github.com/szantob/EventML), an open modelling
language for live-event AV systems, whose L0 (Brief) and L1 (Requirement) layers turned out to be
domain-neutral and to contain exactly the things that do not survive translation to SysML v2.

**[`docs/2026-08-26-kernel-brainstorm.md`](docs/2026-08-26-kernel-brainstorm.md) is this repository's
founding document.** It carries eighteen decisions (K1–K18), eight open questions (OQ1–OQ8), the procedure
the metamodel serves, and the four-phase order of work. Read it before starting anything. Decisions in it
are settled; open questions in it are this project's first work.

**This repository is phase 1 and phase 2** of that order:

1. **Bring the metamodel to a complete draft** — types, edges, states, rules, the binding contract.
2. **The SysML v2 binding**, on paper. This is where a false K2 shows up, and it is early on purpose.

Phases 3 and 4 happen in the EventML repository and are not this project's work.

## 3. Locked decisions

Locked. Do not revisit without an explicit instruction; every later task assumes they hold. The full set,
with reasoning, is in the founding document — these are the ones that constrain how work is done here.

| # | Rule |
|---|---|
| 1 | **Nothing executable ships, and no notation ships.** Prose and diagrams only. See §1 |
| 2 | **English is the only language in this repository**, in every file and every commit message |
| 3 | **The metamodel holds no filled definitions.** It defines the `RequirementDef` type; it declares no requirement kinds and no templates (K15) |
| 4 | **The kernel is the evidence-and-intent chain:** `Source`, `Need`, `Requirement`, `Decision`, the value-state model, the traceability relations, and the checks over them (K1) |
| 5 | **A design language attaches through exactly one seam** — an element outside the kernel carrying `satisfies`, naming a requirement (K3) — declared in a **binding** that states four things: which of its elements may carry `satisfies`, its internal refinement chain, its identifier space, and how far it takes the value model (K4) |
| 6 | **Attachment is symmetric.** SysML v2, UML, EventML and a design language not yet written attach on the same terms. No design language gets a privileged path (K2) |
| 7 | **An implementation is itself a metamodel**, for the project models built with it. Three levels: metamodel, implementation, project model (K16) |
| 8 | **A requirement is never deleted**, only marked no longer in force (K5). **A need carries no lifecycle state** — it belongs to its source, and a quotation cannot cease to be true (K6) |
| 9 | **The requirement model changes only through sources.** Every outcome of a review — a meeting, a call, the team's own decision — enters as a source (K11) |
| 10 | **Adopt, don't invent.** Where a term exists in ISO/IEC/IEEE 29148, ISO/IEC/IEEE 42010, SysML v2 or W3C PROV, use that term and record the source in a `source:` key or an explicit citation. Only coin a term when no standard has one |
| 11 | **Commit after every task.** Never push — pushing is the user's decision, made in GitHub Desktop |

## 4. Where things live

| Path | Responsibility |
|---|---|
| `spec/` | The metamodel: concepts, types, edges, states, rules, the binding contract. Normative. Prose and diagrams |
| `bindings/` | One document per design language, each stating K4's four declarations. The SysML v2 binding lives here (K17) |
| `docs/` | The founding record sits at the top level here, because it is the repository's constitution rather than one release's paperwork. [`eventml-decisions.md`](docs/eventml-decisions.md) sits beside it for the same reason — it is a standing reference, not one release's paperwork. Per-release design records go in `docs/superpowers/specs/` and their plans in `docs/superpowers/plans/`, matching EventML |

`spec/` and `bindings/` do not exist yet. **Phase 1 designs the structure of `spec/` before creating it** —
do not scaffold empty files, and do not copy EventML's eight-file layout without deciding it is right for a
metamodel that carries no notation and no library.

## 5. House rules

- **English everywhere, with no exception** — `spec/`, `bindings/`, `docs/`, commit messages, issues. Unlike
  EventML, which keeps a Hungarian glossary, this repository has no second language anywhere.
- Adopt terms from ISO/IEC/IEEE 29148 (requirements engineering, *stakeholder need*), ISO/IEC/IEEE 42010
  (*Architecture Decision*, *Architecture Rationale*), SysML v2 (`def`/`usage`, `requirement`, `satisfy`)
  and W3C PROV rather than coining synonyms. Record where a term came from on the entry that defines it.
- **Do not describe the metamodel in terms of files.** Say what an element is in the model, not which file
  or header key carries it. A metamodel that talks about files has quietly become an implementation.
- **"Attribute", not "field."**
- Where a diagram and the prose beside it disagree, the prose wins.
- Commit after every task. Never push.

## 6. How verification works

There is no validator, and — unlike EventML — there are no examples either, because examples are written in
a notation and this repository has none. The chain is:

> metamodel ← implementation ← project model

The metamodel is exercised last. **It is not finished until an implementation has been built on it and has
carried a project end to end**, and until the SysML v2 binding exists. Both are settled in the founding
document as OQ7. Until then the metamodel is a complete draft, not a release.

This has a working consequence: a construct nothing will ever exercise is one to leave out. EventML's own
rule applies here unchanged — *an unexercised construct is an unproven one, so it waits.*

## 7. Reference material

The EventML repository is available as a reference. Copy from it what applies — the founding document names
which parts are kernel and which are not — but **copy nothing that carries notation, vocabulary or filled
definitions**, which is most of what EventML contains. In particular: `spec/05-concrete-syntax.md` and
everything under `examples/` are implementation, not metamodel.

**EventML numbers its decisions `D1`–`D55` and keeps no consolidated list of them.**
[`docs/eventml-decisions.md`](docs/eventml-decisions.md) indexes the ones this repository depends on, says
where each lives, and marks which are inherited, which are imported because K15 moves their subject here,
and which one K9 overturns. Cite a `D` number through that index rather than from memory. A `D` number
always means EventML; a `K` number always means ProjectML.

**EventML is read-only from here.** It is frozen for the duration of phases 1 and 2 (K14), and the changes
it eventually needs are phases 3 and 4, which are not this project's work. Read it, quote it, cite it — do
not edit it, do not commit to it, and do not open branches in it.
