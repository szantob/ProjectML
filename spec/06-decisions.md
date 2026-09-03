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
| K33 | A `Requirement` in the product model does not name the `RequirementDef` it came from, nor that definition's kind | K19's independent adoptability forces the exclusion; K13's recoverability condition permits it — the two are not symmetric here, and independent adoptability wins |

## Decision K34 — superseded by K35

**Superseded. K35 reverses it, and K35 is the decision in force.** K34 is kept here because a decision record
keeps its history: a later reader meeting the argument below elsewhere needs to find where it was answered.

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §8, which no longer carries
the argument — it carries K35's.

| # | Decision, superseded | Reason it was taken |
|---|---|---|
| K34 | The projection carries every requirement, whether in force or not. Being no longer in force is projected; nothing else the requirement analysis model adds is | The seam argument alone decides it: dropping retirement at the projection would let a requirement vanish there instead of changing state, the failure K5 exists to prevent, reintroduced at the seam. K12 is read narrowly rather than contradicted |

**Why it fell.** The seam argument that carried it conflated the live projection with a baseline. It reasoned
about a requirement retiring *between* baselines as though a design language were bound to something that
moves under it, when by K21 a design language binds to a named baseline and to nothing else, and a baseline
is frozen. Its second leg fell with the first: K12 needs no narrow reading, and none is taken.

## Decision K35

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §8, which carries the full
argument. K35 supersedes K34.

| # | Decision | Reason |
|---|---|---|
| K35 | The projection carries only the requirements in force. Being no longer in force is a property of the requirement analysis model, not of the product: a requirement that ceases to be in force is dropped at the projection, and no baseline cut afterwards contains it | K21 decides it. A design language binds to a named baseline, never to the live projection, and a baseline is frozen — an element satisfying a requirement in a baseline goes on satisfying a requirement that baseline still contains, so no seam edge can dangle. What K34 read as *vanishing* is the intended signal: a requirement missing from a later baseline is what tells the team to rework what was built on it, which is what rebasing onto a new baseline is for. Traceability is unharmed on two legs — the requirement analysis model holds everything, including what is no longer in force (K5, K11), and every element the projection carries resolves back to its origin there |

## Decision K36

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §6, which carries the full
argument. §10 of that document states the syntactic constraints the decision is measured against.

| # | Decision | Reason |
|---|---|---|
| K36 | The seam test (K25) decides whether an attribute is admissible to the metamodel. The record test (K26) measures whether an attribute already admitted is load-bearing; it is not a second admissibility gate, and the two are not a conjunction. *when it applies* is admitted by K25, does not pass K26, and stays in the core | A presence rule can be written over any attribute at will, so the record test read as a gate is either vacuous — every attribute passes, the admitting rule always being available — or arbitrary, with nothing to say which presence rules are worth writing. K26's word *stated* excludes the rule nobody has written, not the rule anybody could write in an afternoon. K25 has no such weakness: whether an attribute resolves a reference the metamodel does not define is a fact about the attribute, unchanged by which rules exist over it. The core's own definition already joins the two with *or* — what the metamodel can interpret, **or** can fail on. *when it applies* falls on the first clause only: no stated rule fails on it, because its absence is deliberately a gap rather than a claim, and the rule available over it reports a question instead |

## Decisions K37–K39

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §4 and §9, which carry the
full argument. Together they **dissolve** the founding record's OQ3 rather than answering it: OQ3 asks what
to record when a need is examined and found to have deliberately produced nothing, and K37 removes the state
of affairs it asks about.

| # | Decision | Reason |
|---|---|---|
| K37 | A need is a passage of a source that **obliges something**. A passage obliging nothing is not a need and should not have been extracted. A passage of a source is therefore either a need or it is uncited: the model defines no context element, and none is to be invented | Extraction is already selective and always was, so *not need-bearing* is an existing category that needs no record — a passage nobody extracted leaves no element for a record to sit on, and a source permanently containing uncited text is normal rather than defective (D33). The reading that a need might oblige nothing at all was a misanalysis of statements about the environment the work happens in: such a statement constrains the environment the system must work within, and bears a requirement like any other need. The prior art held this without drawing the conclusion — its clustering of definitions by origin carries a class resolved by measuring rather than by asking an opinion, and its comparison with SysML v2 records that SysML's nearest category constrains the *system's* physical properties where this class describes the *environment* constraining the system. K6 is untouched: this narrows what a need is and gives a need no state |
| K38 | A need that no requirement refines is a **failed check**, not a question. It has exactly two resolutions — write the requirement the need obliges, or delete the need — and the criterion deciding between them is whether a declared definition covers the statement. This overturns D31 | It is the mirror of K9, which already moved the rule over a requirement carrying no origin edge from a question to a failed check: the two are one break in the chain read from opposite ends, and they were being treated asymmetrically for no stated reason. Once a need obliges something (K37), a need nothing refines is a record in which something obliged is unaccounted for, which is what a failed check says. The criterion is also the guard OQ3 feared the absence of: a requirement is produced only under a declared `RequirementDef` and names the one it was produced under (K30), so no requirement can be produced from a statement no declared definition covers, and the failure to find one is itself the signal that the extraction was wrong |
| K39 | A need extracted in error is **deleted**. Deletion is permitted for a need where K5 forbids it for a requirement | A source is material of record — quoted whole, never decomposed, never edited (D45, D25) — and a need is a pointer into a passage of it, so deleting the pointer leaves the passage unchanged in the source and loses nothing of record. A requirement is the working model's own construct with no other home: deleting one destroys the only record of it and silences the check that fired on it, which is what K5 exists to prevent. The asymmetry is between a construct and a pointer, not an inconsistency. A deletion here is not retirement and gives a need no lifecycle state (K6) |

## Decisions K40–K41

Taken in [`00-overview.md`](00-overview.md), §5, and in
[`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §4 and §9, which carry the full
argument. K40 states the second boundary the collection draws — between what the model guarantees and what
the modeller must do — and K41 refuses the one instrument that would blur it.

| # | Decision | Reason |
|---|---|---|
| K40 | **This metamodel's job is that no extracted information or decision is lost during the project-management process.** Whether every piece of information has been extracted, and whether every mapping is accurate, is not the model's responsibility but the modeller's: it can only be found by self-review or cross-review after the modelling is done. It follows that a model on which no check fails is not thereby a correct model — it is a model with no *detectable* error | A metamodel that claimed to guarantee completeness would be claiming what it cannot deliver. Completeness of extraction is measured against material the model does not hold — everything a source says that nobody took up — so the claim could never be tested, and a guarantee that cannot be tested devalues the ones that can. The line is the one K24 already draws between a syntactic constraint the metamodel decides and a semantic one it leaves to review, and the one K7 draws for contradiction, applied once more to the metamodel's own promise |
| K41 | The metamodel defines **no source-coverage report** — no report over which passages of a source no need cites — and **no metric over the completeness of extraction**. None is to be added. What it states over extraction is the one rule that runs the other way: a need that no requirement refines is a failed check (K38) | Two reasons, and each carries the decision on its own. **First, a source is free-form and its information density varies.** A salutation can be a twentieth of the text and none of the information, while a single clause buried in a paragraph can carry the only real constraint; a figure that puts those on one denominator says nothing, and a list of everything uncited is mostly noise by construction, because every source permanently contains uncited text (§4, D33). **Second, K10's own criterion says there is nothing to model.** K10 makes a finding a modelled element when it must keep its identity between reviews. A contradiction must: it cannot simply be fixed, it needs adjudication, and the next review has to see that somebody already found it. A missed extraction does not — the moment it is noticed it is extracted, so the finding and the fix are the same act, and nothing persists for a record to hold. That asymmetry is principled rather than convenient, which is why this sits beside K7 without contradicting it. The refusal is recorded rather than left as an absence because the prior art carries such a report (D33), and a later reader meeting it will otherwise propose adding one |

## Decision K42

Taken in [`03-project-lifecycle-model.md`](03-project-lifecycle-model.md), §2, which carries the full
argument.

| # | Decision | Reason |
|---|---|---|
| K42 | The list of what a rule-set may state is not closed at three. Three are what the evidence found so far supports; an implementation needing to state a fourth kind of thing is evidence the metamodel must then account for, not a violation of it | The same structural argument K30 makes for a requirement kind's taxonomy applies one level over: a closed list here would fix one way of thinking about a process into the metamodel, which is exactly what K22 and K23 exist to refuse |

## Decisions K51–K54

Taken in [`05-binding-contract.md`](05-binding-contract.md), §2, which carries the full argument, and in
[the design record of 2026-09-03 on the seam](../docs/superpowers/specs/2026-09-03-seam-cardinality-and-check-design.md),
which settled them. Together they close OQ12. Each was reached by asking what SysML already does rather than
by reasoning from first principles, which is what house rule 10 asks for where a source exists.

| # | Decision | Reason |
|---|---|---|
| K51 | **The seam edge is many-to-many, with no bound in either direction.** A satisfying element may name any number of requirements, and a requirement may be named by any number of satisfying elements | Adopted: it is what SysML does in both of its versions. A bound either way would exclude design languages that organise differently, against K2, and no lower bound is meaningful because an element naming no requirement is not one the metamodel sees |
| K52 | **The metamodel fixes the edge's multiplicity and not its shape.** How a design language writes the relationship down is its own business | SysML settles the shape two ways in one standard family — a stereotyped dependency in one version, a nested requirement usage in the next — so fixing it would bind one and not the other, and not hypothetically but for the language phase 2 binds |
| K53 | **One check runs over the seam — which requirements in a baseline no satisfying element names — and it is a question rather than a failure.** It is stated in the binding contract | A requirement nothing satisfies is the normal state of one captured but not yet designed for: a check whose failing state is the ordinary condition of ongoing work is a question, not a failure. It sits in the binding contract because it reads the seam, and because K19 requires the requirement model to stay adoptable by a reader who has attached nothing |
| K54 | **The metamodel describes the far side of the seam and does not name it**, in prose and in any diagram | K3 says the metamodel does not define what lies below the seam, and a class in a diagram half-defines what it draws. SysML does not name that side either. Naming it would also make K4's first declaration partly redundant |

## Open questions, OQ9–OQ11

Raised in [the design record of 2026-09-02](../docs/superpowers/specs/2026-09-02-spec-structure-and-oq2-design.md),
which carries the full argument for each.

| # | Question | When answerable |
|---|---|---|
| OQ9 | What does specialisation mean? What a subtype of `RequirementDef` may add, narrow or override. K30 chooses the mechanism and does not define its semantics | When something exercises it — realistically phase 4, when the first kinds are declared |
| OQ10 | Does `verifies` become a second edge kind on the one seam? SysML puts verification as an edge from a verification element to a requirement, in the same direction and shape as `satisfies`. K4's first declaration would widen by one word to carry it. Nothing exercises it: no verification elements exist anywhere yet | Phase 2, where the SysML binding meets it, or later |
| OQ11 | Does the metamodel need a subject? SysML requires every requirement to have one. The metamodel does not have one, and the `satisfies` edge appears to determine it, since SysML's own `satisfy` binds the subject to the enclosing element. If that is not enough, a binding must synthesise one, which is the shape a false K2 would take | Phase 2 — this is the binding's job to settle |

## Open question OQ12 — answered

**Answered by K51–K54, and no longer open.** Its three parts: the cardinality is K51 and K52, the check is
K53, and the part asking whether the edge pins a baseline **dissolved on a false premise**. That part assumed
a requirement's content can change while its identity persists. It cannot — a change to what a requirement
obliges produces a new requirement rather than an edit — so an identity never changes meaning, an edge naming
a requirement is unambiguous forever, and nothing had to be added. K54 was found while settling the first and
is recorded with them. The question is kept here as it was asked, because what it got wrong is as much a part
of the record as what it got right.

Raised over the seam that [`05-binding-contract.md`](05-binding-contract.md) §2 states, and recorded here
rather than answered there.

| # | Question, as asked | When answerable |
|---|---|---|
| OQ12 | Is the seam edge under-specified, and in what three respects? Its **cardinality** is fixed nowhere: whether one element may satisfy several requirements, and whether several elements may satisfy one. Every other edge in the collection fixes this and `satisfies` does not. The **check over the seam** that [`05-binding-contract.md`](05-binding-contract.md) §4.1 cites by name — whether every requirement in a baseline is satisfied by something — is stated in no document of the collection, and a declaration exists there to make a check computable that has never been written down. And whether the edge **pins a baseline** is undecided: a requirement's identity persists across baselines, so the edge as specified cannot distinguish satisfying a requirement as of one baseline from satisfying it as of another | Before the SysML v2 binding is written, not during it. That binding is phase 2's test of K2, and a phase filling holes in the seam while testing it cannot tell a false claim of symmetry from a gap it has just closed by hand. The question is a brainstorm's to answer rather than this collection's to settle in passing |

## Open question OQ13

Raised here, over a gap the founding record's own OQ6 already named and left unaddressed. OQ6 lists five
pieces of project-management work as this metamodel's material, on the ground that "not one of those five is
AV-specific." Four have since landed: an implementation declares its requirement kinds (K30); project-
management logic, in the Project Lifecycle Model (K22, K23); what produces a decision, partly — the Project
Lifecycle Model says when a gap becomes one; and how the model survives change (K5, K35). **The fifth, the
question lifecycle, is nowhere: `spec/` does not mention it.**

This is not a theoretical gap. The reasoning that dissolved OQ3 (K37–K39) identified the question lifecycle
as exactly what would hold the interval between somebody being asked and somebody answering — a real stretch
of project time during which a check goes on failing and nothing in the model records that the failure is
being waited on rather than ignored.

| # | Question | When answerable |
|---|---|---|
| OQ13 | Nothing in the metamodel records that a question has been put and that an answer is outstanding. A review finding's *closure* is already modelled — it is opened by a source and closed by a later source that `answers` it (K10, K11; `02-requirement-analysis-model.md` §9) — and a failed check or a question is itself recomputed rather than modelled (same section). What is missing is the *opening* of that interval and the interval itself: nothing distinguishes "this failure is being worked" from "this failure is being ignored," for as long as it stands unanswered | Nothing forces an answer before an implementation runs the loop and actually lives through that interval, so realistically phase 4. It is the last of OQ6's five pieces of work and, as of this decision record, the only one still unrecorded |

## Status of the founding record's open questions

| # | Status |
|---|---|
| OQ1 | Not answered, but shaped: the collection's dependency order is now the adoption order |
| OQ2 | **Answered** — K27, K28, K29, K30 |
| OQ3 | **Dissolved** — K37, K38, K39. Its premise did not hold: it assumed a need might oblige nothing, and a passage obliging nothing is not a need, so there is no disposition left to record |
| OQ4 | **Answered** — K22, K23 |
| OQ5 | **Deliberately deferred, in the founding record itself** — its own §7 says the name "waits for the rest on purpose"; not open by accident |
| OQ6 | **Settled enough to act on, in the founding record itself** — its own §7 says what remains of it "is settled enough by K15 and K16 to act on: the slot is metamodel, the list is implementation" |
| OQ7 | **Answered, in the founding record itself** — its own §4 names the winning option (one implementation, plus the SysML binding on paper) and the decision that settled its placement (K17) |
| OQ8 | **Answered, in the founding record itself** — its own §4 says the circularity is resolved by the four phases in §7, which order the work rather than qualify the freeze |
