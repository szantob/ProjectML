# How the project model changes: the closed list of acts — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K150–K161 and opens
OQ49. It adds a new normative document, `spec/07`, which states the acts through which a project
model changes, and it touches `spec/00`, `spec/01`, `spec/02`, `spec/03` and `spec/06` only to point at it and
to record the decisions.

**Date:** 2026-11-11
**Follows:** the record of 2026-10-06 and the open questions OQ44–OQ48.
**Began as:** the owner's report that the first implementation works best when its modelling tool is a local
server the agent calls: the agent reads the model, or parts of it, and changes it only through the server's
operations, so the server can refuse what the metamodel forbids — a requirement from nothing, a deletion.
Further implementations are planned on the same footing, and the server they will share is to be built
outside this repository.

The next numbers free were K150 and OQ49.

---

## 1. Where this came from

The first implementation's tool wrote one sentence that the metamodel never states: *the model is written only
by the tool's operations.* It then refused, in its own design, every operation that would delete a
requirement or raise a question directly, citing K5 and K87. Each guard it enforces is a rule of this
metamodel, but the rules are stated over states and edges, scattered across `spec/01`–`spec/03`. Nothing says
through *which acts* a model may change, so every implementation has to reconstruct the list, and nothing
measures a tool against it.

Two measurements are wanted of any tool: **safety**, that it admits no change the model forbids, and
**completeness**, that every change the model admits can be reached through it. A list of prohibitions gives
neither for certain, because a negative list is open by construction: a convenient operation nobody thought
to forbid passes it, and it cannot show that a tool is missing anything. `00-overview.md` §4's own argument
— a list is exhausted the moment somebody proposes something not on it — holds for a list of prohibitions,
not for a closed positive one, which excludes everything it does not name.

The owner's position on what the model records, taken while this was designed: **the metamodel "logs" to the
extent that accountability is derivable** — every requirement reaches the source that states it. Logging the
modeller's steps is not the system's task, at least at first.

## 2. Closure and scope

| # | Decision | Reason |
|---|---|---|
| K150 | **A project model changes only through the acts `spec/07` lists.** Nothing enters the model that is not the effect of a listed act, and nothing happens to an element that no listed act does | This is what makes a tool measurable against the metamodel in both directions. A tool is safe when every change it makes decomposes into listed acts, each with its condition met, and complete when every listed act is reachable through it |
| K151 | **The list covers the project model: sources, `SourceElement`s, requirements, `RequirementQuestion`s, `RequirementDecision`s and review findings.** Changing a `RequirementDefinition` or a `RuleSet` is outside it | A rule-set is a model in its own right (K22), and whether its amendment leaves a record is its own business (K88); a definition belongs to an implementation (K15, K16). What either change does to requirements already complete is OQ46 |

## 3. What an act is

| # | Decision | Reason |
|---|---|---|
| K152 | **An act is defined by its effect on the model** — which elements, edges and states appear or cease — and states four things: who performs it, the cause it names, its condition, and its effect. It names no operation, argument or order of calls. An implementation may perform several acts as one step, provided the step happens whole or not at all and every act's condition is met | An act defined by a call would be an interface, and an interface is implementation. Defined by its effect, every entry passes `00-overview.md` §4's test: it is true of a project modelled in YAML, in SysML v2, or in a spreadsheet |
| K153 | **Every act on the project model is performed by the modeller; the project manager performs none of them.** An act that commits the project — recording a decision, and through it retiring requirements — names the `SourceDecision` in which the project manager stated it (K11, K61). The modeller's own judgements — extraction, the flag on a contradiction, relevance while walking — commit nothing | This is the normative form of the rule that asks which actor an act belongs to. A tool enforcing it is what keeps an agent from committing anything itself: with no source, it can commit nothing. Who performs a review is OQ23, and the acts over review findings stay open on that point |
| K154 | **What an act produces carries its cause.** No act produces anything that does not name, on itself, what it came from: a requirement its `SourceNeed`s, a value the `SourceNeed` stating it, a decision its `SourceDecision`, a question its `Rule`, a posed question its `SourceQuestion`, a discharge the element discharging it. **The model records no act as an element of its own**: who performed an act, when, and in what order is an implementation's service if it wants one. The one act that leaves no trace is the deletion of an element extracted in error (K158) | The model carries the derivability of accountability, which K136 already states over everything that enters. A record of each act would state every cause twice, need a check to keep the two equal, and model the modeller rather than the project, on K88's reasoning. **If OQ44 or OQ45 is answered so that the model must know which of two changes came first, this decision is to be revisited**: nothing here prevents adding a record later, where one taken out again would sit badly beside K5 |
| K155 | **Three acts are inseparable from part of their effect,** because each half alone leaves the model in a state no rule admits: producing a requirement opens a `RequirementClarification` for every parameter without a value (K119, K143); a requirement superseding others takes them out of force as it enters (K133, K144); a decision retiring requirements takes them out of force as it is recorded (K62, K144) | Each pairing follows from rules already stated. What the decision adds is that the two halves are one step, so that no tool can leave the first without the second |
| K156 | **Reading is not an act.** Querying, checking, the projection, and **cutting a baseline** change nothing in the working model and stand outside the list. A baseline is the projection given identity — an export — and not a step to be audited or committed to: no source states it, and a new one is cut whenever later information calls for it. K142's condition, that every contradiction choice is decided before a baseline is cut, stays a condition on the export | A baseline need not be final: new information may arrive during the project and call for a new version. Requiring a source for each cut would introduce a source kind for an act that commits nothing the working model does not already hold. SysML v2 likewise has no element for a baseline and leaves a versioned snapshot to the modelling tool or repository, as OQ37's comparison records |
| K160 | **Creating a project model is not an act on it.** It brings into being the model the acts change, and produces no element of it: no source, no `SourceElement`, no requirement | Found by measuring a sketch of the shared server's tools against the list: the first implementation's tool begins with an operation that creates an empty model, and K150 would otherwise forbid it, since no listed act has it as its effect. It is not reading either (K156), so it needs a line of its own |
| K157 | **The term is *act*, already this collection's own** ("the project manager's act", `03-project-lifecycle-model.md` §3). W3C PROV's `Activity` is not adopted | In PROV an `Activity` is a recorded element of the provenance graph, which is exactly the record K154 declines. Adopting the term would import that reading. The collection also calls the project manager's changes to a rule-set acts (`03-project-lifecycle-model.md` §3, K97); they lie outside the list (K151) and are not acts in K150's sense |

## 4. The acts

`spec/07` states fourteen, each with the four entries K152 names. They are summarised here; the conditions
cite the rules they rest on rather than restating them, so that no rule is stated twice.

**Over sources.**

| # | Act | Cause carried | Condition | Effect |
|---|---|---|---|---|
| A1 | Recording a source | none: the chain starts here (K46) | quoted whole and unedited; a `replies` edge names only an earlier source (`02` §3) | a `Source` |
| A2 | Anchoring a `SourceElement` (extraction, K40) | its anchor: exactly one passage of exactly one source | the passage is in the source | a `SourceNeed`, `SourceUpdate`, `SourceDecision` or `SourceQuestion` |
| A3 | Deleting a `SourceElement` extracted in error | none, deliberately (K158, K41) | no requirement or decision refines it, and no question is posed by it (K158); whether a review finding naming it blocks deletion is OQ49 | the element ceases; the source is untouched |

**Over requirements.**

| # | Act | Cause carried | Condition | Effect |
|---|---|---|---|---|
| A4 | Producing a requirement | at least one `SourceNeed` it refines (K145); every value stated by one of them (K136) | its definition is not abstract (K109); a derivation closes no cycle (K147) | a `Requirement` in force; inseparably, a `RequirementClarification` for each parameter without a value (K155); if every parameter has a value, its finished text, computed from the template (K111, K140), and its walk falls due (K128) |
| A5 | Superseding, a form of A4 | also a `SourceUpdate` it refines, and `supersedes` naming at least one requirement (K133) | whether each superseded requirement must be in force is OQ44 | inseparably, the new requirement enters force and the superseded leave it (K155); values carried over keep their source (K138) |
| A6 | Stating a value | the `SourceNeed` stating it, which the requirement then refines (K136) | the requirement is incomplete and the parameter has no value (K129, K140), so the parameter's clarification is open, which it is only while the requirement is in force (K143) | the value; the clarification closes, which is computed (K143); if the requirement becomes complete, its finished text, computed from the template (K111, K140), and its walk falls due (K128) |
| A7 | Adding a derivation to a requirement that exists | what states it is OQ42 | no cycle arises (K147) | a derivation edge; neither end changes (K149) |

**Over questions, decisions and findings.**

| # | Act | Cause carried | Condition | Effect |
|---|---|---|---|---|
| A8 | Walking the `RuleSet`s that reach a requirement (a judgement, K86) | the `Rule`, by *triggered by*, and the triggering requirement | the requirement has just become complete, and is walked once (K128); the rule is in force and its guard does not exclude it (K97, K105) | a `RequirementChoice` (`ConflictRule`) or a `RequirementInquiry` (`CompletenessRule`, at most one open per rule and triggering requirement, K148); a rule judged not relevant leaves nothing (K159) |
| A9 | Raising a contradiction choice | the parameter's `ValueRule` and the requirements of one kind that contradict (K141); the modeller's flag (K142) | a requirement is a triggering requirement of at most one open choice per parameter (`02` §12) | a `RequirementChoice`, or, where one is already open over a requirement the new one contradicts, that choice extended: its triggering requirements and candidate alternatives grow, and no second choice opens (K78, K141, `02` §11); with A4, or later |
| A10 | Posing a question | the `SourceQuestion`, by `poses`, in the source carrying the question sent | the question is raised | the question is posed (K60) |
| A11 | Recording a decision, which commits the project (K153) | at least one `SourceDecision` (K61) | `retires` names only requirements in force (K62). The five attributes are not a condition: one missing is the failed check `02` §12 states, since a source may not give a rationale and the modeller may not supply one (K153) | a `RequirementDecision`; inseparably, the requirements it retires leave force (K155) |
| A12 | Discharging a question | the discharging element: for an inquiry, a requirement of the implied kind deriving from the trigger (K148); for a choice, a decision | — | a `discharges` edge. A clarification is not discharged; its value closes it (K143) |
| A13 | Opening a review finding | the source opening it | it names at least two elements (K10); who performs it is OQ23 | an open finding |
| A14 | Closing a review finding | a later source that `replies` to the one opening it (K11) | — | a closed finding |

**What no act does,** stated in `spec/07` as the other face of K150, so that a reader proposing one of these
finds the refusal rather than an absence: delete a requirement (K5); take a requirement out of force other
than by a decision or a supersession (K144); change a value of a complete requirement, which is a supersession
(K129); raise a question with no rule (K87); record a decision with no `SourceDecision` (K61); edit a source;
close a finding directly (K11).

The first implementation's tool reads against this list as follows: its operation that records a question
sent is A1, A2 and A10 performed as one step, which K152 admits; its operation deleting any wrongly anchored
source element is A3 as K158 now states it.

## 5. Deletion, the walk, and the anchor

| # | Decision | Reason |
|---|---|---|
| K158 | **Any `SourceElement` extracted in error may be deleted, and only while no requirement or decision refines it and no question is posed by it.** Whether a review finding naming it blocks deletion is OQ49. This extends K39 from a `SourceNeed` to every `SourceElement`, and narrows it by the condition | K39's reason — the element is a pointer into a passage, and deleting the pointer loses nothing of record — holds for every `SourceElement`, not only a need: a wrongly anchored `SourceDecision` or `SourceQuestion` is equally a pointer. Deleting one that a requirement or a decision refines would leave that element without its origin at once (K145, K61), and deleting one that poses a question would return a posed question to raised with nothing having caused it (K60) |
| K159 | **A walk leaves no trace beyond the questions it raises.** Whether a walk ran is not a fact the model records: a walk is the reaction to an event the model does record, a requirement becoming complete (K128) | The walk happens as a reaction to every such event, so the event is its record. That every reaction was carried out is the modeller's, on the terms K40 and K41 hold extraction to, and `03-project-lifecycle-model.md` §3 already declines to guarantee that a walk is exhaustive |
| K161 | **A `SourceElement` with no anchor is not a well-formed element of this model.** This revises `02-requirement-analysis-model.md` §5's first rule over a `SourceNeed` and §12's rule over every `SourceElement`, which made it a failed check | The anchor is the cause A2 carries (K154), so A2 cannot produce an element without one, and under K150 the failed check could never fire. An anchorless element is not a record awaiting resolution — there is nothing in it to ask about or weigh — so it is excluded outright, as a `RequirementDecision` with no `SourceDecision` is (K61). The five attributes of a `RequirementDecision` are not treated the same way: a source may lack a rationale, the modeller may not supply one, and the gap must stay visible rather than block the decision being recorded, so that rule stays a failed check |

## 6. What it revises, answers and leaves

- **Revises K39,** extended and narrowed by K158.
- **Revises** `02-requirement-analysis-model.md` §5's first rule over a `SourceNeed` and §12's anchor rule
  over every `SourceElement`, from a failed check to well-formedness (K161).
- **Answers** no open question. It **opens** OQ49: whether a review finding that names a `SourceElement`
  extracted in error keeps it from being deleted. A finding names at least two elements and is never removed
  (K10), and `02` §11 holds that an unrefined `SourceNeed` has exactly two honest resolutions, writing the
  requirement or deleting the need; if a finding blocks deletion, a need named by one has only the first.
  Answerable with OQ23, when a review is worked out as an act. The two other candidates considered, whether a
  walk leaves a trace and whether cutting a baseline commits the project, were settled as K159 and K156.
- **Leaves** OQ23 (A13), OQ35 (whether a baseline may be cut while a requirement in force is incomplete),
  OQ41 and OQ48 (A8, A9), OQ42 (A7), OQ44 (A5), OQ45 and OQ46. `spec/07` marks each act whose condition or
  cause waits on one of them.
- **Leaves, as gaps in the list rather than conditions on an act:** closing a `RequirementInquiry` or
  `RequirementChoice` whose triggering requirements have left force (OQ43), and what happens to the
  requirements deriving from one that has left force (OQ39). `spec/07` names both among what is open. It also
  marks A12's cause for an inquiry as waiting on OQ40 (only a direct derivation?) and OQ26 (is a negative
  answer a requirement?), and A4's clarification and A9's cause as waiting on OQ38 (is the ask a
  `ValueRule`?).
- **Leaves K63's closed state without an act.** K63 gives a `RequirementDecision` an open/closed state and
  leaves what closes one unstated, so no act closes a decision, and none can be listed until that criterion
  is. `spec/07` names the gap among what is open, rather than leaving every decision silently open forever.
- **Corrected after the plan's first two runs,** which stopped at Task 1 and reported both points: A9 first named
  only raising a choice, so under K150 a further contradicting requirement could neither extend the open
  choice — as K78, K141 and `02` §11 require — nor open a second one, since A9's condition forbids it. A9 now
  covers both, because `02` §11 already describes them as one move of the parameter's ask. The second run's sweep found that
  several conditions restated rules that do not exist — an original in force for a derivation, the
  requirement in force for a value — and that A11's five attributes and A2's anchor made two failed checks
  unreachable. The first are removed; the second are settled by the owner as K161 and the A11 condition
  above. K158's condition is narrowed to the references its reason names, and OQ49 opened.

## 7. Notes for the plan

- **`spec/07` is new.** It is not a member of the collection, as `05-binding-contract.md` and
  `06-decisions.md` are not, so K131's count of three members is untouched. Its number follows the last
  document; 04 stays retired. It carries the principles (K150–K157, K160), the fourteen acts, what no act does, and
  K158, K159 and K161 where they bear on A3, A8 and A2.
- **`spec/06`** takes K150–K161 as a new block, in the shape of the existing ones, and OQ49 as an open
  question of its own.
- **`spec/00`** §2: the sentence naming the further documents beyond the collection grows from two to three.
  §5: one paragraph saying that the guarantee is kept by K150's closure, and that the derivability of
  accountability, not a record of the modeller's steps, is what the model carries (K154).
- **`spec/01`** §4: one sentence saying that cutting a baseline is not an act (K156).
- **`spec/02`** §11, at K39's passage: K158's extension and condition. Elsewhere a pointer to `spec/07` only
  where an element's coming into being is described, and only where one helps.
- **`spec/02`** §5's first rule and §12's anchor rule: from a failed check to well-formedness (K161).
- **`spec/03`** §3, *Walking a `RuleSet`*: one sentence for K159.
- **Nothing executable and no notation.** The acts name no operation and no argument. The server that will
  enforce them lives in its own repository, and this record says nothing of it beyond K150's two
  measurements.
- **Re-check the highest K and OQ on `main` before numbering,** since other sessions may number in parallel.
