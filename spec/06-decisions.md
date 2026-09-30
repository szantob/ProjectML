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

### What a `RequirementDefinition` is

| # | Decision | Reason |
|---|---|---|
| K25 | The seam test. An attribute belongs to the metamodel if the metamodel can interpret it without resolving a reference to an element it does not define. Prose that names design-language things is content, and content belongs to the implementation; a typed reference to a design-language element would be a second seam | K3's one-seam rule, applied attribute by attribute rather than only to the relation |
| K26 | The record test. An attribute belongs to the metamodel if a stated metamodel rule can fail on it — including on its absence — without reading its content | K7's posture, made into a criterion: a rule that might one day be written does not qualify |
| K27 | `RequirementDefinition` does not split into two types. It is one type with a core the metamodel interprets or can fail on, and declared points an implementation fills | A split would need a second type coordinated with the first, which is not the metamodel's job |
| K28 | `constraints` leaves the metamodel entirely. The metamodel does not name the concept. An implementation may introduce one | It fails both K25 and K26 on the same structural fact: it is a typed reference into elements the kernel does not define — a second seam |
| K29 | `verification` stays, on the definition rather than on the requirement | It passes both K25 and K26 — required and verifiable independently of any design language — and a verification method is generic to a kind, which places it on the definition rather than the instance |
| K30 | A requirement kind is a specialisation of `RequirementDefinition`, not an attribute on it, and `RequirementDefinition` is abstract | SysML v2's own mechanism for requirement hierarchies is specialisation, and a single kind attribute would fix the number of classification axes at one |

### This repository's own shape

| # | Decision | Reason |
|---|---|---|
| K31 | `spec/` carries one document per member of the collection, plus an overview, a binding contract and a decision record | The collection is the structure (K19), so `spec/`'s layout follows it rather than copying EventML's eight-file layout |
| K32 | The diagrams' vocabulary is a metalanguage, is descriptive only, and adopts existing conventions rather than coining any | CLAUDE.md already distinguishes drawing a metamodel from writing notation; house rule 10 applies to the metalanguage as well |

## Decision K33

Taken in [`01-requirement-model.md`](01-requirement-model.md), §2, which carries the full argument.

| # | Decision | Reason |
|---|---|---|
| K33 | A `Requirement` in the product model does not name the `RequirementDefinition` it came from, nor that definition's kind | K19's independent adoptability forces the exclusion; K13's recoverability condition permits it — the two are not symmetric here, and independent adoptability wins |

## Decision K34 — superseded by K35

**Superseded. K35 reverses it, and K35 is the decision in force.** K34 is kept here because a decision record
keeps its history: a later reader meeting the argument below elsewhere needs to find where it was answered.

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §10, which no longer carries
the argument — it carries K35's.

| # | Decision, superseded | Reason it was taken |
|---|---|---|
| K34 | The projection carries every requirement, whether in force or not. Being no longer in force is projected; nothing else the requirement analysis model adds is | The seam argument alone decides it: dropping retirement at the projection would let a requirement vanish there instead of changing state, the failure K5 exists to prevent, reintroduced at the seam. K12 is read narrowly rather than contradicted |

**Why it fell.** The seam argument that carried it conflated the live projection with a baseline. It reasoned
about a requirement retiring *between* baselines as though a design language were bound to something that
moves under it, when by K21 a design language binds to a named baseline and to nothing else, and a baseline
is frozen. Its second leg fell with the first: K12 needs no narrow reading, and none is taken.

## Decision K35

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §10, which carries the full
argument. K35 supersedes K34.

| # | Decision | Reason |
|---|---|---|
| K35 | The projection carries only the requirements in force. Being no longer in force is a property of the requirement analysis model, not of the product: a requirement that ceases to be in force is dropped at the projection, and no baseline cut afterwards contains it | K21 decides it. A design language binds to a named baseline, never to the live projection, and a baseline is frozen — an element satisfying a requirement in a baseline goes on satisfying a requirement that baseline still contains, so no seam edge can dangle. What K34 read as *vanishing* is the intended signal: a requirement missing from a later baseline is what tells the team to rework what was built on it, which is what rebasing onto a new baseline is for. Traceability is unharmed on two legs — the requirement analysis model holds everything, including what is no longer in force (K5, K11), and every element the projection carries resolves back to its origin there |

## Decision K36

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §8, which carries the full
argument. §12 of that document states the syntactic constraints the decision is measured against.

| # | Decision | Reason |
|---|---|---|
| K36 | The seam test (K25) decides whether an attribute is admissible to the metamodel. The record test (K26) measures whether an attribute already admitted is load-bearing; it is not a second admissibility gate, and the two are not a conjunction. *when it applies* is admitted by K25, does not pass K26, and stays in the core | A presence rule can be written over any attribute at will, so the record test read as a gate is either vacuous — every attribute passes, the admitting rule always being available — or arbitrary, with nothing to say which presence rules are worth writing. K26's word *stated* excludes the rule nobody has written, not the rule anybody could write in an afternoon. K25 has no such weakness: whether an attribute resolves a reference the metamodel does not define is a fact about the attribute, unchanged by which rules exist over it. The core's own definition already joins the two with *or* — what the metamodel can interpret, **or** can fail on. *when it applies* falls on the first clause only: no stated rule fails on it, because its absence is deliberately a gap rather than a claim, and the rule available over it reports a question instead |

## Decisions K37–K39

Taken in [`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §5 and §11, which carry the
full argument. Together they **dissolve** the founding record's OQ3 rather than answering it: OQ3 asks what
to record when a need is examined and found to have deliberately produced nothing, and K37 removes the state
of affairs it asks about.

| # | Decision | Reason |
|---|---|---|
| K37 | A need is a passage of a source that **obliges something**. A passage obliging nothing is not a need and should not have been extracted. A passage of a source is therefore either a `SourceNeed`, another `SourceStatement`, a `SourceQuestion`, or it is uncited: the model defines no context element, and none is to be invented | Extraction is already selective and always was, so *not need-bearing* is an existing category that needs no record — a passage nobody extracted leaves no element for a record to sit on, and a source permanently containing uncited text is normal rather than defective (D33). The reading that a need might oblige nothing at all was a misanalysis of statements about the environment the work happens in: such a statement constrains the environment the system must work within, and bears a requirement like any other need. The prior art held this without drawing the conclusion — its clustering of definitions by origin carries a class resolved by measuring rather than by asking an opinion, and its comparison with SysML v2 records that SysML's nearest category constrains the *system's* physical properties where this class describes the *environment* constraining the system. K6 is untouched: this narrows what a need is and gives a need no state |
| K38 | A need that no requirement refines is a **failed check**, not a question. It has exactly two resolutions — write the requirement the need obliges, or delete the need — and the criterion deciding between them is whether a declared definition covers the statement. This overturns D31 | It is the mirror of K9, which already moved the rule over a requirement carrying no origin edge from a question to a failed check: the two are one break in the chain read from opposite ends, and they were being treated asymmetrically for no stated reason. Once a need obliges something (K37), a need nothing refines is a record in which something obliged is unaccounted for, which is what a failed check says. The criterion is also the guard OQ3 feared the absence of: a requirement is produced only under a declared `RequirementDefinition` and names the one it was produced under (K30), so no requirement can be produced from a statement no declared definition covers, and the failure to find one is itself the signal that the extraction was wrong |
| K39 | A need extracted in error is **deleted**. Deletion is permitted for a need where K5 forbids it for a requirement | A source is material of record — quoted whole, never decomposed, never edited (D45, D25) — and a need is a pointer into a passage of it, so deleting the pointer leaves the passage unchanged in the source and loses nothing of record. A requirement is the working model's own construct with no other home: deleting one destroys the only record of it and silences the check that fired on it, which is what K5 exists to prevent. The asymmetry is between a construct and a pointer, not an inconsistency. A deletion here is not retirement and gives a need no lifecycle state (K6) |

## Decisions K40–K41

Taken in [`00-overview.md`](00-overview.md), §5, and in
[`02-requirement-analysis-model.md`](02-requirement-analysis-model.md), §5 and §11, which carry the full
argument. K40 states the second boundary the collection draws — between what the model guarantees and what
the modeller must do — and K41 refuses the one instrument that would blur it.

| # | Decision | Reason |
|---|---|---|
| K40 | **This metamodel's job is that no extracted information or decision is lost during the project-management process.** Whether every piece of information has been extracted, and whether every mapping is accurate, is not the model's responsibility but the modeller's: it can only be found by self-review or cross-review after the modelling is done. It follows that a model on which no check fails is not thereby a correct model — it is a model with no *detectable* error | A metamodel that claimed to guarantee completeness would be claiming what it cannot deliver. Completeness of extraction is measured against material the model does not hold — everything a source says that nobody took up — so the claim could never be tested, and a guarantee that cannot be tested devalues the ones that can. The line is the one K24 already draws between a syntactic constraint the metamodel decides and a semantic one it leaves to review, and the one K7 draws for contradiction, applied once more to the metamodel's own promise |
| K41 | The metamodel defines **no source-coverage report** — no report over which passages of a source no need cites — and **no metric over the completeness of extraction**. None is to be added. What it states over extraction is the one rule that runs the other way: a need that no requirement refines is a failed check (K38) | Two reasons, and each carries the decision on its own. **First, a source is free-form and its information density varies.** A salutation can be a twentieth of the text and none of the information, while a single clause buried in a paragraph can carry the only real constraint; a figure that puts those on one denominator says nothing, and a list of everything uncited is mostly noise by construction, because every source permanently contains uncited text (§5, D33). **Second, K10's own criterion says there is nothing to model.** K10 makes a finding a modelled element when it must keep its identity between reviews. A contradiction must: it cannot simply be fixed, it needs adjudication, and the next review has to see that somebody already found it. A missed extraction does not — the moment it is noticed it is extracted, so the finding and the fix are the same act, and nothing persists for a record to hold. That asymmetry is principled rather than convenient, which is why this sits beside K7 without contradicting it. The refusal is recorded rather than left as an absence because the prior art carries such a report (D33), and a later reader meeting it will otherwise propose adding one |

## Decision K42

Taken in [`03-project-lifecycle-model.md`](03-project-lifecycle-model.md), §2, which carries the full
argument.

| # | Decision | Reason |
|---|---|---|
| K42 | The list of what a rule-set may state is not closed at three — nor, since K74 added a fourth, at four. Three were what the evidence first found; an implementation needing to state a fifth kind of thing is evidence the metamodel must then account for, not a violation of it | The same structural argument K30 makes for a requirement kind's taxonomy applies one level over: a closed list here would fix one way of thinking about a process into the metamodel, which is exactly what K22 and K23 exist to refuse |

## Decisions K43–K50

Taken in [the design record of 2026-09-03 on the source-element hierarchy](../docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md)
§§2–5, which carries the full argument for each. Written into `spec/02-requirement-analysis-model.md` and
`spec/01-requirement-model.md` by [the integration plan of 2026-09-03](../docs/superpowers/plans/2026-09-03-source-element-hierarchy-integration-plan.md).

| # | Decision | Reason |
|---|---|---|
| K43 | The requirement analysis model's elements divide by which language they are in, not by *extracted* against *produced*. Information travels inward, from words to bound terms; questions travel outward | This is the distinction the metamodel already had for the `Need`/`Requirement` pair, generalised to everything else a source contains. *Extracted against produced* puts a question on the wrong side: it is produced, and still somebody's words addressed to a person |
| K44 | A source yields `SourceElement`s: abstract, carrying identity, an anchor into one passage of one source, and being material of record. Two specialisations exist and the list is not closed: `SourceQuestion`, and `SourceStatement`, itself specialised into `SourceNeed` and `SourceDecision` | Three properties shared across four types is a type rather than a coincidence. The list is left open on K42's own reasoning: a closed list fixes one way of reading a source, and a fifth kind arriving is evidence to account for |
| K45 | `SourceQuestion` and `SourceStatement` are siblings, not one beneath the other, because their discharge conditions differ in opposite directions: an unrefined `SourceStatement` is a failed check; an unanswered `SourceQuestion` is normal | Putting `SourceQuestion` beneath `SourceStatement` would report every unanswered question as a defect. A question asserts nothing; it opens something |
| K46 | Provenance stops at the source. The metamodel does not model what produced a stakeholder's words | A boundary, not a gap. It also explains why `Source` roots the chain: not because nothing precedes it, but because nothing before it is visible |
| K47 | A prefix names the side an element belongs to. `Source` and `Requirement` carry no prefix, and that absence is the signal that they are the model's two protagonists | The prefix marks which language an element is in, disambiguating a source-side question from a derived question and a recorded decision from what it does. The absence of a prefix on the two protagonists is normative |
| K48 | Implementation is the move between the two sides, in both directions. `SourceNeed`→`Requirement` and `SourceDecision`→`RequirementDecision` run inward; `RequirementQuestion`→`SourceQuestion` runs outward | The reversal follows the actor rule: the modeller's only sanctioned output is a question, so only a question originates on the model side and is realised in somebody's words |
| K49 | `RequirementDecision` and `RequirementQuestion` are elements of the model side. `RequirementDecision` is what a decision does to the requirement model; `RequirementQuestion` is what the modeller must find out | `RequirementDecision` resolves the deferral `spec/` recorded against itself — a decision with no edge to what it resolves. `RequirementQuestion` gives OQ13 its shape without answering it |
| K50 | Who authored a passage is a signal, not a dispatch. What an element is follows from what the passage does, never from who wrote it | The founding record's §5 already reaches this for a source's origin attribute. It disposes of edge cases — an experienced client's next-step question, a project manager asking what the model already holds — without new machinery |

## Decisions K57–K65

Taken in [the design record of 2026-09-03 on the source-element hierarchy](../docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md)
§§9–13, which carries the full argument for each. Written into `spec/` by
[the integration plan of 2026-09-03](../docs/superpowers/plans/2026-09-03-source-element-hierarchy-integration-plan.md).
K58 and K59 together close OQ15.

| # | Decision | Reason |
|---|---|---|
| K57 | `SourceElement`s carry no attribute beyond K44's three. `SourceNeed` carries no `value`; a need's value, once extracted, is a reading of the passage and belongs on the `Requirement` side, in `values` | `SourceElement`'s claim to carry only identity, anchor, and record-status should be true of every specialisation, not three of four. Nothing today checks `Need.value` through a stated syntactic constraint, so removing it costs no check anything currently runs |
| K58 | `refine` covers both of K48's inward moves: `SourceNeed`→`Requirement`, unchanged, and `SourceDecision`→`RequirementDecision`, newly | Both are the same mechanism under K43's axis. Coining a second name for a mechanically identical relationship would violate house rule 10 |
| K59 | `poses` is a new, coined edge: `RequirementQuestion`→`SourceQuestion`, K48's outward move | Nothing upstream of this metamodel has had to name a model realising itself outward into somebody's words, so nothing exists to adopt |
| K60 | A `RequirementQuestion` carries one of two states: raised (no `SourceQuestion` yet) or posed (a `poses` edge names one). The edge's presence is the transition. Further discharge is not stated here | This gives OQ13 the opening half of the interval it asks about without answering the interval itself, keeping this pass inside K48/K49's commitments and out of OQ13's territory |
| K61 | A `RequirementDecision`'s origin — `refine` from at least one `SourceDecision` — is mandatory. Missing it is not a failed check; the element is not well-formed at all, on K9's terms. An anticipated but not-yet-made decision is a `RequirementQuestion` in the raised state, never a bare `RequirementDecision` | K49's own definition of `RequirementDecision` as *what a decision does* presupposes the decision happened. This also removes the need for any dedicated edge between `RequirementQuestion` and `RequirementDecision`: the connection already traces through `poses` + `replies` + `refine` |
| K62 | `RequirementDecision` carries `retires`, an edge to zero or more `Requirement`s | This is the edge the first pass's own citation of the defect named directly: none of `Decision`'s five attributes named what it resolved |
| K63 | `RequirementDecision` carries an open/closed state; this record does not state what closes one. The criterion is left to the Project Lifecycle Model | What closes a `RequirementDecision` depends on the same unworked territory as `supersedes` and what finding it closes; stating the slot without the criterion matches `RequirementDefinition`'s own *"when it applies"* |
| K64 | An unrefined `SourceDecision` is a failed check, already covered by K45 and needing no restatement. A `RequirementDecision` without a `SourceDecision` is not a failed check at all — it is excluded by K61 as not well-formed. These are K24's two categories, not two severities of one defect | Confirms rather than revises: `SourceDecision` inherits K45 automatically as a `SourceStatement`; the other half was already K61 |
| K65 | The metamodel introduces no `Task`, or any output shaped like one, for a raised `RequirementQuestion`. The raised state is already the complete signal | Nothing is bought — a `Task` would duplicate what the state already carries, the same objection behind K41's refusal of a coverage report. `00-overview.md` §1 excludes this vocabulary by name |

## Decisions K66–K81

Taken in [the design record of 2026-09-04](../docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md)
§§2–13, which carries the full argument for each, except K81, taken in
[the integration plan of 2026-09-04](../docs/superpowers/plans/2026-09-04-project-lifecycle-model-integration-plan.md)
itself. Written into `spec/` by that same plan. K74 and K78 together answer OQ17's own question; the plan
narrows OQ17 to what was only ever gathered beside it, and opens OQ18–OQ19.

**Three of these rows have since been revised, and stand here as taken.** K68's cardinality is corrected by
K82; K76's description of the matching mechanism — not its verdict — by K86; and K77's classification is
narrowed by K89. All three are in
[the design record of 2026-09-04 on `Rule`'s shape](../docs/superpowers/specs/2026-09-04-rule-shape-design.md).
A decision record keeps its history, on the same terms K34 is kept beside K35.

| # | Decision | Reason |
|---|---|---|
| K66 | `RequirementDefinition` carries an eighth attribute: a well-formedness rule for the wording a requirement produced under it must satisfy | The seam test and the record test both pass, on the argument that already seated *how it would be verified* (K29): a well-formedness rule is exactly ISO/IEC/IEEE 29148's *characteristics of a good requirement*, generic to a kind rather than to one requirement |
| K67 | `Requirement` and `RequirementDefinition` are connected by an association — the *produced under* relation — never by inheritance. `Requirement` stays single and unspecialised regardless of how deep or wide the `RequirementDefinition` tree grows | Verified against SysML v2's primary specification: its kind hierarchy specialises `RequirementCheck`, "the base type of all `RequirementDefinition`s," entirely on the definition side, while `RequirementUsage` stays one uniform type. Confirms rather than revises what K33 already presupposed |
| K68 | A `RuleSet` belongs to a `RequirementDefinition` — zero or one per `RequirementDefinition` — not to a project as a whole | Follows `03-project-lifecycle-model.md` §2's own "stated per kind, not per definition," and narrows the search a check needs to run to walking one `RequirementDefinition`'s ancestry rather than filtering a project-wide collection |
| K69 | A `Rule` attached to a `RequirementDefinition` applies to every specialisation of it, not only to that node | A direct reading of the specialisation tree K30 already builds; needs no mechanism of its own |
| K70 | `spec/03` §1's "what an organisation loads" is corrected to name a project, not an organisation across projects | The phrasing survived from an earlier reading with cross-project tooling in mind — export, import, version comparison — which is tooling territory, not metamodel territory, on the same boundary K46 draws for provenance |
| K71 | `Rule` is abstract, and its specialisations divide by mechanism — what happens when the rule fires — not by `spec/03` §2's four descriptive categories, which remain subject-matter labels | The two axes do not correlate one-to-one: two different subject-matter rows share one mechanism shape, while the other two rows have no worked-out mechanism at all |
| K72 | Two mechanisms are worked out — `ConflictRule` and `CompletenessRule`; two more (silent-vs-owned-default, gap-timeout) are not, and stay open as OQ18 | Named directly rather than glossed over: this record does not claim the other two share this shape merely because they would sit in the same abstract type's list |
| K73 | A conflict-detecting `Rule`, when it finds that a new `Requirement` contradicts an existing in-force one, raises a `RequirementChoice` | Sharpens `spec/03` §2's third row, which describes only the resolution half; detection is the other half a `Rule` must also carry |
| K74 | A completeness-detecting `Rule`, when it finds that a `RequirementDefinition` kind is present without an implied companion kind, raises a `RequirementInquiry` | Closes OQ17's own original case: the rule-set's fourth statement, now given a mechanism rather than only a name |
| K75 | A completeness check is set-level, not per-instance: it asks whether at least one `Requirement` of the implied kind exists anywhere the rule's `RequirementDefinition` reaches. At most one open `RequirementInquiry` per `Rule` at a time; a new triggering `Requirement` extends it | Resolves a cost concern about a growing model re-triggering the same rule combinatorially, by reading the check as a query over current state rather than a per-instance obligation |
| K76 | Rule-matching — whether a `Rule`'s free-text condition holds of a free-text `Requirement` — is a semantic constraint (K24), not a syntactic one | Both a `Rule`'s condition and a `Requirement`'s wording are prose; nothing decides a match without reading content, on the same terms K40/K41 already hold extraction completeness to |
| K77 | `RequirementQuestion` — both specialisations — belongs to the *review finding* family in `spec/02` §11's three-way table, not to the *failed check* or *question* rows | It is judged, modelled, and carries state — the three properties that table already uses to seat *review finding* apart from the other two. No change to `RequirementQuestion` follows; this is a naming of what it already is |
| K78 | `RequirementQuestion` is abstract, and carries, beyond identity and its raised/posed states: a free professional-register statement of the question; a reference to the `Rule` that triggered it; and a list-valued list of every triggering `Requirement`, which may grow while the question stays open | The earlier concern that a "which rule" reference would open a second seam dissolves once `Rule` is itself metamodel-defined: referencing it is no different from `Requirement` referencing `RequirementDefinition` |
| K79 | `RequirementQuestion` specialises into `RequirementInquiry` and `RequirementChoice`, one per mechanism K72 names. Both carry `discharges`, optional, to whatever closes them — `RequirementInquiry` to a `Requirement`, `RequirementChoice` to a `RequirementDecision` | `discharges` is coined rather than reusing `replies`: `replies` is a `Source`↔`Source` evidentiary edge, and this is a different kind of relationship, on the model's own side of K43's axis. The word is already live in this collection's vocabulary, in OQ13's "its discharge" |
| K80 | `RequirementInquiry` carries nothing beyond the shared shape and `discharges`. `RequirementChoice` additionally carries the candidate alternatives being decided among | A completeness gap names a missing kind, which the shared shape already identifies in full. A conflict needs its options named before anyone can decide among them, and these alternatives prefigure `RequirementDecision.the choice` once discharged |
| K81 | `ConflictRule` and `CompletenessRule` name K73 and K74's two worked `Rule` mechanisms | Neither name is stated in the design record, which refers to them only descriptively. No standard names a rule-firing mechanism of this shape, so house rule 10's coining clause applies; each name is taken directly from the descriptive phrase that already identifies it, on the same grounds K59 coined `poses` from K48's own description. Recorded here rather than in the design record because it is this plan's own naming decision, not one the design record itself took |

## Decisions K82–K89

Taken in [the design record of 2026-09-04 on `Rule`'s shape](../docs/superpowers/specs/2026-09-04-rule-shape-design.md),
which carries the full argument for each. Written into `spec/` by
[the integration plan of 2026-09-04](../docs/superpowers/plans/2026-09-04-rule-shape-integration-plan.md).
Together they close OQ20 and open OQ21–OQ23. K82 revises K68, K86 revises K76's description of the mechanism
but not its verdict, and K89 narrows K77.

| # | Decision | Reason |
|---|---|---|
| K82 | Exactly one `RuleSet` belongs to each `RequirementDefinition`, and it may be empty. Revises K68, which made it zero or one | The distinction between no `RuleSet` and an empty one carries no meaning, and removing it removes a null case from every reading of the structure |
| K83 | A `Rule` directs attention rather than prescribing an outcome: it states which subjects must be dealt with, never what the resulting requirement should say. A negative answer to a subject it raises is a full answer, appearing as a `Requirement` the `RequirementInquiry` discharges to; while none exists the question stands open (K79). That closing `Requirement` is produced the ordinary way, never by the question itself | A rule that prescribed content would state what a requirement's wording should be, which `03-project-lifecycle-model.md` §1 already puts outside a rule-set's territory and K66 places on `RequirementDefinition`. Keeping the closing `Requirement` on the ordinary route protects K11 and the actor rule at once, and leaves a record for a subject raised and declined |
| K84 | A `Rule` carries an identity, a *when it applies*, and a *what to consider*. The latter two are prose fields, neither an evaluable expression | The split lets the relevance judgement read one field rather than the whole rule. Neither form is coined: `RequirementDefinition` already carries a *when it applies* on these terms (D20), and a *what to ask* raising a missing parameter where *what to consider* raises a missing subject |
| K85 | A `Rule`'s identity is local to the `RequirementDefinition` that owns it; the full identifier is the composition of the two. How that composition is written down is not fixed | Locality follows the structural attachment K68 establishes. Refusing to fix the spelling is K15 holding — a composed identifier written out would be notation — and an identifier space is what K4's third declaration leaves to an implementation |
| K86 | Rule-matching is a relevance judgement made while walking a `RuleSet`, not the evaluation of a condition against a `Requirement`. The semantic classification K76 gives is unchanged; its description of the mechanism is corrected | A rule-set is a written procedure walked when a requirement arises in its subject, and a reader judges which entries are relevant. The correction also shows *subject* needs no new concept: the `RequirementDefinition` hierarchy is the subject hierarchy, so the rules relevant in a subject are those K69's inheritance already reaches |
| K87 | A `RequirementQuestion`'s *triggered by* is mandatory, without exception. `spec/02` §11's wording, which read a `RequirementDefinition`'s *what to ask* as a second origin, is corrected | The rule-set is the procedure: amendable, but not departable-from. A modeller who finds no rule covering something proposes a rule, and proposing is as far as the modeller goes, since adding one commits the project and is the project manager's act. Every `RequirementQuestion`'s cause is therefore named, not merely guaranteed |
| K88 | A `Rule` carries no provenance. The metamodel does not record what produced a rule or who approved it | K46's boundary from the other side: a rule-set is one of the procedures K46 says the metamodel cannot see behind. The chain does not break where it matters — a rule commits nothing and decides nothing, and the project manager's answer to the question it raises enters as a source under K11 |
| K89 | A `RequirementQuestion` is not a *review finding* and belongs to no row of `spec/02` §11's findings table. It is the product of a third checking mode: static model checking needs no judgement, walking a `RuleSet` does and produces a `RequirementQuestion`, review does and produces a review finding. Narrows K77 | The table classifies what a *review* produces (K10), and walking a rule-set is ordinary modelling work, not an act of review. The table's own rules confirm it: a review finding is opened by a source where a `RequirementQuestion` is raised by a rule firing, and nothing marks a finding closed directly where `discharges` does. K77 saw only the judgement/no-judgement distinction |

## Decisions K90–K100

These come from
[`docs/superpowers/specs/2026-09-07-rule-specialisations-design.md`](../docs/superpowers/specs/2026-09-07-rule-specialisations-design.md),
which gives the `Rule` specialisations the shape the record before it deliberately left them without, and
from the plan that wrote them into `spec/`. They were reached by asking where `ConflictRule` and
`CompletenessRule` had come from at all — a question the corpus answered badly, the two having been measured
and exampled rather than derived.

| # | Decision | Reason |
|---|---|---|
| K90 | A `Rule` specialisation is fixed by what its firing test ranges over — `ConflictRule` a pair, `CompletenessRule` a set — and every other difference follows from it. This supersedes K71's account of the axis | K71 divided the specialisations "by mechanism" while the same paragraph said both share one mechanism shape, which cannot both hold. The range makes each difference derivable rather than stipulated: what is chosen among, how many questions may stand open, and whether the firing is a judgement at all |
| K91 | `Rule`'s third attribute is *what to look for*, renaming K84's *what to consider* | K84's description — the subject a rule raises — is exact where the thing sought is an absence, because there the two readings are one sentence, and false where it is a present clash. The subject-reading is the special case, so the common attribute is named for the general one |
| K92 | A `Rule`'s attachment determines what triggers it, not what its test ranges over. This corrects K75's reach phrase, not its set-level verdict | A rule may look for requirements produced under a definition nowhere in its owner's subtree, so a test confined to that subtree would find nothing in any project and the rule would fire always |
| K93 | A `CompletenessRule` names the kind it implies by a typed reference to a `RequirementDefinition` | Necessary rather than merely admissible: the implied kind is absent when the rule fires, so nothing about it can be read and it must be named in advance. The seam test passes, `RequirementDefinition` being metamodel-defined, which is the whole difference from the candidate K28 rejects; the record test passes and is load-bearing |
| K94 | Exactly one implied `RequirementDefinition` per `CompletenessRule` | Forced by K75 and by `discharges` naming exactly one `Requirement`: a rule naming several implied kinds, more than one missing, would open a single inquiry no single requirement could discharge |
| K95 | The completeness query asks whether at least one in-force `Requirement` produced under the implied definition, or any specialisation of it, exists in the project model | Each qualification carries weight: a retired requirement does not fill a gap (K5); a more specific kind satisfies a more general implication (K30); and the range is the model, not the owner's subtree (K92) |
| K96 | A `ConflictRule` carries nothing beyond the common shape | Its partner is present when it fires and the judgement reads it anyway, so prose suffices. A typed reference here would narrow mechanically what reaches judgement, which is OQ21's guard and belongs there |
| K97 | A `Rule` is never deleted; it carries one of in force or no longer in force, and taking one out of force is the project manager's act | A deletable `Rule` withdraws the target of K87's mandatory *triggered by*, taking back the claim that a question's cause is named. The vocabulary is K5's rather than a synonym beside it. K88 is untouched: a state is not provenance, and the cause of a retraction still enters as a source under K11 |
| K98 | The universal check — no requirement may contradict an in-force one — is not a `Rule`, and this model states none. What it covered divides among the conflicting value state, a cross-kind `ConflictRule`, and review. K73's canonical case and K69's illustration are replaced; neither mechanism changes | All four of a `Rule`'s attributes degenerate on it, which marks something that is not a rule rather than a badly written one. Its first destination is decisive on its own: it duplicated, one level up, a mechanism `04-value-states.md` already had. No fourth checking mode is needed, K89 having already named the one that fits |
| K99 | `spec/03-project-lifecycle-model.md` gains a syntactic-constraints section as its final section, in the shape `01-requirement-model.md` §5 and `02-requirement-analysis-model.md` §12 use | K93, K94 and K97 give the document constraints it never had. Following the two siblings costs no renumbering, nothing in the repository citing that document's §4 or §5. This is the plan's own structural decision, which the design record left to it deliberately, recorded here on the precedent K81 sets |
| K100 | OQ22 is narrowed to one remaining part. K90 answers whether one firing yields one question or several — one per pair for a `ConflictRule`, at most one for a `CompletenessRule` — and answers whether K75's at-most-one is general or specific to `CompletenessRule`, by deriving it from the set-level range. What stays open is whether *what to look for* supplies a template for the question's wording | Both answered parts fall out of K90 rather than being decided separately, which is why they are recorded as a narrowing here rather than as decisions of their own. Also this plan's own, on K81's precedent |

Together they narrow OQ18 and OQ22 and open OQ24–OQ26. Four earlier decisions are revised: K90 supersedes
K71's account of the specialisation axis; K91 renames K84's third attribute; K92 corrects K75's statement of
what its check ranges over, leaving its set-level verdict intact; and K98 replaces K73's canonical case and
K69's illustration without changing either mechanism. The superseded rows stay above as they were taken, on
the same terms K34 is kept beside K35.

## Decisions K101–K108

Taken in [the design record of 2026-09-28 on value domain comparability and the guard](../docs/superpowers/specs/2026-09-28-value-domain-comparability-and-guard-design.md),
which carries the full argument for each. Written into `spec/` by
[the integration plan of 2026-09-28](../docs/superpowers/plans/2026-09-28-value-domain-comparability-and-guard-integration-plan.md).
Together they close OQ21 and OQ27, narrow OQ9, and open OQ29. K102 adds to the shape K84, K91 and K97 give a
`Rule`, and K105 adds a step to the walk K86 and K97 describe; neither revises them.

| # | Decision | Reason |
|---|---|---|
| K101 | A value domain fixes no unit. It declares one of three levels of comparability: not comparable, comparable for equality, or ordered, which includes equality. How the level is achieved is the implementation's | A guard compares one parameter's value with a constant written against that parameter, never values of two domains, so what it needs from a domain is which operations are defined, not a unit. Neither `ConflictRule`'s test nor the conflicting state needs a comparison made by an algorithm |
| K102 | A `Rule` may carry a guard: a list of criteria, each naming a parameter of the owning definition — declared or inherited — an operation, and a constant. It belongs to the shape every `Rule` has | OQ21 named the missing half as the rule-side criterion and the join; the criterion is that half and the parameter it names is the join. A guard only excludes, so what it admits is judged as before, and the firing test's range is untouched (K92) |
| K103 | A criterion is decided only on a stated or derived value. On an assumed, unknown or conflicting value, or an absent parameter, it is undecided | A guard concludes that a rule does not concern a requirement, and that is only as firm as the value. An assumption is the value somebody may need to correct, and excluding on it would let a wrong assumption silence a rule unread |
| K104 | The criteria of a guard are conjunctive: a rule is excluded when at least one is decided false. A disjunction across parameters is two rules | One criterion decided false decides the conjunction, so exclusion stays decidable when others are not. *Is one of* covers disjunction over one parameter; anything more would make a filter a language |
| K105 | The guard is applied after the rules not in force are set aside and before the relevance judgement. An excluded rule raises nothing | Both steps only remove, so their order changes no outcome. The guard narrows what reaches judgement and adds no category beside K24's two, as OQ21 required |
| K106 | A parameter carries an identity local to the definition declaring it | A criterion must name a parameter, and *what to ask* is already per parameter. Locality follows K85's precedent for a `Rule` |
| K107 | A specialisation has every parameter its ancestors declare, in addition to its own. This answers the part of OQ9 concerning added parameters, and nothing else | Without it an inherited rule's guard would be undecided on every descendant. Decided directly by the owner rather than left as a side effect of the guard |
| K108 | A specialisation declares no parameter carrying the identity of one an ancestor declares | K107 is safe only if a guard's parameter is the same parameter, with the same domain, on every descendant. Forbidding redeclaration is the reversible choice: lifting it later breaks nothing, withdrawing a permission breaks every package that used it |

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

## Open questions, OQ9–OQ10

Raised in [the design record of 2026-09-02](../docs/superpowers/specs/2026-09-02-spec-structure-and-oq2-design.md),
which carries the full argument for each.

| # | Question | When answerable |
|---|---|---|
| OQ9 | What does specialisation mean? What a subtype of `RequirementDefinition` may add, narrow or override. K30 chooses the mechanism and does not define its semantics | When something exercises it — realistically phase 4, when the first kinds are declared |
| OQ10 | Does `verifies` become a second edge kind on the one seam? SysML has a construct for it, `verify`, in the same direction as `satisfies` but not the same shape — it is carried by a whole verification case, not by an arbitrary element, so finding it asks a different question of a design language than finding `satisfy` does. Widening K4's first declaration by one word does not carry it; it would need a declaration of its own, and only once the kernel decides it wants a check over verification the way it already has one over satisfaction. Nothing exercises it: no verification elements exist anywhere yet | Phase 2, where the SysML binding meets it, or later |

**OQ9 is narrowed by K107 and K108.** A specialisation has its ancestors' parameters and may not redeclare
one. What stays open is whether an inherited parameter may ever be overridden or narrowed, what
specialisation means for the rest of the core, and whether an inherited parameter must appear in a
descendant's template.

## Decision K56

Taken in [the design record of 2026-09-03 on phase 2](../docs/superpowers/specs/2026-09-03-sysml-binding-approach-design.md)
§3, which carries the full argument. K56 closes OQ11.

| # | Decision | Reason |
|---|---|---|
| K56 | **The metamodel does not need a subject.** Where a design language carries the concept of a requirement's subject, it is entirely that design language's own internal affair, covered by K4's second declaration; where a design language carries no such concept, nothing depending on the kernel notices the absence. The seam edge neither supplies a subject nor needs to | Checked against two design languages rather than reasoned about in the abstract, which is what phase 2 exists to do. SysML v2 carries the concept deeply — a `SubjectMembership` declared on the requirement itself, independent of `satisfy`, and narrowed per requirement kind by its own standard library (`FunctionalRequirementCheck`, `PhysicalRequirementCheck`, and others) through the same specialisation mechanism K30 already chose for this metamodel. EventML, frozen prior art, carries none: its `Requirement` has no subject-shaped attribute, and `Part.satisfies` is an untyped list of identifiers. Both attach to the one-field seam on the same terms. A subject synthesised by the seam itself, richly for the first and out of nothing for the second, is exactly the shape a false K2 would take (OQ11, as raised); refusing to synthesise one at all is the reading that stays symmetric across both |

## Open question OQ11 — answered

**Answered by K56, and no longer open.** SysML v2 turned out to answer half the question on its own terms —
its `satisfy` is defined in terms of a subject the requirement already carries, not one the seam invents —
and EventML answered the other half by carrying no subject concept at all, with nothing breaking as a
result. Neither needed the kernel to supply, carry or synthesise anything for `satisfies` to work, which is
what K56 records.

Raised in [`05-binding-contract.md`](05-binding-contract.md) §5, and recorded here rather than answered
there.

| # | Question, as asked | When answerable |
|---|---|---|
| OQ11 | Does the metamodel need a subject? A design language may require every requirement to name the element it is a requirement of, and the seam edge, by naming both a satisfying element and the requirement it satisfies, may already supply that on its own. Whether it does, or whether a binding must synthesise a subject where the seam does not supply one, is not decided here. Getting this wrong is the shape a false K2 would take — a subject synthesised one way for one design language and another way for another would quietly reopen the privileged path K2 rules out — which is exactly why it is left to the phase built to test K2, rather than guessed at here | Phase 2 — this is the binding's job to settle |

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
question lifecycle, is only partly recorded: `spec/02` gives `RequirementQuestion` raised/posed states
(K60), so the interval's opening is there, but the interval itself and its discharge are nowhere.**

This is not a theoretical gap. The reasoning that dissolved OQ3 (K37–K39) identified the question lifecycle
as exactly what would hold the interval between somebody being asked and somebody answering — a real stretch
of project time during which a check goes on failing and nothing in the model records that the failure is
being waited on rather than ignored.

| # | Question | When answerable |
|---|---|---|
| OQ13 | Nothing in the metamodel records the interval between a question being put and an answer arriving, or how that interval closes. A review finding's *closure* is already modelled — it is opened by a source and closed by a later source that `replies` to it (K10, K11; `02-requirement-analysis-model.md` §11) — and a failed check or a question is itself recomputed rather than modelled (same section). K60 already supplies the *opening* half, through `RequirementQuestion`'s raised/posed states. What remains open is the interval itself and its discharge: nothing distinguishes "this failure is being worked" from "this failure is being ignored," for as long as it stands posed and unanswered | Nothing forces an answer before an implementation runs the loop and actually lives through that interval, so realistically phase 4. It is the last of OQ6's five pieces of work and, as of this decision record, the only one whose interval and discharge remain unrecorded |

## Open questions, OQ14 and OQ16

Raised in [the design record of 2026-09-03 on the source-element hierarchy](../docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md),
which carries the full argument for each. OQ17, raised in the same record, is narrowed below rather than
listed here, once [the design record of 2026-09-04](../docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md)
answered its own two-part question.

| # | Question | When answerable |
|---|---|---|
| OQ14 | Is there a model-side abstract type, as `SourceElement` is for the source side? The owner's judgement is that one will probably be needed and that the side is not yet fully seen | When the model side is understood as well as the source side is |
| OQ16 | How does `SourceQuestion` subdivide, and does it name the party expected to reply? | With OQ13, realistically |

## Open question OQ17 — narrowed

**Its own two-part question is answered, and is not what stays open.** What the Project Lifecycle Model's
fourth rule-set item looks like is `CompletenessRule` (K74); how a `RequirementQuestion` references what it
fires on is the *triggered by* and *triggering `Requirement`s* attributes K78 gives every
`RequirementQuestion`. Both are settled in
[the design record of 2026-09-04](../docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md) and
written into `spec/` by
[the integration plan of 2026-09-04](../docs/superpowers/plans/2026-09-04-project-lifecycle-model-integration-plan.md).

**What was only ever gathered beside OQ17, never part of its own question, is what remains open:
`supersedes`, what finding a `RequirementDecision` closes, and what closes a `RequirementDecision` itself.**
None of the three was answerable without `spec/03-project-lifecycle-model.md` being worked out further than
the 2026-09-04 record takes it, and none of the three is answered by that record either — K63 already records
the closure criterion as deferred, and nothing here revisits it. This is not the same shape as OQ12's third
part, which dissolved on a false premise: nothing here rests on a mistaken assumption, the territory is simply
still unworked, which is why this question is narrowed rather than marked answered.

Raised in [the design record of 2026-09-03 on the source-element hierarchy](../docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md)
§14, and narrowed here rather than there.

| # | Question, as asked | When answerable |
|---|---|---|
| OQ17 | What does `supersedes` mean; what finding does a `RequirementDecision` close; and what closes a `RequirementDecision`? Originally gathered alongside the fourth rule-set item and the `RequirementQuestion` reference mechanism, both answered above, because none of the three was answerable without `spec/03-project-lifecycle-model.md` being worked out first | A further session on the Project Lifecycle Model, once `supersedes` itself is worked out |

## Open questions OQ18–OQ19

Raised in [the design record of 2026-09-04](../docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md),
which carries the full argument for each.

| # | Question | When answerable |
|---|---|---|
| OQ18 | What mechanism do a silent-vs-owned-default `Rule` and a gap-timeout `Rule` carry? Narrowed rather than answered: placed on K90's axis, neither ranges over a pair or a set, and neither shares the "detect, then raise a `RequirementQuestion`" shape. The first ranges over a single value, and its shape is derivable — fire on a value in the assumed state, mark it as one to ask about, raise no `RequirementQuestion`, since `02-requirement-analysis-model.md` §11 already routes a single parameter through the definition's own machinery — but it would rest on a construct with no stated cause, which is OQ25. The second ranges over an open question and elapsed time, and needs two things this collection lacks: a date on a `RequirementQuestion`, and an edge by which an escalation names what it escalated. Both are OQ13's | The first with OQ25, which is close and small. The second with OQ13, realistically phase 4 |
| OQ19 | Does a baseline need to name which `RuleSet`(s), and which version of each, it was checked against — separately from the implementation package and version `01-requirement-model.md` §4 already names? K68 makes a `RuleSet` per-`RequirementDefinition` rather than a single project-wide version, which may mean this question is really *N* small questions — one per `RequirementDefinition` a baseline's requirements touch — rather than one | Needs `01-requirement-model.md` §4 read again with K68–K70 in view |

## Open question OQ15 — answered

**Answered by K58 and K59, and no longer open.** It asked whether implementation is a new edge or the
generalisation of one that exists; the answer is neither alone — the inward half generalises `refine`
(K58), the outward half is the new, coined `poses` (K59), and no umbrella "implementation" edge sits above
both.

Raised in [the design record of 2026-09-03 on the source-element hierarchy](../docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md)
§6, and recorded here rather than answered there.

| # | Question, as asked | When answerable |
|---|---|---|
| OQ15 | Is implementation a new edge, or the generalisation of one that already exists? The refinement edge already runs from a requirement to the needs it refines, which is the inward half of K48 under another name — and that name is adopted from SysML v2, so it cannot simply be replaced | With OQ14, and for the same reason: it is a question about the model side |

## Open question OQ20 — answered

**Answered by K82–K89, and no longer open.** Its three parts were answered, but not in the form they were
asked — two corrections reframed the question first. `Rule`'s attributes are K84's three plus K85's
locality, reachable only once K83 settled what a rule is *for*. *triggered by* turned out to need no
optional form at all: K87 makes it mandatory without exception, because a rule-set is a procedure that can
be amended but not departed from. And K77's classification was not qualified but narrowed — K89 finds that
`RequirementQuestion` belongs to no row of the findings table, being the product of a third checking mode
the table never named. The question is kept below as it was asked.

Raised in this session's own whole-branch review of the K66–K81 integration (2026-09-04), not in a design
record — three related gaps in `Rule`'s shape, found only once the material was read as a finished whole
rather than task by task.

**`Rule` has no stated attributes.** `spec/03-project-lifecycle-model.md` §3's `### Rule` subsection says only
that the type is abstract and specialises by mechanism; it names no identity, no condition, nothing. Yet K76
already reasons about *"a `Rule`'s free-text condition"*, and `spec/02-requirement-analysis-model.md` §11
already gives `RequirementQuestion` a *triggered by* attribute holding *"a reference to the `Rule` ... that
fired"* — a reference that needs an identity to name. Every other new type this collection defines gets an
attribute table opening with identity; `Rule` and `RuleSet` are the only ones that do not.

**Whether *triggered by* is optional is unstated.** `spec/02` §11 states it as one of three things carried by
*"every"* `RequirementQuestion` specialisation, without qualification. But the same section's own "Where a
`RequirementQuestion` comes from" discussion names a `RequirementDefinition`'s *what to ask* gap (§7) as one origin
of a `RequirementQuestion` — and that origin is not a `Rule` firing. Either that case does not actually
produce a `RequirementQuestion` in the sense this document means, or *triggered by* needs to be optional the
way `discharges` already is, stated explicitly rather than left to be inferred.

**K77's classification does not fully hold.** K77 seats `RequirementQuestion` — both specialisations — in
`spec/02` §11's *review finding* family, on the ground that it is judged, modelled, and carries state. But
that family's own stated rules do not all hold of it: a review finding *"is opened by a source"* (§11's
Findings subsection), where a `RequirementQuestion` is raised by a `Rule` firing over the model, not by a
source entering it; and *"nothing marks a finding closed directly"*, where `discharges` does exactly that.
Whether the classification needs qualifying, or the findings family's own stated rules need to admit an
exception, is open.

| # | Question | When answerable |
|---|---|---|
| OQ20 | What is `Rule`'s full attribute shape (at minimum, whatever an identity and a condition require); is `RequirementQuestion`'s *triggered by* optional, and if so what marks its absence; and does `RequirementQuestion`'s membership in the *review finding* family need qualifying against that family's own stated rules? | A dedicated session on `Rule`'s shape — naturally alongside OQ18, since working out the two still-unworked `Rule` mechanisms (silent-vs-owned-default, gap-timeout) needs `Rule`'s actual attribute shape settled first, and this question is what that session would need to resolve anyway |

## Open questions OQ21–OQ23

Raised in [the design record of 2026-09-04 on `Rule`'s shape](../docs/superpowers/specs/2026-09-04-rule-shape-design.md),
which carries the full argument for each.

| # | Question | When answerable |
|---|---|---|
| OQ21 | Should a `Rule` carry parameter criteria, letting an algorithmic filter run before the semantic judgement? The shape is a guard, as a flowchart uses the word: it could *exclude* a rule mechanically, never admit one, so the judgement K86 describes still decides everything reaching it — a narrowing of what reaches judgement, not a third category beside K24's two. Half the machinery exists: a `RequirementDefinition` declares `parameters` and a `Requirement` carries `values`. The `Rule`-side criterion and the join between them are missing | When the cost of judging every rule in a walked `RuleSet` is actually felt, which needs an implementation running the loop. Until then it is an unexercised construct and waits |
| OQ22 | How does a fired rule become a posed question? Narrowed by K100: what remains is whether *what to look for* supplies a template for the question's wording, or the modeller writes it freely. The other two parts are answered — K90 gives one question per contradicting pair and at most one per `CompletenessRule`, the latter specific to that specialisation rather than general, and `03-project-lifecycle-model.md` §3 now states the relation between judging relevance and firing that this question was partly about | Unforced. What remains is a question about wording, which nothing exercises until an implementation writes questions for real |
| OQ23 | What is a review, as an act? `spec/02` §11 states a review finding's lifecycle — a source opens it, a later source that `replies` to it closes it — but nothing states the act producing one: who performs it, when, against what. Only static model checking and, since K86, walking a `RuleSet` are worked out | Unforced. Recorded because K89 makes the gap visible while deliberately not entering it |

**OQ21 is answered by K102–K105 and is no longer open.** A `Rule` may carry a guard; the criterion and the
join OQ21 found missing are K102's criterion and the parameter it names. It was answered before the cost of
judging every rule was felt, because OQ27 needed it: the guard is the first construct that compares values
without judgement, so what a value domain declares could not be settled without it.

## Open questions OQ24–OQ26

All three were opened by the `Rule` specialisation work rather than by anything it settled. Each is recorded
where it was found rather than pursued, and the full argument for each is in
[`docs/superpowers/specs/2026-09-07-rule-specialisations-design.md`](../docs/superpowers/specs/2026-09-07-rule-specialisations-design.md).

| # | Question | When |
|---|---|---|
| OQ24 | When do two disagreeing sources produce one `Requirement` carrying a conflicting value, and when two `Requirement`s? K98's first destination assumes the former — `04-value-states.md` §2's **conflicting** state holding both competing values with their sources — and the model permits it without requiring it: `refine` is list-valued precisely so one requirement may be assembled from several statements (`02-requirement-analysis-model.md` §10), but nothing forbids two in-force requirements of the same kind carrying incompatible values. That would be a refinement error, and no stated rule catches it. It cannot become a syntactic constraint, because deciding whether two requirements are about the same thing is a judgement | Raised deliberately rather than settled inside K98, being a question about the derivation rather than about rules. It is the residue K98's third destination — review — would otherwise absorb silently |
| OQ25 | What produces `04-value-states.md` §2's marking of a value as one to ask about? The marking occurs once in the whole of `spec/` and nothing states its origin, which the rule that every event record its cause makes a defect however sensible it reads. Two origins are available — a modeller's judgement, and a `Rule` — and whether the marking is modelled or recomputed cannot be settled until it is known which applies when. That document permits stated, derived and conflicting values to be marked as well as assumed ones, so the question is not confined to defaults | Before OQ18's first half, which it blocks. Small, and close |
| OQ26 | Does K83's negative-answer path hold? K83 says a subject a rule raises may be answered negatively, and that the answer appears as a `Requirement` like any other, which the `RequirementInquiry` then `discharges` to. This rests on two premises the corpus never states: that "this project needs nothing here" obliges something, as K37 requires of every `SourceNeed`; and that some declared `RequirementDefinition` covers a negative statement, as K8 requires of every requirement. The second has a plausible answer — the implied kind's own definition — and the first may be false in a way that matters: a decision to need nothing reads more naturally as a `SourceDecision` refining into a `RequirementDecision`, and K79 fixes `RequirementInquiry`'s `discharges` on a `Requirement`, which would not admit it | Used by K97's statement that a retracted rule leaves its open questions to close the ordinary way, but never worked, so it is recorded rather than settled |

## Open question OQ27 — answered

**Answered by K101, and no longer open.** A value domain fixes no unit; it declares a level of comparability,
which is what a `Rule`'s guard needs, since a guard never compares values of two domains. The two
constructs the question named turned out not to need an algorithmic comparison: a `ConflictRule`'s test is a
judgement, and the conflicting state does not say what establishes a disagreement. The package that raised it
is correct under K101 — two ordered domains, each in its own unit.

Raised by the first implementation package written with a working editor: a live-event AV company's own
domain, declaring twenty-six value domains, among them one for a length in metres and one for a height in
centimetres. Both are lengths. They differ in nothing but their unit, and the package uses each for a height
of a different piece of equipment. Nothing in this collection forbids that, and nothing in it says whether it
is right.

| # | Question | When answerable |
|---|---|---|
| OQ27 | Does a value domain fix a unit, and if it does, what makes two values comparable? `04-value-states.md` §5 leaves *which* domains exist to an implementation, but leaving the set open is not the same as leaving a value's **shape** open, and two constructs already assume values can be compared. `04-value-states.md` §2's **conflicting** state holds competing values, each with its source — competing presupposes comparable. `03-project-lifecycle-model.md`'s `ConflictRule` fires when a requirement contradicts an in-force one, and its canonical case is a headcount falling short of another (K73) — a comparison, not a textual difference. If a domain fixes a unit, then 2 m and 200 cm sit in different domains, a genuine contradiction between them is undetectable, and an implementation needs one domain per pair of measure and unit. If a domain does not fix a unit, a value must carry its own, and nothing here says a value has that structure | When something actually compares two values: a `ConflictRule`'s firing test, or the **conflicting** state being populated from two sources. Both need an implementation running a walk over a project model, which is why the question could not arise until one existed. Until then it is an unexercised construct and waits |

## Open question OQ28

Raised by the same implementation package as OQ27. Several of its rules put a **computation** into *what to
look for*: a height rounded up to a module size and a piece count derived from it; a count of support parts
derived from a width, with extra bracing above a threshold height; a port count derived from a pixel total,
with the named kit that covers it. That is not what has to be found. It is what the answer is.

**K83 and K91 are not ambiguous about this.** A `Rule` directs attention and never states what the resulting
requirement should say; *what to look for* carries what the rule seeks, "never what the answer should be"; and
`03-project-lifecycle-model.md`'s `CompletenessRule` puts it flatly — an implied kind, yes; an implied
parameter value, never. The package holds the line elsewhere in its own rule-set, which is why this reads as a
displaced thing rather than a misunderstanding: one of its rules detects that a limit on a measure is exceeded
and hands over to a decision, and another rule names the alternatives a clash forces without choosing among
them.

So the question is not whether these belong in a `Rule`. It is where a company's sizing knowledge belongs at
all — and whether the evidence yet shows a gap.

| # | Question | When answerable |
|---|---|---|
| OQ28 | Where does an implementation's sizing knowledge live — the derivation from a requirement's parameters to what the answer must be? It is not the wording (`text` is the template), not *how it would be verified*, not the *wording rule*, and K83 excludes a `Rule`. K27 appears to answer it already: beyond the core of eight, a definition holds whatever an implementation's own notation and rule-set need, the core being "a floor the metamodel can reason over, not a ceiling". **But the evidence cannot yet distinguish two readings.** Either the metamodel has no home for this and one is missing, or K27's opening is the home and a real implementation simply did not use it — because the editor that produced this package implements the core eight and nothing else, so a `Rule`'s prose was the only field wide enough to type into. The tool, not the metamodel, may be what displaced it | When an implementation carries beyond-core content on a `RequirementDefinition` and one can see whether sizing knowledge sits there naturally. A second reading becomes available once a walk runs: if a rule turns out to need a computation to *detect* at all — rather than to answer — then K83's line falls in a different place than it reads today |

## Open question OQ29

Raised in [the design record of 2026-09-28 on value domain comparability and the guard](../docs/superpowers/specs/2026-09-28-value-domain-comparability-and-guard-design.md).

| # | Question | When answerable |
|---|---|---|
| OQ29 | Does the walk run again when a value's state changes — an unknown value becoming stated, an assumed one confirmed or corrected? The walk begins when a requirement arises, so a rule a guard left undecided then was judged then, and a rule a guard would now exclude, or no longer exclude, is not revisited. K103 keeps the guard safe without an answer, since it never excludes on a value not yet firm; the question is whether the walk is complete without one | When an implementation runs the walk over a project whose values change after its requirements arise, which is every real project |

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
