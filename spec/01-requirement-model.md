# The requirement model

This is the requirement model, one member of the collection ProjectML metamodels (K19), and it is the
**product**: the member a design language actually binds to. It is reached by projection from the
requirement analysis model, its source (K20), which is written next, in `02-requirement-analysis-model.md`.

## 1. What this model is

A reader who wants a requirements register with traceability between requirements, and nothing else, can
stop at the end of this document. Everything this document defines — a requirement, the edge by which one
requirement is derived from another, and the baseline that gives a cut of the register a name and a date —
stands on its own, without the analysis apparatus that produced it: no source, no `SourceElement`, no
definition, no `RequirementDecision`, no finding. That apparatus is what
`02-requirement-analysis-model.md` adds, and it is reached from here, not required by here.

This asymmetry is what "product" means in K20's terms: the requirement analysis model is where a requirement
is assembled and justified, and the requirement model is what survives being handed to somebody who was not
in the room for that. A design language attaches to the product, not to the process that produced it — K21
says the same thing again, one step later, of the baseline specifically.

This document leans on nothing outside itself. A requirement here carries no values: its finished text states
every value it was produced with, and the values, with the `SourceNeed`s stating them, stay in
`02-requirement-analysis-model.md`, where they were settled (K130). The value-state model this document once
leaned on is withdrawn (K125, K131). The requirement analysis model is a member in the order, adopted after
this one or not at all, and §2 turns on that.

## 2. `Requirement`

A requirement carries three things: the two attributes below, and the derivation edge that follows them.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other requirement's, that persists for the requirement's whole life in the model, and across every baseline that carries it |
| text | Its finished text: the statement itself, in the form it holds in the register, with every value it was produced with written into it, once the requirement is complete; none before (K111, K130, K140) |

A requirement may also be derived from one or more other requirements, its originals. A derivation records
elaboration agreed with the client: the derived requirement states something the original asks for, worked out
with the client, and the original implies it, as SysML v2's derivation states (K146). No requirement is
refined or decomposed into others here; that belongs to a design language, beyond the seam. The derivation is
an edge between requirements, list-valued, because several originals may each, on its own, imply the same one;
a requirement that only several together imply is not derived from them (K147). Derivation admits no cycle;
adding a derivation to a requirement that exists does not change that requirement; and a derivation is never a
requirement's origin (below).

**The derivation edge is named at both ends** (K135), as SysML v2's derivation connection names them: an
*original requirement* end and a *derived requirement* end. A requirement derived from several others stands
at the derived end of several derivations, and one from which several derive stands at the original end of
each. Either end can be navigated; how either is written down is notation (K15).

**Every requirement names its origin, and its origin is what it refines** (K9, K145): the `SourceNeed`s it was
assembled from, by the refinement edge. A derivation from other requirements is never an origin, since a
requirement derived from others and refining nothing would carry content nobody answerable stated. The
refinement edge has been projected away; it lives in `02-requirement-analysis-model.md`, where `SourceNeed` is
defined, and that is where the invariant is checked.

### K33 — does a requirement name the definition it came from, and its kind?

A further question about origin belongs here, and answering it is this document's own decision to take
rather than one to inherit. `02-requirement-analysis-model.md` defines `RequirementDefinition`: the
definition whose rules turned a stater's free words into a requirement's bound wording, and whose
specialisation (K30) gives a requirement its kind. Does a `Requirement` in this product model name the
`RequirementDefinition` it was produced under, and that definition's kind?

Two things must both hold, and they pull in opposite directions. K13's condition on the projection asks that
everything in force be present, that nothing in force be dropped, and that whatever is dropped remain
recoverable in the working model. K19's independent adoptability asks that this document stand alone;
`RequirementDefinition`, and the specialisation hierarchy K30 gives it, are defined only in
`02-requirement-analysis-model.md`, which a reader adopting only this document has not read and, by §1, is
not required to read.

**Decision: no.** A `Requirement` in the product model does not name the `RequirementDefinition` it came
from, nor that definition's kind. The two criteria are not symmetric here. K19 admits no partial reading:
naming a `RequirementDefinition` — even by a bare identifier — asks the reader to accept that such a thing
exists, that it has a kind, and that the kind comes from a specialisation hierarchy, none of which this
document defines; that is exactly the forward dependency independent adoptability rules out. K13, on the
other hand, is satisfied by the same escape clause that already carries the rest of the analysis
apparatus: the binding between a requirement and its definition is not lost, only not projected into this
type. It stays in the requirement analysis model, in exactly the sense K13 asks of anything dropped —
recoverable, not deleted — and it sits there beside the refinement edge, `Source`, `SourceNeed` and
`RequirementDecision`, none of which are named on the product `Requirement` either. Recorded as K33 in
`spec/06-decisions.md`.

## 3. Requirements in force

This model carries the requirements **in force**, and a baseline (§4) is a cut of exactly those: the
requirements in force at the moment the cut was taken, and no others.

Being no longer in force is therefore not a state a requirement carries here. There is no retired
requirement in this model to find, and no attribute on a `Requirement` marking one. A requirement that
ceases to be in force is absent from every baseline cut after that point, and that absence is a signal
rather than a loss: a design language rebasing onto a later baseline and not finding a requirement it was
built against learns, from the absence, that what was built on that requirement needs rework (K35). Within
any one baseline nothing moves, because a baseline is frozen — an element satisfying a requirement in a
baseline goes on satisfying a requirement that baseline still contains.

None of this permits deletion, and none of it makes retirement invisible. **A requirement is never
deleted** (K5). The property of being no longer in force, and the reasoning that requires it, belong to the
requirement analysis model, where every requirement a project has ever held is kept and where being
retired is a state to carry — `02-requirement-analysis-model.md` §10. That is the same shape §2 already
describes for the refinement edge: a pointer to where something is recorded, not a dependency this document
has on the other. A reader who stops here has the register of what is in force, with traceability between its
requirements, which is what §1 promises and nothing less.

## 4. The baseline

A **baseline** is a named, dated instance of the requirement model. The term is adopted from ISO/IEC/IEEE
29148 rather than coined (K12).

A baseline has identity; the requirement model's live projection does not. A design language binds to a
named baseline, never to the projection as it stands at the moment of reading, because a recomputed view has
no identity across runs — read it twice and nothing guarantees the second reading names the same thing as
the first — while a binding depends on identifiers that hold still. K21 states this for the baseline
directly; K10 makes the same argument one model over, for why what a review produces over the requirement
analysis model is itself modelled rather than recomputed each time.

A baseline's condition is losslessness and recoverability: everything in force at the moment it is cut is
present in it, nothing in force is dropped, and anything dropped stays in the working model rather than
being lost (K13).

**A baseline carries each requirement in force with its finished text, and the derivation edges between them,
and nothing else of the working model** (K130). A requirement in force that is not yet complete has no
finished text to carry; whether a baseline may be cut while one exists is OQ35.

A baseline is not, itself, a model that must pass the requirement analysis model's checks. It has no
`SourceNeed` layer — `SourceNeed`s, and the rules written over them, belong to the requirement analysis
model, not to this one — so running a rule over `SourceNeed`s against a baseline is not a check that fails;
it is a check that does not apply, asked of a model that carries nothing for it to inspect (K13).

One further thing a baseline names, beyond a date and an identifier: the implementation package and the
version of it the baseline was cut under. An implementation carries a rule-set a project may vary as it
runs, so a check result is only meaningful when measured against the rule-set that produced it — a baseline
validated last month and one validated afterwards, under a changed rule-set, are not comparable unless each
says which rule-set stood behind it. The date and identifier alone do not carry that; naming the
implementation package and version does.

## 5. The syntactic constraints of this model

K24 divides constraints over the collection in two: a syntactic constraint refers only to elements the
metamodel defines and is decidable without judgement, where a semantic one judges content and is a matter
for review. A syntactic constraint over `Requirement`, the derivation edge, and the baseline needs nothing
beyond what this document already defines to decide, so it sits here, beside the elements it refers to,
rather than in a checks document that would otherwise have to gather constraints from across the whole
collection.

- A requirement's identity is unique among every requirement the baseline contains.
- The derivation edge admits no cycle: no requirement derives, through any chain of derivation edges, from
  itself.
- A baseline names a date, an identifier, and the implementation package and version it was cut under.
  Absence of any one of the three is a failed check on the baseline itself, independent of anything any
  requirement it contains says.
