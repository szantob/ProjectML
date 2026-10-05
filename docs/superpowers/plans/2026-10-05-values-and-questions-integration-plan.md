# Integrate values from sources, the complete requirement and the value rule (K115–K131) into `spec/` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Write two settled design records into `spec/`: every value names its source and the value-state
model is withdrawn; a requirement is walked once, when complete, and never changed in place; a baseline
carries finished text only; every parameter's ask is a `ValueRule` raising a `RequirementClarification` for a
missing value and a `RequirementChoice` for a disagreement; `spec/04` folds into `spec/02` and the number 04 is
retired. This is prose, not code: a "test" for each task is a re-read for internal consistency, cross-reference
correctness and conformance to `CLAUDE.md`, plus the grep checks the task names.

**Architecture:** `spec/02` carries most of it — §5 and §7 (values, domains, the ask), §10 (the complete
requirement, the correction, the projection), §11 (the questions), §12 (constraints). `spec/03` gains
`ValueRule` and a walk that runs once. `spec/01` and `spec/00` lose the value model and gain the baseline's
new content. `spec/05` and `bindings/sysml-v2.md` lose the fourth declaration. `CLAUDE.md` follows. `spec/06`
records K115–K131 and the open questions; `CHANGELOG.md` the release note. `spec/04-value-states.md` is
deleted.

**Tech Stack:** Markdown, Mermaid (GitHub-rendered), git.

## Read first — the decisions this plan writes

Both records are in `docs/superpowers/specs/` and are the only authority for what follows; this plan cites
their K-numbers rather than re-arguing them. Read both before Task 1.

1. `2026-10-05-value-rule-and-clarification-design.md` — K115–K124 (a rule over one value, the clarification,
   one requirement over disagreeing sources, the wording of a question). It is **amended** by record 2.
2. `2026-10-05-values-from-sources-and-the-complete-requirement-design.md` — K125–K131. **Its §6 says which of
   record 1's decisions stand and how they are restated**: K117 is withdrawn, K124 is superseded by K128, and
   record 1's "unknown / assumed / conflicting state" become "a missing value" and "a disagreement". Where the
   two records differ, record 2 wins.

## Global Constraints

- **The owner has explicitly instructed two revisions of locked decisions** (`CLAUDE.md` §3): rule 4's
  "value-state model" (by K125) and rule 5's four declarations (by K130). Task 7 edits `CLAUDE.md` accordingly.
  Nothing else in §3 changes.
- **English only. No notation, no filled `RequirementDefinition`, nothing executable. "Attribute", not
  "field". Do not describe the metamodel in terms of files** (`CLAUDE.md` §1, §5). No example in this plan's
  texts names a real domain; keep it so.
- **Historical documents are not edited:** the founding record, `docs/eventml-decisions.md`, and everything
  under `docs/superpowers/` keep their wording and their `04-value-states.md` links (`CLAUDE.md` §4).
- **The number 04 is retired** (K131). No task creates a file numbered 04.
- **Vocabulary, used verbatim:** `ValueRule`; `RequirementClarification`; *missing value* (never "unknown
  state" in new text); *disagreement* (never "conflicting state" in new text); *complete requirement*;
  *names the source that states it*.
- **Decision rows in `spec/06` are single table lines.** K117 and K124 get rows marked withdrawn / superseded
  before integration; their numbers are not reused.
- **Line wrapping.** Prose wraps greedily at 110 characters, counted in characters (`—` and `§` are one).
  Table rows and Mermaid blocks are not wrapped. Rewrap only paragraphs you change.
- **Match anchor text, not line numbers.** Line numbers drift as tasks land.
- **Commit after every task, staging only the files the task names** — another session may commit to this
  repository at the same time. Never push. End every commit message with a blank line and
  `Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>`.
- **Out of scope:** OQ14 (deferred again), OQ17, OQ26, OQ33–OQ35's answers, the editor and skills repositories.

---

### Task 1: `spec/02` §5 and §7 — a value names its source; domains move in; the ask is a rule (K101, K116, K125, K127)

**Files:** Modify `spec/02-requirement-analysis-model.md` — §5's paragraph containing `the value-state model
still governs every value`; §7's *parameters* and *what to ask* rows; the paragraph beginning `Two of the nine
bottom out in the value-state model`.

**Interfaces:** Produces the statements later tasks cite as `§7, K125`, `§7, K127` and `§7, K101`.

- [ ] **Step 1: §5, the `SourceNeed` paragraph**

Replace the sentence `This revises how D27 was previously read as applying directly to this element: the
value-state model still governs every value wherever one occurs (`04-value-states.md` §4), but a `SourceNeed`
is not a place a value occurs, because nothing on the source side is a value at all.` with:

```
This revises how D27 was previously read as applying directly to this element: a `SourceNeed` is not a place
a value occurs, because nothing on the source side is a value at all. A value occurs on a `Requirement`, and
names the source that states it (§7, K125).
```

Rewrap the paragraph.

- [ ] **Step 2: §7, two rows of the core table**

Replace the *parameters* row's closing `(K30, and `04-value-states.md` §5)` with `(K30). What a domain
declares about its values is stated below (K101)`.

Replace the *what to ask* row with:

```
| what to ask | For each parameter, how a non-expert is asked for what is missing. It is a rule: where the parameter has no value on a requirement, or the sources stating one disagree, it raises the question (K116) |
```

- [ ] **Step 3: §7, values and domains**

Replace the paragraph beginning `Two of the nine bottom out in the value-state model` (it ends `...written per
parameter rather than per definition.`) with these three paragraphs:

```
**A value exists only where a source states it** (K125). A parameter's value on a requirement names the
source that states it, and where no source states one the value is missing; nothing else puts a value into
the model. A value is never supplied by the modeller, who administers and decides nothing for the project. A
value supplied to keep work moving is stated by somebody with standing, in a source like any other; a quantity
computed from other values is design, beyond the seam, or is stated by whoever computed it, as a source; and an
implementation's default is a suggestion a parameter's ask may carry, which becomes a value only when somebody
states it (K127). How a value and the source it names are written down is notation, and an implementation's
(K15). The ask is how a missing value is obtained from somebody who holds it — which is why *what to ask* sits
beside *parameters* and is written per parameter rather than per definition.

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
one of three levels of comparability, **not comparable**, **comparable for equality**, or **ordered**, the last
including the second. How an implementation achieves the level it declares — a fixed unit, a dimension with
its conversions, an enumeration, anything else — is its own business, exactly as the set of domains is. Two
domains for one measure in different units are, under this, two ordered domains, each in its own unit, and no
guard ever converts between them.
```

- [ ] **Step 4: Check**

Re-read §5 and §7. Run `grep -n "04-value-states\|value-state\|unknown state" spec/02-requirement-analysis-model.md`
— hits may remain only in §10–§12, which later tasks change.

- [ ] **Step 5: Commit** — `git add spec/02-requirement-analysis-model.md`; message
`Make every value name its source, and move value domains into the definition (K101, K116, K125, K127)`.

---

### Task 2: `spec/02` §10 — the complete requirement, the correction, and the projection (K123, K128, K129, K130)

**Files:** Modify `spec/02-requirement-analysis-model.md` — §10's first paragraph under *The derivation*; a new
subsection before `### No longer in force`; *The projection*'s two paragraphs beginning `**What the projection
carries**` and `**What it drops**`.

**Interfaces:** Consumes Task 1. Produces `§10, K128` and `§10, K129`, which Tasks 3, 5 and 6 cite.

- [ ] **Step 1: The derivation**

In the paragraph beginning `A requirement is not written; it is **derived**.`, replace `are filled from the
`SourceNeed`'s passage and from whatever else the model already holds, and each filled value carries a value
state on the same terms as any other value in the collection.` with `are filled from the `SourceNeed`'s passage
and from other sources that state them, and each value names the source that states it (§7, K125).` Rewrap.

- [ ] **Step 2: A new subsection**

Insert, directly before `### No longer in force`:

```
### The complete requirement

**A requirement is complete when every parameter it has has a value and no choice about its values is open**
(K128). Until then it is incomplete, and it is not hidden: each missing value is an open
`RequirementClarification`, and each disagreement an open `RequirementChoice` (§11), so the project manager
sees what it still lacks. **The walk of the `RuleSet`s that reach a requirement runs once, when the requirement
becomes complete, and never on one that is not** (`03-project-lifecycle-model.md` §3). A rule therefore always
judges values that sources state and that nobody disputes.

**Where sources disagree about the same thing, there is one requirement, not two** (K123). Their statements
are refined into one `Requirement`; while they disagree its value is missing, and the disagreement is a choice
between them (§11, K126). Whether two statements are about the same thing is the modeller's judgement (K24).
Two `Requirement`s in force, of one kind, carrying incompatible values for the same thing are a refinement
error, which review finds; no syntactic constraint can, because sameness is a judgement.

**A complete requirement is never changed in place** (K129). A change of value is a decision stated in a
source: a `SourceDecision`, refined into a `RequirementDecision` that retires the requirement (K62), and a new
requirement refining the same `SourceNeed`s, whose value names the deciding source and which is walked once
when it is complete. A source that states a different value without deciding anything raises a
`RequirementChoice` on the complete requirement between its value and the new one. *We now want 800, not 300*
decides; *800 are coming* says something else. Which a passage does is read when it is extracted, and is the
modeller's responsibility (K40); whether its speaker has standing to decide is OQ16's. Nothing resolves
itself: every change of a value has a decision behind it, and the decision has an owner.
```

- [ ] **Step 3: The projection**

Replace the paragraph `**What the projection carries** is the requirement model as `01-requirement-model.md`
defines it: the requirements in force, with their identity, text and values, and the derivation edges between
them.` with:

```
**What the projection carries** is the requirement model as `01-requirement-model.md` defines it: the
requirements in force, each complete, with their identity and finished text, and the derivation edges between
them (K130). A requirement's values do not cross: its finished text states every one of them (§9, K111), and
the source each value names belongs to this model. A requirement in force that is not yet complete has no
finished text to carry; whether a baseline may be cut while one exists is OQ35.
```

In the paragraph beginning `**What it drops**`, after `` `RequirementDecision`s; and findings. `` insert
`A requirement's values, and the sources they name, are dropped with them (K130).` Rewrap.

- [ ] **Step 4: Check and commit**

Re-read §10 whole. `git add spec/02-requirement-analysis-model.md`; message
`Walk a requirement once, when complete; change one only by decision; carry finished text (K123, K128-K130)`.

---

### Task 3: `spec/02` §11 — the three questions, the ask as a rule, and the decision (K87–K89, K115–K122, K126)

**Files:** Modify `spec/02-requirement-analysis-model.md` §11: the paragraph beginning `**A `RequirementDecision`
is not an assumed value`; the `RequirementQuestion` class diagram and its first paragraph; the paragraph
beginning `**`RequirementQuestion` specialises into`; a block of new paragraphs after the one beginning
`**`RequirementChoice` additionally carries`; the paragraph beginning `**A `RequirementDefinition`'s *what to
ask* (§7) is not a second origin.**`; the paragraph beginning `**Two mechanisms are worked out`; the paragraph
beginning `**`RequirementQuestion` is not a *review finding*`.

**Interfaces:** Consumes Tasks 1–2. Produces `§11, K119` and `§11, K121`, cited by Tasks 4, 5 and 8.

- [ ] **Step 1: Check the adopted term**

K119 adopts ISO/IEC/IEEE 29148's *to be determined* (TBD) as the name of what a clarification records. Check
it against the standard (or a reliable secondary source quoting it) before writing it. If you cannot confirm
that 29148 uses *to be determined* for a requirement whose content is not yet known, omit the sentence that
names it in Step 4 and say so in the commit message.

- [ ] **Step 2: The decision paragraph**

Replace the paragraph beginning `**A `RequirementDecision` is not an assumed value, and the difference is how
each is resolved.**` (it ends `...assumed and derived, for the same reason.`) with:

```
**A `RequirementDecision` is not a value, and nothing about deciding sets one silently.** A decision is a
choice made in the presence of alternatives, by somebody with standing, recorded with the alternatives and
the rationale; what changes it is deciding again, which under K11 means a new source and a new
`RequirementDecision` beside the old one rather than an edit to it. Where a decision settles a value — a choice
between disagreeing sources (K126), or a change to a complete requirement (§10, K129) — the value names the
source the decision came from, as every value names its source (§7, K125).
```

- [ ] **Step 3: The class diagram and the opening**

In the `RequirementQuestion` class diagram, add the line `    RequirementQuestion <|-- RequirementClarification`
after the `RequirementChoice` specialisation line. In the paragraph directly above the diagram, replace `and it
is abstract because K79 gives it two.` with `and it is abstract because K79 and K119 give it three.`

Replace the first sentence of the paragraph beginning `**`RequirementQuestion` specialises into
`RequirementInquiry` and `RequirementChoice`, one per mechanism` — up to and including `Both carry
`discharges`:` — with:

```
**`RequirementQuestion` specialises into `RequirementInquiry`, `RequirementChoice` and
`RequirementClarification`, and which one a question is follows from what is present when its rule fires**
(K79, K119, K120): two or more things to choose between raise a `RequirementChoice`, a missing companion kind a
`RequirementInquiry`, a missing value a `RequirementClarification`. The first two carry `discharges`:
```

Keep the rest of that paragraph. Rewrap.

- [ ] **Step 4: The clarification, the disagreement, and the wording**

Directly after the paragraph beginning `**`RequirementChoice` additionally carries the candidate alternatives`,
insert:

```
**`RequirementClarification` carries nothing beyond the shared shape, and no `discharges`** (K119, K121). A
parameter's ask raises it where the parameter has no value on a requirement, one per `Requirement` and
parameter. It is open while the value stays missing, and closes when a source states the value, or when its
requirement is no longer in force. What closed it needs no edge of its own: the value names the source that
states it (§7, K125), and the chain from the question runs through `poses`, `replies` and `refine` to that
source. One posed `SourceQuestion` may carry several clarifications, each naming it by `poses`.
ISO/IEC/IEEE 29148's *to be determined* is the adopted name for what it records.

**The process, end to end.** A `SourceNeed`'s passage is refined into a `Requirement`, and the passage does not
state a parameter's value, so the value is missing. The parameter's ask raises a `RequirementClarification`,
naming the ask as its *triggered by* and the requirement as its triggering `Requirement`; the project manager
can see it from here. The modeller puts the question to somebody, in a source, and the clarification poses that
`SourceQuestion`. A later source replies; its passage is refined into the same requirement, whose refinement
edge is list-valued (§10); the value names that source, and the clarification closes. If the answer is that
nobody knows yet, the value stays missing and the clarification posed; how long it may wait is OQ13's interval
and OQ34's question. If the answer disagrees with another source, the clarification closes and the same ask
raises a `RequirementChoice`. If the answer arrives unasked, the clarification closes all the same, and the
chain lacks only its `poses` and `replies` links.

**A disagreement between sources raises a `RequirementChoice`, not a state of the value** (K118, K126). The
parameter's ask raises it, one per `Requirement` and parameter; its candidate alternatives are every competing
statement, each with its source, and a further disagreeing source adds to them rather than opening another
choice. One choice over all of them, not one per pair, because three or more sources may disagree. The project
manager decides, and the decision enters as a source (K11, K61), which the value then names.

**How a question is worded** (K122). A clarification's *statement* starts from its parameter's ask, which the
modeller may fit to the requirement in hand. A `RequirementChoice`'s and a `RequirementInquiry`'s statement the
modeller writes freely, informed by the rule's *what to look for*, from no template: a choice is about its own
alternatives and an inquiry about its own gap, so no wording written in advance fits them, and what prose
means is not an algorithm's to decide (K24).
```

(Omit the sentence naming 29148 if Step 1 could not confirm it.)

- [ ] **Step 5: The ask as a rule**

Replace the paragraph `**A `RequirementDefinition`'s *what to ask* (§7) is not a second origin.** It covers a
single missing parameter through the definition's own machinery — for an inherited parameter, the ask
inherited with it (§9, K112) — which is why that case raises no `RequirementQuestion` at all.` with:

```
**A `RequirementDefinition`'s *what to ask* (§7) is not a second origin either: it is a `Rule`.** Every
parameter's ask is a `ValueRule` belonging to the rule-set of the definition that declares the parameter,
inherited with it (§9, K112), in force for as long as the parameter is declared and never taken out of force on
its own (K115, K116; `03-project-lifecycle-model.md` §3). A missing or disputed value therefore reaches the
project manager by the same route as every other question, and no value can be missing or disputed in silence.
```

- [ ] **Step 6: The worked mechanisms**

Replace the paragraph beginning `**Two mechanisms are worked out, and the rest are open.**` (it ends `...named in
`06-decisions.md` under OQ18.`) with:

```
**Three mechanisms are worked out, and one is open.** A `Requirement` incompatible with one already in force,
canonically on terms a project had to state because the two are of different kinds, is
`03-project-lifecycle-model.md` §3's `ConflictRule`, raising a `RequirementChoice`. A `Requirement` whose kind
implies that another kind should also exist is that section's `CompletenessRule`, raising a
`RequirementInquiry` — this was OQ17's own original case, now answered. A parameter with no value, or with
sources that disagree about it, is that section's `ValueRule`, raising a `RequirementClarification` or a
`RequirementChoice` (K115–K118). When a wait for an answer becomes a decision has no worked mechanism; it is
held by a prerequisite named in `06-decisions.md` under OQ18. Whether a default may stay silent needs none:
no value is a default, since every value names its source (K127).
```

- [ ] **Step 7: Not a review finding**

In the paragraph beginning `**`RequirementQuestion` is not a *review finding*`, replace `and walking a rule-set
is ordinary modelling work performed when a requirement arises, not a separate act of review.` with:

```
and walking a rule-set is ordinary modelling work performed when a requirement becomes complete (§10, K128),
not a separate act of review. A question a `ValueRule` raises is not even judged: whether a value is present,
and whether its sources agree, is decided without judgement — so questions now come from two modes of
checking, and neither is review (K89, narrowed).
```

Rewrap.

- [ ] **Step 8: Check and commit**

Re-read §11 whole; nothing may still say a `RequirementQuestion` has two specialisations, or that *what to ask*
raises no question. `git add spec/02-requirement-analysis-model.md`; message
`Give every parameter's ask a question to raise: the clarification and the choice (K115-K122, K126)`.

---

### Task 4: `spec/02` §12 — the constraints (K116, K118, K119, K121, K125, K128)

**Files:** Modify `spec/02-requirement-analysis-model.md` §12.

- [ ] **Step 1: Over `RequirementDefinition`**

In the bullet beginning `- Every parameter a definition declares names the value domain it draws from.`,
replace `(§7, `04-value-states.md` §5)` with `(§7)`.

In the bullet beginning `- Every parameter a definition declares carries its own ask.`, replace `*what to ask*
exists so that a value in the unknown state has a stated route out of it, and a parameter missing its ask is
exactly the case where that route is absent (§7).` with `*what to ask* exists so that a missing value has a
stated route out of it, and it is the `ValueRule` that raises the question; a parameter missing its ask is
exactly the case where that route is absent (§7, K116).` Rewrap.

- [ ] **Step 2: Over the derivation**

Under **Over the derivation, and over being no longer in force.**, add as the first two bullets:

```
- Every value a `Requirement` carries names the source that states it. A value naming none is not a
  well-formed element of this model (§7, K125).
- A `Requirement` is complete exactly when every parameter it has has a value and no `RequirementChoice` raised
  on its values is open (§10, K128).
```

- [ ] **Step 3: Over the questions**

Rename the heading `**Over `RequirementQuestion`, `RequirementInquiry`, and `RequirementChoice`.**` to `**Over
`RequirementQuestion` and its three specialisations.**`. In the bullet `- No element is a `RequirementQuestion`
and nothing more: every `RequirementQuestion` in a model is an instance of `RequirementInquiry` or
`RequirementChoice` (§11, K79).` replace the ending with `an instance of `RequirementInquiry`,
`RequirementChoice` or `RequirementClarification` (§11, K79, K119).` After the `discharges` bullet, add:

```
- A `RequirementClarification` is one per `Requirement` and parameter, and is open exactly while that
  requirement is in force and has no value for the parameter. It carries no `discharges` (§11, K119, K121).
- A `RequirementChoice` raised by a disagreement is one per `Requirement` and parameter, and its alternatives
  are every statement of a value for that parameter (§11, K118, K126).
```

- [ ] **Step 4: Check and commit**

`grep -n "04-value-states\|value-state\|unknown state\|conflicting\|assumed" spec/02-requirement-analysis-model.md`
must print nothing except `primary specification rather than assumed` (§9, a different sense).
`git add spec/02-requirement-analysis-model.md`; message
`State the constraints over values and the three questions (K116-K128)`.

---

### Task 5: `spec/03` — `ValueRule`, the guard, and a walk that runs once (K103, K115–K120, K125–K128)

**Files:** Modify `spec/03-project-lifecycle-model.md`: §2 (after its table); §3's class diagram and the note
under it; the guard paragraphs; the K90 paragraphs; the paragraph beginning `Two specialisations are worked out
here`; the paragraph beginning `**What such a rule would have covered is already covered, three ways.**`; a new
`### ValueRule` subsection; *What each firing produces*; *Walking a `RuleSet`* and its flowchart; §6.

**Interfaces:** Consumes Tasks 1–4.

- [ ] **Step 1: §2, the silent-default row**

Directly after the table in §2 (the row list ending `Which other requirement kinds a given kind implies should
also be present`), insert:

```
**The first row no longer has a gap to fill** (K127). No value is ever a default, because every value names
the source that states it; what an implementation offers as a default is a suggestion a parameter's ask
carries, and becomes a value only when somebody states it. The row is kept, as what the evidence measured, and
states nothing a rule-set still needs to say.
```

- [ ] **Step 2: §3's class diagram**

Add to the diagram, after `Rule <|-- CompletenessRule`: `    Rule <|-- ValueRule` and
`    ValueRule --> Parameter : ranges over`. Replace the note's last sentence pair, from `Two further `Rule`
specialisations are named but not shaped` to `nothing here defines them yet.`, with `One further `Rule`
specialisation, the gap-timeout rule, is named but not shaped — see the `Rule` subsection — and is left off the
diagram for the same reason a design record leaves an open question out of a decision table: nothing here
defines it yet.` Rewrap.

- [ ] **Step 3: The guard**

In the paragraph beginning `**A guard excludes exactly what can be decided without judgement, and nothing
else.**`, keep that first sentence and replace everything after it (through `...would let a wrong assumption
silence a rule nobody then reads.`) with:

```
A criterion is decided on the value a source states for its parameter: there it either holds or is decided
false. A walk runs only on a complete requirement (`02-requirement-analysis-model.md` §10, K128), every
parameter of which has a value and none a disagreement, so every criterion the walk reads is decided (K103,
narrowed by K125 and K128). A guard therefore never excludes on a guess: no value is one, since every value
names the source that states it.
```

In the paragraph beginning `**The criteria of one guard are conjunctive**`, replace `(`04-value-states.md` §5,
K101)` with `(`02-requirement-analysis-model.md` §7, K101)`. In the paragraph beginning `**An exclusion creates
nothing**`, replace `and each value in its state and, when stated, its source.` with `and each value in the
source it names.` Rewrap each.

- [ ] **Step 4: The range axis (K90, K115, K120)**

In the paragraph beginning `**A `Rule` specialisation is fixed by what its firing test ranges over`, after
`` `CompletenessRule` tests a **set** — the requirements the implied kind would have to appear among. `` insert
`` `ValueRule` tests a **single value** — one parameter's value on one requirement (K115). ``

At the end of the paragraph beginning `Each consequence below is derived rather than stipulated.`, add:

```
A single-value test finds either no value, or several that disagree: the first is a gap in one requirement,
hence a `RequirementClarification`, the second alternatives, hence a `RequirementChoice`; each is one
requirement's and one parameter's, hence one open per requirement and parameter; and whether a value is
present, or disputed, reads no text, hence no judgement. Which question a firing raises therefore follows from
what is present when it fires, not from the specialisation alone (K120).
```

Rewrap both.

- [ ] **Step 5: Worked specialisations**

Replace the paragraph beginning `Two specialisations are worked out here: `ConflictRule` and
`CompletenessRule`, below.` (it ends `(K72, K90).`) with:

```
Three specialisations are worked out here: `ConflictRule`, `CompletenessRule` and `ValueRule`, below. A
silent-vs-owned-default rule, section 2's first row, needs none: no value is ever a default (K127). A
gap-timeout rule, the second row, is not worked out; it ranges over an open question together with elapsed
time, which nothing in the collection yet records, and stays in OQ18 (K72, K90).
```

- [ ] **Step 6: `ConflictRule`'s three destinations (K98)**

In the paragraph beginning `**What such a rule would have covered is already covered, three ways.**`, replace
the two sentences from `Where two sources disagree about the same thing, the value-state model carries it and
needs no rule:` to `...is what `06-decisions.md` records as OQ24.` with:

```
Where two sources disagree about the same thing, the parameter's own ask carries it: the statements are one
requirement, whose value is missing while a `RequirementChoice` between them is open
(`02-requirement-analysis-model.md` §10, §11, K123, K126).
```

Rewrap.

- [ ] **Step 7: The `ValueRule` subsection**

Insert, directly before `### What each firing produces`:

```
### `ValueRule`

A `ValueRule` ranges over a single value: one parameter's value on one `Requirement` (K115). It fires where that
value is missing, raising a `RequirementClarification`, and where the sources stating it disagree, raising a
`RequirementChoice` whose alternatives are the competing statements (`02-requirement-analysis-model.md` §11,
K118, K119).

**Every parameter's *what to ask* is a `ValueRule`** (K116). It belongs to the `RuleSet` of the
`RequirementDefinition` that declares the parameter, reaches every specialisation of it as every rule there does
(K69), and is inherited with the parameter (`02-requirement-analysis-model.md` §9, K112). Its identity is the
parameter's (K106), and its *what to look for* is the ask itself. It is in force for as long as its parameter is
declared, and cannot be taken out of force on its own: that would let a value go missing in silence, which is
what it exists to prevent. Because every parameter carries an ask (`02-requirement-analysis-model.md` §12), every
parameter is covered.

**It carries no *when it applies* and no guard, and it is not walked.** Its test reads whether a value is
present and whether its sources agree, which is decided without judgement, so it needs no relevance judgement
(K86) and nothing to exclude it; and it fires on an incomplete requirement, which no walk reaches (K128). On a
complete requirement it fires only where a source disagrees with a value the requirement already carries; that
raises a choice, and a change of the value goes as `02-requirement-analysis-model.md` §10 says a change goes
(K129).

**K98's test does not bite here.** K98 recognised the universal contradiction rule as no rule because every
attribute of the shape degenerated on it and it carried no content. An ask carries content no other parameter's
ask carries — what to ask, of whom, about which parameter — and states something about this definition, not
something true of every project.
```

- [ ] **Step 8: What each firing produces**

Add to that subsection's diagram: `    RequirementClarification --> ValueRule : triggered by` and
`    RequirementChoice --> ValueRule : triggered by`.

- [ ] **Step 9: The walk (K128)**

In the paragraph beginning `**A `RuleSet` is a written procedure, and matching is a relevance judgement made
while walking it**`, replace `When a new requirement arises in a subject, the `RuleSet`s that reach it are
walked.` with `When a requirement becomes complete (`02-requirement-analysis-model.md` §10, K128), the
`RuleSet`s that reach it are walked, once.` Rewrap.

Directly after the paragraph beginning `A guard is not a third step beside these two.`, insert:

```
**The walk runs once, and a `ValueRule` is not part of it.** A complete requirement is never changed in place
(`02-requirement-analysis-model.md` §10, K129), so nothing the walk read moves beneath it, and no change of a
value calls for a second walk (K128). A `ValueRule` reads presence and agreement, not relevance, and fires on
incomplete requirements the walk never reaches.
```

In the flowchart, change node `A` to `A["A Requirement becomes complete under a RequirementDefinition"]` and the
edge label `"no — or the guard is undecided"` to `"no"`.

- [ ] **Step 10: §6**

In the bullet `- No element is a `Rule` and nothing more: every `Rule` in a model is an instance of
`ConflictRule` or `CompletenessRule` (§3, K90).` replace the ending with `an instance of `ConflictRule`,
`CompletenessRule` or `ValueRule` (§3, K90, K115).` In the guard-operation bullet, replace
`` `04-value-states.md` §5 `` with `` `02-requirement-analysis-model.md` §7 ``. After the **Over
`CompletenessRule`.** block, add:

```
**Over `ValueRule`.**

- Every parameter a `RequirementDefinition` declares has exactly one `ValueRule`, its ask, in the `RuleSet` of
  that definition, in force for as long as the parameter is declared (§3, K116).
- A `ValueRule` carries no *when it applies* and no guard (§3).
```

In the paragraph beginning `**One rule over these elements reports rather than fails.**`, after `A `Rule` that
does not say when it applies is reported as a question, not a failed check.` insert `A `ValueRule`, which
carries none by definition, is outside it.` Rewrap.

- [ ] **Step 11: Check and commit**

`grep -n "04-value-states\|value-state\|conflicting\|assumed\|undecided" spec/03-project-lifecycle-model.md`
— every remaining hit must be about something other than a value's state. Re-read §3 whole.
`git add spec/03-project-lifecycle-model.md`; message
`Add ValueRule, decide every guard, and walk a requirement once (K103, K115-K128)`.

---

### Task 6: `spec/01`, `spec/00`, and `spec/04`'s withdrawal (K125, K130, K131)

**Files:** Modify `spec/01-requirement-model.md`, `spec/00-overview.md`; delete `spec/04-value-states.md`.

- [ ] **Step 1: `spec/01` §1**

Replace the paragraph beginning `One thing this document does lean on, and whoever adopts it takes up along with
it: the value-state model.` (it ends `...and §2 turns on the difference.`) with:

```
This document leans on nothing outside itself. A requirement here carries no values: its finished text states
every value it was produced with, and the values, with the sources they name, stay in
`02-requirement-analysis-model.md`, where they were settled (K130). The value-state model this document once
leaned on is withdrawn (K125, K131). The requirement analysis model is a member in the order, adopted after this
one or not at all, and §2 turns on that.
```

- [ ] **Step 2: `spec/01` §2**

Replace `A requirement carries four things: the three attributes below, and the derivation edge that follows
them.` with `A requirement carries three things: the two attributes below, and the derivation edge that follows
them.` Delete the table's *values* row. Replace the *text* row with:

```
| text | The requirement's bound wording: the statement itself, in the form it holds in the register, with every value it was produced with written into it (K111, K130) |
```

- [ ] **Step 3: `spec/01` §4**

After the paragraph beginning `A baseline's condition is losslessness and recoverability`, insert:

```
**A baseline carries each requirement in force with its finished text, and the derivation edges between them,
and nothing else** (K130). A requirement in force that is not yet complete has no finished text to carry;
whether a baseline may be cut while one exists is OQ35.
```

- [ ] **Step 4: `spec/00` §2**

Replace `Four members make it up.` with `Three members make it up (K131).` Delete the table row beginning
`| The value-state model |`. In the *requirement analysis model* row, after `to the decisions and findings that
stand behind it` insert ` — and where a requirement's values are, each naming the source that states it`.

Replace `The four connect three ways, and the value-state model crosscuts all three connections rather than
joining them as a fourth.` with:

```
A fourth member, the value-state model, was withdrawn once every value had to name its source: what remained
of it is one rule and a value domain's comparability, both stated where values are, in
`02-requirement-analysis-model.md` (K125, K131). The three connect as follows.
```

In the Mermaid graph, delete the `VS[...]` node line and the three `VS -.-` lines.

In the paragraph beginning `The requirement analysis model projects to the requirement model (K20)`, replace
`beyond a requirement's identity, its text, its values, and the edge by which it derives from another
requirement` with `beyond a requirement's identity, its finished text, and the edge by which it derives from
another requirement — a requirement's values and the sources they name are dropped with the rest, the finished
text already stating every value (K130) —`; and delete the sentences from `The value-state model has no box of
its own` through `in a design language's own elements beyond it.` Rewrap.

Replace the paragraph beginning `The numbered order of the documents after this one is not incidental` (it ends
`...the order in which the collection is taken up.`) and the paragraph after it (beginning `The order therefore
runs over the other three.`, ending `...where a binding answers that one.`) with:

```
The numbered order of the documents after this one is not incidental: it is adoption order. A reader who wants
a requirements register with traceability, and nothing else, reads `01-requirement-model.md` and stops there.
Reading `02-requirement-analysis-model.md` next adds the working model behind it — the source a requirement was
refined from, the definition it was produced under, the values and the sources they name, and the decisions and
findings that justify it. `03-project-lifecycle-model.md` after that adds the slot an organisation's own way of
working fills. This ordering is what answers OQ1: what a binding can take from this collection without the rest,
and what it cannot, is answered by naming how far down this order it reaches, rather than by inventing a
separate scale to measure it against. Nothing of a requirement's values crosses the seam (K130), so there is no
second scale beside it.
```

After the paragraph beginning `Two further documents round out `spec/``, add:

```
**The number 04 is retired.** It belonged to the value-state model; no later document of `spec/` takes it, so
that every citation of `04-value-states.md` in the decision record and in the dated design records keeps
meaning the document it meant (K131).
```

- [ ] **Step 5: Delete `spec/04`**

`git rm spec/04-value-states.md`. Then `grep -rn "04-value-states" spec bindings README.md CLAUDE.md` — hits may
remain only in `spec/05` and `bindings/` (Task 7) and in `spec/06` rows (Task 8, which keeps historical rows
as they are). `grep -n -i "value-state\|value state" spec/00-overview.md spec/01-requirement-model.md` must
print only the two sentences this task wrote that name the withdrawn model.

- [ ] **Step 6: Commit** — `git add spec/00-overview.md spec/01-requirement-model.md` (the deletion is already
staged); message `Withdraw the value-state model, fold it into spec/02, and retire the number 04 (K125, K130, K131)`.

---

### Task 7: `spec/05`, `bindings/sysml-v2.md`, `CLAUDE.md` — three declarations (K130, K131)

**Files:** Modify `spec/05-binding-contract.md`, `bindings/sysml-v2.md`, `CLAUDE.md`.

- [ ] **Step 1: `spec/05` §1**

Replace `not the other members of the collection beyond the two this document draws on,
`01-requirement-model.md` for the requirement and the baseline, and `04-value-states.md` for the value-state
model —` with `not the other members of the collection beyond the one this document draws on,
`01-requirement-model.md` for the requirement and the baseline —`. Replace `a binding carries K4's four
declarations` with `a binding carries K4's declarations, three since K130,`. Rewrap.

- [ ] **Step 2: `spec/05`, the count elsewhere**

Replace `the first of the four declarations below` with `the first of the three declarations below`, and `each
declare the same four things` with `each declare the same three things`.

- [ ] **Step 3: `spec/05` §4**

Retitle `## 4. The four declarations` to `## 4. The three declarations`. In its opening paragraph, replace `A
binding states four things about the design language it attaches.` with `A binding states three things about
the design language it attaches.`, `the four reasons are not all the same reason. Three of the four exist so that
a check the metamodel could not otherwise run becomes one it can.` with `the three reasons are not all the same
reason. Two of the three exist so that a check the metamodel could not otherwise run becomes one it can.`, and
`why leaving any one of the four out` with `why leaving any one of the three out`. Rewrap.

Replace the whole of `### 4.4 How far it takes the value model` (heading and body, up to the next `##`
heading) with:

```
### A fourth declaration, withdrawn

K4 asked a binding a fourth thing: how far it carries the value model. It is withdrawn (K130). Nothing of a
requirement's values crosses the seam — a baseline carries each requirement's finished text and the derivation
edges between them — and a value's source stays in the working model, so there is no value model for a binding
to carry.
```

- [ ] **Step 4: `bindings/sysml-v2.md`**

In the opening paragraph, replace `it states the four things that document's §4 asks` with `it states the three
things that document's §4 asks`. Replace section `## 4. How far it takes the value model` (heading and body)
with:

```
## 4. The value model, no longer declared

`05-binding-contract.md` no longer asks a binding how far it carries the value model: nothing of a requirement's
values crosses the seam (K130). This section answered, before that, that SysML v2 carries none of it, since a
SysML attribute carries a value or does not and records nothing about where it came from. That answer stands as
part of why the declaration could go.
```

- [ ] **Step 5: `CLAUDE.md`**

In §3's table, rule 4: replace `the value-state model,` with `the rule that every value names the source that
states it (K125),`. Rule 5: replace `declared in a **binding** that states four things: which of its elements may
carry `satisfies`, its internal refinement chain, its identifier space, and how far it takes the value model (K4)`
with `declared in a **binding** that states three things: which of its elements may carry `satisfies`, its
internal refinement chain, and its identifier space (K4, narrowed by K130)` — match the row's actual wording and
keep everything else in it. In §4's table, replace `each stating K4's four declarations` with `each stating K4's
three declarations`. After §4's table, add:

```
**The number 04 in `spec/` is retired** (K131). It belonged to the value-state model, withdrawn when what
remained of it moved into `spec/02`. Do not give a new document the number 04: the decision record and the
dated design records cite `04-value-states.md`, and must go on meaning that document.
```

- [ ] **Step 6: Check and commit**

`grep -rn "04-value-states\|four declarations\|four things" spec/05-binding-contract.md bindings CLAUDE.md` must
print only `CLAUDE.md`'s new paragraph. `git add spec/05-binding-contract.md bindings/sysml-v2.md CLAUDE.md`;
message `Withdraw the fourth declaration, and record the retired number 04 (K130, K131)`.

---

### Task 8: `spec/06` — K115–K131, revisions, and the open questions

**Files:** Modify `spec/06-decisions.md`.

- [ ] **Step 1: The decisions**

After the `## Decisions K113–K114` section's table, insert:

```
## Decisions K115–K131

Taken in [the design record of 2026-10-05 on a rule over one value](../docs/superpowers/specs/2026-10-05-value-rule-and-clarification-design.md)
(K115–K124) and [the design record of the same day on values from sources](../docs/superpowers/specs/2026-10-05-values-from-sources-and-the-complete-requirement-design.md)
(K125–K131), which amends the first before either reached `spec/`; written in by
[the integration plan of 2026-10-05](../docs/superpowers/plans/2026-10-05-values-and-questions-integration-plan.md).
K117 and K124 were withdrawn and superseded before integration and keep their rows, so that their numbers are not
reused. Two founding decisions are revised by the owner's explicit instruction: K1's value-state model, by
K125, and K4's four declarations, narrowed to three by K130. K19 is revised by K131; K79 by K120; K86 by K128;
K89 is narrowed; K98's first destination becomes K126's choice; K103 is narrowed by K125 and K128; K72's list
of worked mechanisms gains `ValueRule`.

| # | Decision | Reason |
|---|---|---|
| K115 | A third `Rule` specialisation, `ValueRule`, ranges over a single value: one parameter's value on one `Requirement`. It fires where the value is missing and where the sources stating it disagree | The silent cases share one range, and K90 makes the range what fixes a specialisation. Its test reads presence and agreement, decided without judgement |
| K116 | Every parameter's *what to ask* is a `ValueRule`, in the rule-set of the definition declaring the parameter and inherited with it. It is in force while the parameter is declared and cannot be taken out of force alone | K87 holds unchanged, and the route `spec/02` §11 gave a missing value joins the question route. Every parameter carries an ask, so every parameter is covered by construction |
| K117 | **Withdrawn before integration.** An organisation's `ValueRule` was to decide whether an assumed value raises a question | K125 leaves no assumed value to name |
| K118 | A disagreement raises a `RequirementChoice`, one per `Requirement` and parameter, whose alternatives are every competing statement with its source; a further one adds to them. It is discharged by a `RequirementDecision` | Several statements present is something to choose between, and the project manager chooses. One choice over all of them, not one per pair, since three or more sources may disagree |
| K119 | A missing value raises a `RequirementClarification`, a third specialisation of `RequirementQuestion`, carrying the shared shape | Nothing is present to choose between, so it is no choice; and the gap is a value of a requirement that exists, not a missing kind, so it is no inquiry. Reusing `RequirementInquiry` would break K75 |
| K120 | Which specialisation a question takes follows from what is present when its rule fires, not from the rule's specialisation alone. This revises K79's "one per mechanism" | `ValueRule` raises two specialisations; what K90 derived the question from was what is present at firing, and stated that way it holds for all three ranges |
| K121 | A `RequirementClarification` is one per `Requirement` and parameter, open while the value is missing, closed when a source states it or the requirement leaves force, and carries no `discharges` | The value names its source, and the chain from the question through `poses`, `replies` and `refine` records what closed it; an edge would name nothing the chain does not |
| K122 | A clarification's statement starts from its ask, which the modeller may fit to the requirement. A choice's or an inquiry's the modeller writes freely, informed by *what to look for*, from no template | *What to look for* says what to notice, not how to ask; a choice's alternatives and an inquiry's gap differ every time; what prose means is not an algorithm's to decide (K24) |
| K123 | Where sources state values for the same thing, they are refined into one `Requirement`, whose value is missing while the choice between them is open. Whether two statements are about the same thing is a judgement; two such requirements in force are a refinement error, found by review | It is the reading under which the disagreement reaches the project manager as one choice. Sameness cannot be a syntactic constraint. A later source correcting an earlier one is not a disagreement but a change (K129) |
| K124 | **Superseded before integration by K128.** The walk was not to run again when a value's state changed | K128 makes the walk run once, on a complete requirement |
| K125 | Every value names the source that states it, and a value no source states does not exist. The value-state model's five states are withdrawn: a value exists or is missing. This revises the founding K1 | A value enters the model only with somebody answerable for it. *Assumed* and *derived* let the modeller put in a value with nobody answerable, and the modeller decides nothing for the project |
| K126 | A disagreement between sources is not a value: the value is missing, and the disagreement is an open `RequirementChoice`. *Conflicting* is withdrawn as a state | The competing statements already stand in the choice. Nobody is answerable for a contested value until the project manager chooses, and the choice enters as a source |
| K127 | A value supplied to keep moving is stated by somebody with standing; a computed quantity is design or is stated by whoever computed it; an implementation's default is a suggestion carried by an ask, never a value | What *assumed* and *derived* were for is kept, each with the actor it always had |
| K128 | A requirement is complete when every parameter it has has a value and no choice about its values is open. The walk of the `RuleSet`s that reach it runs once, when it becomes complete, and never on one that is not. This revises K86's "when a requirement arises" | A rule always judges settled values; no criterion is undecided; no second walk arises. The incomplete requirement is not hidden: its missing values and open choices are questions |
| K129 | A complete requirement is never changed in place. A change is a decision in a source, refined into a `RequirementDecision` that retires it, and a new requirement refining the same needs. A source stating a different value without deciding raises a `RequirementChoice` | Every walk is over a requirement that will not move under it, and the history stays whole. Whether a passage decides is read at extraction (K40) |
| K130 | A baseline carries only the requirements in force, each complete, with its finished text and the derivation edges between them; nothing of their values, sources or questions crosses. K35 is confirmed, and K4's fourth declaration is withdrawn | The finished text states every value, so carrying the values carries nothing a design language needs and something it cannot hold. With no value states, a binding has no value model to declare |
| K131 | The collection has three members; what remained of the value-state model is stated in `spec/02`, `04-value-states.md` is withdrawn, and the number 04 is retired. This revises K19 | Values now occur only in the working model and its rules, so nothing is left to crosscut. The number is not reused so that every citation of `04-value-states.md` keeps its meaning |
```

- [ ] **Step 2: The open questions**

Add each paragraph directly after the table of the section that lists the question (the pattern the OQ9 and
OQ31 paragraphs already follow):

- after the OQ14/OQ16 table, below the existing `**OQ14 waits for OQ30.**` paragraph: `**OQ14 is deferred
  again** (2026-10-05). OQ30 is answered and the model side's three question specialisations are settled; the
  owner has deferred the abstract type all the same. If it is taken up, the owner's name for it is
  `RequirementModelElement`.`
- after the OQ18/OQ19 table: `**OQ18 is narrowed to its second half.** Its first half, the silent-vs-owned
  default, dissolves: no value is a default, since every value names its source (K125, K127). The gap-timeout
  rule stays open, and OQ34 meets it at the level of a requirement.`
- after the OQ21–OQ23 table, below the existing OQ21 paragraph: `**OQ22 is closed by K119 and K122.** A
  clarification's wording starts from its ask; a choice's and an inquiry's the modeller writes freely, from no
  template.`
- after the OQ24–OQ26 table: `**OQ24 is closed by K123**: disagreeing sources about the same thing are one
  requirement, whose disagreement is a choice; two such requirements are a refinement error, found by review.
  **OQ25 dissolves**: the marking it asked about was a marking on a value's state, and the open question about
  the value is now the marking (K119, K125, K126).`
- after the OQ28 table: `**OQ28 gains a reading** (K127): sizing knowledge, a computation from a requirement's
  values to its answer, is not held in the requirement model — it is design, or is stated by whoever computed
  it.`
- after the OQ29 table: `**OQ29 is answered by K128.** The walk runs once, on a complete requirement, which is
  never changed in place; no change of a value calls for a second walk.`
- after the OQ30/OQ31 table, below the OQ31 paragraph: `**OQ30 is closed by K115–K121, K125 and K126.** A
  missing value reaches the project manager as a `RequirementClarification` and a disagreement as a
  `RequirementChoice`, each raised by the parameter's own ask.`

- [ ] **Step 3: OQ33–OQ35**

Before `## Status of the founding record's open questions`, insert a section `## Open questions OQ33–OQ35`,
with the line `Raised in [the design record of 2026-10-05 on values from sources](../docs/superpowers/specs/2026-10-05-values-from-sources-and-the-complete-requirement-design.md).`
and a three-row `| # | Question | When answerable |` table copying OQ33, OQ34 and OQ35 from that record's §5
verbatim.

- [ ] **Step 4: OQ1's status row**

In the OQ1 row of the founding record's status table, replace `The value-state model is no step on that scale,
being carried from the first, so the premise of two specifications is gone, and no separate scale of named
levels is needed` with `The value-state model, whose separability first raised the question, has since been
withdrawn (K125, K131), so the premise of two specifications is gone, and no separate scale of named levels is
needed`, and replace `together with the binding's fourth declaration, how far it takes the value model
(`05-binding-contract.md` §4, K4); the SysML v2 binding takes none of it.` with `and nothing of a requirement's
values crosses the seam (K130).`

- [ ] **Step 5: Check and commit**

Every new K row is one line; K-numbers run K115–K131 without a gap. `git add spec/06-decisions.md`; message
`Add K115-K131 to spec/06-decisions.md; close OQ22, OQ24, OQ29, OQ30; dissolve OQ25; open OQ33-OQ35`.

---

### Task 9: `CHANGELOG.md`, and the sweep

**Files:** Modify `CHANGELOG.md`; any file the sweep shows still needs a line.

- [ ] **Step 1: The entry** — at the end of `## [Unreleased]` / `### Added`:

```
- Values only from sources, the complete requirement walked once, and a rule over one value. Every value
  names the source that states it; *assumed*, *derived* and *conflicting* are withdrawn, and with them the
  value-state model, whose remainder moves into `spec/02` — the number 04 is retired. A requirement is
  complete when every parameter has a value and no choice about its values is open; the rule-set is walked
  once, then, and a complete requirement is changed only by a decision that retires it. Every parameter's ask
  is a `ValueRule`, raising a `RequirementClarification` for a missing value and a `RequirementChoice` for a
  disagreement. A baseline carries finished text only, and a binding declares three things, not four.
  K115–K131 record the decisions; OQ22, OQ24, OQ29 and OQ30 are closed, OQ25 dissolves, OQ18 narrows, and
  OQ33–OQ35 open. Findings are in
  [`docs/superpowers/specs/2026-10-05-value-rule-and-clarification-design.md`](docs/superpowers/specs/2026-10-05-value-rule-and-clarification-design.md)
  and
  [`docs/superpowers/specs/2026-10-05-values-from-sources-and-the-complete-requirement-design.md`](docs/superpowers/specs/2026-10-05-values-from-sources-and-the-complete-requirement-design.md).
```

- [ ] **Step 2: The sweep**

Run each, from the repository root, and resolve every hit that is not historical:

```
grep -rn "04-value-states" spec bindings README.md CLAUDE.md
grep -rn -i "value-state\|value state\|five states" spec bindings README.md CLAUDE.md
grep -rn -i -w "assumed\|conflicting" spec bindings
grep -rn -i "four declarations\|four things\|§4\.4" spec bindings CLAUDE.md
grep -rn -i "unknown state\|derived value" spec bindings
```

Allowed: rows and paragraphs of `spec/06` that record earlier decisions as taken; the sentences Tasks 6 and 7
wrote naming the withdrawn model and the retired number; *assumed* or *conflicting* in a sense that is not a
value's state. Anything else gets a one-line fix in the file it is in, citing the K-number that settled it.

- [ ] **Step 3: Commit** — `git add CHANGELOG.md` and the files Step 2 touched; message
`Record values from sources and the value rule in CHANGELOG, and sweep the remaining references`.
