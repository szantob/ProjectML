# Contradictions as requirements, derivation as elaboration, and the review of K115–K135 — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K136–K149 and opens
OQ38–OQ40. It comes out of the owner's review, on 2026-10-06, of the branch that writes K115–K135 into
`spec/` by [the integration plan of 2026-10-05](../plans/2026-10-05-values-and-questions-integration-plan.md),
measured against [the record on a rule over one value](2026-10-05-value-rule-and-clarification-design.md),
[the record on values from sources](2026-10-05-values-from-sources-and-the-complete-requirement-design.md),
which governs it, and `CLAUDE.md`. It revises several of those decisions before the branch reaches `main`; the
branch is corrected by a plan of its own.

**Date:** 2026-10-06
**Follows:** the integration of K115–K135, and OQ37, opened during the same review.
**Began as:** a review of that integration, which turned, finding by finding, into a reworking of how a
contradiction is held and of what derivation is for.

The next numbers free were K136 and OQ38.

---

## 1. Where this came from

The review found that the integration was faithful to its plan and its records, and that several of its
findings went back past the plan into the records themselves. Three of them carried the rest:

- **A disagreement held inside one requirement** (K123, K126) left a complete requirement open to change in
  place, made a clarification open and closed at once, and descended, the owner observed, from the
  value-state model's machinery of a state per value of every requirement, which made the model exponentially
  complicated and which K125 had just withdrawn.
- **A value naming its source** (K125) skips a layer: a requirement reaches a source only through the
  `SourceNeed` it refines.
- **A requirement whose only origin is a derivation** came into `spec/01` with the first draft, from EventML's
  D49, and was never decided for ProjectML. EventML did not yet have the two-step analysis — from a source,
  through a need, to a requirement, and from the requirement model on to the next model — that makes it wrong
  here.

The owner's principle behind the answers: **the requirement model models the state the project is actually
in, and raises its problems; it does not model an ideal world and press the project into it.**

## 2. Values, sources and layers

| # | Decision | Reason |
|---|---|---|
| K136 | **No element skips a layer to name its provenance.** On a `Requirement`, every value is stated by a `SourceNeed` the requirement refines. Across the whole model, everything that enters it names the `SourceElement` that states it: a `Requirement` its `SourceNeed`s, a `RequirementDecision` its `SourceDecision`. A requirement never names a `Source`. This restates K125 | A requirement is related to a source only through the passage a `SourceNeed` anchors into, and `refine` already crosses at exactly that point. K125's guarantee, that a value has somebody answerable for it, is kept unchanged |
| K137 | **The working model's `Requirement` carries, beyond what `01-requirement-model.md` gives it:** its values, at most one per parameter, each stated by a `SourceNeed` it refines (K136); its finished text once it is complete, and none before; `refine`; the definition it is produced under (K67); `supersedes` and `supersededBy`; and `retiredBy` | K130 took the values out of `spec/01`, and nothing in `spec/02` took them in, so the requirement analysis model used an attribute it never defined. At most one value per parameter follows from K139 |
| K138 | **A requirement superseding another also refines those `SourceNeed`s of the old one that still state what it keeps.** Its values are stated by the `SourceUpdate` for what that replaces, and by those needs for the rest | A correction replaces what it states and nothing else. A value the update does not state cannot be stated by it (K136) |

## 3. A contradiction is two requirements

| # | Decision | Reason |
|---|---|---|
| K139 | **Contradicting `SourceNeed`s are refined into separate `Requirement`s, and a `RequirementChoice` is raised over the requirements that contradict.** A disagreement never makes a value missing, and a requirement never holds two values for one parameter. This reverses K123 and replaces K126; K129's different value stated without replacing anything is the same case | The model records the state the project is in. Holding a disagreement inside one requirement was the value-state model's last trace, and it left a complete requirement changing in place (K129) and a clarification both open and closed. Two requirements change nothing in place, and the choice already has the machinery to settle them |
| K140 | **A requirement is complete when every parameter it has has a value.** An open choice over it does not make it incomplete. This revises K128 | Under K139 a choice is between requirements, not about one requirement's values. A requirement waiting on such a choice until a baseline is cut (K142) would otherwise never be walked |
| K141 | **The parameter's ask raises the choice.** Its `ValueRule` fires where the modeller has judged, at extraction, that requirements of one kind state values of that parameter for the same thing (K24, K40); the choice names every one of them as triggering. This is K98's first destination now, and a universal contradiction rule stays rejected. It revises K115's range, K118 and K120's wording | Two requirements of one kind are produced from one template, so where they contradict they differ in a parameter's value, and the ask for that parameter is always present and carries content (K116). K98's degeneracy objection does not reach it, and no contradiction of one kind can go unraised |
| K142 | **The modeller flags each contradiction as real or not; the project manager decides, at the latest when a baseline is cut.** Keep one, and the other is retired; or not real, and both stay in force and both enter the baseline. Nothing is merged, in the model or in a baseline. A baseline is cut only once every contradiction choice is decided | The flag is advice, the same judgement the modeller makes at extraction; deciding commits the project, which is the project manager's (K11). Leaving the contradictions standing until then means a branch that falls away meanwhile needs nothing unmerged. A merged requirement would have no `SourceNeed` behind it and no identity stable across baselines (K21), and the projection only filters. One design element satisfying both requirements is design, beyond the seam |
| K143 | **A `RequirementClarification` is open while no `SourceNeed` the requirement refines states the parameter's value**, and while the requirement is in force. This restates K121 | Under K139 a stated value is never withdrawn by a disagreement, so the clarification closes once, when a value is first stated |

## 4. Leaving force

| # | Decision | Reason |
|---|---|---|
| K144 | **A requirement is no longer in force when it has a `retiredBy` or a `supersededBy`, whatever the state of the element at the other end. It has at most one `retiredBy`.** This corrects the integration's reading of K133 as a biconditional over requirements in force | Read that way, a requirement whose superseding requirement later left force would return to force with no source behind it. Leaving force is an event in a requirement's life (K134) and is final (K5) |

## 5. Origin, and what derivation is for

| # | Decision | Reason |
|---|---|---|
| K145 | **Every `Requirement` refines at least one `SourceNeed`. Derivation is never an origin.** EventML's D49 — an origin of refinement, derivation, or both — is not adopted, and K9 narrows to it | A requirement derived only from others has content the modeller produced, and the modeller is answerable for nothing in the project. A wrong one is a silent failure that runs through everything built on it |
| K146 | **Derivation is elaboration agreed with the client.** A derived requirement refines its own `SourceNeed`, the client's answer, and derives from the requirement it elaborates. The original implies the derived, as SysML v2's derivation states, because the client stated the elaboration as part of what the original asks. No requirement is refined or decomposed into others in this model; that belongs to the design language beyond the seam | It is the reason derivation is kept at all: it lets a requirement system be worked out with the client inside the model, and it is the trace of that working-out that reaches the baseline, where the questions behind it do not |
| K147 | **A requirement may derive from several originals where each alone implies it. One that only several together imply is not derived.** Derivation is acyclic. Adding a derivation to a requirement that exists does not change it in place | Several originals each asking for the same thing is the common case, and one shared requirement avoids stating it again under each. Each such derivation is true as SysML v2 reads it, where a synthesis would not be. K135 makes the edge an association, so adding one changes neither end. What only one original asks for goes beneath that original alone |
| K148 | **A `CompletenessRule` looks for its implied kind among the requirements that derive from the requirement that triggered it.** One inquiry is open per rule and triggering requirement, and a requirement so derived discharges it. This revises K75, K92 and K95 | Elaboration happens beneath the requirement being elaborated. Looked for anywhere in the model, one requirement of the implied kind under one original silenced the question for every other. K75's concern was cost; a question per triggering requirement is work the project actually has |

## 6. Edges between requirements

| # | Decision | Reason |
|---|---|---|
| K149 | **Every edge between two requirements is an association with two ends:** the derivation, named *original* and *derived requirement*, and `supersedes`/`supersededBy`. `retires`/`retiredBy` is one by K134. Nothing is said of other edges. This narrows K135 | K135 was meant over the edges between requirements and was written over every edge. OQ36 asked which other edges have their second end named on the premise of the wider reading, and dissolves |

`refine` and SysML v2's refinement were checked against each other and are one relation: SysML v2 defines a
refinement as a dependency whose source elements give "a more precise and/or accurate representation than the
target elements", which is what a requirement is of its need. OQ15's statement that the name is adopted from
SysML v2 stands. SysML v2 draws it as a dependency rather than an association, which K149 leaves alone.

## 7. What it opens

| # | Question | When answerable |
|---|---|---|
| OQ38 | Does a parameter's ask need to be a `Rule` at all? K116 made it a `ValueRule` so that K87 holds for a missing value, but the `ValueRule` departs from a `Rule`'s shape almost everywhere: it carries no *when it applies* and no guard, is never taken out of force, is not walked (K140), and takes its identity and its *what to look for* from its parameter — which risks an identity colliding with another `Rule` of the set (K85, K106), and the ask living in two places. The alternative is an ask that is itself an origin of a question, which K87 refused | When the processes are worked out again after this record, or when an implementation shows whether the `ValueRule` earns its place |
| OQ39 | What happens to the requirements deriving from one that leaves force? K11 forbids them following on their own; a question raised over them, for the project manager to answer, is the reading closest to K142 | When the processes are worked out again after this record |
| OQ40 | Does a `CompletenessRule` look among the requirements deriving from its triggering requirement directly, or among every requirement deriving from it through others? The second keeps a requirement placed one level deeper from opening a false gap | When the processes are worked out again after this record |

## 8. What it revises, answers and leaves

**Revised.** K9 by K145; K75, K92 and K95 by K148; K98's first destination by K141; K115, K118 and K120 by
K141; K121 by K143; K123 reversed and K126 replaced by K139; K125 restated by K136; K128 by K140; K129's
choice by K139; K133's reading by K144; K135 narrowed by K149. EventML's D49 is not adopted (K145).

**Answered.** OQ36 dissolves (K149). OQ37's deferred row on what derivation means is answered by K146 and
K147: derivation is an implication the client stated. OQ35 gains a condition, that every contradiction choice
is decided before a baseline is cut (K142); whether one may be cut over an incomplete requirement stays open.

**Left.** OQ37's main question — K33 keeping a requirement's kind out of the baseline that SysML v2 is to read
— is not answered here. The processes the owner means to work out again after this record are where OQ38–OQ40
are expected to close.

## 9. Notes for the plan

- `spec/02`: §5 and §7 (K136; the `SourceUpdate` paragraph's "disagreement" sentence follows K139); §10, the
  derivation and the complete requirement (K136–K140, K145–K147), the attribute table of K137, the
  constraint on leaving force (K144); §11, the clarification and the choice (K139, K141–K143), the decision
  paragraph (its value-setting sentence goes); §12 throughout.
- `spec/01` §2: the origin paragraph (K145), the synthesis example (K147); §4 nothing beyond K142's condition
  if OQ35's paragraph moves there.
- `spec/03`: the `ValueRule` subsection (K141); K90's range paragraph; the `ConflictRule` paragraph listing
  three destinations (K141); the `CompletenessRule` subsection (K148), including the paragraph on what the
  set-level reading cannot express, which goes; §6's constraints; the walk's flowchart excludes `ValueRule`s
  until OQ38 is settled.
- `spec/00` §2: the paragraph on edges (K149).
- `spec/06`: K136–K149, OQ38–OQ40, and the revisions of §8; OQ36's dissolution; OQ37's derivation row; OQ35.
- Housekeeping the review found, owing nothing to a decision: `spec/05` §2's sentence giving a baseline a
  requirement's values (K130); `spec/03` §2's sentence counting four gaps and its paragraph on how a rule-set
  treats a default (K127); `spec/06`'s OQ30/OQ31 section, whose heading says closed and whose text still says
  high priority and open; `CLAUDE.md` rule 4 and `spec/00` §1 restated after K136.
- **An audit, not a decision:** every EventML decision `docs/eventml-decisions.md` marks as inherited is to be
  confirmed by the owner for ProjectML, since D49 was inherited without being decided here.
