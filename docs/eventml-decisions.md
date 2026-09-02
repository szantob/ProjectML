# EventML decisions this repository depends on

**What this is.** A citation index. ProjectML's founding record —
[`2026-08-26-kernel-brainstorm.md`](2026-08-26-kernel-brainstorm.md) — argues from decisions taken in the
[EventML repository](https://github.com/szantob/EventML), and cites several of them by number. Nothing in
ProjectML resolves those numbers. This file does, so that a reader here can follow an argument without
leaving the repository, and so that house rule 10 — record the source — is met for material inherited
rather than coined.

**What this is not.** It is not a copy of EventML's design records, and it carries no rationale: the
reasoning stays where it was written, and a reader who needs it follows the link. It is not normative, and
it decides nothing. Where a statement below and a ProjectML decision disagree, the ProjectML decision
governs here — see *Overturned*, which is currently one entry.

**Two numbering series, and they do not collide.** EventML numbers its decisions `D1`–`D55`. ProjectML
numbers its own `K1`–`K18` and continues that series. A `D` number always means EventML; a `K` number
always means ProjectML.

**Scope.** An EventML decision appears below when a locked decision, an open question or a recorded finding
in ProjectML's founding record depends on it. That criterion selects 25 of EventML's 55. It produced more
entries than the six to eight estimated when this file was proposed, because the founding record leans on
more decisions than it names: K6 rests on D25 and D45, and K9 on D32, D46 and D49, none of which it cites
by number.

## Where these decisions live

EventML keeps no consolidated decision list. The 55 are distributed across five design records.

| Record | Decisions |
|---|---|
| [v0.1 core design](https://github.com/szantob/EventML/blob/main/docs/superpowers/specs/2026-08-08-eventml-core-design.md) | D1–D7 |
| [v0.2 decisions design](https://github.com/szantob/EventML/blob/main/docs/superpowers/specs/2026-08-09-eventml-v0.2-decisions-design.md) | D8–D14 |
| [v0.3 library design](https://github.com/szantob/EventML/blob/main/docs/superpowers/specs/2026-08-09-eventml-v0.3-library-design.md) | D15–D21 |
| [v0.4 needs design](https://github.com/szantob/EventML/blob/main/docs/superpowers/specs/2026-08-24-eventml-v0.4-needs-design.md) | D22–D50 |
| [v0.5 requirement types (work in progress)](https://github.com/szantob/EventML/blob/eventml-core-v0.5/docs/superpowers/specs/2026-08-26-eventml-v0.5-requirement-types-WIP.md) | D51–D55 |

The v0.5 record is incomplete and unimplemented, and it lives on a branch rather than on `main`. Its own
header says so, and the founding record's OQ6 treats it as such: the starting point for ProjectML is
`v0.4.0` plus two brainstorm records.

## Inherited — depended on, and unchanged

| # | Decision | What depends on it here |
|---|---|---|
| D20 | `applies_when` is prose | OQ2. Also the kernel's general posture: a rule that would need an evaluator before an evaluator exists is written so that a person or an agent can apply it |
| D22 | `Need` is a new entity in the Brief layer | K1 — `Need` is a kernel entity |
| D23 | The name comes from ISO/IEC/IEEE 29148's *stakeholder needs* | House rule 10, and K1's naming of the entity |
| D25 | A source is never decomposed | K6 — a need belongs to its source, and a quotation cannot cease to be true |
| D26 | Passage references adopt the W3C Web Annotation Data Model | The requirement that a need anchor into its source. Also the SysML mapping's finding that the stage before the model has no standard downstream of SysML |
| D27 | A need's value is optional, and the value-state model is untouched by it | The value-state model crosscutting every kernel entity |
| D28 | The refinement edge retargets from a path into the brief to a `Need` identifier | K1 — the traceability relations between kernel entities |
| D29 | One edge between sources: `answers` | The founding record's finding that `answers` is the natural closing edge for a review finding |
| D31 | A need that no requirement refines is reported (question rule 8) | OQ3 — the orphan need |
| D32 | Question rule 9 fires on a requirement carrying no origin edge at all — neither refinement nor derivation | K9 |
| D33 | Source coverage is a report, not a question rule | The founding record's §2, where step 2 of the procedure is already checkable |
| D34 | A need with no passage anchor fails a syntactic check | The distinction between syntactic constraints, which the kernel decides, and semantic ones, which it does not |
| D35 | `answers` sits on the later source and names the earlier | The closing-edge finding |
| D37 | `answers` changes no value | The closing-edge finding. Both statements were made; which prevailed belongs to a decision |
| D38 | `answers` points backward in time | The closing-edge finding |
| D40 | `Source` becomes an entity | K1 — `Source` is a kernel entity |
| D45 | `Source` is material of record, and its kind and origin attributes extend to admit riders, plans and regulations | K6, and the founding record's §5 finding that those two enumerations carry domain leaks |
| D48 | The refinement edge holds a list of need identifiers, not one | K1 — a requirement is assembled from more than one statement |
| D49 | Every requirement names its origin — refinement, derivation, or both; carrying neither is an incomplete record rather than a root | K9, which rests on this invariant |
| D50 | Checks are organised in two columns: what a script decides, and what a person or an agent decides | The syntactic/semantic distinction, and K7's posture that the kernel defines what a finding is without detecting it |

## Imported — the subject moves to ProjectML

K15 places the declaration of requirement kinds in the kernel: *"Its subject — a library declares the kinds
of requirement it handles — is kernel material entire."*

These five are therefore **not carried over as decisions in force**. Their subject becomes ProjectML's, and
ProjectML takes its own decisions on it with its own reasoning. They are listed as the prior art those
decisions answer to.

| # | Decision | Note |
|---|---|---|
| D51 | An implementation declares the kinds of requirement it handles | Stated in EventML as a property of a library |
| D52 | The declaration is a standalone list, not a label carried on each definition | A list can be read and questioned on its own — *how many kinds, and is the list complete?* |
| D53 | Each `RequirementDef` names the kind it belongs to | Cited by K8: the need's subject selects the definition, and the kind rides along |
| D54 | A kind carries a name and a description, and no project-management logic | EventML deferred that logic to a later release. In ProjectML it becomes the Project Lifecycle Model |
| D55 | The taxonomy is not fixed by the language | The founding record's §5 records a stronger reason than the one written with the decision: a fixed taxonomy would fix a single classification axis, and would make the kernel unattachable. That structural reason, not the ownership one, is what carries here |

## Overturned

One EventML decision is contradicted by a ProjectML decision. It is recorded rather than quietly dropped,
because it shipped in `v0.4.0`.

| # | EventML | ProjectML |
|---|---|---|
| D46 | Question rule 9 reports a requirement whose origin was never recorded, and **stays a question** | **K9** — rule 9 is a failed check, not a question. The rule's own text calls it an invariant, and the parallel case one layer down is already routed to a failed check |

The founding record's §6 treats this as a change to released work, available independently of everything
else in ProjectML, and names the consequence that goes with it: the claim that rule 9 is another rule "one
layer up, and the symmetry is exact" stops holding, and that sentence needs correcting in EventML. That
correction is EventML's to make, not this repository's.

## One citation that does not resolve

The founding record cites **"the v0.3 ownership rule"** twice — in K17's reasoning, and behind D55. It is
not a numbered decision. The v0.3 design record states library ownership as prose rather than as a decision,
and no `D` number was found for it.

Nothing here depends on resolving it, because the founding record's §5 supplies a different and stronger
reason for the same conclusion. It is recorded so that the citation is not mistaken for a resolvable one,
and so that the substitution is made deliberately when ProjectML takes its own decision on requirement
kinds.

## Deliberately absent

The remaining 30 decisions are not indexed. Most concern notation, file layout, versioning, release
mechanics or the migration of worked examples — manifests and references, where definitions are stored,
what a layer is called, how offsets are measured, which examples migrate. CLAUDE.md §7 forbids carrying
notation, vocabulary or filled definitions across, and most of EventML's decisions are one of the three.

An absent decision is not a rejected one. If later work here turns out to depend on one, it is added.
