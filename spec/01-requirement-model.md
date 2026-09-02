# The requirement model

This is the requirement model, one member of the collection ProjectML metamodels (K19), and it is the
**product**: the member a design language actually binds to. It is reached by projection from the
requirement analysis model, its source (K20), which is written next, in `02-requirement-analysis-model.md`.

## 1. What this model is

A reader who wants a requirements register with traceability between requirements, and nothing else, can
stop at the end of this document. Everything this document defines — a requirement, the edge by which one
requirement is derived from another, the property of being no longer in force, and the baseline that gives
a cut of the register a name and a date — stands on its own, without the analysis apparatus that produced
it: no source, no need, no definition, no decision, no finding. That apparatus is what
`02-requirement-analysis-model.md` adds, and it is reached from here, not required by here.

This asymmetry is what "product" means in K20's terms: the requirement analysis model is where a requirement
is assembled and justified, and the requirement model is what survives being handed to somebody who was not
in the room for that. A design language attaches to the product, not to the process that produced it — K21
says the same thing again, one step later, of the baseline specifically.

## 2. `Requirement`

A requirement carries three things.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other requirement's, that persists for the requirement's whole life in the model — including after it is marked no longer in force (§3) |
| text | The requirement's bound wording: the statement itself, in the form it holds in the register |
| values | Any values the text parametrises. Each value carries a value state, on the same terms as a value anywhere else in the collection — see `04-value-states.md` |

A requirement may also be derived from one or more earlier requirements. This derivation is an edge between
requirements, and, like the refinement edge it stands beside, it is list-valued rather than singular: a
requirement synthesising two earlier ones has two origins, not one, and an edge that could only name a single
predecessor would force an arbitrary choice among equally contributing ones (K1).

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

## 3. No longer in force

A requirement is never deleted. When it is retired, superseded, or found wrong, it carries the property of
being no longer in force rather than being removed from the model (K5).

The reason is not traceability for its own sake. The checks over this model are pairwise: a finding fires
between two requirements, or between a requirement and something naming it. Deleting a requirement silences
exactly the one check that fired on it, and nothing else — the finding disappears along with the element it
was about, and nothing in the model any longer shows that a check ever ran there, let alone that removing
the requirement was a considered choice rather than an oversight. Marking a requirement no longer in force
keeps the requirement, the finding, and the fact that somebody acted on it all present at once, so a later
reader can tell "this was resolved" apart from "this was made to disappear."

## 4. The baseline

A **baseline** is a named, dated instance of the requirement model. The term is adopted from ISO/IEC/IEEE
29148 rather than coined (K12).

A baseline has identity; the requirement model's live projection does not. A design language binds to a
named baseline, never to the projection as it stands at the moment of reading, because a recomputed view has
no identity across runs — read it twice and nothing guarantees the second reading names the same thing as
the first — while a binding depends on identifiers that hold still. K21 states this for the baseline
directly; K10 makes the same argument one model over, for why the findings above the requirement model are
themselves modelled rather than recomputed each time.

A baseline's condition is losslessness and recoverability: everything in force at the moment it is cut is
present in it, nothing in force is dropped, and anything dropped stays in the working model rather than
being lost (K13).

A baseline is not, itself, a model that must pass the requirement analysis model's checks. It has no need
layer — needs, and the coverage rules written against them, belong to `02-requirement-analysis-model.md`,
not to this model — so running a need-coverage rule against a baseline is not a check that fails; it is a
check that does not apply, asked of a model that carries nothing for it to inspect (K13).

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

- A requirement's identity is unique among every requirement the baseline contains, in force or not.
- The derivation edge admits no cycle: no requirement derives, through any chain of derivation edges, from
  itself.
- A requirement carries exactly one of "in force" or "no longer in force" at any time — never both, and
  never neither.
- A baseline names a date, an identifier, and the implementation package and version it was cut under.
  Absence of any one of the three is a failed check on the baseline itself, independent of anything any
  requirement it contains says.
