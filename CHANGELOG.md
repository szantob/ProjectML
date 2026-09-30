# Changelog

All notable changes to this project are documented in this file.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

`projectml-core` is the metamodel, and it is the only thing versioned here. Implementations — a notation, a
set of requirement definitions, a rule-set — version separately in their own repositories, except the
SysML v2 binding, which lives in `bindings/` and moves with the metamodel.

**Nothing has been released.** The metamodel is a draft, and under `CLAUDE.md` §6 it cannot reach 1.0 until
an implementation has been built on it and has carried a project end to end. Until then this file records
work, not releases.

## [Unreleased]

### Added

- Repository initialised: conventions in `CLAUDE.md`, the founding record in
  `docs/2026-08-26-kernel-brainstorm.md` — eighteen decisions, eight open questions, and the four-phase
  order of work carried over from the session that started this project.
- `spec/`, the metamodel's first complete draft: an overview, one document per member of the collection —
  the requirement model, the requirement analysis model, the Project Lifecycle Model and the value states —
  a binding contract, and a decision record continuing the K series. Prose and diagrams; no notation and no
  filled definitions. OQ2 and OQ4 are answered; OQ9, OQ10 and OQ11 are opened.
- The binding contract's seam finished: cardinality, the check that runs over it, and the far end's naming
  (K51–K54), closing OQ12.
- `bindings/sysml-v2.md`, phase 2's binding — the four declarations K4 asks of a design language, stated for
  SysML v2 and checked against the OMG SysML v2 and KerML specifications directly rather than secondary
  sources. Its findings, including where a declaration turns out to buy less than the binding contract
  claims for it, are recorded in
  [`docs/superpowers/specs/2026-09-03-sysml-binding-approach-design.md`](docs/superpowers/specs/2026-09-03-sysml-binding-approach-design.md).
- K56, closing OQ11: the metamodel needs no subject, checked against both SysML v2 (which carries the
  concept richly, independent of `satisfy`) and EventML (which carries none).
- The binding contract's seam clarified: a binding may give the requirement a native element of its own, or
  a bare reference, and both are symmetric under K2 — the identifier map is what does the work of getting a
  requirement into a design language's own model, not `satisfy`. OQ10's premise is corrected accordingly.
- The requirement analysis model's source side rebuilt around a `SourceElement` family — abstract, carrying
  identity, an anchor, and being material of record — specialised into `SourceQuestion` and, itself
  specialised, `SourceStatement`, with `SourceNeed` (`Need`, renamed, its `value` attribute dropped per K57)
  and `SourceDecision` beneath it. `refine` now covers both `SourceNeed`→`Requirement` and
  `SourceDecision`→`RequirementDecision`; a newly coined edge, `poses`, covers the reverse direction,
  `RequirementQuestion`→`SourceQuestion`. `Decision` is replaced by `RequirementDecision` — carrying a
  mandatory origin, a `retires` edge to the requirements it makes no longer in force, and an open/closed
  state whose closure criterion is left to the Project Lifecycle Model — and by `RequirementQuestion`,
  carrying raised/posed states. K43–K50 and K57–K65 record the decisions; OQ15 is closed, OQ17 is opened.
  Findings from the two design-record passes and the integration itself are in
  [`docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md`](docs/superpowers/specs/2026-09-03-source-element-hierarchy-design.md).
- The Project Lifecycle Model's rule metamodel: a `RuleSet`, zero or one per `RequirementDef`, gathering the
  `Rule`s stated over it and inherited down its specialisation tree; `Rule`, abstract, specialising by
  mechanism rather than subject matter into `ConflictRule` (raises a `RequirementChoice` on a contradiction)
  and `CompletenessRule` (raises a `RequirementInquiry` on a missing implied kind, closing the rule-set's
  fourth statement). `RequirementQuestion` is now abstract, carrying a statement, a reference to the `Rule`
  that triggered it, and the list of triggering `Requirement`s, and specialises into `RequirementInquiry` and
  `RequirementChoice`, both carrying a new `discharges` edge to whatever closes them. `RequirementDef` gains
  an eighth attribute, a wording rule. K66–K81 record the decisions; OQ17 is narrowed rather than closed, and
  OQ18–OQ19 are opened. Findings are in
  [`docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md`](docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md).
- Two corpus-wide renames, both mechanical — no design change: `RequirementDef` is now
  `RequirementDefinition`, matching SysML v2's own term rather than an abbreviation of it; the `Source` edge
  previously named `answers` is now `replies`, correcting a likely translation artefact (the edge covers any
  later source responding to an earlier one, disagreement included, not only a question being answered).
- `Rule` given a shape and, first, a purpose: it directs attention rather than prescribing an outcome,
  carrying an identity local to the `RequirementDefinition` that owns it, a *when it applies* and a *what to
  consider*. Matching is corrected to what it actually is — a relevance judgement made while walking a
  `RuleSet`, which is a written procedure — leaving K76's semantic classification intact and its description
  replaced. Every `RequirementQuestion` now names the `Rule` that produced it without exception, and
  `RequirementQuestion` is removed from the *review finding* family: it is the product of a third checking
  mode, between static checking and review. A `RuleSet` is now exactly one per `RequirementDefinition`,
  possibly empty, correcting the zero-or-one above. K82–K89 record the decisions; OQ20 is closed, OQ21–OQ23
  opened.
  Findings are in
  [`docs/superpowers/specs/2026-09-04-rule-shape-design.md`](docs/superpowers/specs/2026-09-04-rule-shape-design.md).
- OQ28 opened, from the same package: several of its rules put a computation into *what to look for* — a
  skirt's height, a support's part count, a processor's ports — which K83 and K91 exclude from a `Rule`.
  Where an implementation's sizing knowledge does belong is unsettled, and the evidence is not yet clean:
  K27 already lets a definition carry more than the core, but the editor that produced the package
  implements the core eight and nothing else, so a rule's prose was the only field wide enough to type into.
- OQ27 opened, the first open question raised by an implementation rather than by reading the metamodel: a
  live-event AV company's domain declared `Hossz (m)` and `Magasság (cm)` — two value domains for one
  measure, differing only in unit. `04-value-states.md` §5 leaves *which* domains exist to an
  implementation, but not what a domain constrains, and both the **conflicting** state and `ConflictRule`
  presuppose that two values can be compared.
- The `Rule` specialisations given their own shape, and the axis that decides how many there are: a
  specialisation is fixed by what its firing test ranges over — `ConflictRule` a pair of requirements,
  `CompletenessRule` the set a kind would appear among — from which every other difference between them
  follows. `Rule` gains a fourth attribute, a state of in force or no longer in force, since a deletable rule
  would withdraw the target of the reference every `RequirementQuestion` must carry; its third attribute is
  renamed *what to look for*, the earlier name having generalised from one of the two cases. `ConflictRule`
  carries nothing of its own, and the universal check against contradicting an in-force requirement is
  removed as not being a rule at all — what it covered divides among the conflicting value state, a
  cross-kind `ConflictRule`, and review. `CompletenessRule` names exactly one implied `RequirementDefinition`
  by a typed reference, and its query is stated in full. Three diagrams are added or replaced, and
  `03-project-lifecycle-model.md` gains a syntactic-constraints section in the shape its two siblings use.
  K90–K100 record the decisions; OQ18 and OQ22 are narrowed, and OQ24–OQ26 opened. Findings are in
  [`docs/superpowers/specs/2026-09-07-rule-specialisations-design.md`](docs/superpowers/specs/2026-09-07-rule-specialisations-design.md).
- Value domain comparability and the guard, closing OQ27 and OQ21 together. A value domain fixes no unit; it
  declares one of three levels of comparability — not comparable, comparable for equality, or ordered — and
  how it achieves that level is the implementation's. A `Rule` may carry a guard, a list of criteria each
  naming a parameter, an operation and a constant, applied after the in-force check and before the relevance
  judgement; it excludes a rule only where a criterion is decided false on a stated or derived value, and so
  excludes exactly what is decidable without judgement. A parameter gains an identity local to its
  definition, and a specialisation has its ancestors' parameters without redeclaring any, which answers the
  part of OQ9 the guard needs. K101–K108 record the decisions; OQ9 is narrowed and OQ29 opened. Findings are
  in
  [`docs/superpowers/specs/2026-09-28-value-domain-comparability-and-guard-design.md`](docs/superpowers/specs/2026-09-28-value-domain-comparability-and-guard-design.md).
