# The requirement model

This is the requirement model, one member of the collection ProjectML metamodels (K19), and it is the
**product**: the member a design language actually binds to. It is reached by projection from the
requirement analysis model, its source (K20), which is written next, in `02-requirement-analysis-model.md`.

## 1. What this model is

A reader who wants a requirements register with traceability between requirements, and nothing else, can
stop at the end of this document. Everything this document defines — a requirement, the edge by which one
requirement is derived from another, and the baseline that gives a cut of the register a name and a date —
stands on its own, without the analysis apparatus that produced it: no source, no need, no definition, no
decision, no finding. That apparatus is what `02-requirement-analysis-model.md` adds, and it is reached from
here, not required by here.

This asymmetry is what "product" means in K20's terms: the requirement analysis model is where a requirement
is assembled and justified, and the requirement model is what survives being handed to somebody who was not
in the room for that. A design language attaches to the product, not to the process that produced it — K21
says the same thing again, one step later, of the baseline specifically.

One thing this document does lean on, and whoever adopts it takes up along with it: the value-state model. A
requirement's values carry value states, and `04-value-states.md` is where those are stated. That is not a
forward dependency of the kind §2's decision rules out. The value-state model is a prerequisite every member
of the collection carries rather than a member reached later in the numbered order — which is what K19
already means by saying it crosscuts, and what `00-overview.md` §2 says where the order is described. The
requirement analysis model is not like that: it is a member in the order, adopted after this one or not at
all, and §2 turns on the difference.

## 2. `Requirement`

A requirement carries four things: the three attributes below, and the derivation edge that follows them.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other requirement's, that persists for the requirement's whole life in the model, and across every baseline that carries it |
| text | The requirement's bound wording: the statement itself, in the form it holds in the register |
| values | Any values the text parametrises. Each value carries a value state, on the same terms as a value anywhere else in the collection — see `04-value-states.md` |

A requirement may also be derived from one or more earlier requirements. This derivation is an edge between
requirements, and, like the refinement edge it stands beside, it is list-valued rather than singular: a
requirement synthesising two earlier ones has two origins, not one, and an edge that could only name a single
predecessor would force an arbitrary choice among equally contributing ones.

**Every requirement names its origin.** This is the invariant K9 rests on: a requirement's origin is its
refinement, its derivation, or both, and a requirement carrying neither is an incomplete record rather than
a root (D49). In this model, only the derivation half of that origin is directly visible — the edge just
described, between one requirement and another. The refinement half, which names the needs a requirement was
assembled from, has been projected away; it lives in `02-requirement-analysis-model.md`, where `Need` is
defined. A requirement with no derivation edge here is therefore not yet known to be a root: it may still
name its origin through refinement, recorded one document over.

### K33 — does a requirement name the definition it came from, and its kind?

A further question about origin belongs here, and answering it is this document's own decision to take
rather than one to inherit. `02-requirement-analysis-model.md` defines `RequirementDef`: the definition whose
rules turned a stater's free words into a requirement's bound wording, and whose specialisation (K30) gives a
requirement its kind. Does a `Requirement` in this product model name the `RequirementDef` it was produced
under, and that definition's kind?

Two things must both hold, and they pull in opposite directions. K13's condition on the projection asks that
everything in force be present, that nothing in force be dropped, and that whatever is dropped remain
recoverable in the working model. K19's independent adoptability asks that this document stand alone;
`RequirementDef`, and the specialisation hierarchy K30 gives it, are defined only in
`02-requirement-analysis-model.md`, which a reader adopting only this document has not read and, by §1, is
not required to read.

**Decision: no.** A `Requirement` in the product model does not name the `RequirementDef` it came from, nor
that definition's kind. The two criteria are not symmetric here. K19 admits no partial reading: naming a
`RequirementDef` — even by a bare identifier — asks the reader to accept that such a thing exists, that it
has a kind, and that the kind comes from a specialisation hierarchy, none of which this document defines;
that is exactly the forward dependency independent adoptability rules out. K13, on the other hand, is
satisfied by the same escape clause that already carries the rest of the analysis apparatus: the binding
between a requirement and its definition is not lost, only not projected into this type. It stays in the
requirement analysis model, in exactly the sense K13 asks of anything dropped — recoverable, not deleted —
and it sits there beside the refinement edge, `Source`, `Need` and `Decision`, none of which are named on
the product `Requirement` either. Recorded as K33 in `spec/06-decisions.md`.

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
retired is a state to carry — `02-requirement-analysis-model.md` §8. That is the same shape §2 already
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

A baseline is not, itself, a model that must pass the requirement analysis model's checks. It has no need
layer — needs, and the rules written over them, belong to the requirement analysis model, not to this one —
so running a rule over needs against a baseline is not a check that fails; it is a check that
does not apply, asked of a model that carries nothing for it to inspect (K13).

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
