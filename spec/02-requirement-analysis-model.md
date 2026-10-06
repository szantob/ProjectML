# The requirement analysis model

## 1. What this model is

This is the requirement analysis model, one member of the collection ProjectML metamodels (K19). It is the
**working model**: the model in which a requirement system is actually built, rather than the model handed
to somebody who was not in the room while it was assembled.

Its elements divide by which language they are in (K43): one side is somebody's words, anchored in a passage
of a source; the other is the model's own bound terms. A `Source` yields `SourceElement`s — `SourceQuestion`,
and `SourceStatement`, itself specialised into `SourceNeed` and `SourceDecision` (K44, K45) — and each has a
counterpart on the model's own side. `SourceNeed` and `SourceDecision` cross inward, by `refine`, into
`Requirement` and `RequirementDecision`; `RequirementQuestion` crosses outward, by `poses`, into
`SourceQuestion` (K47, K48, K58, K59). `RequirementDefinition` is the definition a `Requirement` is produced
under, and findings are what a review produces over all of it.

This document defines the source side first — `Source`, the edge between sources, and the `SourceElement`
family — then the model side: `RequirementDefinition`, the derivation it governs, `RequirementDecision`,
`RequirementQuestion`, and findings.

This model **projects** to the requirement model: the product model defined in `01-requirement-model.md`,
which a reader can adopt on its own, without ever having read this document (K20). The projection itself —
what it keeps, what it drops, and on what condition it may drop anything — is defined in section 10 of this
document, once everything the projection draws from has been introduced.

One rule governs everything that follows, and it is stated here because every section after this one assumes
it: **the requirement model changes only through sources** (K11). A review does not update a `SourceNeed`, a
definition, or a requirement directly; whatever a review decided — in a meeting, on a call, or by the team's
own judgement, with no other party involved — is itself entered as a source first, and the change follows
from it. This is what keeps the chain total rather than merely well-intentioned: there is no path from
"something changed" back to "nothing said so."

## 2. `Source`

A source carries five things.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other source's |
| text | The material itself, in the words it was given in |
| from | Who or what it came from |
| kind | What kind of material it is |
| date | When it was made |

Three rules govern a source, and each rests on a decision already taken.

1. **A source is material of record.** It is quoted whole and never edited (D45). Everything anchored to a
   source — a `SourceNeed`'s passage, a citation in a review — points at words that stay exactly as given; if the
   source could change after the fact, an anchor into it would drift from what was actually said, and a
   later reader could no longer tell whether a quotation still means what it meant when it was captured.
2. **A source is never decomposed.** The raw material stays raw rather than being broken into parts and
   classified as it is captured, which would impose a classification taxonomy on it before anyone has asked
   what the material is for (D25).
3. **The kind and from attributes are open.** The metamodel names them and leaves their vocabularies to an
   implementation. The founding record's section 5 records why: EventML's own enumerations for these two
   attributes carry a domain leak — a kind of material and two things it can come from that exist only in
   that domain — and a vocabulary fixed here would carry the same leak into every project that adopts this
   metamodel. K30 makes the same move for a requirement's kind, though through a different mechanism: there
   the vocabulary is carried by specialisation of `RequirementDefinition` rather than by an attribute. What
   the two share is the metamodel naming a slot and leaving an implementation to fill it.

## 3. The edge between sources

Sources connect to each other through exactly one edge: a later source **`replies`** to an earlier one. Four
properties hold of it, each already settled:

- It sits on the later source and names the earlier one it responds to, not the reverse (D35).
- It points backward in time: a source can only reply to something that came before it (D38).
- It changes no value on its own. Both the earlier statement and the later one stand as made; which of them
  prevails, if they disagree, is not decided by the edge but by a decision recorded separately (D37).
- One edge relates exactly one source to exactly one source, but a source may carry any number of them —
  replying to several earlier sources, or being replied to by several later ones (D29).

The founding record's section 5 makes a further finding about this edge worth carrying forward here: `replies`
is the natural closing edge for a review finding. A finding is opened by a source and closed by a later one
that replies to it, so closing a finding is not a tick somebody applies to a record — it is itself evidence,
carrying the same source that closes it as everything else in this model does. Section 11 uses the edge on
exactly these terms.

## 4. `SourceElement`, `SourceQuestion`, and `SourceStatement`

A source is never read directly by the rest of this model. What a source yields — what a `Source`'s text is
segmented into, one unit at a time — is a `SourceElement`.

```mermaid
classDiagram
    class SourceElement {
        <<abstract>>
        identity
        anchor into one passage of one source
        material of record
    }
    class SourceStatement {
        <<abstract>>
    }
    SourceElement <|-- SourceQuestion
    SourceElement <|-- SourceStatement
    SourceStatement <|-- SourceNeed
    SourceStatement <|-- SourceDecision
    SourceNeed <|-- SourceUpdate
```

The diagram draws what this section and the next state; where the two disagree, the prose wins. The
abstraction and generalisation conventions are the diagram language's own, adopted rather than coined (K32).

### `SourceElement`

`SourceElement` is **abstract**. Every element a source yields is some specialisation of it, and it carries
three things, shared by every specialisation and nothing beyond them.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other `SourceElement`'s |
| anchor | A passage of exactly one source, on the same terms `01-requirement-model.md`'s predecessor attribute did — adopting the W3C Web Annotation Data Model |
| material of record | Never edited, and carrying no lifecycle state. Inherited from the source it anchors into: a source is quoted whole and never decomposed (§2), so nothing anchored into one can cease to be true while the source behind it stays what it was |

**A `SourceElement` segments; it does not interpret.** Nothing a `SourceElement` carries is a reading of
what its passage says — no extracted value, no restated content, nothing beyond the fact that this passage
exists and is anchored. Interpretation happens only on the crossing to the model's own side, governed by
whatever definition's rules apply there (K43, K57). This is a syntactic constraint over every
`SourceElement`: none of them carries an attribute beyond the three above, and a candidate attribute naming a
reading of the passage's content does not belong here, on K43's own axis — it would let something already
interpreted sit on the side of the model that K43 keeps to segmentation alone.

**Provenance stops at the source.** This model does not say what produced a stakeholder's own words, because
it cannot see the procedures behind them. `Source` is the root of this model's traceability not because
nothing precedes it, but because nothing before it is visible to a model built from what a project was told.

**The list of specialisations is not closed.** Two exist today, on the terms the next two subsections state:
`SourceQuestion`, and `SourceStatement`. K42's reasoning, stated for the Project Lifecycle Model, applies
here without change: a closed list would fix one way of reading a source into this model, and a fifth kind
arriving is evidence to account for, not a violation of what stands today.

### `SourceQuestion`

A `SourceQuestion` is a `SourceElement` — nothing more. It carries the three shared attributes and no
others: an unanswered `SourceQuestion` names no state of its own beyond what `SourceElement` already gives
it.

**An unanswered `SourceQuestion` is normal.** It is the ordinary condition of a project with something
outstanding, not a defect. This is the reason `SourceQuestion` sits beside `SourceStatement` rather than
beneath it: were it a `SourceStatement`, every unanswered question would report as a failed check under the
rule the next section states, which would be wrong — a question asserts nothing, so nothing about it can go
unfulfilled the way an unrefined statement can. It opens something instead, and what closes it is the
`replies` edge (§3): a later source `replies` to the source the question's passage sits in.

**How `SourceQuestion` subdivides, and whether it names the party expected to reply to it, is not settled
here.** Nothing today reads such a subdivision. This is recorded as OQ16 in `06-decisions.md`.

### `SourceStatement`

`SourceStatement` is **abstract**, a specialisation of `SourceElement` carrying nothing beyond what
`SourceElement` already gives it. It has two specialisations of its own, `SourceNeed` (§5) and
`SourceDecision` (§6).

**A `SourceStatement` that produces nothing is a failed check, not a question.** Unlike `SourceQuestion`'s
silence, a `SourceStatement` asserts something — it is the source-side counterpart of a passage that
obliges an outcome on the model's own side — so one that never crosses over is a record in which something
asserted went unaccounted for. §11 states this rule for `SourceNeed` in full, on terms `SourceDecision`
inherits without needing its own restatement of the same rule, because both are `SourceStatement`s.

## 5. `SourceNeed`

A `SourceNeed` is a `SourceStatement` (§4) whose passage **obliges something**. That narrowing is the whole
of it, and what follows in this section and in §11 follows from it: a passage of a source that obliges
nothing is not a `SourceNeed` which happened to produce nothing, it is not a `SourceNeed` at all, and
anchoring it as one was a mistake (K37).

**Extraction is already selective, and always was.** Nobody anchors a greeting or a signature as a
`SourceNeed`, and nothing in this model ever asked anybody to. A source permanently contains text no
`SourceNeed` cites, and that text is not a defect in the model. The prior art reached the same conclusion
from the other end and declined to make uncited text a rule, on the ground that a condition which never
clears is not a question (D33); this metamodel goes one step further and states nothing over uncited text at
all, for reasons §11 gives and K41 records. *Not need-bearing* is therefore an existing and ordinary
category, and it needs no record of its own. A passage nobody anchored leaves no element behind for a record
to sit on.

**A passage of a source is either a `SourceNeed`, another `SourceStatement`, a `SourceQuestion`, or it is
uncited.** This model defines no context element — no element for a statement worth keeping that obliges
nothing — and it is not to acquire one, because the case that would motivate one does not survive
examination. A statement about the environment the work happens in, about where it happens or when a place
is available, is a constraint on the environment the system must work within, and a constraint on the
environment obliges something exactly as a statement of what somebody wants does. The prior art already held
this without drawing the conclusion out of it: the record indexed at D51–D55 in
[`docs/eventml-decisions.md`](../docs/eventml-decisions.md) groups its definitions by origin and carries a
class brought in by where the work happens, whose missing information is resolved by measuring rather than
by asking anybody's opinion — a fact to be established, not a remark to be filed. The same record's
comparison with SysML v2 finds SysML's nearest category a poor fit on the same ground: SysML's constrains
the *system's* physical properties where this class describes the *environment* constraining the system.
Both readings treat such a passage as bearing a requirement. Neither treats it as inert.

A `SourceNeed` carries nothing beyond `SourceElement`'s three shared attributes — identity, its anchor, and
being material of record (§4, K57). It does not carry a value: what a `SourceNeed`'s passage expresses, once
interpreted, is a reading of the passage rather than a fact about it, and a reading belongs on the model's own
side, in the values a `Requirement` carries in this model once `refine` (§10) has run (§7, K57, K136). This
revises how D27 was previously read as applying directly to this element: a `SourceNeed` is not a place a
value occurs, because nothing on the source side is a value at all. A value occurs on a `Requirement`, and is
stated by a `SourceNeed` the requirement refines (§7, K136).

The name is adopted rather than coined: *stakeholder need* is ISO/IEC/IEEE 29148's term (D23), carried by
`SourceNeed` on the same terms K47 states for every prefixed element — the prefix marks which side of the
model the element is on, exchanging one qualifier (*stakeholder*) for another that also disambiguates it
from `RequirementDefinition`'s and `01-requirement-model.md`'s independent use of *statement*-adjacent language.

Two rules govern a `SourceNeed`, beyond what `SourceElement` already governs for every `SourceElement`.

1. **A `SourceNeed` anchors into exactly one passage of exactly one source.** A `SourceNeed` with no anchor
   fails a syntactic check: there is nothing in it to ask a stakeholder about, and nothing for a reviewer to
   weigh — only an omission to fix (D34, D26). This is `SourceElement`'s own anchor requirement, restated
   here because it is the constraint a `SourceNeed`'s own syntactic-constraints entry (§12) cites.
2. **A `SourceNeed` that produces no `Requirement` is a failed check, not a question** (K38, D31, and §4's
   general statement of this rule for every `SourceStatement`). A `SourceNeed` obliges something by
   definition, so one nothing refines is a record in which something obliged is unaccounted for. EventML
   shipped this as a question rule (D31); K38 overturns that, and
   [`docs/eventml-decisions.md`](../docs/eventml-decisions.md) records the overturn.

Passage anchoring adopts the W3C Web Annotation Data Model (D26), stated once for every `SourceElement` in
§4 rather than repeated per specialisation. No requirements standard was adopted for it instead, because
none serves here: a `SourceNeed` anchors into a source before any requirement exists, at a stage SysML v2
places outside itself and has nothing to say about.

### `SourceUpdate`

A `SourceUpdate` is a `SourceNeed` whose passage, besides obliging something, **replaces what an earlier
statement said** (K132) — a client's *we now want 800, not 300*. It is a `SourceNeed`, so everything this
section says of one holds of it: it obliges something, it is refined into a `Requirement`, and one nothing
refines is a failed check (K38). Like every `SourceElement` it carries nothing beyond identity, anchor and
being material of record (K57): what it replaces is not read off the passage on this side but on the model's
own, where the requirement refining it names, by `supersedes`, the requirements it replaces (§10, K133).

**A correction is neither a contradiction nor a decision.** A passage that states a different value without
replacing anything — *800 are coming* — contradicts what was said: the `SourceNeed` anchored in it is refined
into a requirement of its own, and the parameter's ask raises a choice over the requirements that contradict
(§10, §11, K139, K141). A passage that drops something with nothing in its place — *we no longer need this* —
is a `SourceDecision` (§6). A correction is neither: read as a decision it would owe alternatives and a
rationale it rarely states, which the modeller would then have to supply. Which of the three a passage is, is
read when it is extracted, and is the modeller's responsibility (K40); whether its speaker has standing to
replace what was said is OQ16's.

## 6. `SourceDecision`

A `SourceDecision` is a `SourceStatement` (§4) whose passage records a decision as somebody stated it — a
project manager's note that the client decided X, a client's own email settling a choice, a meeting record of
an agreed outcome. Like `SourceNeed`, it carries nothing beyond `SourceElement`'s three shared attributes:
identity, its anchor, and being material of record. What the decision means for the requirement model — what
it retires, and, once the Project Lifecycle Model states the criterion, what finding it closes — is not read
off the `SourceDecision` itself; it is produced on the model's own side, by `refine`, as `RequirementDecision`
(§10, §11).

The name and its shape are adopted from the same source `SourceNeed`'s is: ISO/IEC/IEEE 42010's *Architecture
Decision*, carried here as the record of a decision **as stated**, prefixed on K47's terms to mark it as
belonging to this side of the model. The interpreted decision — the choice among alternatives, and the
rationale for it — is not this element's to carry; §11 states where that interpretation lands and why.

**A `SourceDecision` that produces no `RequirementDecision` is a failed check, not a question.** This is not
a new rule: `SourceDecision` is a `SourceStatement`, and §4 already states that a `SourceStatement` producing
nothing is a failed check (K38's rule, generalised by K45). Nothing about `SourceDecision` needs its own
version of this rule; it inherits it.

**A `RequirementDecision` never exists without a `SourceDecision` behind it, and this is the sharper,
asymmetric half of the same pair.** Where an unrefined `SourceDecision` is a failed check — a record that
can exist, awaiting resolution — a `RequirementDecision` with no `SourceDecision` origin is not a lesser
version of the same problem; it is excluded outright, as not a well-formed element of this model at all (K61,
K64, and §11 states the rule in full where `RequirementDecision` is defined). What stands for "a decision the
modeller knows must be taken, but has not been" is a `RequirementQuestion` in the raised state (§11), never a
bare `RequirementDecision`.

## 7. `RequirementDefinition`

`RequirementDefinition` is **abstract**. No element in a model is a `RequirementDefinition` and nothing
more: a definition exists only as a specialisation of it, and which specialisations exist is declared by an
implementation rather than here (K30). Section 9 gives the reasoning and says what the specialisation
relation carries; this section says what every definition carries, whatever it specialises.

The name is adopted rather than coined. SysML v2 splits an element into a definition and a usage of it, and
`RequirementDefinition` is the definition half of that split on the same terms: the thing a requirement is
produced under, not the requirement itself.

A definition carries nine things, three of which do not apply to an abstract definition (below, K110). This is
the **core** — what the metamodel can interpret, or can fail on, without reading anything an implementation
supplies (K27).

| Attribute | What it is |
|---|---|
| identity | The definition's identifier |
| name | Human-readable |
| abstract | Whether the definition is abstract: no requirement is produced under it, only under its specialisations (K109). A definition is abstract only where it says so, and otherwise is not; abstractness is never read off the absence of anything else |
| text | The template the requirement's wording is produced from, with places for its parameters |
| when it applies | One sentence stating when this definition comes into play. It is prose, not an evaluable expression (D20). Its absence means applicability has not been written down, which is a gap, not a claim that the definition applies unconditionally |
| parameters | Each parameter declares a value domain, and carries an identity local to the definition declaring it (K106). Which domains exist is an implementation's business, exactly as the set of kinds is (K30). What a domain declares about its values is stated below (K101) |
| what to ask | For each parameter, how a non-expert is asked for what is missing. It is a rule: where the parameter has no value on a requirement it raises a clarification, and where the modeller has judged, at extraction, that requirements of one kind state values of it for the same thing, and the values contradict, it raises a choice between them (K116, K141) |
| how it would be verified | The method by which a requirement produced under this definition would be shown to hold. Prose |
| wording rule | A well-formedness rule for the wording a requirement produced under this definition must satisfy. Prose, on the same terms *how it would be verified* is prose (K66) |

**A value exists only where a `SourceNeed` states it** (K125, K136). A parameter's value on a requirement is
stated by a `SourceNeed` the requirement refines, and where none states one the value is missing; nothing else
puts a value into the model, and a requirement reaches a source only through the `SourceNeed`s it refines. A
value is never supplied by the modeller, who administers and decides nothing for the project. A value supplied
to keep work moving is stated by somebody with standing, in a source like any other, and reaches the
requirement through a `SourceNeed` anchored there; a quantity computed from other values is design, beyond the
seam, or is stated by whoever computed it, as a source; and an implementation's default is a suggestion a
parameter's ask may carry, which becomes a value only when somebody states it (K127). How a value and the
`SourceNeed` stating it are written down is notation, and an implementation's (K15). The ask is how a missing
value is obtained from somebody who holds it — which is why *what to ask* sits beside *parameters* and is
written per parameter rather than per definition.

**The metamodel enumerates no value domains.** A value has a domain — the range of things it could be — but
which domains exist, and what they are called, is declared by an implementation rather than fixed here. This
is the same move K30 makes for requirement kinds: the metamodel provides the slot a domain fills without
naming what goes into it.

**A domain fixes no unit; it declares how its values compare** (K101). Leaving the *set* of domains to an
implementation left open whether a domain also fixes a unit, and what makes two values comparable. A
`ConflictRule` does not need an algorithm to answer that: its test reads both requirements' texts and is a
judgement (`03-project-lifecycle-model.md` §3, K90). The first construct that compares values without
judgement is a `Rule`'s guard, and a guard compares one parameter's value with a constant written against that
same parameter — never values of two domains. What a domain declares is therefore what a guard needs: exactly
one of three levels of comparability, **not comparable**, **comparable for equality**, or **ordered**, the
last including the second. How an implementation achieves the level it declares — a fixed unit, a dimension
with its conversions, an enumeration, anything else — is its own business, exactly as the set of domains is.
Two domains for one measure in different units are, under this, two ordered domains, each in its own unit, and
no guard ever converts between them.

**A definition may be abstract** (K109). No requirement is produced under an abstract definition, only under
its specialisations: it exists so that the definitions beneath it share what it declares. A definition is
abstract only where it says so, and otherwise is not: abstractness is never read off the absence of anything
else — an empty template is a template nobody has written yet, not a declaration that nobody may produce a
requirement under the definition. The term is adopted rather than coined: SysML v2 marks a definition
abstract, and KerML's `isAbstract` says that whatever an abstract type classifies is also classified by one of
its specialisations, which is this reading one level down. `RequirementDefinition` itself is abstract on the
same terms (K30).

**An abstract definition carries no template, and neither *how it would be verified* nor the *wording rule*
applies to it** (K110). All three speak of a requirement produced under the definition: the wording it is
produced from, how it would be shown to hold, what its wording must satisfy. No requirement is produced under
an abstract definition. The other six apply as they do to any definition — *when it applies* still decides
when the definition comes into play, and a parameter it declares is still filled, through its specialisations,
so it still needs its *what to ask*.

**A parameter's identity is local to the definition declaring it** (K106). A `Rule`'s guard names a parameter
(`03-project-lifecycle-model.md` §3), and *what to ask* is already written per parameter, so a parameter has
to be nameable. It exists only on the definition that declares it, so an identity unique beneath that
definition is enough — the precedent K85 sets for a `Rule`'s identity. How the identity is written down is not
fixed here.

**Why *how it would be verified* sits on the definition rather than on the requirement.** A verification
method is generic to a kind of requirement: how a thing of this kind would be shown to hold is a property of
the kind, and the definition-and-usage split puts a generic property on the definition. ISO/IEC/IEEE 29148
makes verifiability a required characteristic of a *requirement* rather than of a definition, and the two
statements do not conflict — a requirement takes its definition's method, so a requirement produced under a
definition that carries one is verifiable in 29148's sense without carrying the method itself. What is
genuinely instance-side is not the method but what actually verified one particular requirement, and nothing
in this model carries it: what verifies a requirement is a design decision, taken beyond the seam (K29; OQ10,
closed). Whether a definition needs the method at all is OQ32.

**Why a definition also states a wording rule, beside its template.** *text* gives the structural template a
requirement's wording is produced from; it says nothing about the qualities that wording must have once
produced. `03-project-lifecycle-model.md` §1 already states, of a rule-set, that it *"says nothing about...
what a requirement's wording should be"* — which settles where such a rule does **not** belong, without saying
where it does. ISO/IEC/IEEE 29148's own *characteristics of a good requirement* (unambiguous, singular, and so
on) is exactly this second, missing thing, generic to a kind rather than to one requirement, which is what
places it on the definition rather than the instance (K66). The seam test and the record test both admit it
on exactly the argument that seated *how it would be verified* (K29): the rule can be stated without resolving
a reference to an element the metamodel does not define, and a stated rule can fail on its absence.

**What the metamodel does not say about any of the nine is how it is written down.** Two definitions carrying
the same nine things are the same definition to this metamodel however differently they are set out. Beyond
the core, a definition holds whatever an implementation's own notation and rule-set need: the core is a floor
the metamodel can reason over, not a ceiling (K27).

**Whether that opening is where a company's sizing knowledge belongs is open.** The derivation from a
requirement's parameters to what its answer must be — how many pieces, which kit, what height — is none of the
nine, and K83 (`03-project-lifecycle-model.md` §3) excludes it from a `Rule`. The first implementation to
carry a real domain put it in a rule's prose anyway. `06-decisions.md` records the question as OQ28, including
why that evidence does not yet settle it.

## 8. What is not on a `RequirementDefinition`, and the two tests

The list in section 7 needs a criterion that outlives it, because the next attribute somebody proposes will
not be one of the nine. Two tests decide the question, and they are stated here in full, because they are the
part of this section a later reader actually reuses.

> **The seam test.** An attribute belongs to the metamodel if the metamodel can interpret it without
> resolving a reference to an element it does not define. Prose that names a design language's things is
> content, and content belongs to an implementation; a typed reference to a design language's element would
> be a second seam.
>
> **The record test.** An attribute belongs to the metamodel if a *stated* metamodel rule can fail on it,
> including on its absence, without reading its content.

The first is K25 and the second is K26. The seam test is the one-seam rule (K3) applied attribute by
attribute rather than only to the relation between the kernel and a design language: an attribute that has to
be resolved into elements the metamodel does not define is a second seam whatever it is called, and it runs
in the direction K3 forbids. The record test is K7's posture made into a criterion, and its load-bearing word
is *stated* — a rule that might one day be written does not qualify, or everything qualifies. Note what the
record test does not require: it is satisfied by a rule that fails on an attribute's **absence**, which is
how *how it would be verified* passes without anything ever reading its prose. Section 12 states the rule
that does it.

The two tests are independent of each other. How they relate, and what that relation costs one of the nine, is
stated at the end of this section, after the one candidate both of them reject.

**The one candidate that fails both.** A rule attached to a definition, stating what must hold of the design
elements a requirement produced under it constrains, names a design language's element kinds. It fails the
seam test, because the metamodel cannot interpret it without resolving those names into elements it does not
define. It fails the record test on the same fact: no stated rule can fail on it without that same
resolution. One structural fact, not two, which is why the verdict is not close. **The metamodel therefore
defines no such concept.** K28 records the candidate under the name it was considered by. An implementation
may introduce one, and nothing is lost that had anywhere else to go, because an implementation is itself a
metamodel for the project models built with it (K16), and the elements such a rule would name are exactly the
ones an implementation and its design language define.

**One finding belongs to phase 2 and is recorded here, beside the test that produces it.** SysML makes a
requirement a specialised constraint whose formal statement is evaluated over its own subject, and the subject
is bound through the satisfy edge. That routes a constraint through the one seam rather than opening a second
— which is what the seam test predicts a design language must do, and it is the difference between a rule that
fails the test and a mechanism that passes it. Whether the seam determines the subject on its own is OQ11, and
settling it is the binding's job.

### How the two tests relate

**They are not a conjunction, and on the core they do not agree everywhere.** An earlier reading of this
section said they did. It had one candidate to reason from, and that candidate fails both tests on a single
structural fact, so it could not have separated them however they related. Applied to the nine, they separate.
Section 12 states the syntactic constraints this model genuinely carries, and no rule among them fails on
*when it applies*: its absence is deliberately a gap rather than a claim, so the rule that is available over
it reports rather than fails, and the record test's word is *fail*. Nor does any of them fail on *name*, which
the metamodel reads for no purpose of its own.

**The seam test decides admissibility. The record test measures whether an admitted attribute is
load-bearing, and is not a second gate.** The reason is that a presence rule can be written over any
attribute at will: *a definition states its name* is a rule, it takes one line, and it fails on an absence
without reading a word of content. Were the record test the admissibility criterion, it would therefore be
vacuous — every attribute passes, because the rule that admits it is always available — or else arbitrary,
with nothing to say which presence rules are worth writing and which were written to admit a favoured
attribute. The word *stated* excludes the rule nobody has written; it does not exclude the rule anybody could
write in an afternoon, and an attribute's place in a metamodel cannot turn on whether somebody got round to
it. The seam test has no such weakness. Whether an attribute can be interpreted without resolving a reference
to an element the metamodel does not define is a fact about the attribute, unchanged by which rules happen to
exist over it.

Section 7's own definition of the core already reads this way, in one word: the core is what the metamodel
**can interpret, or can fail on**, without reading anything an implementation supplies. The two clauses are
the two tests, and the connective between them is *or*. An attribute the metamodel can carry without opening
a second seam is in the core; an attribute a stated rule can fail on is in the core and is checkable as well.

**Where this leaves *when it applies*: it stays.** It is admitted by the seam test, being one sentence of
prose that resolves nothing the metamodel does not define. It is not load-bearing under the record test, and
it cannot be made so without reversing the position that its absence is a gap and not a claim — a position
taken deliberately, because a definition whose applicability nobody wrote down is not thereby one that
applies unconditionally, and a rule failing on the absence would report a defective record where what is
actually there is a question for whoever knows. Admitting this costs nothing the collection needs: the record
test's verdict on an attribute is a report on how much work that attribute does, not a verdict on whether it
belongs. Recorded as K36 in `spec/06-decisions.md`.

## 9. Requirement kinds are specialisations

A requirement kind is a **specialisation of `RequirementDefinition`**, never an attribute on it (K30). Three
reasons carry this, and they are independent of each other.

1. **It is the mechanism the neighbouring language already recommends.** SysML v2's own way of building a
   hierarchy of requirement definitions is specialisation rather than a dependency between them, and SysML
   v1's stereotypes are specialisations too. That is the only external evidence available, since neither form
   is exercised anywhere yet, and house rule 10 says to adopt where a neighbour has already chosen.
2. **It does not fix the number of classification axes; an attribute would fix it at one.** A single kind
   attribute leaves the vocabulary free but silently decides that a definition is classified in exactly one
   respect. An implementation classifying in two respects at once would then have to encode the second inside
   the first. This is D55's argument one level down: a taxonomy the language fixes excludes every design
   language that classifies on another axis, and would make the metamodel unattachable.
3. **It is the same relation the three levels already use.** K16 makes an implementation a metamodel for the
   project models built with it, and what an implementation supplies at that boundary is filled specialisation
   of the types this metamodel defines. Using specialisation for the level boundary and something else for
   kinds would describe one relation twice, in two mechanisms that could then disagree.

**`Requirement` itself never specialises, however deep or wide the `RequirementDefinition` tree grows.** The
kind hierarchy lives entirely on the definition side, and `Requirement` and `RequirementDefinition` connect
only by the *produced under* relation (§10), never by inheritance: a requirement produced under a leaf kind
is a plain `Requirement` naming which `RequirementDefinition` it was produced under, not a member of some
`Requirement`-subtype (K67). This is SysML v2's own `def`/`usage` split, checked directly against the
primary specification rather than assumed: its kind hierarchy — `FunctionalRequirementCheck`,
`PhysicalRequirementCheck`, and the rest — specialises `RequirementCheck`, stated as *the base type of all
`RequirementDefinition`s*, entirely on the definition side; `RequirementUsage` stays one uniform type
throughout, typed by whichever definition it names, never itself specialised. This confirms rather than
revises what `01-requirement-model.md` §2's K33 already reads as though true — a `Requirement` naming no
kind of its own presupposes it has none to name — stated explicitly here because the question turned out not
to be obvious without it.

**This revises prior art, and does so by permission.** D51–D55 are *imported* rather than inherited — K15
moves their subject into the metamodel, so this repository takes its own decision on it. The one revised is
D53, which has each definition name the kind it belongs to. What the others protect survives: a set of
subtypes is as enumerable and as questionable on its own as the standalone list D52 asked for, a check reads
the type where D53's check read an attribute, and D55 is not revised at all — K30 is how D55 is kept.

```mermaid
classDiagram
    class RequirementDefinition {
        <<abstract>>
    }
    RequirementDefinition <|-- KindA
    RequirementDefinition <|-- KindB
```

The diagram says what the paragraphs above it say: `RequirementDefinition` is abstract, and a kind is a subtype of
it. `KindA` and `KindB` are placeholders named after no real kind, because the metamodel declares no kinds
(K15) and naming one here would be a filled definition. The diagram uses the abstraction and
generalisation conventions the diagram language already carries and coins nothing (K32); where it and the
prose disagree, the prose wins.

**What an implementation must do:** declare its kinds, as subtypes of `RequirementDefinition`. **What the
metamodel does not do:** name any of them, say how many there are, or say on what axis they divide.

**What specialisation means is open, except for parameters.** A specialisation has every parameter its
ancestors declare, in addition to its own (K107), and declares no parameter carrying the identity of one an
ancestor declares (K108). Both are decided because a `Rule` stated on a definition reaches every
specialisation of it (`03-project-lifecycle-model.md` §3, K69), and a guard naming a parameter must find the
*same* parameter — the same domain, so the same comparability — on every descendant it reaches. An inherited
parameter is inherited whole, its *what to ask* included, and a specialisation has no ask of its own for it
(K112): every descendant is after the same value, so one ask, written on the definition declaring the
parameter, serves them all.

**A definition that is not abstract uses every parameter it has in its template** (K111). Each of its
parameters, its own and those it inherits, appears as a placeholder in its template, and each placeholder
names one of them. A template that could not name an inherited parameter would leave a filled value nowhere in
the requirement's wording; one that need not name it would let a requirement carry a value its wording never
states. An abstract definition has no template (§7, K110), so the parameters it declares are met in the
templates of the definitions beneath it.

**A definition's template and its *when it applies* are not inherited** (K113, K114). A template is the
sentence a requirement of exactly this definition is produced from, so a specialisation handed its ancestor's
would lose what makes it one; what it inherits is the parameters, never the sentence (K111). *When it applies*
guides the modeller, a person or an agent, while a new requirement is being classified, by saying which branch
of the tree is worth following, so each definition states its own. Abstractness is not inherited either: a
definition is abstract only where it says so (§7, K109).

Everything else is not defined here: whether an inherited parameter may ever be overridden or narrowed;
whether a definition's wording rule is inherited, and whether a rule constraining how one parameter's value is
written belongs to that parameter rather than to the definition; and whether its *how it would be verified* is
inherited. That is OQ9, and it waits for something to exercise it. K30 chooses the mechanism; it does not
define its semantics.

## 10. The derivation, retirement, and the projection

### The derivation

A requirement is not written; it is **derived**. The founding record's procedure states the step: a
`SourceNeed`'s passage selects the definition, and the rules on that definition turn the stater's free words
into the requirement's bound professional wording. The parameters the definition has, its own and those it
inherits (§9, K107), are filled from the passages of the `SourceNeed`s the requirement refines, and each value
is stated by one of them (§7, K136). The same crossing — a passage anchored on the source side, restated on
the model's own, under a definition's rules — is `refine`, and it is not particular to `SourceNeed`: a
`SourceDecision` crosses the same way, into a `RequirementDecision`, on the terms K58 states and this
document's §11 uses (K43, K58).

**The kind rides along with the definition, and `SourceNeed`s are not classified** (K8). This is what keeps
the two axes from colliding: a `SourceNeed` is selected against by its passage, and the classification of the
requirement that results is fixed by which definition produced it — by what that definition specialises — so
it arrives with the definition rather than being decided separately. A `SourceNeed` carries no kind for the
same reason it carries no lifecycle state — it belongs to its source, and nothing about a quotation is the
modeller's to classify.

The edge that records the derivation is the **refinement** edge. It sits on the requirement and names the
`SourceNeed`s the requirement was assembled from, by their identifiers (D28). It is list-valued rather than
singular, because a requirement is routinely assembled from more than one statement, and an edge that could
name only a single `SourceNeed` would force an arbitrary choice among equally contributing ones (D48). This is
a requirement's origin, which `01-requirement-model.md` §2 describes as projected away (K145): it is defined
here, where `SourceNeed` is defined, and the invariant that every requirement names its origin is only
decidable with this edge in view. **Every requirement refines at least one `SourceNeed`** (K145). The
derivation edge between requirements (`01-requirement-model.md` §2) is never an origin: a requirement derived
from others and refining no need would carry content the modeller produced, and the modeller is answerable for
nothing in the project. A requirement derived from another refines a `SourceNeed` of its own, the client's
answer stating the elaboration, and derives from the requirement it elaborates (`01-requirement-model.md` §2,
K146).

**A requirement also names the definition it was produced under.** Beside the refinement edge, and unlike it,
a requirement carries an edge naming exactly one `RequirementDefinition` — the definition a `SourceNeed`'s
passage selected, whose rules turned the stater's free words into the requirement's bound wording. Exactly
one, because the kind rides along with the definition (K8): a requirement's kind is read off what that one
definition specialises (K30), and a requirement produced under two definitions would have two kinds or none.
The definition it names is never abstract (§7, K109): an abstract definition exists to be specialised, and a
requirement is produced under one of its specialisations instead. This edge is what makes K13's recoverability
condition true of what K33 drops. K33 decides that the product `Requirement` names neither its definition nor
its kind, and that decision is legitimate only because the binding is not thereby lost: it is recorded here,
in the working model, recoverable rather than deleted, exactly as K13 asks of anything the projection drops.
The projection drops this edge along with the definitions it points at.

**The relation between a `SourceNeed`'s words and a requirement's wording is a semantic constraint** (K24).
The metamodel states what the relation is — a requirement's text is the professional restatement of the
`SourceNeed`s it refines — and states what a reviewer must cite when it is found broken: both texts, the
`SourceNeed`'s and the requirement's. It does not evaluate the relation. This is K7's posture applied to a
second subject: the metamodel defines what the finding is, what must be cited and how it is recorded, and
leaves the judgement to a human or an AI review, because a rule-based test would catch only the restatements
somebody anticipated by writing a rule, which is the case that least needs catching. The wording rules that
produce a restatement for a given kind belong to an implementation, and who reviews it and when belongs to a
rule-set.

### What a requirement carries in this model

A `Requirement` in this model carries what `01-requirement-model.md` §2 gives it — its identity, the
derivation edge, and its finished text once it is complete — and what this model adds (K137).

| Attribute or edge | Carries |
|---|---|
| values | At most one value per parameter the requirement has, each stated by a `SourceNeed` the requirement refines (§7, K136). A parameter with none has no value |
| text | Its finished text, produced from its definition's template once every parameter has a value (§9, K111); none before |
| `refine` | The `SourceNeed`s it is assembled from: at least one, list-valued (D48, K145) |
| produced under | The `RequirementDefinition` it was produced under: exactly one, and not abstract (K8, K67, K109) |
| `supersedes`, `supersededBy` | The requirements it replaces, and those replacing it (K133, K134) |
| `retiredBy` | The `RequirementDecision` that took it out of force, where one did: at most one (K134, K144) |

### The complete requirement

**A requirement is incomplete exactly when a parameter it has has no value, so that its finished text cannot
be produced from its template; otherwise it is complete** (K140). An incomplete requirement is not hidden:
each missing value is an open `RequirementClarification` (§11), so the project manager sees what it still
lacks. An open choice over a requirement does not make it incomplete, since a choice is between requirements,
not about one requirement's values (K139). **The walk of the `RuleSet`s that reach a requirement runs once,
when the requirement becomes complete, and never on one that is not** (`03-project-lifecycle-model.md` §3,
K128). A rule therefore always judges values that `SourceNeed`s state.

**Where `SourceNeed`s contradict one another, there are two requirements, not one** (K139). Each contradicting
`SourceNeed` is refined into a requirement of its own, carrying its own value, and a `RequirementChoice` is
raised over the requirements that contradict (§11, K141). Whether two statements are about the same thing is
the modeller's judgement, made when they are extracted (K24, K40). A contradiction never makes a value
missing, and a requirement never holds two values for one parameter. The model records the state the project
is in, contradictions included, and raises them; it does not resolve them on the project's behalf.

**A complete requirement is never changed in place** (K129). Information that replaces what an earlier
statement said arrives as a `SourceUpdate` (§5, K132). The requirement refining it carries `supersedes`, a
list-valued edge naming the requirements it replaces (K133), and also refines those `SourceNeed`s of the old
requirement that still state what it keeps: its values are stated by the update for what that replaces, and by
those needs for the rest (K136, K138). It is walked once, when it is complete. A derivation edge from another
requirement, added to one that exists, does not change it in place either (K147); what states such an edge is
OQ42. A `SourceNeed` that states a different value without replacing anything contradicts what was said, and
is refined into a requirement of its own (K139). A decision to drop a requirement with nothing in its place is
a `SourceDecision`, whose `RequirementDecision` retires it (K62). Nothing resolves itself: every change has a
source behind it, and the source a speaker.

### No longer in force

**A requirement is never deleted.** When it is retired, superseded, or found wrong, it carries the property
of being no longer in force rather than being removed from the model (K5). This model is where that property
lives, because this model is where the record of everything lives: `01-requirement-model.md` carries the
requirements in force and nothing else, and what a requirement used to be has nowhere to sit in a model that
holds only what stands now.

The reason is not traceability for its own sake. The checks over this model are pairwise: a finding fires
between two requirements, or between a requirement and something naming it. Deleting a requirement silences
exactly the one check that fired on it, and nothing else — the finding disappears along with the element it
was about, and nothing in the model any longer shows that a check ever ran there, let alone that removing
the requirement was a considered choice rather than an oversight. Marking a requirement no longer in force
keeps the requirement, the finding, and the fact that somebody acted on it all present at once, so a later
reader can tell "this was resolved" apart from "this was made to disappear."

Retirement arrives the way everything else here arrives: through a source (K11), and now with a traceable
element behind it rather than a bare phrase. A `RequirementDecision` carries `retires`, an edge to zero or
more `Requirement`s (K62), and a `Requirement`'s becoming no longer in force is that edge taking effect: a
`RequirementDecision`, which never exists without a `SourceDecision` origin (K61), names it. A requirement
also leaves force when another requirement supersedes it (K133, K144): one refining a `SourceUpdate` names, by
`supersedes`, the requirements it replaces, and they are no longer in force. The cause is still a source (K11)
— the update the superseding requirement refines — and nothing is deleted (K5). A requirement is no longer in
force once it has a `retiredBy` or a `supersededBy`, whatever the state of the element at the other end
(K144): a superseding requirement that later leaves force does not return the one it replaced to force,
because leaving force is an event in a requirement's life and is final. What follows for the requirements
deriving from one that leaves force is OQ39, and for the questions it triggered, OQ43.

**Leaving force is named from both ends** (K134). Each way out of force is one association with two named
ends: a `RequirementDecision` `retires` a requirement, which is `retiredBy` it, at most one decision per
requirement; a `Requirement` `supersedes` another, which is `supersededBy` it, in any number. A requirement no
longer in force therefore names, on its own side, what took it out — one decision, or one or more superseding
requirements, never both — because the event happens in its life, and its cause is named where it happens. How
an association is written down is notation (K15). Both associations stay in this model and are dropped at the
projection, as retirement is (K35).

**One syntactic constraint follows** (K24), and it is argued here, beside the property it refers to: a
requirement in this model carries exactly one of "in force" or "no longer in force" at any time — never
both, and never neither. Section 12 gathers it with the rest of this model's syntactic constraints.

### The projection

The requirement analysis model **projects** to the requirement model (K20). The projection is a mapping
between two members of the collection rather than an element of either: it has no identity, it is recomputed
whenever it is read, and a design language never binds to it. What a design language binds to is a baseline,
which does have identity (K21).

**What the projection carries** is the requirement model as `01-requirement-model.md` defines it: the
requirements in force, each complete, with their identity and finished text, and the derivation edges between
them (K130). A requirement's values do not cross: its finished text states every one of them (§9, K111), and
the `SourceNeed` stating each value belongs to this model. A requirement in force that is not yet complete has
no finished text to carry; whether a baseline may be cut while one exists is OQ35. A baseline is cut only once
every `RequirementChoice` raised over contradicting requirements is decided (§11, K142). A decision that the
contradiction is not real keeps every one of them in force, and every one crosses.

**What it drops** is everything this model adds, and every requirement no longer in force. Sources and the
`replies` edge between them; `SourceNeed`s, `SourceDecision`s and the `refine` edge that names them;
`SourceQuestion`s and `RequirementQuestion`s — new elements of this model, and no more able to cross into the
product than anything else this list names; definitions, the edge by which a requirement names the one it was
produced under, their specialisation hierarchy and therefore the kind of any requirement (K33);
`RequirementDecision`s; and findings. A requirement's values are dropped with them (K130). A requirement no
longer in force is dropped with them, and so is the property that says it is: being no longer in force is a
property of this model, not of the product (K35). A reader of the requirement model alone sees a register of
what is in force, with traceability between its requirements and nothing else, which is exactly what makes
that document independently adoptable (K19).

**Why retirement does not cross.** One argument says it should, and it does not hold. That argument is a
seam argument: a design language binds to the product, so a requirement retiring between baselines would not
change state in the product at all — it would vanish, and the `satisfies` edge pointing at it would dangle,
with nothing recording that a choice was made. It conflates the live projection with a baseline. By K21 a
design language binds to a **named baseline**, never to the live projection, and a baseline is frozen: it
stays internally consistent forever, so an element satisfying a requirement in baseline B goes on satisfying
a requirement B still contains. No edge ever dangles.

What that argument called *vanishing* is in fact the intended signal. A baseline is a dated, closed picture,
treated as final at the moment it is taken and built upon; when the requirements change, a new baseline is
issued, and what depends on the old one is reworked. A design language that rebases onto a later baseline and
does not find a requirement it was built against is being told precisely that — rework what was built on it.
Absence is how the product says so, and saying it needs no state in the product to say it with.

**Traceability is not harmed, on two legs.** The first is this model: it contains everything, including the
requirements no longer in force, which is what K5 and K11 already guarantee — nothing about a retired
requirement is lost, because nothing about it was ever held anywhere but here. The second is the second
condition on the projection below: every element the projection carries resolves back to its origin here, so
backward traceability holds for everything the product contains.

With retirement out of the product, **K12 stands exactly as written** — *a dated, identified cut of the
requirements in force*. It needs no narrow reading and gets none. The founding record's section 2 procedure
narrative agrees with it rather than having to be overridden: it describes a cycle ending in a clean model
reached by dropping the model above and the deprecated requirements. Recorded as K35 in
`spec/06-decisions.md`, superseding K34.

**The two conditions on the projection.** The first is K13's, unchanged: everything in force at the moment of
the cut is present, nothing in force is dropped, and everything dropped stays here, in the working model,
recoverable. Nothing the projection drops is deleted by dropping it.

The second is stated here because the argument above rests on it: **every element the projection carries
resolves back to its origin in this model.** A requirement in the product is the same requirement here, under
the same identity, and everything this model holds about it — the `SourceNeed`s it refines and the sources
they anchor into, its definition, the `RequirementDecision`s and the findings around it — is reachable from
that identity. This is the leg that lets a baseline carry only what is in force without losing anything: the
product is a narrower view of this model, never a separate register that could drift from it, so no element of
the product is a dead end and nothing about one has to be reconstructed.

Together the two conditions are what makes the drop legitimate, and they are why this model, and not the
product, is where a project is worked.

## 11. `RequirementDecision`, `RequirementQuestion`, and findings

### `RequirementDecision`

A `RequirementDecision` is what a decision **does** to the requirement model: what it retires, and — once the
Project Lifecycle Model states the criterion — what finding it closes (K49). It does not supersede: a
requirement is replaced by the requirement refining a `SourceUpdate`, not by a decision (§10, K129, K133). It
is produced from a `SourceDecision` (§6) by `refine` (K58), on the same terms a `Requirement` is produced from
a `SourceNeed`.

**A `RequirementDecision` never exists without at least one `SourceDecision` origin.** This is not a
question and not a failed check in the sense an unrefined `SourceStatement` is one (§4, K45); a
`RequirementDecision` missing this origin is **not a well-formed element of this model at all**, on the same
footing as a `Requirement` naming no origin (K9, K61). What represents "a decision the modeller knows must
be taken, but has not been" is a `RequirementQuestion` in the raised state, defined below — never a bare
`RequirementDecision`, because `RequirementDecision`'s own definition already presupposes the decision it
records has happened.

A `RequirementDecision` carries five things beyond its origin, adopted from ISO/IEC/IEEE 42010's
*Architecture Decision* and the *Architecture Rationale* that stands behind it, held together in one
element — this pairing, and these five attributes, carry over unchanged from what this model called
`Decision` before this section's revision.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other `RequirementDecision`'s |
| the choice | What was decided, and the genuine alternatives it was chosen among |
| by | The party that took it |
| date | When it was taken |
| rationale | The reasoning that justifies the choice over the alternatives |

The vocabulary of the *by* attribute is open, on exactly the terms §2 sets out for a source's *kind* and
*from*: the metamodel names the attribute and leaves the list of parties to an implementation, because any
list fixed here would carry one domain's parties into every project that adopted the metamodel.

**`RequirementDecision` carries `retires`: an edge to zero or more `Requirement`s.** List-valued, and may be
empty, as the derivation edge is elsewhere in this collection; `refine`, list-valued too, never is (K62,
K145). This is the edge this model previously lacked entirely — `Decision`, as this document defined it before
this revision, named nothing it resolved, which is the defect this whole restructuring exists to fix. §10
states how `retires` now carries the weight `01-requirement-model.md` §3's *"no longer in force"* property
depends on.

**`RequirementDecision` carries one of two states, open or closed — but this document does not state what
closes one.** Working out the criterion depends on the same territory as what finding a `RequirementDecision`
closes: neither is settled without the Project Lifecycle Model being worked out further than it is today
(K63). `supersedes`, once a third item in the same territory, is settled, and is not a decision's edge (§10,
K133). This is an admitted gap, on the same terms `RequirementDefinition`'s *"when it applies"* is one (§7): a
slot this document states without a claim about what fills it.

**A `RequirementDecision` is not a value, and nothing about deciding sets one silently.** A decision is a
choice made in the presence of alternatives, by somebody with standing, recorded with the alternatives and the
rationale; what changes it is deciding again, which under K11 means a new source and a new
`RequirementDecision` beside the old one rather than an edit to it. A decision settling a contradiction sets
no value: it keeps one of the contradicting requirements and retires the others, or decides that the
contradiction is not real and keeps them all (K142). A correction is not a decision: it arrives as a
`SourceUpdate` (§5, K132).

### `RequirementQuestion`

`RequirementQuestion` is **abstract**. What the modeller must find out (K49) — the model-side record of a gap
the modeller has identified, before anybody has been asked to close it — is common to every specialisation,
and it is abstract because K79 and K119 give it three.

```mermaid
classDiagram
    class RequirementQuestion {
        <<abstract>>
        identity
        statement
        triggered by (Rule)
        triggering Requirements
        state: raised | posed
    }
    class RequirementChoice {
        candidate alternatives
        modeller's flag
    }
    RequirementQuestion <|-- RequirementInquiry
    RequirementQuestion <|-- RequirementChoice
    RequirementQuestion <|-- RequirementClarification
```

The diagram draws what this subsection states; where the two disagree, the prose wins.

A `RequirementQuestion` carries, beyond its identity, three things shared by every specialisation (K78).

| Attribute | Carries |
|---|---|
| statement | A free, professional-register text statement of the question |
| triggered by | A reference to the `Rule` (`03-project-lifecycle-model.md` §3) that fired and produced it |
| triggering `Requirement`s | Every `Requirement` that triggered it. List-valued: a choice over contradicting requirements names each of them, and grows, while it is open, when a further one contradicts (`03-project-lifecycle-model.md` §3, K141) |

It also carries one of two states (K60).

| State | Meaning |
|---|---|
| raised | Identified; no `SourceQuestion` yet names it |
| posed | A `poses` edge names an actual `SourceQuestion` — the edge's presence is the transition itself, not a marker recorded beside it |

It is not itself a `SourceQuestion`: it crosses outward, by `poses` (K59), into one once the modeller actually
puts the question to somebody. **What happens after posing — whether and how the question is answered —
carries no further state here.** That discharge is OQ13's own territory, which this document does not attempt
to close; `RequirementQuestion` gives OQ13 the *opening* half of the interval it asks about, and no more.

**`RequirementQuestion` specialises into `RequirementInquiry`, `RequirementChoice` and
`RequirementClarification`, and which one a question is follows from what is present when its rule fires**
(K79, K119, K120): two or more things to choose between raise a `RequirementChoice`, a missing companion kind
a `RequirementInquiry`, a missing value a `RequirementClarification`. The first two carry `discharges`: an
edge to whatever closes them, optional because it is absent for as long as the question stands open.
`RequirementInquiry` discharges to a `Requirement`; `RequirementChoice` discharges to a `RequirementDecision`.

`discharges` is a coined edge rather than a reuse of `replies`: `replies` is a `Source`↔`Source`, evidentiary
edge — one passage of material responding to another — where `discharges` names, on the model's own side,
what closed a question, a different kind of relationship entirely. Reusing `replies` here would blur exactly
the distinction K47 draws between the two sides of this model. The word itself is not new to this collection:
OQ13 already speaks of *"its discharge"* as the machinery it still asks for, and `discharges` names the
phenomenon the corpus was already calling by this word.

**`RequirementInquiry` carries nothing beyond the shared shape above and its `discharges` edge** (K80). A
completeness gap (`03-project-lifecycle-model.md` §3) names a missing kind, and the shared *triggering
`Requirement`s* list plus the `Rule` itself already identify it in full — nothing further is needed before
discharge.

**`RequirementChoice` additionally carries the candidate alternatives being decided among** (K80). A conflict
needs the options named before anyone can decide among them, and these alternatives deliberately prefigure
what `RequirementDecision`'s own *the choice* attribute (above) will record once discharged — the same
alternatives, read once as open and once as settled. **A `RequirementChoice` raised over contradicting
requirements also carries the modeller's flag**: whether the modeller judges the contradiction real (K142).
The flag is advice, not a decision. The project manager decides, at the latest when a baseline is cut, to keep
one requirement and retire the others, or that the contradiction is not real and every one stays in force.
Nothing is merged, in this model or in a baseline; one design element satisfying several of them is design,
beyond the seam. For a choice over contradicting requirements, a decision that the contradiction is not real
settles it on an alternative the candidates do not list (K142). Whether a choice a `ConflictRule` raises is
one over contradicting requirements, and so carries the flag and holds back a baseline, is OQ41.

**`RequirementClarification` carries nothing beyond the shared shape, and no `discharges`** (K119, K121). A
parameter's ask raises it where the parameter has no value on a requirement, one per `Requirement` and
parameter. It is open while no `SourceNeed` the requirement refines states the parameter's value and the
requirement is in force, and closes when one does, or when the requirement leaves force (K143). What closed it
needs no edge of its own: the value is stated by a `SourceNeed` the requirement refines (§7, K136), and the
chain from the question runs through `poses`, `replies` and `refine` to it. One posed `SourceQuestion` may
carry several clarifications, each naming it by `poses`.

**The process, end to end.** A `SourceNeed` is refined into a `Requirement`, and does not state a parameter's
value, so the value is missing (K136). The parameter's ask raises a `RequirementClarification`, naming the ask
as its *triggered by* and the requirement as its triggering `Requirement`; the project manager can see it from
here. The modeller puts the question to somebody, in a source, and the clarification poses that
`SourceQuestion`. A later source replies; a `SourceNeed` anchored in it is refined into the same requirement,
whose refinement edge is list-valued (§10); that `SourceNeed` states the value, and the clarification closes.
If the answer is that nobody knows yet, the value stays missing and the clarification posed; how long it may
wait is OQ13's interval and OQ34's question. If the value the answer states contradicts the value another
requirement of the same kind carries for the same thing, the clarification closes all the same, and the same
ask raises a `RequirementChoice` over the two requirements, or extends the one already open over the other
(K139, K141). If the answer arrives unasked, the clarification closes all the same, and the chain lacks only
its `poses` and `replies` links.

**A contradiction raises a `RequirementChoice` between requirements, not a state of a value** (K139, K141).
Where the modeller has judged, at extraction, that requirements of one kind state values of a parameter for
the same thing, and the values contradict, the parameter's ask raises a choice naming every one of them as its
triggering requirements; a further contradicting requirement extends it while it is open, rather than opening
another. Its candidate alternatives are the requirements, each with the `SourceNeed`s it refines. One choice
over all of them, not one per pair, because three or more may contradict. Two requirements of one kind are
produced from one template, so where they contradict they differ in a parameter's value, and no contradiction
of one kind goes unraised. The project manager decides, and the decision enters as a source (K11, K61).

**How a question is worded** (K122). A clarification's *statement* starts from its parameter's ask, which the
modeller may fit to the requirement in hand. A `RequirementChoice`'s and a `RequirementInquiry`'s statement
the modeller writes freely, informed by the rule's *what to look for*, from no template: a choice is about its
own alternatives and an inquiry about its own gap, so no wording written in advance fits them, and what prose
means is not an algorithm's to decide (K24).

**Closing a `RequirementInquiry` or `RequirementChoice` needs no dedicated edge to reach a
`RequirementDecision`, and `discharges` does not change that.** The connection was already traceable through
machinery this document has independently of `discharges`: `RequirementQuestion` --poses--> `SourceQuestion`,
whose source a later source `replies` to (§3); if that replying source carries a `SourceDecision`, it `refine`s
into the `RequirementDecision` that answers the question (K61). `discharges` sits *beside* that traceable
chain as a direct, optional convenience reference, not in place of it: the chain is what guarantees the
connection always exists and is consistent; `discharges` is what lets a reader, or a checker, find the answer
without walking three hops to get there. Both readings are true at once, deliberately.

**Every `RequirementQuestion` comes from a `Rule`, without exception** (K87). Its *triggered by* is never
absent, because a rule-set is the procedure a project works to: it can be amended, but it cannot be departed
from. A modeller who finds that no rule covers something proposes a rule — adding one is the project
manager's act, since it commits the project (`03-project-lifecycle-model.md` §3) — rather than raising a
question outside the procedure. This is the strongest available reading of the rule that every event record
its cause: a `RequirementQuestion`'s cause is not merely guaranteed to exist, it is named.

**A `RequirementDefinition`'s *what to ask* (§7) is not a second origin either: it is a `Rule`.** Every
parameter's ask is a `ValueRule` belonging to the rule-set of the definition that declares the parameter,
inherited with it (§9, K112), in force for as long as the parameter is declared and never taken out of force
on its own (K115, K116; `03-project-lifecycle-model.md` §3). A missing value, and a contradiction between
requirements of one kind, therefore reach the project manager by the same route as every other question, and
neither can stand in silence. Whether the ask needs to be a `Rule` at all is OQ38.

**Three mechanisms are worked out, and one is open.** A `Requirement` incompatible with one already in force,
canonically on terms a project had to state because the two are of different kinds, is
`03-project-lifecycle-model.md` §3's `ConflictRule`, raising a `RequirementChoice`. A `Requirement` whose kind
implies a requirement of another kind deriving from it is that section's `CompletenessRule`, raising a
`RequirementInquiry` — this was OQ17's own original case, now answered. A parameter with no value is that
section's `ValueRule`, raising a `RequirementClarification`; requirements of one kind contradicting in a
parameter's value are the same rule, raising a `RequirementChoice` between them (K115, K116, K139, K141). When
a wait for an answer becomes a decision has no worked mechanism; it is held by a prerequisite named in
`06-decisions.md` under OQ18. Whether a default may stay silent needs none: no value is a default, since every
value is stated by a `SourceNeed` (K127, K136).

**`RequirementQuestion` is not a *review finding*, and belongs to no row of the findings table below** (K89,
narrowing K77). It does share the three properties that table uses to seat a review finding apart from the
other two — it is judged, it is modelled, and it carries state (K60) — but the table classifies what a
**review** produces over this model (K10), and walking a rule-set is ordinary modelling work performed when a
requirement becomes complete (§10, K128), not a separate act of review. A clarification a `ValueRule` raises
is not even judged: whether a value is present is decided without judgement. The choice it raises rests on the
modeller's judgement, made at extraction, that requirements state values for the same thing (K141). Questions
therefore come from two modes of checking, and neither is review (K89, narrowed).

The table's own rules confirm the separation rather than merely failing to fit it. A review finding *"is
opened by a source"*, where a `RequirementQuestion` is raised by a `Rule` firing over the model; and
*"nothing marks a finding closed directly"*, where `discharges` does exactly that.

**Three checking modes exist, and only two had names before this.** Static model checking decides without
judgement and produces a failed check or a question, recomputed rather than modelled. Walking a `RuleSet`
takes judgement and produces a `RequirementQuestion` (`03-project-lifecycle-model.md` §3, K86). A `ValueRule`
produces its questions without a judgement of its own, since it reads no text: a clarification on whether a
value is present, and a choice on the judgement the modeller made at extraction, that requirements state
values for the same thing (K141); its questions are modelled all the same, so a `RequirementQuestion` comes
from two of these modes, and from review from neither (K89, narrowed). A review takes judgement and produces a
review finding. K77 saw only the first distinction — judgement or none — and so placed `RequirementQuestion`
with review findings on the strength of the three shared properties. What a review *is*, as an act, this
document still does not state; that gap is recorded as OQ23 in `06-decisions.md`.

This metamodel introduces no `Task`, or any output shaped like one, for a `RequirementQuestion` in the raised
state. The state itself is already the complete signal: querying for raised `RequirementQuestion`s is finding
the worklist, on the same terms K53 already reads an unsatisfied requirement without a dedicated element for
it (see `05-binding-contract.md` §2), and `00-overview.md` §1 excludes task-shaped vocabulary from this
metamodel by name (K65).

### Findings

What a review produces over the requirement analysis model is a **model, not a derived view** (K10). A
finding links at least two elements and must keep its identity between reviews: a recomputed report has no
identity across readings, so a reviewer opening it twice cannot tell whether the thing in front of them is
the thing somebody already adjudicated. K11 is what makes this tractable rather than a maintenance burden —
no finding can go stale unnoticed, because nothing moves beneath it without a source accounting for the move.

Findings come in three kinds, and they do not behave alike.

| Kind | How it is decided | Modelled? |
|---|---|---|
| A failed check | Without judgement | No — recomputed rather than modelled |
| A question | Without judgement | No — recomputed rather than modelled |
| A review finding | By judgement | Yes, and it alone can carry a state |

This follows the two-column organisation D50 already uses — what a script decides on one side, what a person
or an agent decides on the other — which is K24's division seen from the reporting end. The first two kinds
are decidable without judgement, so modelling them buys nothing and risks staleness: a modelled failed check
can outlive the fact that produced it. A review finding cannot be recomputed at all, is lost unless it is
modelled, and is therefore the only one of the three with a state to carry — it is open until it is closed.

**Closure is evidence, not a tick.** A review finding is opened by a source and closed by a later source that
`replies` to it, on exactly the terms section 3 sets out for that edge. Nothing marks a finding closed directly;
what closes it is a source entering the model, which is K11 holding at the top of the chain as it holds
everywhere else. A finding closed this way carries the material that closed it, so a later reader can read
what was said rather than only that somebody was satisfied.

**Two rules already exist over `SourceNeed`s and `Requirement`s, and they are each other's mirror.** Both are
failed checks; neither is a question.

1. **A `SourceNeed` that no `Requirement` refines is a failed check, not a question** (K38, D31). A
   `SourceNeed` obliges something (§5, K37), so a `SourceNeed` nothing refines is a record in which something
   obliged is unaccounted for. EventML shipped this as a question rule (D31); K38 overturns that, and
   [`docs/eventml-decisions.md`](../docs/eventml-decisions.md) records the overturn.
2. **A requirement that refines no `SourceNeed` is a failed check, not a question** (K9, K145, D32). The
   invariant behind it is that every requirement names its origin, and its origin is a `SourceNeed` it
   refines: a derivation from other requirements is never one (`01-requirement-model.md` §2). EventML allowed
   an origin of derivation alone (D49), which ProjectML does not adopt; it also shipped the rule as a question
   (D46), which K9 overturns. [`docs/eventml-decisions.md`](../docs/eventml-decisions.md) records both.

The two are one break in the chain, read from opposite ends: a `SourceNeed` with no `Requirement` beneath it,
and a `Requirement` with no `SourceNeed` above it (K145). They were treated asymmetrically — one a question,
one a failed check — and nothing about either justified the difference.

**A `SourceNeed` that no `Requirement` refines has exactly two honest resolutions.** Write the requirement
the `SourceNeed` obliges, or delete the `SourceNeed`, because extracting it was a mistake. There is no third,
and in particular there is no record that the `SourceNeed` was examined and deliberately produced nothing: a
passage obliging nothing is not a `SourceNeed` (§5, K37), so the state of affairs such a record would attest
to does not arise.

**Deleting a `SourceNeed` loses nothing of record, which is why deletion is safe here and forbidden one
element over** (K39). A source is material of record — quoted whole, never decomposed, never edited (§2,
D45, D25) — and a `SourceNeed` is a pointer into a passage of it. Delete the pointer and the passage remains,
unchanged, in the source, available to be pointed at again by whoever extracts better. A requirement is not
like that. It is this model's own construct, with no other home, so deleting one destroys the only record of
it and silences the check that fired on it, which is what K5 exists to prevent (§10). The asymmetry between K5
and this is not an inconsistency; it is the difference between a construct and a pointer into material held
elsewhere. Nor is a deletion here retirement, and nor does it give a `SourceNeed` a state: what is removed is
a wrong pointer (§4, K6).

**The criterion that decides between the two resolutions is whether a declared definition covers the
statement** (K38). A requirement is produced under a `RequirementDefinition`, and it names the definition it was
produced under (§10); definitions exist only for the kinds an implementation has declared (§9, K30). Where
some declared definition covers the statement, the resolution is to write the requirement under it. Where
none does, there was nothing for a requirement to be produced from, and the extraction was wrong.

**That criterion is also the guard against silencing the rule.** The objection to permitting deletion at all
is that the other resolution invites the same abuse from the other side: clear the check by writing a
tautological requirement over anything at all. It does not. No requirement can be produced except under a
declared definition, nothing an implementation declares covers a passage that obliges nothing, and the
failure to find a definition is itself the signal that the extraction was wrong rather than an obstacle to
be worked around. The rule is unsilenceable at both resolutions: the first needs a definition it cannot
invent, and the second removes a pointer while leaving the source it pointed into intact and inspectable.

**What is genuinely open in such a case is not whether the `SourceNeed` is a `SourceNeed`, but what follows
from it.** That is a **dilemma**, and it needs no new element, because it already has a home. It is a review
finding — the one of the three kinds above decided by judgement, and the only one carrying a state. It is
opened by a source and closed by a later source that `replies` to it, on the terms this section has already
set out, and its answer therefore arrives as a source like every other change (K11). The requirement that
finally issues refines a stater's own words, and may also derive from another requirement where the client
stated it as elaboration of what that one asks (`01-requirement-model.md` §2, K146); only what it refines is
its origin (K145). The derivation is named at both ends, original and derived requirement, and a chain of
justification read forward — this holds, therefore that does — runs from the original to the derived (K146,
K149).

**This rule checks the extraction from the only side a model can check it, and that is why it belongs with
extraction rather than only with refinement.** What it catches is over-extraction: something was extracted
that obliges nothing, and a passage obliging nothing is not a `SourceNeed` (§5, K37). That is not a complaint
that the refinement step was lazy; it is the record saying the boundary was drawn in the wrong place at
extraction, which is why one of its two resolutions is to delete the `SourceNeed`.

**The other direction is not the model's to check, and this document does not claim it is.** Whether
everything a source obliged was extracted at all, and whether each `SourceNeed` says what the passage behind
it said, are judgements over material the model does not hold and content it does not read. They belong to
the modeller, and self-review or cross-review after the modelling is done is what finds them —
`00-overview.md` §5 states that boundary in full (K40). The metamodel therefore defines no report over which
passages of a source no `SourceNeed` cites, and no metric of extraction built on one, and none is to be
added: a source's
information density varies too much for any denominator to mean anything, and K10's own criterion finds
nothing to model, because a missed extraction is fixed by the same act that notices it (K41).

## 12. The syntactic constraints of this model

K24 divides constraints over the collection in two: a syntactic constraint refers only to elements the
metamodel defines and is decidable without judgement, where a semantic one judges content and is a matter for
review. This section states the syntactic constraints over the elements this document defines, in the shape
`01-requirement-model.md` §5 uses for the product model's own.

Several of them were argued above, beside the element they refer to, and are given here in one line so that
the set is visible at once rather than assembled by a reader out of ten sections; where that is so, the
section carrying the argument is named. None of them reads the content of anything: every one is decidable
from what is present and what is absent, which is what K24 means by *without judgement* and what the record
test (§8) means by *without reading its content*.

**Over `Source`, and the edge between sources.**

- A source's identity is unique among every source in the model (§2).
- The `replies` edge points backward in time: the source carrying it is dated later than the source it names
  (§3, D38).
- One `replies` edge names exactly one earlier source; a source may carry any number of them (§3, D29).

**Over `SourceElement`, and every specialisation of it.**

- A `SourceElement`'s identity is unique among every `SourceElement` in the model (§4).
- A `SourceElement` anchors into exactly one passage of exactly one source. One with no anchor is a failed
  check and not a question: there is nothing in it to ask a stakeholder about and nothing for a reviewer to
  weigh, only an omission to fix (§4, D34, D26).
- A `SourceElement` carries no attribute beyond identity, anchor, and being material of record. A candidate
  attribute reading the content of the passage fails this constraint regardless of which specialisation
  proposes it (§4, K57).

**Over `SourceStatement`, `SourceNeed`, and `SourceDecision`.**

- A `SourceStatement` that produces no counterpart on the model's own side — no `Requirement` for a
  `SourceNeed`, no `RequirementDecision` for a `SourceDecision` — is a failed check, not a question (§4, §5,
  §6, K38, K45).

**`SourceQuestion` and `poses` carry no constraint beyond what `SourceElement` already states above.** This
is deliberate, not an omission: an unanswered `SourceQuestion` is the normal case, so no failed-check rule is
stated over the absence of an answer, which is K45's own reasoning (§4, K45).

**Over `RequirementDefinition`.**

- A definition's identity is unique among every definition in the model (§7).
- No element is a `RequirementDefinition` and nothing more: every definition in a model is an instance of some
  specialisation of it (§7, §9, K30).
- A definition that is not abstract states the template its requirements' wording is produced from (§7). A
  definition without one produces nothing, and the derivation §10 describes cannot be run against it. An
  abstract definition states none: nothing is produced under it (§7, K109, K110).
- In the template of a definition that is not abstract, every placeholder names a parameter the definition
  has, declared or inherited, and every parameter it has appears as a placeholder (§9, K107, K111).
- Every parameter a definition declares names the value domain it draws from. Which domains exist is an
  implementation's business; that a parameter names one is not (§7).
- Every parameter a definition declares carries its own ask. A parameter with no ask is a failed check on the
  definition: *what to ask* exists so that a missing value, and a contradiction between requirements of one
  kind, each have a stated route out, and it is the `ValueRule` that raises the question; a parameter missing
  its ask is exactly the case where that route is absent (§7, K116, K141).
- A parameter's identity is unique among the parameters its definition declares (§7, K106).
- A definition declares no parameter carrying the identity of a parameter one of its ancestors declares. Every
  parameter a definition has, its own and those it inherits, is therefore one parameter on every descendant,
  drawing from one domain (§7, §9, K106, K107, K108).
- **Every definition that is not abstract states how a requirement produced under it would be verified.**
  Absence of the statement is a failed check on the definition itself, independent of anything any requirement
  produced under it says. A definition whose requirements cannot be verified independently meets this
  constraint by saying so, in that same attribute: the check reads whether the statement is there, never which
  of the two things it says. This is ISO/IEC/IEEE 29148's verifiability characteristic held one level up,
  where §7 places the method — 29148 requires verifiability of a requirement, and a requirement takes its
  definition's method — and it is the stated rule K29 rests on. An abstract definition is outside it (§7,
  K110): when the constraint was written, `RequirementDefinition` was the only abstract definition, and it
  never met the constraint either.
- **Every definition that is not abstract states a well-formedness rule for the wording a requirement produced
  under it must satisfy.** Absence of the statement is a failed check on the definition itself, independent of
  anything any requirement produced under it says. This is ISO/IEC/IEEE 29148's *characteristics of a good
  requirement* held one level up, on the same footing §7 already places *how it would be verified* — it is the
  stated rule K66 rests on. An abstract definition is outside it, on the same terms (§7, K110).

**Over the derivation, and over being no longer in force.**

- Every value a `Requirement` carries is stated by a `SourceNeed` the requirement refines, and it carries at
  most one value per parameter (§7, §10, K136, K137).
- A `Requirement` is incomplete exactly when a parameter it has has no value, and complete otherwise (§10,
  K140).
- A `Requirement` refining a `SourceUpdate` names at least one requirement by `supersedes` (§5, §10, K132,
  K133).
- A `Requirement` is no longer in force exactly when it has a `retiredBy` or a `supersededBy`, whatever the
  state of the element at the other end, and never both. It has at most one `retiredBy` (§10, K62, K133, K134,
  K144).
- A requirement in this model names exactly one `RequirementDefinition`: never none, and never two (§10, K8).
  The definition it names is not abstract; a requirement naming an abstract definition is not a well-formed
  element of this model (§7, §10, K109).
- A requirement in this model carries exactly one of "in force" or "no longer in force" at any time — never
  both, and never neither (§10).
- A `RequirementDecision`'s `retires` edge names only `Requirement`s that were in force at the moment the
  `RequirementDecision` was produced. `retires` may be empty (§10, §11, K62).
- A `SourceNeed` that no `Requirement` refines is a failed check (§11, K38, D31).
- A requirement that refines no `SourceNeed` is a failed check (§11, K9, K145, D32). This constraint and the
  one above it are the same break read from opposite ends, which is why they are stated together.

**Over `RequirementDecision`.**

- A `RequirementDecision`'s identity is unique among every `RequirementDecision` in the model (§11).
- **A `RequirementDecision` names at least one `SourceDecision` it was produced from. A `RequirementDecision`
  with no such origin is not a well-formed element of this model — this is stronger than a failed check, on
  the same terms K9's rule over `Requirement`'s origin already is (§11, K9, K61).**
- A `RequirementDecision` names the party that took it, the date it was taken, the choice, the alternatives
  the choice was made among, and the rationale. Absence of any of the five is a failed check on the
  `RequirementDecision`. Whether the alternatives recorded were genuine ones is a judgement and therefore a
  semantic matter under K24, outside this check.

**Over `RequirementQuestion` and its three specialisations.**

- A `RequirementQuestion`'s identity is unique among every `RequirementQuestion` in the model (§11).
- No element is a `RequirementQuestion` and nothing more: every `RequirementQuestion` in a model is an
  instance of `RequirementInquiry`, `RequirementChoice` or `RequirementClarification` (§11, K79, K119).
- A `RequirementQuestion` names exactly one `Rule` as the origin that produced it. One naming none is not a
  well-formed element of this model, on the same footing as a `RequirementDecision` naming no `SourceDecision`
  (§11, `03-project-lifecycle-model.md` §3, K61, K87).
- A `RequirementQuestion` carries exactly one of "raised" or "posed" at any time. A `RequirementQuestion` in
  the posed state names, by its `poses` edge, exactly the `SourceQuestion` that made it so (§11, K60).
- A `RequirementInquiry`'s `discharges` edge, where present, names a `Requirement`. A `RequirementChoice`'s
  `discharges` edge, where present, names a `RequirementDecision`. Both are optional (§11, K79).
- A `RequirementClarification` is one per `Requirement` and parameter, and is open exactly while that
  requirement is in force and no `SourceNeed` it refines states a value for the parameter. It carries no
  `discharges` (§11, K119, K143).
- A `RequirementChoice` raised by a parameter's ask names as its triggering requirements exactly the
  requirements among its candidate alternatives, and carries the modeller's flag. A `Requirement` is a
  triggering requirement of at most one open choice raised by one parameter's ask (§11, K141, K142).
- A `RequirementInquiry` a `CompletenessRule` raises names exactly one triggering `Requirement`, and at most
  one per `Rule` and triggering `Requirement` is open at a time. One a `CompletenessRule` raised is discharged
  only by a `Requirement` of the implied kind, or a specialisation of it, deriving from the triggering
  requirement; whether only directly is OQ40 (§11, `03-project-lifecycle-model.md` §3, K148).

**Over findings.**

- A review finding names at least two elements (§11, K10).
- A review finding recorded as closed names the later source that closed it, and that source `replies` to the
  source the finding was opened by. Nothing marks a finding closed directly (§11, K11).

**One rule over these elements reports rather than fails.** §11's table separates a failed check from a
question: both are decided without judgement and neither is modelled, but a failed check says the record is
wrong where a question says something was left open and somebody has to look at it. **A definition that does
not say when it applies is reported as a question.** It is not a failed check, and making it one would
contradict the row of §7 that defines the attribute: an unwritten applicability is a gap, not a claim that
the definition applies unconditionally, and the honest report is that nobody has written it down rather than
that the record is defective. It is the only rule in this document that reports rather than fails. The two
origin rules §11 states were once read this way and are not: in each of those the record is wrong rather than
merely incomplete, which is what makes both of them failed checks (K9, K38). This is also why *when it
applies* does not pass the record test, which §8 settles.

**What is not stated here, and why the omission is deliberate.** No rule requires a definition to carry a
*name*. One could be written in a line, and it would fail on an absence without reading any content — which
is exactly §8's point about what a presence rule proves. The metamodel reads a name for no purpose of its
own, so requiring one is record hygiene an implementation is better placed to state over its own definitions,
the core being a floor rather than a ceiling (K27). Nothing here is stated in order to make an attribute pass
a test: a rule nothing would ever fire on is worse than an admitted gap.
