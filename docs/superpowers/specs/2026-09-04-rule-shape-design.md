# What a `Rule` is for, and the shape that follows from it — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K82–K89, closes OQ20 in
full, and opens three narrower successors, OQ21–OQ23. `spec/` has not been changed: the change touches
`spec/03-project-lifecycle-model.md` (§3's `RuleSet` and `Rule` subsections) and
`spec/02-requirement-analysis-model.md` (§11's `RequirementQuestion` subsection and its findings discussion) —
a change of this shape needs a plan of its own, on the same terms the two integration passes before it had.

**Date:** 2026-09-04
**Follows:** [`2026-09-04-project-lifecycle-model-design.md`](2026-09-04-project-lifecycle-model-design.md),
whose K66–K81 introduced `RuleSet`, `Rule` and the `RequirementQuestion` split without giving `Rule` a shape.
**Began as:** OQ20, raised in that integration's own whole-branch review — three gaps visible only once the
material was read as a finished whole rather than task by task.

---

## 1. Where this record came from

OQ20 named three gaps: `Rule` had no stated attributes though two other decisions already reasoned about its
condition and its identity; whether `RequirementQuestion`'s *triggered by* was optional was never stated; and
K77's seating of `RequirementQuestion` in the *review finding* family did not survive contact with that
family's own rules.

The session did not answer those three as posed. Two corrections from the owner reframed them first, and the
answers follow from the reframing rather than from the questions as written.

**The first correction: K76 describes the wrong mechanism.** K76 reads as though a `Rule`'s condition were
evaluated for its truth value against a `Requirement`. What actually happens is looser and more human: a
`RuleSet` is a written procedure — a checklist, a knowledge base — walked when a new requirement arises in
that subject, with judgement deciding which entries are relevant. The classification K76 gives (semantic, not
syntactic) is right and survives; its description of the mechanism does not.

**The second correction: a `Rule` does not prescribe an outcome.** It directs attention. It says which
subjects must be dealt with, never what the answer should be — and a negative answer to a subject it raises
is still an answer. This is what separates `Rule` from `RequirementDefinition`: a definition says what a
requirement of some kind looks like; a rule says only that this subject has to be thought about.

Everything below follows from those two.

## 2. What a `Rule` is for

| # | Decision | Reason |
|---|---|---|
| K83 | **A `Rule` directs attention rather than prescribing an outcome.** It states which subjects must be dealt with when a requirement arises under the `RequirementDefinition` it hangs on; it never states what the resulting requirement should say. A negative answer to a subject a rule raises — *this project needs nothing here* — is a full answer, and appears as a `Requirement` like any other, which is what the `RequirementInquiry` then `discharges` to. While no such `Requirement` exists, the question stands open, which is K79's existing rule rather than a new one. The closing `Requirement` is produced the ordinary way — a source, a `SourceNeed`, `refine` — never by the question itself | The distinction is what keeps a rule-set from quietly becoming a second `RequirementDefinition` layer. A rule that prescribed content would state what a requirement's wording should be, which `spec/03` §1 already rules out of a rule-set's territory, and which K66 places on `RequirementDefinition` instead. Stating that the closing `Requirement` arrives by the ordinary route protects two rules at once: K11, because the commitment still enters through a source, and the actor rule, because the modeller's own instrument — the question — is not what commits the project. It also means a subject raised and declined leaves a record, so a later reader asking why some subject carries no requirement finds an answer rather than silence |

## 3. `RuleSet` and `Rule`'s shape

| # | Decision | Reason |
|---|---|---|
| K82 | **Exactly one `RuleSet` belongs to each `RequirementDefinition`, and it may be empty.** This revises K68, which made it zero or one | The distinction between "no `RuleSet`" and "a `RuleSet` holding nothing" carries no meaning, and removing it removes a null case from every reading of the structure. What is left is a `RuleSet` that is effectively a property of the `RequirementDefinition`, modelled separately because K22 makes a rule-set a model of its own |
| K84 | **A `Rule` carries three things: an identity, *when it applies*, and *what to consider*.** *when it applies* states in prose when the rule is relevant; *what to consider* states in prose which subject it raises. Both are prose, neither is an evaluable expression | The split is what makes the judgement in K86 tractable: the reader deciding relevance reads one field, not the whole rule. Neither field is coined — `RequirementDefinition` already carries a *when it applies* on exactly these terms (D20), and a *what to ask* that raises a missing **parameter** where *what to consider* raises a missing **subject**, one level up. House rule 10 is met by adopting this collection's own established forms rather than inventing a third |
| K85 | **A `Rule`'s identity is local to the `RequirementDefinition` that owns it; the full identifier is the composition of the two.** The metamodel does not say whether that composition is realised as text or as a reference | Locality follows the structural attachment K68 already establishes: a `Rule` is reachable only through its owner, so an identifier that is unique beneath that owner is unique in the model. Refusing to fix how the composition is written down is K15 holding — spelling out a composed identifier would be notation, and an implementation's identifier space is exactly what K4's fourth declaration already leaves to it |

## 4. How matching actually works

| # | Decision | Reason |
|---|---|---|
| K86 | **Rule-matching is a relevance judgement made while walking a `RuleSet`, not the evaluation of a condition against a `Requirement`.** A `RuleSet` is a written procedure walked when a new requirement arises in its subject; a reader — human or AI — judges which entries are relevant. The classification K76 gives is unchanged: this is a semantic constraint (K24), because the meaning of free text is matched against the meaning of free text, which no conventional algorithm decides. This revises K76's description of the mechanism, not its verdict | K76's phrasing implied a per-requirement truth evaluation, which is not what anybody does and not what a rule-set is. The correction also shows that *subject* needs no new concept: a `RuleSet` hangs on a `RequirementDefinition`, and the `RequirementDefinition` hierarchy is the subject hierarchy, so "the rules relevant in this subject" is exactly "the `RuleSet`s on this `RequirementDefinition` and its ancestors" — the walk K69's inheritance already defines |

## 5. Where a `RequirementQuestion` comes from

| # | Decision | Reason |
|---|---|---|
| K87 | **A `RequirementQuestion`'s *triggered by* is mandatory, without exception.** Every `RequirementQuestion` names the `Rule` that produced it. K78 stated this already; what was missing was the reason it holds without exception, and the correction of `spec/02` §11's own wording, which lists a `RequirementDefinition`'s *what to ask* in a way that reads as a second origin when it is not one — that gap is covered by the definition's own mechanism, not by raising a question | The rule-set **is** the procedure: it can be amended, but not departed from. A modeller who notices something no rule covers proposes a rule rather than raising a question outside the procedure — and proposing is as far as the modeller goes, since adding one commits the project and is therefore the project manager's act, which is the actor rule holding exactly where it should. The consequence is the strongest available reading of the provenance house rule: every `RequirementQuestion`'s cause is not merely guaranteed to exist but named |

## 6. What a `Rule` does not carry

| # | Decision | Reason |
|---|---|---|
| K88 | **A `Rule` carries no provenance. The metamodel does not record what produced a rule or who approved it** | This is K46's boundary seen from the other side. K46 stops provenance at the source because the metamodel cannot see the procedures behind a stakeholder's words; a rule-set is one of those procedures — the organisation's or the project manager's way of working — and its own origin is outside what this model can see. The chain does not break where it matters: a `Rule` commits nothing and decides nothing, it only raises a question, and when the project manager answers that question the answer enters as a source and runs through K11 like every other commitment. Recording a rule's own authorship would model the organisation rather than the project |

## 7. Where `RequirementQuestion` sits among findings

| # | Decision | Reason |
|---|---|---|
| K89 | **A `RequirementQuestion` is not a *review finding*. It is the product of a third checking mode, which this decision names for the first time.** This narrows K77, which seated it in the review-finding row of `spec/02` §11's three-way table | Three checking modes exist, and only two had names. **Static model checking** decides without judgement and produces a failed check or a question, recomputed rather than modelled. **Walking a `RuleSet`** requires judgement (K86) and produces a `RequirementQuestion`. **Review** requires judgement and produces a review finding. K77 saw only two categories — judgement or none — and so pushed `RequirementQuestion` toward review findings on the strength of three shared properties. But the table classifies what a *review* produces over the model (K10), and walking a rule-set is ordinary modelling work performed when a requirement arises, not a separate act of review. The rules the table's third row states confirm the separation rather than merely failing to fit: a review finding *"is opened by a source"* where a `RequirementQuestion` is raised by a rule firing, and *"nothing marks a finding closed directly"* where `discharges` does exactly that. A further argument stands on its own: the review act itself is unworked (OQ23), and binding a finished element to an unfinished family would move the element for reasons that have nothing to do with it |

## 8. What this record leaves open

| # | Question | When |
|---|---|---|
| OQ21 | **Should a `Rule` carry parameter criteria, allowing an algorithmic filter to run before the semantic judgement?** The shape is a guard, as a flowchart uses the word: it could *exclude* a rule mechanically, never admit one, so the judgement K86 describes still decides everything that reaches it — a narrowing of what reaches judgement, not a third category beside K24's two. Half of the machinery exists already: a `RequirementDefinition` declares `parameters` and a `Requirement` carries `values`, each with a value state. What is missing is the criterion on the `Rule` side and the join between them | When the cost of judging every rule in a walked `RuleSet` is actually felt, which needs an implementation running the loop. Until then it is an unexercised construct and waits |
| OQ22 | **How does a fired rule become a posed question?** A rule detects that a subject needs dealing with; the modeller writes the question. What governs that step is unstated: whether one firing yields one question or several, whether *what to consider* supplies a template for the wording or the modeller writes freely, and whether K75's "at most one open `RequirementInquiry` per `Rule` at a time" is a general rule or specific to `CompletenessRule` | With the `Rule` specialisations, since the last part is a question about one of them |
| OQ23 | **What is a review, as an act?** `spec/02` §11 states a review finding's lifecycle — a source opens it, a later source that `replies` to it closes it — but nothing states the act that produces one: who performs it, when, against what. Only static model checking and, now, walking a `RuleSet` are worked out | Unforced. It is recorded because K89 makes the gap visible while deliberately not entering it |

## 9. What this record does not do

**The `Rule` specialisations keep no shape of their own here.** `ConflictRule` and `CompletenessRule` were
named and given a mechanism by K73 and K74, but neither has been worked out as a type — nor have the two
mechanisms OQ18 still holds open. In particular, what names the companion kind a `CompletenessRule` looks
for — a typed reference to a `RequirementDefinition`, or prose inside *what to consider* — is not settled
here. That question was raised in this session and withdrawn as premature: it is answerable when the
specialisation itself is worked out, and not before.

This record therefore fixes only what is common to every `Rule`. The specialisations wait for a session of
their own, alongside OQ18.
