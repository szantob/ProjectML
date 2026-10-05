# Values only from sources, the complete requirement, and what a baseline carries — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K125–K131. It revises a
locked foundation by the owner's explicit instruction — the value-state model, part of the kernel under
`CLAUDE.md` §3, rule 4 — and it amends the record of the same day on a rule over one value
([`2026-10-05-value-rule-and-clarification-design.md`](2026-10-05-value-rule-and-clarification-design.md)),
whose decisions K115–K124 are not yet in `spec/` either; §6 says what of that record stands. Both go into
`spec/` together, by one plan.

**Date:** 2026-10-05
**Follows:** the OQ clean-up of the same day, which had settled how a missing value reaches the project
manager and was turning to what produces a value at all.
**Began as:** the owner asking whether a rule-set should run on a requirement that is not yet finished.

The next numbers free were K125 and OQ33.

---

## 1. Where this came from

The record on a rule over one value made every parameter's ask a rule, so that a missing, assumed or contested
value would raise a question. Working out where that left the walk showed how much of the model existed only
because a rule-set could run on a requirement whose values were not yet settled: a guard undecided on a value
that is not firm (K103), a walk that might have to run again when a value firms up (OQ29, K124), a rule an
organisation writes to decide whether an assumption must be owned (OQ18, K117), a marking of a value as one
to ask about with no stated cause (OQ25).

The owner proposed the simpler model: **a rule-set does not run on a half-finished requirement. It runs once,
when every value is there.** Asking what "there" means led to the five value states, and the owner found two
of them suspect for a reason the repository's own house rules give: *assumed* and *derived* let a value enter
the model without a source, and so without anybody answerable for it. A value nobody is answerable for is a
value the modelling procedure is answerable for, which is to say a defect of the method. The owner then cut
the third: *conflicting*, examined, is not a value at all.

## 2. A value exists only where a source states it

| # | Decision | Reason |
|---|---|---|
| K125 | **Every value names the source that states it, and a value no source states does not exist.** Of the five value states, *stated* becomes the only way a value exists and *unknown* the absence of one; *assumed* and *derived* are withdrawn. This revises the value-state model the founding record placed in the kernel | A value enters the model only with somebody answerable for it. *Assumed* and *derived* each required "the reasoning" where *stated* required a source, and nothing in `spec/` said who writes that reasoning; in practice it is the modeller, who by the house rules administers and decides nothing for the project. A value the modeller supplied is a decision with no traceable owner |
| K126 | **A disagreement between sources is not a value.** While two or more sources state different values for the same thing, the value is missing, and the disagreement is an open `RequirementChoice` whose alternatives are the competing statements, each with its source. *Conflicting* is withdrawn as a state | The competing values already stand in the choice (K118 of the record this amends); holding them a second time as a state of the value says nothing more. Nobody is answerable for a contested value until the project manager chooses, and the choice enters as a source (K11), after which the value exists and names that decision |
| K127 | **What *assumed* and *derived* were for is kept, with an owner.** A value supplied to keep moving is stated by somebody with standing — a project manager's "work with 300 until we know" is a source, and the value is stated by it. A quantity computed from other values is either design, beyond the seam (as verification is, OQ10), or stated by whoever computed it, as a source. An implementation's default is a suggestion carried by a parameter's ask, never a value | Nothing the two states made possible is lost; each case gets the actor it always had. This also bears on OQ28: sizing knowledge, a computation from a requirement's values to its answer, is not held in the requirement model |

**What remains of the value-state model** is one rule — a value names its source; where there is none, the
value is missing — and the questions that carry what the states used to: a missing value is a
`RequirementClarification`, a disagreement a `RequirementChoice`. `spec/04-value-states.md` is rewritten
around that rule, and the K-decisions that read the five states are restated (§7).

## 3. The complete requirement, walked once

| # | Decision | Reason |
|---|---|---|
| K128 | **A requirement is complete when every parameter it has has a value and no choice about its values is open. The walk of the `RuleSet`s that reach it runs once, when it becomes complete, and never on a requirement that is not.** This revises K86's "when a requirement arises" | A rule then always judges settled values. K103's undecided criterion cannot occur, OQ29's second walk does not arise, and no rule has to be written to decide what may run on a half-finished requirement. The half-finished requirement is not hidden meanwhile: its missing values and its open choices are questions the project manager sees |
| K129 | **A complete requirement is never changed in place.** A change of value is a decision stated in a source — a `SourceDecision`, refined into a `RequirementDecision` that retires the requirement (K62) — and a new requirement, refining the same need with the new value, which is walked once when it is complete. A source that states a different value without deciding anything raises a `RequirementChoice` on the complete requirement, whose alternatives are its value and the new one | Every walk is then over a requirement that will not move under it, and the history stays whole: what the requirement was, what replaced it, and who decided. Whether a later passage decides a change or merely says something different is read when it is extracted — the modeller's judgement, the modeller's responsibility (K40) — and whether its speaker has standing to decide is OQ16's |

**A correction is not a disagreement.** "We now want 800, not 300" decides, and goes the way of K129's first
sentence; "800 are coming" says something else, and raises a choice. Neither path lets anything resolve itself:
every change of a value has a decision behind it, and the decision has an owner.

## 4. What a baseline carries

| # | Decision | Reason |
|---|---|---|
| K130 | **A baseline carries only the requirements in force, each complete, with its finished wording and the derivation edges between them. Nothing of a requirement's values, sources or questions crosses the seam: the requirement model's `Requirement` no longer carries values.** K35 is confirmed, and the binding contract's fourth declaration — how far a binding carries the value model — is withdrawn | K35 already projects only the requirements in force, having superseded K34, which carried retired ones across: a design language has to be able to take a baseline on its own terms (K2), and SysML v2 has no way to hold a requirement no longer in force. What is new is the values. The finished wording already states every value a requirement has (K111), so carrying the values beside it carries nothing a design language needs and something it cannot hold — a value's source. K13's condition still holds, since what the baseline leaves out stays in the working model, and with values no longer carrying states a binding has no value model to declare |

| K131 | **The collection has three members: the requirement model, the requirement analysis model and the Project Lifecycle Model.** What remains of the value-state model — a value names its source or is missing, and a value domain with its comparability — is stated in `02-requirement-analysis-model.md`, where parameters and a requirement's values are. `04-value-states.md` is withdrawn, and **the number 04 is retired: no later document of `spec/` takes it**. This revises K19 | Values now occur only in the requirement analysis model and in a rule's guard over it, not in the product and not beyond the seam (K130), so nothing is left for the value model to crosscut; what was a member is two paragraphs of the document where values live. The number is retired rather than reused because every citation of `04-…` in the decision record and in the dated design records must keep meaning the value-state model, and a later document numbered 04 on another subject would make each of them ambiguous. K31's rule — one document per member — still holds |

**A baseline over an incomplete requirement** in force cannot carry its finished wording, since its template
cannot be filled. Whether a baseline may be cut while one exists is OQ35.

## 5. What it opens

| # | Question | When answerable |
|---|---|---|
| OQ33 | Does a rule whose test reads no value — a `CompletenessRule`, which asks only whether a requirement of some kind exists — wait for its requirement to be complete? Waiting keeps the walk single; it also delays the discovery of a structural gap until the last value of a requirement arrives | When a project shows whether a completeness gap found late costs anything |
| OQ34 | What happens to a requirement that never becomes complete, because a value never arrives? Its rules never run. This is OQ18's second half, the gap-timeout rule, met at the level of a requirement rather than a question | With OQ13 and OQ18, which already hold the interval and the elapsed time |
| OQ35 | May a baseline be cut while a requirement in force is incomplete? K13 asks that everything in force be present, and an incomplete requirement cannot be present with finished wording | Before the first baseline is cut from a project model |

## 6. What stands of the record on a rule over one value

That record (K115–K124) was written against the five states. Restated against K125–K129:

- **K115, K116 stand, restated.** `ValueRule` ranges over one parameter of one requirement; every parameter's
  ask is one, and it fires on a **missing** value and on a **disagreement**, not on states.
- **K117 is withdrawn.** There is no assumed value for an organisation's rule to name; an assumption is a
  stated value with an owner (K127).
- **K118 stands, restated by K126:** a disagreement raises a `RequirementChoice` per requirement and parameter,
  over every competing statement; a further one adds to it.
- **K119 stands, narrowed:** a `RequirementClarification` is raised by a missing value. The TBR half of its
  adopted vocabulary goes with *assumed*; TBD stays.
- **K120 stands.** What is present when the rule fires still decides the specialisation.
- **K121 stands, restated:** a clarification is open while the value is missing, and closes when a source
  states it, or when its requirement leaves force.
- **K122 and K123 stand.** K123's "whose value is conflicting" reads, after K126, "whose value is missing while
  the choice between their values is open".
- **K124 is superseded by K128.** There is no second walk, because there is only one.

**What that record answered, after this one.** OQ30, OQ22 and OQ24 stay closed. OQ25 dissolves: the marking
it asked about was a marking on a state, and the open question now is the marking. OQ18's first half
dissolves rather than being answered by K117: with no assumed value, nothing is a silent default. OQ29 is
answered by K128 in full, including the case K124 left open, since nothing is ever judged twice.

## 7. What it revises elsewhere

- **The founding record's value-state model** (kernel, `CLAUDE.md` §3 rule 4): five states become one rule.
  The founding record is a historical snapshot and is not edited; `spec/04` carries the revision.
- **K103** narrows to nothing to decide: a guard always reads a value a source stated.
- **K86** — when the walk runs — by K128.
- **K98's first destination,** the conflicting state, becomes K126's choice.
- **K35** is confirmed and **K4's fourth declaration** withdrawn, by K130; `spec/05` §4 and
  `bindings/sysml-v2.md` §4 follow. `spec/01` §2's *values* attribute of a `Requirement` goes, and `spec/00`
  §2's account of the projection drops a requirement's values.
- **K19** is revised by K131: three members, and the number 04 retired.
- **OQ1's answer** keeps its verdict and loses one reason: the value-state model is no longer a step that is
  carried from the first, because it is no longer a model.
- **OQ28** gains K127's reading.

## 8. Notes for the plan

- `spec/04-value-states.md`: withdrawn by K131. What remains of it (K125–K127, value domains, K101) moves into
  `spec/02`, and every live link to it is redirected; the dated design records and the founding record are
  snapshots and keep theirs. The retired number is stated in `spec/00` and in `CLAUDE.md` §4 so that it is not
  reused.
- `spec/02`: §10 (the derivation; the complete requirement, K128; the correction, K129), §11 (questions,
  together with the other record's K115–K123 as restated in §6), §12.
- `spec/03` §3: the walk runs once on a complete requirement; K103's guard text; the flowchart.
- `spec/01` §4: the baseline, K130. `spec/05` §4 and §5; `bindings/sysml-v2.md` §4.
- `spec/00`: the reading order and the place of the value model in it.
- `spec/06`: K115–K131 (the other record's, as restated here, and these); OQ22, OQ24, OQ25, OQ29, OQ30
  closed or dissolved; OQ18 narrowed to its second half; OQ33–OQ35 opened; K117 revoked; K19 revised; the founding
  value-state model noted as revised.
- The editor and the contract carry no values today, so nothing there changes; a `ValueRule` an organisation
  would state for assumed values, which the other record expected the editor to need, is no longer needed.
