# The Project Lifecycle Model's rule metamodel, and RequirementQuestion's full shape — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K66–K80, closing most
of OQ17 and opening two narrower successors, OQ18 and OQ19. `spec/` has not been changed: the change touches
`spec/02-requirement-analysis-model.md` (RequirementDef's seventh attribute; RequirementQuestion becoming
abstract with two specialisations), `spec/03-project-lifecycle-model.md` (the rule metamodel itself, and a
scope correction to K22), and `spec/01-requirement-model.md` (possibly, for the baseline/RuleSet question
OQ19 raises) — a change of this shape needs a plan of its own, on the same terms the source-element
hierarchy's own first pass did.

**Date:** 2026-09-04
**Follows:** [`2026-09-03-source-element-hierarchy-design.md`](2026-09-03-source-element-hierarchy-design.md),
whose K61 finding — that closing a `RequirementQuestion` traces through `poses`+`answers`+`refine` without
a dedicated edge — this record both relies on and extends.
**Began as:** an attempt at OQ17 (`spec/06-decisions.md`): what the Project Lifecycle Model's fourth
rule-set item looks like, and how a `RequirementQuestion` references what it fires on. The session found
substantially more than a fourth table row.

---

## 1. Where this record came from

The session opened on OQ17's own wording — draft a fourth row for `spec/03-project-lifecycle-model.md` §2's
three-item table. Answering *how a `RequirementQuestion` references what it fires on* immediately required
knowing what a `RequirementQuestion` actually carries, which `spec/02-requirement-analysis-model.md` had
never worked out beyond its name and its two states (K60). Working that out required knowing what a `Rule`
is, which `spec/03` had never worked out beyond three descriptive labels. Each layer opened the one beneath
it, in the same way the source-element hierarchy's own first pass opened onto its second.

## 2. `RequirementDef`'s seventh attribute

`spec/03` §1 states, of a rule-set, that it *"says nothing about... what a requirement's wording should
be"* — which settles where a well-formedness rule for a requirement kind's own wording does **not** belong,
without saying where it does.

| # | Decision | Reason |
|---|---|---|
| K66 | **`RequirementDef` carries a seventh attribute: a well-formedness rule for the wording a requirement produced under it must satisfy.** Prose, on the same terms `RequirementDef`'s existing *"how it would be verified"* is prose (K29) | The seam test (K25) and the record test (K26) both pass, on exactly the argument that seated K29: the rule can be stated without resolving a reference to an element the metamodel does not define, and a stated rule can fail on its absence. `text` already gives the structural template a requirement's wording is produced from; it says nothing about the *qualities* the produced wording must have — ISO/IEC/IEEE 29148's own *"characteristics of a good requirement"* (unambiguous, singular, and so on) is exactly this second, missing thing, generic to a kind rather than to one requirement, which is what places it on the definition rather than the instance |

**Where this does not belong, confirmed rather than merely inferred:** `spec/03` §1's *"says nothing about...
what a requirement's wording should be"* is explicit and pre-existing — a rule-set does not state this,
`RequirementDef` does. No new decision was needed to rule the Project Lifecycle Model out; the document
already had.

## 3. `Requirement` does not specialise `RequirementDef`

A question was raised mid-session: if `RequirementDef` is the specialisation tree's root (K30), must
`Requirement` itself derive from that tree, or `RequirementDef` derive from `Requirement`, for a
specialised kind's instances to be insertable into the requirement analysis model as its elements?

| # | Decision | Reason |
|---|---|---|
| K67 | **`Requirement` and `RequirementDef` are connected by an association — the *"produced under"* edge (§10, K8) — never by inheritance.** `Requirement` stays a single, unspecialised type regardless of how deep or wide the `RequirementDef` tree grows; a requirement produced under a leaf kind does not become a member of a `Requirement`-subtype, it is a plain `Requirement` that names which `RequirementDef` it was produced under | This is the SysML v2 `def`/`usage` split (K17, K30's own citation), verified directly against the primary specification rather than assumed: `FunctionalRequirementCheck`, `PhysicalRequirementCheck`, and the rest of §9.2.14.2's kind hierarchy all specialise `RequirementCheck`, stated as *"the base type of all `RequirementDefinition`s"* — the kind hierarchy lives entirely on the definition side. `RequirementUsage` (K67's `Requirement`) stays one uniform type, typed by whichever definition it names, never itself specialised. A SysML v1 diagram was raised mid-session showing the opposite shape — `FunctionalReqt`, `InterfaceReqt` and the rest as direct UML subclasses of `Requirement`, with no definition/usage split at all — and confirms the point by contrast: that is the pre-v2 mechanism the def/usage split replaced, and K30 already commits this metamodel to v2's mechanism, not v1's |

This is a confirmation of what K30 and K33 already implied rather than a change to either: K33 already reads
as though `Requirement` is single and unspecialised (*"does not name the `RequirementDef` it came from, nor
that definition's kind"* — a sentence that presupposes `Requirement` has no kind of its own to name). K67
states the reason explicitly, because the question turned out not to be obvious without it.

## 4. `RuleSet` and `Rule`

### The shape

| # | Decision | Reason |
|---|---|---|
| K68 | **A `RuleSet` belongs to a `RequirementDef` — zero or one per `RequirementDef` — not to a project as a whole.** It gathers the `Rule`s stated over that `RequirementDef` specifically. There is no reification of "everything a project has loaded" as its own element | Two reasons. First, it is the natural unit: `spec/03` §2 already says a rule-set's statements are *"stated per kind, not per definition"* (§2), and a kind is exactly a `RequirementDef`, so attaching the `RuleSet` there rather than centrally is following that sentence rather than adding to it. Second, and decisively, it narrows the search a check needs to do: finding every `Rule` that could apply to a `Requirement` is walking that `Requirement`'s own `RequirementDef` ancestry chain and reading each node's own `RuleSet`, not filtering a project-wide collection by an `appliesTo` reference. A candidate design with `Rule.appliesTo` pointing at an arbitrary `RequirementDef` node was considered and rejected on exactly this ground — the attachment being structural (which `RequirementDef` a `RuleSet` belongs to) does the same job as a reference, for free |
| K69 | **A `Rule` attached to a `RequirementDef` applies to every specialisation of it, not only to that node.** This needs no mechanism of its own: it is a direct reading of the specialisation tree K30 already builds — a rule stated at the root applies everywhere, a rule stated three levels down applies only beneath that point | This is exactly the property a worked example surfaced: a project's most consequential rules — "no requirement may contradict an active one," discussed under K73 below — belong at the root precisely because they should reach every requirement kind a project declares, and a project-organisation-level rule about, say, invoicing should not have to be restated once per leaf kind |
| K70 | **`spec/03` §1's *"what an organisation loads"* is corrected to name a project, not an organisation across projects.** How an organisation manages `RuleSet`s across more than one project — export, import, version comparison between projects — is explicitly out of this model's scope; `spec/03`'s own scope is one project, the same scope every other member of the collection keeps | The phrasing survived from an earlier reading that had cross-project tooling in mind (export/import/version-comparison), which the session recognised, once asked, as tooling territory rather than metamodel territory — the same kind of boundary K46 already draws for provenance ("stops at the source"). Nothing about K68/K69 needs a project-spanning concept; `RuleSet`, scoped to one `RequirementDef` in one project, is already sufficient on its own |

### Rule's specialisation axis is mechanism, not subject matter

`spec/03` §2's three (soon four) rows — silent-vs-owned default, gap-timeout, conflict resolution, kind
implication — describe **what a rule-set's statement is about**. Working out how a `Rule` connects to
`RequirementQuestion` found a second, independent axis: **what a `Rule` does when it fires**, and the two
axes do not line up one-to-one.

| # | Decision | Reason |
|---|---|---|
| K71 | **`Rule` is abstract, and its specialisations divide by mechanism — what happens when the rule fires — not by `spec/03` §2's four descriptive categories.** §2's categories remain a description of *subject matter*, closer to an open, `Source.kind`-shaped label than to a type boundary | The two axes were conflated at first, and a direct question — *"these four types are types of what?"* — forced them apart. The two do not correlate the way a first pass assumed: the two mechanisms worked out below (K73, K74) — conflict resolution and kind implication, two *different* subject-matter rows — share the *same* mechanism shape, "detect a gap, then raise a `RequirementQuestion` subtype." Meanwhile the other two subject-matter rows (silent-vs-owned default, gap-timeout) have no worked-out mechanism at all yet (OQ18) and may turn out to need something structurally different from this shape, or from each other. Subject matter does not predict mechanism in either direction, which is what rules it out as the type boundary: a specialisation axis has to track what actually varies structurally |
| K72 | **Two mechanisms are worked out; two more (§2's first and second rows) are not, and stay open.** A detecting `Rule` that finds a gap and raises a `RequirementChoice`; a detecting `Rule` that finds a gap and raises a `RequirementInquiry`. Whether a silent-vs-owned-default rule or a gap-timeout rule shares this "detect, then raise" shape, or needs a different one entirely — a direct value-state edit, an escalation of something already raised — is not settled here | Named directly rather than glossed over: this record answers OQ17 for the two mechanisms a worked example actually exercised, and explicitly does not claim the other two are the same shape merely because they sit in the same abstract type's specialisation list. K42's own reasoning already anticipates a `Rule`'s mechanism list growing past what this record settles |

### The two worked mechanisms

| # | Decision | Reason |
|---|---|---|
| K73 | **A conflict-detecting `Rule`, when it finds that a new `Requirement` contradicts an existing in-force one, raises a `RequirementChoice`.** The worked example — *"no requirement may contradict an active requirement,"* stated once at the `RequirementDef` root and inherited everywhere by K69 — is this mechanism's canonical case | This sharpens `spec/03` §2's third row (*"how a conflict of a given kind is resolved"*), which describes only the resolution half; detection is the other half a `Rule` must also carry, and resolution is exactly what a `RequirementChoice`, discharged by a `RequirementDecision` (K79), records |
| K74 | **A completeness-detecting `Rule`, when it finds that a `RequirementDef` kind is present without an implied companion kind, raises a `RequirementInquiry`.** This is OQ17's original case — *"which other requirement kinds a given kind implies should also be present"* — now given a mechanism rather than only a name | Closes the concrete question OQ17 opened with. `spec/03` §2 gains a fourth row on these terms, once this record is written into `spec/`: *"Which other requirement kinds a given kind implies should also be present \| Nothing today says whether one requirement's kind, on its own, calls for other kinds to co-exist."* |
| K75 | **A completeness check is set-level, not per-instance: it asks whether at least one `Requirement` of the implied kind exists anywhere the rule's `RequirementDef` reaches, not whether every triggering `Requirement` has its own.** Consequently, while a given `Rule`'s gap stays open, a newly triggering `Requirement` extends the existing open `RequirementInquiry`'s list of triggering requirements rather than raising a second one — at most one open `RequirementInquiry` per `Rule` at a time | This was raised as a cost concern — a growing model re-triggering the same rule combinatorially — and resolves almost entirely once the check is read as a query over current state rather than a per-instance obligation: once the implied kind exists once, the query returns no gap for every requirement thereafter, without anything needing to be closed by hand. What is left of the concern (several triggering requirements accumulating before the gap is filled) is handled by the extend-rather-than-duplicate rule, not by a cost-control mechanism the metamodel would otherwise have had to invent |
| K76 | **Rule-matching — whether a `Rule`'s free-text condition holds of a free-text `Requirement` — is a semantic constraint (K24), not a syntactic one.** The metamodel does not guarantee it runs exhaustively or automatically; it is carried out by judgement, human or AI, on the same terms K40/K41 already hold extraction completeness to | Both a `Rule`'s condition and a `Requirement`'s wording are prose; nothing decides whether one matches the other without reading content, which is exactly K24's dividing line. This is the same posture K41 already took over a source-coverage report, applied one level over: the metamodel does not promise this checking is complete or cheap, because it cannot deliver that promise honestly, and a guarantee it cannot keep would devalue the ones it can |
| K77 | **`RequirementQuestion` — both specialisations — belongs to the *review finding* family in `spec/02` §11's three-way table (failed check / question / review finding), not to the *failed check* or *question* rows.** It is judged (K76), modelled, and carries state (K60) — the three properties that table already uses to seat *review finding* apart from the other two | This was implicit once K76 was settled and is worth stating outright: `RequirementQuestion` was designed on K48/K49's terms without this classification in view, and it turns out to already satisfy every criterion the table uses. No change to `RequirementQuestion` follows from this — it is a naming of what it already is, for a later reader who reaches §11's table looking for where it fits |

## 5. `RequirementQuestion`'s full shape

`spec/02` (§11, K60) gives `RequirementQuestion` two states and the `poses` edge, and no other content. Working
out how it references what triggered it, and what closes it, produced the rest.

### The shared shape

| # | Decision | Reason |
|---|---|---|
| K78 | **`RequirementQuestion` is abstract, and carries, beyond its existing identity and raised/posed states: a free professional-register text statement of the question; a reference to the `Rule` that triggered it; and a list of every `Requirement` that triggered it (list-valued, may grow while the question stays open, per K75).** The `poses` edge (K59) is unchanged | The text and the triggering-requirements list were both named directly, without much argument needed once `Rule` existed as a real element (K71) to reference — the earlier concern that a "which rule" reference would open a second seam (K28's own shape of problem) dissolves once `Rule` is itself metamodel-defined rather than an undefined placeholder: referencing it is no different from `Requirement` referencing `RequirementDef` |
| K79 | **`RequirementQuestion` specialises into `RequirementInquiry` and `RequirementChoice`**, one per mechanism K72 names. Both carry a `discharges` edge to whatever closes them — `RequirementInquiry` to a `Requirement`, `RequirementChoice` to a `RequirementDecision` — optional, because it is absent for as long as the question stands open | `discharges` was chosen over reusing `answers`: `answers` is a `Source`↔`Source`, evidentiary edge — one passage of material responding to another — and this is a different kind of relationship entirely, a model-side element naming what closed it, on the model's own side of K43's axis. Reusing `answers` here would repeat exactly the confusion K47 was written to prevent. `discharges` was already live in this collection's own vocabulary, in OQ13's *"its discharge is the machinery OQ13 still asks for"* — naming the edge after a word the corpus already uses for this phenomenon, rather than coining a fresh one |
| K80 | **`RequirementInquiry` carries nothing beyond the shared shape and its `discharges` edge.** `RequirementChoice` additionally carries the candidate alternatives being decided among | The asymmetry follows from what each mechanism actually needs: a completeness gap (K74) names a missing kind, which the shared *triggering `Requirement`s* list plus the `Rule` itself already identify — nothing further to add before discharge. A conflict (K73) needs the *options* named before anyone can decide among them, and `RequirementChoice`'s alternatives deliberately prefigure what `RequirementDecision.the choice` (already, since before this record, *"What was decided, and the genuine alternatives it was chosen among"*) will record once discharged — the same alternatives, read once as open and once as settled |

### Closing `RequirementInquiry`/`RequirementChoice` without a dedicated edge, confirmed a second time

The source-element hierarchy record's K61 already found that closing a `RequirementQuestion` traces through
`poses`+`answers`+`refine` without needing a new edge between `RequirementQuestion` and `RequirementDecision`.
K79's `discharges` edge does not reopen that finding — it sits *beside* the traceable chain as a direct,
optional convenience reference, not in place of it. Both readings are true at once and were kept
deliberately: the chain is what guarantees the connection always exists and is consistent; `discharges` is
what lets a reader (or a checker) find the answer without walking three hops to get there.

## 6. What this record leaves open

| # | Question | When |
|---|---|---|
| OQ18 | **What mechanism do a silent-vs-owned-default `Rule` and a gap-timeout `Rule` actually carry?** K72 names the gap directly rather than guessing at it. Both may share `RequirementChoice`/`RequirementInquiry`'s "detect, then raise" shape, or may need something structurally different — a direct value-state edit for the first, an escalation of something already raised for the second | When one of the two is actually exercised, the same discipline K71's own finding already used for the other two |
| OQ19 | **Does a baseline need to name which `RuleSet`(s), and which version of each, it was checked against — separately from the implementation package and version `01-requirement-model.md` §4 already names?** Raised mid-session and not pursued: K68 makes a `RuleSet` per-`RequirementDef` rather than a single project-wide version, which may mean this question is really *N* small questions (one per `RequirementDef` a baseline's requirements touch) rather than one | Needs `01-requirement-model.md` §4 read again with K68–K70 in view, which this record does not attempt |

**Recorded here rather than as open questions, because they are not questions about this model — they are
renaming work already decided, tracked outside `spec/`:** `RequirementDef` → `RequirementDefinition`, to
match SysML v2's own term rather than an abbreviation of it; and `Source`'s `answers` edge → `replies` or
`replies to`, a likely translation artefact flagged mid-session (Hungarian *"válaszol"* renders more
accurately as *"replies to"* than *"answers,"* which implies a posed question where the edge in fact covers
any later source responding to an earlier one, disagreement included). Both are tracked in this session's
own memory rather than `spec/`, because neither is a design question — both are already decided, only not
yet executed, and each is its own corpus-wide mechanical pass on the scale of the `Need`→`SourceNeed` rename
already done once.
