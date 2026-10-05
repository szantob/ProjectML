# A rule over one value, and the clarification it raises — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K115–K124. It closes
OQ30, OQ22 and OQ24, answers OQ25 and the first half of OQ18, and narrows OQ29. The owner means to add further
material before the integration plan is written, so this record may be extended first; nothing in `spec/`
has changed. The change will touch `spec/02-requirement-analysis-model.md` §7, §11 and §12,
`spec/03-project-lifecycle-model.md` §3 and §6, `spec/04-value-states.md` §2, and `spec/06-decisions.md`.

**Date:** 2026-10-05
**Follows:** OQ30, opened at high priority on the same day, and the OQ clean-up that took it up first among
the open questions about how questions arise.
**Began as:** the owner's principle that only what rule-raised questions and conflicts carry reaches the
project manager, set against a model in which a missing value raised nothing.

The next numbers free were K115 and OQ33.

---

## 1. The defect, wider than OQ30 said

OQ30 named one case: a parameter with no value produces a value in the unknown state and no
`RequirementQuestion`, because `spec/02` §11 routes it through the definition's own *what to ask*, and K87
refused to read *what to ask* as an origin of a question. Working the case through showed two more of the same
shape. Today, for a single value of a single requirement:

| The value is | What happens | Where it is stated |
|---|---|---|
| unknown — missing | *what to ask* covers it; no question is raised | `spec/02` §11; K87 |
| assumed — supplied to keep moving | OQ18's derived shape marks it as one to ask about; no question is raised | OQ18, first half; OQ25 |
| conflicting — two sources disagree | "no rule is involved at all"; no question is raised | `spec/02` §11; `spec/04` §2 |

All three are about one value, not a pair of requirements and not a set. By the owner's principle each is a
silent failure: the project manager learns of none of them.

## 2. A third range, and the rule that runs over it

K90 fixes a `Rule` specialisation by what its firing test ranges over: `ConflictRule` a pair,
`CompletenessRule` a set. All three cases above range over a single value, which is a third point on that axis
and has no specialisation.

| # | Decision | Reason |
|---|---|---|
| K115 | **A third `Rule` specialisation, `ValueRule`, ranges over a single value: one parameter's value on one `Requirement`.** It fires when that value is in a state the rule names | The three silent cases share one range, and K90 makes the range what fixes a specialisation. Its firing test reads a recorded state, so it is decided without judgement, as a guard's criterion is (K103, K105): a `ValueRule` fires on the state, not on a reader's view of relevance (K86) |
| K116 | **Every parameter's *what to ask* is a `ValueRule`, belonging to the rule-set of the definition that declares the parameter and inherited with it (K112). It fires on the unknown state and on the conflicting state.** It is in force for as long as its parameter is declared, and cannot be taken out of force on its own | K87 holds unchanged — every question still comes from a `Rule` — and the route `spec/02` §11 gave a missing parameter is channelled into the question route rather than beside it. Because *what to ask* is required on every parameter (§12), every parameter is covered by construction, and a missing or contested value can no longer be silent. Taking it out of force alone would reopen the very silence it closes |
| K117 | **An assumed value raises a question only where an organisation's rule-set states a `ValueRule` for it.** Such a rule is an ordinary `Rule`: it may carry a guard, and it is in force or not (K97) | Whether a default may stay silent or must be owned is the organisation's policy, not the metamodel's: every assumption about a safety matter may need confirming and none about a cosmetic one. This is OQ18's first half, answered: its rule is a `ValueRule` on the assumed state |

**Where the rule sits.** A parameter's ask belongs to the declaring definition's rule-set, so it reaches every
specialisation of it, as every rule there does (K69) — which is also what K112 already says of the ask.

## 3. Which question each case raises

K79 gave `RequirementQuestion` one specialisation per mechanism, and K90 derived the difference from the range:
a pairwise test has both elements present, so there is something to choose between, and raises a
`RequirementChoice`; a set-level test finds something absent, and raises a `RequirementInquiry`. The owner
compared three ways of extending this — reusing the two specialisations, one new specialisation for every
value case, and a mix — on consistency with K79 and K90, on K75's limit of one open inquiry per rule, on
what closing a question would mean, on how a conflict between values is decided, on what the project manager
sees, and on what each would revise. The mix won.

| # | Decision | Reason |
|---|---|---|
| K118 | **A conflicting value raises a `RequirementChoice`, one per `Requirement` and parameter, whose candidate alternatives are every competing value, each with its source (`spec/04` §2).** A further source disagreeing adds its value to the alternatives rather than opening another choice. It is discharged by a `RequirementDecision` as any choice is | Two or more values present is something to choose between. The project manager decides, and the decision enters as a source (K11, K61): exactly the machinery a `RequirementChoice` already has. One choice over all the values, not one per pair, because three or more sources may disagree, and pairwise choices would multiply with every one of them |
| K119 | **An unknown value, and an assumed value a `ValueRule` names, raise a `RequirementClarification`: a third specialisation of `RequirementQuestion`.** It carries the shared shape, and its statement is drawn from the parameter's *what to ask* when the ask raised it | Nothing is present to choose between, so it is no choice; and the gap is one value of a requirement that exists, not a missing kind, so it is no inquiry. Reusing `RequirementInquiry` would break K75's limit of one open inquiry per rule, since one ask fires on many requirements at once, and would give its `discharges` two meanings. ISO/IEC/IEEE 29148's TBD and TBR are the adopted vocabulary for the two cases: a value to be determined, and one to be resolved |
| K120 | **Which specialisation a question takes follows from what is present when its rule fires: two things to choose between raise a `RequirementChoice`, a missing companion kind a `RequirementInquiry`, a missing or unconfirmed value a `RequirementClarification`.** This revises K79's "one per mechanism" | `ValueRule` raises two specialisations, so the rule's specialisation no longer fixes the question's. What K90 actually derived the question from was what is present when the test fires, and stated that way it holds for all three ranges |

## 4. How a clarification lives and closes

| # | Decision | Reason |
|---|---|---|
| K121 | **A `RequirementClarification` is one per `Requirement` and parameter. It is open while the value stays in the state that raised it, and carries no `discharges`.** It closes when the value leaves that state, and when its `Requirement` is no longer in force | What closed it is already recorded: a value in the stated state names the source it came from (`spec/04` §2), and the chain from the question runs through `poses`, `replies` and `refine` to that source. A `discharges` edge would name nothing the chain does not. One per requirement and parameter, because each requirement's value is answered on its own; one posed `SourceQuestion` may still carry several clarifications, each naming it by `poses` |

**The process, end to end.** A `SourceNeed`'s passage is refined into a `Requirement`, and the passage does not
state a parameter's value, so the value is unknown. The parameter's ask fires and raises a
`RequirementClarification`, in the raised state, naming the ask as its *triggered by* and the requirement as
its triggering `Requirement`; from here the project manager can see it. The modeller puts the question to
somebody, in a source, and the clarification poses that `SourceQuestion`. A later source replies; its passage
is refined into the same requirement, whose refinement edge is list-valued; the value becomes stated, naming
that source; and the clarification closes.

**Three paths through it.** If the answer is that nobody knows yet, the value stays unknown and the
clarification stays posed; how long it may wait is OQ13's interval and OQ18's second half. If the answer
contradicts another source, the value becomes conflicting: the clarification closes, and the same ask raises a
`RequirementChoice` between the two values. If the answer arrives unasked, the value becomes stated and the
clarification closes; the chain lacks only its `poses` and `replies` links, and the value's source still names
where the answer came from.

**A clarification is not a review finding,** for the reason K89 gives for every `RequirementQuestion`.

## 5. Three questions answered on the same machinery

Taken in the same session, once the machinery above was settled.

| # | Decision | Reason |
|---|---|---|
| K122 | **A `RequirementClarification` raised by an ask starts from the ask's wording, which the modeller may fit to the requirement in hand. A `RequirementChoice`'s and a `RequirementInquiry`'s statement the modeller writes freely, informed by the rule's *what to look for*, from no template.** This answers what OQ22 still held | *What to look for* says what to notice, not how to ask. A choice's question is about its own alternatives and an inquiry's about its own gap, which differ every time, so no template written in advance fits them, and what prose means is not an algorithm's to decide (K24). An ask can serve as a start because it is always about one parameter |
| K123 | **Where two or more sources state values for the same thing, they are refined into one `Requirement`, whose value is conflicting and carries every value with its source.** Whether two statements are about the same thing is the modeller's judgement (K24). Two `Requirement`s in force, of one kind, carrying incompatible values for the same thing are a refinement error, which review finds, not a failed check. This answers OQ24 | It is the reading K98's first destination already assumed, and with K118 it is the reading under which the disagreement reaches the project manager, as one choice. Two requirements would raise nothing by themselves, or pairwise choices if a rule compared them. OQ24 itself observed that sameness cannot be a syntactic constraint; what was missing was a statement of where the error is caught. A later source correcting an earlier one is not a disagreement but a replacement, which is OQ17's |
| K124 | **The walk does not run again when a value's state changes.** A question once raised stands until it is answered; that a firmer value has made it moot is itself an answer, which the project manager gives and which enters as a source (K11). How such an answer closes a `RequirementInquiry`, whose discharge names a `Requirement`, is OQ26's. A `ValueRule` needs no second walk, since it reads the value's current state (K121). This answers OQ29 except for one case | K103 already keeps a guard from excluding on a value that is not firm, so a rule that should have fired never fails to; the most a firmer value can do is leave a question raised that no longer needed to be, and saying so is an answer like any other. Running the walk again would put every change of a value through a fresh relevance judgement (K86) |

**What OQ29 still holds.** A rule that the first walk judged not relevant may become relevant once a value is
firmer. That is a judgement, not a guard, and a change of value starts no judgement; whether anything should is
left open.

**OQ14 is deferred again,** by the owner, though the model side's question specialisations are now settled. If
it is taken up, the owner's choice of name for the abstract type is `RequirementModelElement`.

## 6. What this answers, revises and leaves

**Answered.**
- **OQ30** is closed: a missing value reaches the project manager as a `RequirementClarification`, and a
  contested one as a `RequirementChoice`, each raised by the parameter's own ask.
- **OQ25** is answered: the marking of a value as one to ask about is what a `ValueRule` firing on it
  produces. It is no longer a marking with no stated cause — it is the open question, and its cause is named.
- **OQ18's first half** is answered by K117. Its second half, the gap-timeout rule, stays open with OQ13.
- **OQ22** is closed by K119 and K122.
- **OQ24** is closed by K123.
- **OQ29** is answered by K124, except for re-judging a rule the first walk dismissed. K124 leans on OQ26
  for how a moot inquiry closes.

**Revised.** K79's "one per mechanism", by K120. `spec/02` §11's two sentences — that *what to ask* raises no
question, and that two sources disagreeing involves no rule — are replaced. K72's list of worked mechanisms
gains `ValueRule`. K89's account of where a question comes from is narrowed: it names walking a `RuleSet`, a
mode that needs judgement, as what produces a `RequirementQuestion`; a `ValueRule` produces one without
judgement, so questions now come from two modes, and K89's verdict — a question is not a review finding —
holds for both. K87, K75, K80 and K97 are not revised.

**Bears on OQ14,** which waited for this record and is deferred again (§5).

## 7. Notes for the plan

- `spec/02` §7: *what to ask*'s row says it is a `ValueRule` (K116).
- `spec/02` §11: `RequirementClarification` beside the two; the class diagram gains it; K120 replaces the
  "one per mechanism" sentence; the two sentences named above are replaced; the process and its three paths.
- `spec/02` §12: a clarification is open while its value stays in the raising state; one per requirement and
  parameter.
- `spec/03` §3: `ValueRule` beside the two specialisations, the range axis with three points, and K116's
  standing ask-rule; §6's constraints over rules.
- `spec/04` §2: the marking paragraph says what produces the marking (OQ25).
- `spec/02` §11 and `spec/06`: K89's account of the checking mode that produces a question names both modes.
- `spec/02` §10 and §11: K123's single requirement over disagreeing sources, and the review finding for two;
  K122's wording of a question.
- `spec/03` §3: K124 in the walk.
- `spec/06`: K115–K124; OQ22, OQ24 and OQ30 closed; OQ25 answered; OQ18 and OQ29 narrowed; OQ14's deferral
  noted with its name; K72, K79 and K89 noted as revised.
- **Check K119's citation of ISO/IEC/IEEE 29148's TBD and TBR against the standard before it is written in.**
- **Out of this record, for the implementation:** the editor and the contract carry rule types; a `ValueRule`
  an organisation states for assumed values is a new rule type there, and a schema change. It gets its own
  design in that repository.
