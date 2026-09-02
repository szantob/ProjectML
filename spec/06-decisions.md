# Decisions

This is the normative decision record. The design records under
[`docs/superpowers/specs/`](../docs/superpowers/specs/) carry the reasoning behind each decision below; this
file carries the decisions in force. Where a decision or a question needs more than the one line given here,
the design record it came from is linked once, above the table it belongs to.

The `K` series is continuous. `K1`–`K18` were taken in
[the founding record](../docs/2026-08-26-kernel-brainstorm.md) and are not restated here; this document
begins at `K19`.

A `D` number always means EventML and a `K` number always means ProjectML. `D` numbers resolve through
[`docs/eventml-decisions.md`](../docs/eventml-decisions.md).

**The convention that binds every later task.** A decision taken while writing a document in `spec/` is
added to this file in the same commit as that document. This file is never left behind.

## Decisions, K19–K32

Taken in [the design record of 2026-09-02](../docs/superpowers/specs/2026-09-02-spec-structure-and-oq2-design.md),
which carries the full argument for each.

### The collection

| # | Decision | Reason |
|---|---|---|
| K19 | ProjectML metamodels a collection of connected models, not one model. Four members: the requirement model, the requirement analysis model, the Project Lifecycle Model, and the value-state model, which crosscuts the other three | Names the separability the founding record's OQ1 already found, making it structural rather than a caveat |
| K20 | The product of the projection is the `requirement model`; its source is the `requirement analysis model`; a baseline is a named, dated instance of the requirement model | Names the projection, which the founding record leaves unnamed beyond *baseline*, because a design language sees the projection and needs a name for what it sees |
| K21 | A baseline has identity; the projection does not. A design language binds to a baseline, never to the live projection | A design language needs something with identity to point at; the projection is recomputable, the baseline is not — K10's argument, one model over |
| K22 | A rule-set is a model, built with its own metamodel, not a layer. Different teams working in the same domain load different rule-sets; they do not sit at different levels | K16's three levels survive intact; a rule-set is one more model below them, not a fourth level |
| K23 | The metamodel names the Project Lifecycle Model and says what a rule-set may state. It states no rules | The same move K15 makes for requirement kinds and K7 makes for contradictions: the metamodel provides the slot, something below fills it |

### Constraints over the model

| # | Decision | Reason |
|---|---|---|
| K24 | Constraints over the model divide in two. A syntactic constraint refers only to elements the metamodel defines and is decidable without judgement; the metamodel states these and they are checkable. A semantic constraint judges content; the metamodel defines what it is, what a reviewer must cite and how a finding is recorded, and does not evaluate it | K7 generalised: the metamodel already takes this posture for contradictions |

### What a `RequirementDef` is

| # | Decision | Reason |
|---|---|---|
| K25 | The seam test. An attribute belongs to the metamodel if the metamodel can interpret it without resolving a reference to an element it does not define. Prose that names design-language things is content, and content belongs to the implementation; a typed reference to a design-language element would be a second seam | K3's one-seam rule, applied attribute by attribute rather than only to the relation |
| K26 | The record test. An attribute belongs to the metamodel if a stated metamodel rule can fail on it — including on its absence — without reading its content | K7's posture, made into a criterion: a rule that might one day be written does not qualify |
| K27 | `RequirementDef` does not split into two types. It is one type with a core the metamodel interprets or can fail on, and declared points an implementation fills | A split would need a second type coordinated with the first, which is not the metamodel's job |
| K28 | `constraints` leaves the metamodel entirely. The metamodel does not name the concept. An implementation may introduce one | It fails both K25 and K26 on the same structural fact: it is a typed reference into elements the kernel does not define — a second seam |
| K29 | `verification` stays, on the definition rather than on the requirement | It passes both K25 and K26 — required and verifiable independently of any design language — and a verification method is generic to a kind, which places it on the definition rather than the instance |
| K30 | A requirement kind is a specialisation of `RequirementDef`, not an attribute on it, and `RequirementDef` is abstract | SysML v2's own mechanism for requirement hierarchies is specialisation, and a single kind attribute would fix the number of classification axes at one |

### This repository's own shape

| # | Decision | Reason |
|---|---|---|
| K31 | `spec/` carries one document per member of the collection, plus an overview, a binding contract and a decision record | The collection is the structure (K19), so `spec/`'s layout follows it rather than copying EventML's eight-file layout |
| K32 | The diagrams' vocabulary is a metalanguage, is descriptive only, and adopts existing conventions rather than coining any | CLAUDE.md already distinguishes drawing a metamodel from writing notation; house rule 10 applies to the metalanguage as well |

## Decision K33

Taken in [`01-requirement-model.md`](01-requirement-model.md), §2, which carries the full argument.

| # | Decision | Reason |
|---|---|---|
| K33 | A `Requirement` in the product model does not name the `RequirementDef` it came from, nor that definition's kind | K19's independent adoptability forces the exclusion — naming `RequirementDef` or its kind pulls in a type and a specialisation hierarchy defined only in `02-requirement-analysis-model.md`, which a reader of the product model alone has not read. K13's condition on the projection permits the exclusion — the binding to the definition is not lost, only not projected into this type; it stays recoverable in the requirement analysis model, the working model K13 asks it to survive in. The two criteria are not symmetric: K19 admits no partial reading, K13 is satisfied by the same retention clause that already carries the rest of the analysis apparatus, so the balance is not close |

## Decision K34

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §8, which carries the full
argument.

| # | Decision | Reason |
|---|---|---|
| K34 | The projection carries every requirement, whether in force or not. Being no longer in force is projected; nothing else the requirement analysis model adds is | [`01-requirement-model.md`](01-requirement-model.md) defines the property in the product model and argues for it there, so a projection that dropped retired requirements would make that property unobservable in the model that defines it; that document's own constraints already speak of every requirement a baseline contains "in force or not"; and K13's condition governs what may be dropped, not what must be. K12's *a cut of the requirements in force* names what a baseline is for rather than stating an exclusion. Where this and the founding record's procedure — which ends the cycle by dropping "the deprecated requirements" — pull apart, the procedure describes where a cycle comes to rest and K20's projection is what this decision governs |

## Open questions, OQ9–OQ11

Raised in [the design record of 2026-09-02](../docs/superpowers/specs/2026-09-02-spec-structure-and-oq2-design.md),
which carries the full argument for each.

| # | Question | When answerable |
|---|---|---|
| OQ9 | What does specialisation mean? What a subtype of `RequirementDef` may add, narrow or override. K30 chooses the mechanism and does not define its semantics | When something exercises it — realistically phase 4, when the first kinds are declared |
| OQ10 | Does `verifies` become a second edge kind on the one seam? SysML puts verification as an edge from a verification element to a requirement, in the same direction and shape as `satisfies`. K4's first declaration would widen by one word to carry it. Nothing exercises it: no verification elements exist anywhere yet | Phase 2, where the SysML binding meets it, or later |
| OQ11 | Does the metamodel need a subject? SysML requires every requirement to have one. The metamodel does not have one, and the `satisfies` edge appears to determine it, since SysML's own `satisfy` binds the subject to the enclosing element. If that is not enough, a binding must synthesise one, which is the shape a false K2 would take | Phase 2 — this is the binding's job to settle |

## Status of the founding record's open questions

| # | Status |
|---|---|
| OQ1 | Not answered, but shaped: the collection's dependency order is now the adoption order |
| OQ2 | **Answered** — K27, K28, K29, K30 |
| OQ3 | Open, and next. [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md) §9 states the rule that reports an orphan need and deliberately records no disposition for one |
| OQ4 | **Answered** — K22, K23 |
| OQ5 | Unchanged |
| OQ6 | Unchanged |
| OQ7 | Unchanged |
| OQ8 | Unchanged |
