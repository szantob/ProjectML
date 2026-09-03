# The seam's cardinality, and the check nobody had written — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K51–K54 and **closes
OQ12**, which was the only open question blocking phase 2. The edits it calls for are contained in
`05-binding-contract.md` and are small; they are not made here.

**Date:** 2026-09-03
**Related:** [`2026-09-03-source-element-hierarchy-design.md`](2026-09-03-source-element-hierarchy-design.md),
which began as an attempt on this same question and went elsewhere.

---

## 1. What OQ12 asked, and why it is now three questions short of one

OQ12 held that the seam edge was under-specified in three respects: its cardinality, a check that
`05-binding-contract.md` §4.1 cites by name but that no document states, and whether the edge pins a
baseline.

**The third part dissolved.** It assumed a requirement's content can change while its identity persists, so
that an edge naming a requirement is ambiguous between one baseline and the next. It cannot: a change to
what a requirement obliges produces a new requirement rather than an edit, because *retirement is a property
of an element and replacement is an edge between two*. An identity therefore never changes meaning, an edge
naming a requirement is unambiguous forever, and nothing needs adding. The companion record of the same date
sets out the reasoning that produced this.

The remaining two are settled below, and a fourth decision joins them, found while stating the first.

## 2. Cardinality

The question was answered by looking at what SysML v2 does rather than by reasoning from first principles,
because house rule 10 asks for adoption where a source exists and K2 makes SysML the case that matters.

| # | Decision | Rationale |
|---|---|---|
| K51 | **The seam edge is many-to-many, with no bound in either direction.** A satisfying element may name any number of requirements, and a requirement may be named by any number of satisfying elements | Both directions are what SysML already does, in both of its versions: more than one element may help satisfy a single requirement, and a satisfying element may carry any number of the edges. Adopted rather than reasoned to. A bound in either direction would also exclude design languages that organise differently, which K2 forbids. No lower bound is meaningful either: an element that names no requirement is not a satisfying element, and the metamodel does not see it — beyond the seam it sees only what carries the edge |
| K52 | **The metamodel fixes the multiplicity and not the shape.** Whether a design language writes the relationship as one edge carrying several names or as several edges each carrying one is its own business | The argument is not that the shape is unimportant but that SysML settles it two ways in one standard family: SysML v1 spells it as a stereotyped dependency standing between the two ends, and SysML v2 spells it as a requirement usage nested inside the satisfying element. A metamodel that fixed the shape could not bind both — and not as a hypothetical, but for the very language phase 2 binds. The shape is notation, and beyond the seam the notation is not ours (K15's reasoning, applied at the edge) |

**One inherited limit, which we adopt too.** SysML records as an *optional* requirement, asked for and not
supplied, that there is no way to say whether two satisfying elements naming one requirement mean that
**both** are needed or that **either** suffices. See §5.

## 3. The check

| # | Decision | Rationale |
|---|---|---|
| K53 | **The check `05-binding-contract.md` §4.1 names is stated in that document, and it is a question rather than a failed check**: which requirements in a baseline no satisfying element names | It belongs in that document for two reasons. It reads the seam, and the seam is defined there; and `01-requirement-model.md` has to stay independently adoptable (K19), which it would not be if it carried a rule presupposing a design language that may not be attached. Placing it there also makes that document self-sufficient for a binding's author, which is its whole purpose. It is a question and not a failure because a requirement that nothing satisfies is *not a modelling error; it is the normal state of one that has been captured but not yet designed for*, and a model is expected to hold requirements in that state for weeks. That is the same posture K45 takes for a question nobody has answered, which is a second place the collection reaches it |

**The check survives the limit in §5 untouched**, and this is worth stating where the check is. *Is any
element naming this requirement* has the same answer under both readings of two incoming edges, because both
require at least one. Only the stronger question — *is the requirement actually met* — divides on it, and
that question is verification, which the metamodel does not undertake (K7, K40).

## 4. The far side of the seam

| # | Decision | Rationale |
|---|---|---|
| K54 | **The metamodel describes the far side of the seam and does not name it.** A satisfying element is referred to by what it does, not given a type the metamodel defines — in prose, and in any diagram | K3 is explicit that the metamodel does not define what lies below the seam, and a class in a diagram half-defines what it draws. SysML does not name that side either: it says *model items*, or *NamedElement*, or simply nests the edge in whatever element carries it. Naming it would be the metamodel claiming a type it has no standing to declare, and would make the first of K4's four declarations partly redundant, since that declaration exists precisely so a design language can say which of *its* kinds may carry the edge |

The consequence for `spec/` is small and specific: `05-binding-contract.md` §2's diagram draws a class for
the satisfying side. The prose beside it is already right — it says *the satisfying element*, descriptively.
The diagram should follow the prose rather than the other way round.

## 5. What was deliberately not decided

**Whether two satisfying elements naming one requirement means both are needed or either suffices.** SysML
asked for this and does not supply it, so leaving it open is not falling behind SysML but matching it —
which is what K2 asks for, in the one place where matching is more informative than improving. It is not
recorded as an open question of ours, because nothing obliges us to answer it: if a binding turns out to
need it, that is evidence, and evidence is what would reopen it.

## 6. Status of OQ12

**Closed.** Its three parts: the cardinality is K51 and K52, the check is K53, and the baseline part
dissolved on a false premise. K54 was found while settling the first and is recorded with them.

**Phase 2 is no longer blocked.** The binding for SysML v2 can be written against
`05-binding-contract.md` once the edits above are made, and it should be, because that binding is the first
real test of K2 and everything in this record was decided by asking what SysML already does.

**Sources for the SysML findings**, all secondary; the normative check against the OMG specification is
phase 2's own work:
[PTC — Satisfy (SysML relationship)](https://support.ptc.com/help/modeler/r10.1/en/Modeler/sysml/SysML_Satisfy_relationship.html),
[Webel IT — Satisfying Requirements and the Satisfy relationship](https://www.webel.com.au/node/1666),
[Sensmetry — Requirement Satisfaction and Verification](https://sensmetry.com/advent-of-sysml-v2-lesson-24-requirement-satisfaction-and-verification/),
[Sparx — Ensuring a Requirement is Satisfied](https://sparxsystems.com/enterprise_architect_user_guide/17.0/guide_books/ensuring_a_requirement_is_satisfied.html),
[OMG SysML v2](https://www.omg.org/sysml/sysmlv2/).
