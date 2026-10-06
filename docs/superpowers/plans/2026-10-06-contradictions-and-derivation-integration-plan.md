# Integrate contradictions as requirements and derivation as elaboration (K136–K149) into `spec/` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Write the owner's review of the K115–K135 integration into `spec/`, on the same branch, before it
reaches `main`: a value reaches a requirement only through the `SourceNeed`s it refines; contradicting needs
are separate requirements under one choice, which the project manager settles by the time a baseline is cut,
and nothing is merged; leaving force is final; every requirement refines a need, and derivation is elaboration
agreed with the client, which a `CompletenessRule` looks for beneath the requirement that triggered it; K135
narrows to edges between requirements. This is prose, not code: a "test" for each task is a re-read for
internal consistency, cross-reference correctness and conformance to `CLAUDE.md`, plus the grep checks the task
names.

**Architecture:** `spec/02` carries most of it — §5 and §7 (values and needs), §10 (what a requirement carries,
completeness, contradiction, correction, leaving force, the projection), §11 (the decision, the choice, the
clarification, the mechanisms), §12. `spec/03` gains the `CompletenessRule` beneath its trigger and the
`ValueRule` over contradicting requirements. `spec/01` gets derivation and origin; `spec/00`, `spec/05` and
`CLAUDE.md` follow. `spec/06` records K136–K149 and OQ38–OQ40; `docs/eventml-decisions.md` records D49's
overturn; `CHANGELOG.md` the release note.

**Tech Stack:** Markdown, Mermaid (GitHub-rendered), git.

## Read first — the decision this plan writes

[`docs/superpowers/specs/2026-10-06-contradictions-derivation-and-the-review-of-k115-k135-design.md`](../specs/2026-10-06-contradictions-derivation-and-the-review-of-k115-k135-design.md)
is the only authority for what follows. It revises K115–K135 as
[the plan of 2026-10-05](2026-10-05-values-and-questions-integration-plan.md) wrote them; where this plan
replaces text that plan wrote, this plan wins. Read the record whole before Task 1. Its §1 states the owner's
principle every task serves: the requirement model records the state the project is actually in, and raises
its problems; it does not press the project into an ideal.

## Global Constraints

- **Branch:** `integrate-k115-k135`, whose last commits are this plan's record and OQ37. Work on it; do not
  rebase it.
- **English only. No notation, no filled `RequirementDefinition`, nothing executable. "Attribute", not
  "field". Do not describe the metamodel in terms of files** (`CLAUDE.md` §1, §5). No example names a real
  domain; the record's examples are abstract, keep them so.
- **Historical documents are not edited:** the founding record and everything under `docs/superpowers/` keep
  their wording (`CLAUDE.md` §4). `docs/eventml-decisions.md` is the exception Task 8 names: it records
  overturns, as it already does for D31 and D46.
- **Vocabulary, used verbatim:** *stated by a `SourceNeed` the requirement refines*; *contradiction* and
  *contradicting requirements* (never "disagreement" for the K139 case in new text); *the modeller's flag*;
  *deriving from the triggering requirement*; *elaboration agreed with the client*.
- **Decision rows in `spec/06` are single table lines**, copied verbatim from the record. Earlier rows (K123,
  K126 and the rest) keep their wording; the section preamble names what revises them.
- **Line wrapping.** Prose wraps greedily at 110 characters, counted in characters (`—` and `§` are one).
  Table rows and Mermaid blocks are not wrapped. Rewrap only paragraphs you change.
- **Match anchor text, not line numbers.** Line numbers drift as tasks land.
- **Commit after every task, staging only the files the task names.** Never push. End every commit message
  with a blank line and `Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>`.
- **Out of scope:** OQ37's main question (K33 and the baseline's kind), the answers to OQ38–OQ40, the audit
  of EventML's inherited decisions (D32 among them, which also names derivation as an origin), the editor and
  skills repositories.

---

### Task 1: `spec/02` §5 and §7 — a value is stated by a `SourceNeed`; a contradiction is not a disagreement (K136, K139, K141)

**Files:** Modify `spec/02-requirement-analysis-model.md` — §5's paragraph beginning `A `SourceNeed` carries
nothing beyond`; the `SourceUpdate` subsection's paragraph beginning `**A correction is neither`; §7's *what to
ask* row; the paragraph beginning `**A value exists only where a source states it**`.

**Interfaces:** Produces `§7, K136`, which every later task cites for where a value comes from.

- [ ] **Step 1: §5, the `SourceNeed` paragraph**

Replace `in the values a `Requirement` carries in this model once `refine` (§10) has run, each naming the
source that states it (§7, K57, K125).` with `in the values a `Requirement` carries in this model once
`refine` (§10) has run (§7, K57, K136).` Replace its last sentence, `A value occurs on a `Requirement`, and
names the source that states it (§7, K125).`, with:

```
A value occurs on a `Requirement`, and is stated by a `SourceNeed` the requirement refines (§7, K136).
```

Rewrap.

- [ ] **Step 2: §5, the correction paragraph**

Replace `**A correction is neither a disagreement nor a decision.** A passage that states a different value
without replacing anything — *800 are coming* — is a disagreement, and raises a choice (§11, K126).` with:

```
**A correction is neither a contradiction nor a decision.** A passage that states a different value without
replacing anything — *800 are coming* — contradicts what was said: it is refined into a requirement of its
own, and a choice is raised between the two (§10, §11, K139).
```

Keep the rest of the paragraph. Rewrap.

- [ ] **Step 3: §7, the *what to ask* row**

Replace the row with:

```
| what to ask | For each parameter, how a non-expert is asked for what is missing. It is a rule: where the parameter has no value on a requirement it raises a clarification, and where requirements of one kind state contradicting values of it for the same thing it raises a choice between them (K116, K141) |
```

- [ ] **Step 4: §7, where a value comes from**

Replace the paragraph beginning `**A value exists only where a source states it** (K125).` (it ends `...written
per parameter rather than per definition.`) with:

```
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
```

- [ ] **Step 5: Check**

Re-read §5 and §7. `grep -n "names the source\|K126\|disagreement" spec/02-requirement-analysis-model.md` —
hits may remain only in §10–§12, which later tasks change.

- [ ] **Step 6: Commit** — `git add spec/02-requirement-analysis-model.md`; message
`State every value by a SourceNeed the requirement refines, and a contradiction as two requirements (K136, K139, K141)`.

---

### Task 2: `spec/02` §10 — what a requirement carries, completeness, contradiction, correction, leaving force (K136–K140, K142, K144, K145)

**Files:** Modify `spec/02-requirement-analysis-model.md` §10: the paragraph beginning `A requirement is not
written; it is **derived**.`; the paragraph beginning `The edge that records the derivation is the
**refinement** edge.`; a new subsection before `### The complete requirement`; the three paragraphs of `### The
complete requirement`; the paragraph beginning `Retirement arrives the way everything else here arrives`; the
paragraphs beginning `**What the projection carries**` and `**What it drops**`.

**Interfaces:** Consumes Task 1. Produces `§10, K137`, `§10, K139`, `§10, K140` and `§10, K144`, which Tasks
3–7 cite.

- [ ] **Step 1: The derivation**

Replace `are filled from the `SourceNeed`'s passage and from other sources that state them, and each value
names the source that states it (§7, K125).` with `are filled from the passages of the `SourceNeed`s the
requirement refines, and each value is stated by one of them (§7, K136).` Rewrap.

- [ ] **Step 2: The refinement edge, and the origin**

At the end of the paragraph beginning `The edge that records the derivation is the **refinement** edge.`
(after `...decidable with this edge in view.`), add:

```
**Every requirement refines at least one `SourceNeed`** (K145). The derivation edge of
`01-requirement-model.md` §2 is never an origin: a requirement derived from others and refining no need would
carry content the modeller produced, and the modeller is answerable for nothing in the project.
```

Rewrap.

- [ ] **Step 3: What a requirement carries in this model**

Insert, directly before `### The complete requirement`:

```
### What a requirement carries in this model

A `Requirement` in this model carries what `01-requirement-model.md` §2 gives it — its identity, its text and
the derivation edge — and what this model adds (K137).

| Attribute or edge | Carries |
|---|---|
| values | At most one value per parameter the requirement has, each stated by a `SourceNeed` the requirement refines (§7, K136). A parameter with none has no value |
| text | Its finished text, produced from its definition's template once every parameter has a value (§9, K111); none before |
| `refine` | The `SourceNeed`s it is assembled from: at least one, list-valued (D48, K145) |
| produced under | The `RequirementDefinition` it was produced under: exactly one, and not abstract (K8, K67, K109) |
| `supersedes`, `supersededBy` | The requirements it replaces, and those replacing it (K133, K134) |
| `retiredBy` | The `RequirementDecision` that took it out of force, where one did: at most one (K134, K144) |
```

- [ ] **Step 4: The complete requirement**

Replace the three paragraphs of `### The complete requirement` — beginning `**A requirement is complete when`,
`**Where sources disagree about the same thing` and `**A complete requirement is never changed in place**` —
with:

```
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
those needs for the rest (K136, K138). It is walked once, when it is complete. A source that states a different
value without replacing anything contradicts what was said, and is refined into a requirement of its own
(K139). A decision to drop a requirement with nothing in its place is a `SourceDecision`, whose
`RequirementDecision` retires it (K62). Nothing resolves itself: every change has a source behind it, and the
source a speaker.
```

- [ ] **Step 5: No longer in force (K144)**

In the paragraph beginning `Retirement arrives the way everything else here arrives`, after `...and nothing is
deleted (K5).`, add:

```
A requirement is no longer in force once it has a `retiredBy` or a `supersededBy`, whatever the state of the
element at the other end (K144): a superseding requirement that later leaves force does not return the one it
replaced to force, because leaving force is an event in a requirement's life and is final.
```

Rewrap.

- [ ] **Step 6: The projection (K142)**

In the paragraph beginning `**What the projection carries**`, replace `and the source each value names belongs
to this model.` with `and the `SourceNeed` stating each value belongs to this model.` After its last sentence
(`...whether a baseline may be cut while one exists is OQ35.`), add:

```
A baseline is cut only once every `RequirementChoice` raised over contradicting requirements is decided (§11,
K142). A decision that the contradiction is not real keeps every one of them in force, and every one crosses.
```

In the paragraph beginning `**What it drops**`, replace `A requirement's values, and the sources they name, are
dropped with them (K130).` with `A requirement's values are dropped with them (K130).` Rewrap both.

- [ ] **Step 7: Check and commit**

Re-read §10 whole. `grep -n "names the source\|sources they name\|source each value\|K123\|K126" spec/02-requirement-analysis-model.md`
— hits may remain only in §11–§12. `git add spec/02-requirement-analysis-model.md`; message
`Give a requirement its attributes, make contradiction two requirements, and leaving force final (K136-K140, K142, K144, K145)`.

---

### Task 3: `spec/02` §11 — the decision, the choice and its flag, the clarification, the mechanisms (K139, K141–K143, K145)

**Files:** Modify `spec/02-requirement-analysis-model.md` §11: the paragraph beginning `**A
`RequirementDecision` is not a value`; the *triggering `Requirement`s* row of the `RequirementQuestion`
attribute table; the paragraph beginning `**`RequirementChoice` additionally carries`; the three paragraphs
beginning `**`RequirementClarification` carries nothing`, `**The process, end to end.**` and `**A
disagreement between sources raises`; the paragraphs beginning `**A `RequirementDefinition`'s *what to ask*`,
`**Three mechanisms are worked out`, `**`RequirementQuestion` is not a *review finding*` and `**Three
checking modes exist`; item 2 of the list under `**Two rules already exist over `SourceNeed`s`.

**Interfaces:** Consumes Tasks 1–2. Produces `§11, K141`, `§11, K142` and `§11, K143`.

- [ ] **Step 1: The decision sets no value**

In the paragraph beginning `**A `RequirementDecision` is not a value`, replace `Where a decision settles a
value — a choice between disagreeing sources (K126) — the value names the source the decision came from, as
every value names its source (§7, K125).` with:

```
A decision settling a contradiction sets no value: it keeps one of the contradicting requirements and retires
the others, or decides that the contradiction is not real and keeps them all (K142).
```

Rewrap.

- [ ] **Step 2: The triggering requirements**

Replace the *triggering `Requirement`s* row with:

```
| triggering `Requirement`s | Every `Requirement` that triggered it. List-valued: a choice over contradicting requirements names each of them, and grows when a further one contradicts (`03-project-lifecycle-model.md` §3, K141) |
```

- [ ] **Step 3: The choice's flag (K142)**

At the end of the paragraph beginning `**`RequirementChoice` additionally carries the candidate
alternatives`, add:

```
**A `RequirementChoice` raised over contradicting requirements also carries the modeller's flag**: whether the
modeller judges the contradiction real (K142). The flag is advice, not a decision. The project manager
decides, at the latest when a baseline is cut, to keep one requirement and retire the others, or that the
contradiction is not real and every one stays in force. Nothing is merged, in this model or in a baseline;
one design element satisfying several of them is design, beyond the seam.
```

- [ ] **Step 4: The clarification (K143)**

Replace the paragraph beginning `**`RequirementClarification` carries nothing beyond the shared shape` with:

```
**`RequirementClarification` carries nothing beyond the shared shape, and no `discharges`** (K119, K121). A
parameter's ask raises it where the parameter has no value on a requirement, one per `Requirement` and
parameter. It is open while no `SourceNeed` the requirement refines states the parameter's value and the
requirement is in force, and closes when one does, or when the requirement leaves force (K143). What closed it
needs no edge of its own: the value is stated by a `SourceNeed` the requirement refines (§7, K136), and the
chain from the question runs through `poses`, `replies` and `refine` to it. One posed `SourceQuestion` may
carry several clarifications, each naming it by `poses`.
```

- [ ] **Step 5: The process**

In the paragraph beginning `**The process, end to end.**`, replace `A later source replies; its passage is
refined into the same requirement, whose refinement edge is list-valued (§10); the value names that source, and
the clarification closes.` with `A later source replies; a `SourceNeed` anchored in it is refined into the
same requirement, whose refinement edge is list-valued (§10); that `SourceNeed` states the value, and the
clarification closes.` Replace `If the answer disagrees with another source, the clarification closes and the
same ask raises a `RequirementChoice`.` with:

```
If the value it states contradicts the value another requirement of the same kind carries for the same thing,
the clarification closes all the same, and the same ask raises a `RequirementChoice` between the two
requirements (K139, K141).
```

Rewrap.

- [ ] **Step 6: The contradiction**

Replace the paragraph beginning `**A disagreement between sources raises a `RequirementChoice`, not a state
of the value**` with:

```
**A contradiction raises a `RequirementChoice` between requirements, not a state of a value** (K139, K141).
Where the modeller has judged, at extraction, that requirements of one kind state values of a parameter for
the same thing, and the values contradict, the parameter's ask raises a choice naming every one of them as
its triggering requirements; a further contradicting requirement extends it rather than opening another. Its
candidate alternatives are the requirements, each with the `SourceNeed`s it refines. One choice over all of
them, not one per pair, because three or more may contradict. Two requirements of one kind are produced from
one template, so where they contradict they differ in a parameter's value, and no contradiction of one kind
goes unraised. The project manager decides, and the decision enters as a source (K11, K61).
```

- [ ] **Step 7: The ask as a rule**

In the paragraph beginning `**A `RequirementDefinition`'s *what to ask* (§7) is not a second origin
either`, replace `A missing or disputed value therefore reaches the project manager by the same route as every
other question, and no value can be missing or disputed in silence.` with:

```
A missing value, and a contradiction between requirements of one kind, therefore reach the project manager by
the same route as every other question, and neither can stand in silence. Whether the ask needs to be a
`Rule` at all is OQ38.
```

Rewrap.

- [ ] **Step 8: The worked mechanisms**

In the paragraph beginning `**Three mechanisms are worked out, and one is open.**`, replace `A parameter with
no value, or with sources that disagree about it, is that section's `ValueRule`, raising a
`RequirementClarification` or a `RequirementChoice` (K115–K118).` with `A parameter with no value is that
section's `ValueRule`, raising a `RequirementClarification`; requirements of one kind contradicting in a
parameter's value are the same rule, raising a `RequirementChoice` between them (K115, K116, K139, K141).`
Replace `no value is a default, since every value names its source (K127).` with `no value is a default, since
every value is stated by a `SourceNeed` (K127, K136).` Rewrap.

- [ ] **Step 9: Not a review finding, and the checking modes**

In the paragraph beginning `**`RequirementQuestion` is not a *review finding*`, replace `A question a
`ValueRule` raises is not even judged: whether a value is present, and whether its sources agree, is decided
without judgement — so questions now come from two modes of checking, and neither is review (K89, narrowed).`
with:

```
A clarification a `ValueRule` raises is not even judged: whether a value is present is decided without
judgement. The choice it raises rests on the modeller's judgement, made at extraction, that requirements state
values for the same thing (K141). Questions therefore come from two modes of checking, and neither is review
(K89, narrowed).
```

In the paragraph beginning `**Three checking modes exist`, replace `A `ValueRule` produces one without
judgement, since it reads no text, only whether a value is present and whether its sources agree;` with `A
`ValueRule` produces a clarification without judgement, since it reads no text, only whether a value is
present;`. Rewrap both.

- [ ] **Step 10: The origin rule**

Replace item 2 of the list under `**Two rules already exist over `SourceNeed`s and `Requirement`s` with:

```
2. **A requirement that refines no `SourceNeed` is a failed check, not a question** (K9, K145, D32). The
   invariant behind it is that every requirement names its origin, and its origin is a `SourceNeed` it
   refines: a derivation from other requirements is never one (`01-requirement-model.md` §2). EventML allowed
   an origin of derivation alone (D49), which ProjectML does not adopt; it also shipped the rule as a question
   (D46), which K9 overturns. [`docs/eventml-decisions.md`](../docs/eventml-decisions.md) records both.
```

- [ ] **Step 11: Check and commit**

Re-read §11 whole. `grep -n "disagree\|names the source\|value names\|K126" spec/02-requirement-analysis-model.md`
— hits may remain only in §12. `git add spec/02-requirement-analysis-model.md`; message
`Make the choice settle contradicting requirements, flag them, and close a clarification once (K139, K141-K143, K145)`.

---

### Task 4: `spec/02` §12 — the constraints (K136, K137, K140–K145, K148)

**Files:** Modify `spec/02-requirement-analysis-model.md` §12, under **Over the derivation, and over being no
longer in force.** and **Over `RequirementQuestion` and its three specialisations.**

- [ ] **Step 1: Over the derivation**

Replace, each whole bullet:

- `- Every value a `Requirement` carries names the source that states it.` … `(§7, K125).` with
  ```
  - Every value a `Requirement` carries is stated by a `SourceNeed` the requirement refines, and it carries at
    most one value per parameter (§7, §10, K136, K137).
  ```
- `- A `Requirement` is complete exactly when` … `(§10, K128).` with
  ```
  - A `Requirement` is incomplete exactly when a parameter it has has no value, and complete otherwise (§10,
    K140).
  ```
- `- A `Requirement` is no longer in force exactly when` … `(§10, K62, K133, K134).` with
  ```
  - A `Requirement` is no longer in force exactly when it has a `retiredBy` or a `supersededBy`, whatever the
    state of the element at the other end, and never both. It has at most one `retiredBy` (§10, K62, K133,
    K134, K144).
  ```
- `- A requirement carrying no origin edge at all — neither refinement nor derivation — is a failed check`
  … `(§11, K9, D49, D32).` with
  ```
  - A requirement that refines no `SourceNeed` is a failed check (§11, K9, K145, D32).
  ```

Keep the sentence that follows it (`This constraint and the one above it are the same break...`).

- [ ] **Step 2: Over the questions**

Replace, each whole bullet:

- `- A `RequirementClarification` is one per `Requirement` and parameter, and is open exactly while` …
  `(§11, K119, K121).` with
  ```
  - A `RequirementClarification` is one per `Requirement` and parameter, and is open exactly while that
    requirement is in force and no `SourceNeed` it refines states a value for the parameter. It carries no
    `discharges` (§11, K119, K143).
  ```
- `- A `RequirementChoice` raised by a disagreement` … `(§11, K118, K126).` with
  ```
  - A `RequirementChoice` raised by a parameter's ask names, as its triggering requirements, every requirement
    it is raised over, and carries the modeller's flag (§11, K141, K142).
  ```
- `- At most one `RequirementInquiry` per `Rule` is open at a time;` … `(§11,
  `03-project-lifecycle-model.md` §3, K75).` with
  ```
  - At most one `RequirementInquiry` per `Rule` and triggering `Requirement` is open at a time. One a
    `CompletenessRule` raised is discharged only by a `Requirement` deriving from its triggering requirement;
    whether only directly is OQ40 (§11, `03-project-lifecycle-model.md` §3, K148).
  ```

- [ ] **Step 3: Check and commit**

`grep -n "names the source\|value names\|disagree\|K123\|K126\|derivation, or both\|neither refinement nor derivation" spec/02-requirement-analysis-model.md`
must print nothing. `git add spec/02-requirement-analysis-model.md`; message
`State the constraints over values, contradicting requirements, leaving force and origin (K136-K145, K148)`.

---

### Task 5: `spec/03` — the inquiry beneath its trigger, the rule over contradicting requirements, and housekeeping (K127, K136, K140, K141, K148)

**Files:** Modify `spec/03-project-lifecycle-model.md`: §2 (three places); the guard and exclusion paragraphs;
the K90 range paragraphs; the K92 paragraph; the `ConflictRule` paragraph beginning `**What such a rule would
have covered`; the `CompletenessRule` subsection; the `ValueRule` subsection; the walk and its flowchart; §6.

**Interfaces:** Consumes Tasks 1–4.

- [ ] **Step 1: §2, housekeeping (K127)**

Replace `Each of the four found so far closes a gap `02-requirement-analysis-model.md` leaves open on
purpose, because closing it there would fix an organisation's way of working into the metamodel itself.` with
`Each of the four found so far named a gap `02-requirement-analysis-model.md` left open on purpose, because
closing it there would fix an organisation's way of working into the metamodel itself; the first has since
closed (below, K127).`

In the paragraph beginning `**The first row no longer has a gap to fill**`, replace `because every value names
the source that states it;` with `because every value is stated by a `SourceNeed` (K136);`.

In the paragraph beginning `**They are stated per kind, not per definition.**`, replace `A rule-set says how a
default belonging to a kind of requirement is treated, how long a gap of that kind is waited on, how a conflict
between requirements of that kind is resolved — not how one particular definition's default is treated.` with
`A rule-set says how long a gap belonging to a kind of requirement is waited on, how a conflict between
requirements of that kind is resolved, and which other kinds that kind implies — not how one particular
definition's gap or conflict is treated.` Rewrap all three.

- [ ] **Step 2: The guard and the exclusion**

In the paragraph beginning `**A guard excludes exactly what can be decided without judgement`, replace
everything after its first sentence with:

```
A criterion is decided on the value a `SourceNeed` states for its parameter: there it either holds or is
decided false. A walk runs only on a complete requirement (`02-requirement-analysis-model.md` §10, K128,
K140), every parameter of which has a value, so every criterion the walk reads is decided (K103, narrowed by
K125 and K128). A guard therefore never excludes on a guess: no value is one, since every value is stated by a
`SourceNeed` (K136).
```

In the paragraph beginning `**An exclusion creates nothing`, replace `and each value in the source it names.`
with `and each value in the `SourceNeed` stating it.` Rewrap both.

- [ ] **Step 3: The range axis (K141, K148)**

In the paragraph beginning `**A `Rule` specialisation is fixed by what its firing test ranges over`,
replace ``CompletenessRule` tests a **set** — the requirements the implied kind would have to appear among.`
with ``CompletenessRule` tests a **set** — the requirements the implied kind would have to appear among,
which derive from the requirement that triggered it (K148).` and replace ``ValueRule` tests a **single
value** — one parameter's value on one requirement (K115).` with ``ValueRule` tests **one parameter** — its
value on one requirement, and the values of it requirements of one kind state for the same thing (K115,
K141).`

In the paragraph beginning `Each consequence below is derived rather than stipulated.`, replace `the gap is one
property of the whole set, hence at most one open at a time;` with `the gap is one property of what derives
from the triggering requirement, hence at most one open per rule and triggering requirement;` and replace
everything from `A single-value test finds either no value,` to the end of the paragraph with:

```
A test over one parameter finds either no value on a requirement, or requirements of one kind that contradict
in it. The first is a gap in one requirement, hence a `RequirementClarification`, one open per requirement and
parameter, and whether a value is present reads no text, hence no judgement. The second is alternatives, hence
a `RequirementChoice` over the contradicting requirements, and whether they state values for the same thing
is the modeller's judgement, made at extraction (K24, K40). Which question a firing raises therefore follows
from what is present when it fires, not from the specialisation alone (K120).
```

Rewrap both.

- [ ] **Step 4: Attachment and reach (K92, K148)**

Replace the paragraph beginning `**A `Rule`'s attachment determines what *triggers* it` with:

```
**A `Rule`'s attachment determines what *triggers* it, not what its test ranges over** (K92). The inheritance
above says which requirements bring a rule into play; the test then ranges over what its specialisation names.
A rule stated on one `RequirementDefinition` may look for requirements produced under another, anywhere in the
specialisation tree of definitions, since searching only the owner's own subtree would find nothing in any
project. Among requirements, a `CompletenessRule` looks only at those deriving from the one that triggered it,
not anywhere in the project model (K148). This corrects the reach the `CompletenessRule` subsection below once
stated for its own check (K75, K95).
```

- [ ] **Step 5: `ConflictRule`'s first destination (K141)**

In the paragraph beginning `**What such a rule would have covered is already covered, three ways.**`, replace
`Where two sources disagree about the same thing, the parameter's own ask carries it: the statements are one
requirement, whose value is missing while a `RequirementChoice` between them is open
(`02-requirement-analysis-model.md` §10, §11, K123, K126).` with:

```
Where requirements of one kind contradict about the same thing, the parameter's own ask carries it: the
contradicting statements are separate requirements, and the ask raises a `RequirementChoice` between them
(`02-requirement-analysis-model.md` §10, §11, K139, K141). Two requirements of one kind are produced from one
template, so where they contradict they differ in a parameter's value, and that parameter's ask is always
present.
```

Rewrap.

- [ ] **Step 6: `CompletenessRule` beneath its trigger (K146–K148)**

In the subsection's first paragraph, replace `fires when a `RequirementDefinition` kind is present without an
implied companion kind,` with `fires when a requirement has no requirement of an implied companion kind
deriving from it,`. Rewrap.

Replace the two paragraphs beginning `**The check is set-level, not per-instance.**` and `**What the set-level
reading cannot express, said where a reader will need it.**` with:

```
**The check is made beneath the triggering requirement** (K148). It asks whether at least one **in-force**
`Requirement` produced under the implied `RequirementDefinition`, **or under any specialisation of it**,
**derives from the requirement that triggered the rule**. All three qualifications carry weight. *In force*,
because a retired requirement stays in the model (`02-requirement-analysis-model.md` §10, K5) and does not fill
a gap. *Or any specialisation*, because the definition tree is a kind hierarchy, so a more specific kind
satisfies a more general implication. *Deriving from the triggering requirement*, because a requirement is
elaborated beneath itself, with the client (`01-requirement-model.md` §2, K146): a requirement of the implied
kind beneath one original says nothing about another, and looked for anywhere in the model it would silence the
question for every other. Consequently **at most one `RequirementInquiry` per `Rule` and triggering
`Requirement` is open at a time**, and a requirement deriving from the triggering one discharges it. Whether
only a requirement deriving directly counts, or one deriving through others too, is OQ40. K75 read the check as
set-level to keep a growing model from re-triggering one rule combinatorially; a question per triggering
requirement is work the project actually has, and the model records it.

**Several originals may share what they imply** (K147). Where requirements on several branches each imply the
same thing, one requirement deriving from each of them closes each one's inquiry, and what only one of them
asks for derives from that one alone. Adding a derivation to a requirement that exists does not change that
requirement in place.
```

- [ ] **Step 7: `ValueRule` (K141)**

Replace the subsection's first paragraph (beginning `A `ValueRule` ranges over a single value`) with:

```
A `ValueRule` ranges over one parameter (K115, K141). It fires where the parameter has no value on a
requirement, raising a `RequirementClarification` (`02-requirement-analysis-model.md` §11, K119, K143), and
where requirements of one kind contradict in its value for the same thing, raising a `RequirementChoice` over
them (K139, K141).
```

In the second paragraph, after `...every parameter is covered.`, add `Whether the ask needs to be a `Rule` at
all is OQ38.` In the paragraph beginning `**It carries no *when it applies* and no guard, and it is not
walked.**`, replace everything after that bold sentence with:

```
Its first test reads whether a value is present, which is decided without judgement; its second rests on the
modeller's judgement, made at extraction, that requirements state values for the same thing (K141). Neither is
a relevance judgement (K86), and it fires on incomplete requirements, which no walk reaches (K128, K140). A
contradiction changes no requirement in place: the contradicting statement is a requirement of its own (K139).
```

At the end of the paragraph beginning `**K98's test does not bite here.**`, add `It is also where K98's first
destination now lies (K141).` Rewrap all.

- [ ] **Step 8: The walk and the flowchart**

In the paragraph beginning `**The walk runs once, and a `ValueRule` is not part of it.**`, replace `A
`ValueRule` reads presence and agreement, not relevance, and fires on incomplete requirements the walk never
reaches.` with `A `ValueRule` is no relevance test, and fires on incomplete requirements the walk never
reaches (K141).` Rewrap.

In the flowchart, change node `B` to `B["Walk the RuleSets on that definition and on its ancestors, except their
ValueRules"]`, node `F` to `F{"tests beneath the trigger:<br/>does an in-force Requirement of the implied kind
derive from it?"}`, and node `H` to `H["RequirementInquiry, at most one open per Rule and triggering
Requirement"]`. Mermaid lines are not wrapped.

- [ ] **Step 9: §6**

After the bullet under **Over `CompletenessRule`.**, add:

```
- A `RequirementInquiry` a `CompletenessRule` raises is discharged only by a `Requirement` of the implied kind,
  or a specialisation of it, that derives from the inquiry's triggering requirement (§3, K148).
```

- [ ] **Step 10: Check and commit**

`grep -n "names the source\|source it names\|disagree\|set-level\|at most one open .RequirementInquiry. per .Rule. at a time\|K123\|K126" spec/03-project-lifecycle-model.md`
must print nothing. Re-read §3 whole. `git add spec/03-project-lifecycle-model.md`; message
`Look for an implied kind beneath its trigger, and raise contradictions over requirements (K136, K140, K141, K146-K148)`.

---

### Task 6: `spec/01`, `spec/00`, `spec/05`, `CLAUDE.md` — derivation, origin, edges, and the rule restated (K136, K145–K147, K149)

**Files:** Modify `spec/01-requirement-model.md`, `spec/00-overview.md`, `spec/05-binding-contract.md`,
`CLAUDE.md`.

- [ ] **Step 1: `spec/01` §1**

Replace `and the values, with the sources they name, stay in` with `and the values, with the `SourceNeed`s
stating them, stay in`. Rewrap.

- [ ] **Step 2: `spec/01` §2, derivation (K146, K147)**

Replace the paragraph beginning `A requirement may also be derived from one or more earlier requirements.` (it
ends `...an arbitrary choice among equally contributing ones.`) with:

```
A requirement may also be derived from one or more earlier requirements. A derivation records elaboration
agreed with the client: the derived requirement states something the earlier one asks for, worked out with
the client, and the earlier one implies it, as SysML v2's derivation states (K146). No requirement is refined
or decomposed into others here; that belongs to a design language, beyond the seam. The derivation is an edge
between requirements, list-valued, because several earlier requirements may each, on its own, imply the same
one; a requirement that only several earlier ones together imply is not derived from them (K147). Derivation
admits no cycle, adding one to a requirement that exists does not change that requirement, and it is never a
requirement's origin (below).
```

Keep the paragraph that follows, beginning `**The derivation edge is named at both ends** (K135)`.

- [ ] **Step 3: `spec/01` §2, origin (K145)**

Replace the paragraph beginning `**Every requirement names its origin.**` (it ends `...recorded one document
over.`) with:

```
**Every requirement names its origin, and its origin is what it refines** (K9, K145): the `SourceNeed`s it was
assembled from, by the refinement edge. A derivation from earlier requirements is never an origin, since a
requirement derived from others and refining nothing would carry content nobody answerable stated. The
refinement edge has been projected away; it lives in `02-requirement-analysis-model.md`, where `SourceNeed` is
defined, and that is where the invariant is checked.
```

- [ ] **Step 4: `spec/00` (K136, K149)**

In §1, replace `that every value names the source that states it,` with `that everything entering the model
names the `SourceElement` that states it,`. In the paragraph beginning `The requirement analysis model
projects to the requirement model (K20)`, replace `a requirement's values and the sources they name are
dropped with the rest,` with `a requirement's values are dropped with the rest,`. In the paragraph beginning
`The numbered order of the documents after this one is not incidental`, replace `the values and the sources
they name,` with `its values and the `SourceNeed`s stating them,`. In §4, replace `what source a value must
name,` with `what must state each value,`. Rewrap each.

Replace the paragraph beginning `**Every edge in the collection is an association with two ends** (K135).` with:

```
**Every edge between two requirements is an association with two ends** (K135, narrowed by K149): the
derivation, named *original requirement* and *derived requirement* as SysML v2 names them, and
`supersedes`/`supersededBy`. Leaving force by a decision is an association too, `retires`/`retiredBy`
(K134). How an end is written down, or whether it is stored at all, is notation (K15). Nothing is said here
of other edges.
```

- [ ] **Step 5: `spec/05`, housekeeping (K130)**

In §2, replace `it carries a requirement's identity, its bound wording, its values, and the edge by which one
requirement derives from another — and nothing else.` with `it carries a requirement's identity, its finished
text, and the edge by which one requirement derives from another — and nothing else.` In the subsection `### A
fourth declaration, withdrawn`, replace `and a value's source stays in the working model,` with `and what states
each value stays in the working model,`. Rewrap both.

- [ ] **Step 6: `CLAUDE.md` (K136)**

In §1, replace `what source a value must name,` with `what must state each value,`. In §3's table, rule 4,
replace `the rule that every value names the source that states it (K125),` with `the rule that everything
entering the model names the `SourceElement` that states it (K125, K136),`. Keep everything else in the row.

- [ ] **Step 7: Check and commit**

`grep -rn "names the source\|sources they name\|source a value\|value's source\|derivation, or both\|its values, and the edge" spec/00-overview.md spec/01-requirement-model.md spec/05-binding-contract.md CLAUDE.md`
must print nothing. `git add spec/00-overview.md spec/01-requirement-model.md spec/05-binding-contract.md CLAUDE.md`;
message `Make derivation elaboration and never an origin, narrow edges to requirements, and restate the rule (K136, K145-K147, K149)`.

---

### Task 7: `spec/06` — K136–K149, the open questions they touch, OQ38–OQ40

**Files:** Modify `spec/06-decisions.md`.

- [ ] **Step 1: The decisions**

Directly before `## Decisions K51–K54`, insert:

```
## Decisions K136–K149

Taken in [the design record of 2026-10-06 on contradictions and derivation](../docs/superpowers/specs/2026-10-06-contradictions-derivation-and-the-review-of-k115-k135-design.md),
from the owner's review of the integration of K115–K135 before it reached `main`; written in by
[the integration plan of 2026-10-06](../docs/superpowers/plans/2026-10-06-contradictions-and-derivation-integration-plan.md).
K9 is narrowed by K145; K75, K92 and K95 are revised by K148; K98's first destination becomes K141's; K115,
K118 and K120 are revised by K141; K121 by K143; K123 is reversed and K126 replaced by K139; K125 is restated
by K136; K128 is revised by K140; K129's choice by K139; K133's reading by K144; K135 is narrowed by K149.
EventML's D49 is not adopted (K145).

| # | Decision | Reason |
|---|---|---|
```

followed by the fourteen rows K136–K149, copied verbatim, in order, from the record's tables in §2–§6. Each is
one line.

- [ ] **Step 2: The open questions K136–K149 touch**

- After the paragraph beginning `**OQ24 is closed by K123**`, add: `**OQ24 is answered again, the other way, by
  K139** (2026-10-06). Contradicting sources are two requirements, with a choice between them; K123's single
  requirement is reversed.`
- In the section `## Open questions OQ30 and OQ31 — closed`, insert directly under the heading: `**Both are
  closed.** OQ31 dissolved on a false premise; OQ30 is closed by K115–K121, K125 and K126, as revised by
  K136–K143. Both paragraphs below are kept as written when each was taken.` Replace `**Both are high
  priority.**` with `**Both were high priority when raised.**` In the paragraph beginning `**OQ30 is closed by
  K115–K121, K125 and K126.**`, replace `a disagreement as a `RequirementChoice`, each raised by the
  parameter's own ask.` with `a contradiction between requirements of one kind as a `RequirementChoice`
  between them (K139, K141), each raised by the parameter's own ask.`
- After the OQ33–OQ36 table, add two paragraphs: `**OQ35 gains a condition** (K142): a baseline is cut only
  once every choice raised over contradicting requirements is decided. Whether one may be cut over an
  incomplete requirement stays open.` and `**OQ36 dissolves** (K149). Its premise was that every edge is an
  association; K135 was meant over the edges between requirements, and is narrowed to them.`
- In OQ37's comparison table, replace the row beginning `| What derivation means: nothing beyond` with:
  ```
  | What derivation means: elaboration agreed with the client, which the original implies (K146) | Whenever the original requirement is satisfied, every derived requirement is satisfied too | Answered by K146 and K147: each original on its own implies the derived, and a requirement only several together imply is not derived |
  ```

Rewrap each paragraph.

- [ ] **Step 3: OQ38–OQ40**

Directly before `## Status of the founding record's open questions`, insert a section `## Open questions
OQ38–OQ40`, with the line `Raised in [the design record of 2026-10-06 on contradictions and derivation](../docs/superpowers/specs/2026-10-06-contradictions-derivation-and-the-review-of-k115-k135-design.md).`
and a three-row `| # | Question | When answerable |` table copying OQ38, OQ39 and OQ40 from that record's §7
verbatim.

- [ ] **Step 4: Check and commit**

Every new K row is one line; K-numbers run K136–K149 without a gap; OQ numbers run to OQ40.
`git add spec/06-decisions.md`; message
`Add K136-K149 and OQ38-OQ40 to spec/06-decisions.md; re-answer OQ24, close OQ30 cleanly, dissolve OQ36`.

---

### Task 8: `docs/eventml-decisions.md`, `CHANGELOG.md`, and the sweep

**Files:** Modify `docs/eventml-decisions.md`, `CHANGELOG.md`; any file the sweep shows still needs a line.

- [ ] **Step 1: D49's overturn**

In `docs/eventml-decisions.md`, delete the row beginning `| D49 |` from the table under `## Inherited —
depended on, and unchanged`. In the table under `## Overturned`, add after the `D46` row:

```
| D49 | Every requirement names its origin — refinement, derivation, or both; carrying neither is an incomplete record rather than a root | **K145** — every requirement refines at least one need, and a derivation is never an origin. EventML did not yet separate analysing a requirement from elaborating it beyond the seam, so a requirement derived from others alone could stand there; here its content would have nobody answerable for it |
```

Replace `Two EventML decisions are contradicted by ProjectML decisions.` with `Three EventML decisions are
contradicted by ProjectML decisions.` and `The two overturns are one move made twice.` with `The overturns of
D31 and D46 are one move made twice.` Leave D32 where it is: whether it stands is the audit the record names,
out of this plan's scope.

- [ ] **Step 2: The entry** — at the end of `## [Unreleased]` / `### Added` in `CHANGELOG.md`:

```
- Contradictions as requirements, derivation as elaboration, from the owner's review of K115–K135. A value
  reaches a requirement only through the `SourceNeed`s it refines. Contradicting needs are separate
  requirements under one choice, which the modeller flags and the project manager settles by the time a
  baseline is cut — keeping one, or both where the contradiction is not real; nothing is merged. Leaving force
  is final. Every requirement refines a need; derivation is elaboration agreed with the client and never an
  origin, and a completeness rule looks for its implied kind beneath the requirement that triggered it. K136–
  K149 record the decisions; OQ36 dissolves, OQ24 is answered again, and OQ38–OQ40 open. Findings are in
  [`docs/superpowers/specs/2026-10-06-contradictions-derivation-and-the-review-of-k115-k135-design.md`](docs/superpowers/specs/2026-10-06-contradictions-derivation-and-the-review-of-k115-k135-design.md).
```

- [ ] **Step 3: The sweep**

Run each, from the repository root, and resolve every hit that is not historical:

```
grep -rn "names the source\|name the source\|sources they name\|source it names\|value names" spec bindings CLAUDE.md README.md
grep -rn -i "disagree" spec bindings
grep -rn "K123\|K126" spec bindings
grep -rn "derivation, or both\|neither refinement nor derivation" spec bindings
grep -rn "set-level\|in the project model\*\*" spec/03-project-lifecycle-model.md
grep -rn "Every edge is an association\|Every edge in the collection" spec CLAUDE.md
```

Allowed: rows and paragraphs of `spec/06` that record earlier decisions as taken; K123 and K126 named as
reversed or replaced. Anything else gets a one-line fix in the file it is in, citing the K-number that settled
it.

- [ ] **Step 4: Commit** — `git add docs/eventml-decisions.md CHANGELOG.md` and the files Step 3 touched;
message `Record D49's overturn and K136-K149 in CHANGELOG, and sweep the remaining references`.
