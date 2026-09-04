# Integrate `Rule`'s shape and role (K82–K89) into `spec/` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Write K82–K89 into `spec/` — what a `Rule` is for, the shape that follows from it, the corrected
account of how matching actually works, the boundary on a rule's own provenance, and the narrowing of K77's
findings-family classification — closing OQ20 and opening OQ21–OQ23. This is prose and diagrams, not code: a
"test" for each task is a careful re-read for internal consistency, cross-reference correctness, and
conformance to this repository's own house rules (`CLAUDE.md`), not a runnable suite.

**Architecture:** `spec/03-project-lifecycle-model.md` §3 carries most of the change — its `RuleSet`
subsection gains K82's corrected cardinality, its `Rule` subsection gains a role statement and an attribute
table (with the section's Mermaid diagram updated to match), its matching subsection is rewritten and
renamed, and a new subsection states what a `Rule` does not carry.
`spec/02-requirement-analysis-model.md` §11 has two paragraphs rewritten (the origin discussion and the
findings classification) and §12 gains one constraint. `spec/06-decisions.md` gains K82–K89, notes the three
revisions they make, closes OQ20 and opens OQ21–OQ23.

**Tech Stack:** Markdown, Mermaid diagrams (GitHub-rendered), git.

## Global Constraints

- **English only, no exception** (CLAUDE.md §5). Every sentence written by this plan is English.
- **No notation, no filled `RequirementDefinition`, nothing executable** (CLAUDE.md §1). In particular, K85
  deliberately refuses to say how a composed identifier is written down — spelling one out would be notation.
  No task may add an example identifier, and no task may name a requirement kind (K15).
- **Where a diagram and the prose beside it disagree, the prose wins** (CLAUDE.md §5). Task 2 updates
  `spec/03` §3's Mermaid diagram alongside the prose it draws; an earlier plan in this repository shipped a
  diagram that contradicted its own prose because the brief scoped only backtick-quoted text, and the
  reviewer caught it — do not repeat that.
- **Everything this plan writes is already decided**, in
  [`docs/superpowers/specs/2026-09-04-rule-shape-design.md`](../specs/2026-09-04-rule-shape-design.md)
  (K82–K89, closing OQ20, opening OQ21–OQ23). This plan cites decisions by number rather than re-arguing
  them.
- **Three earlier decisions are revised, and each revision is stated openly rather than applied silently**:
  K82 revises K68's cardinality; K86 revises K76's *description* of the matching mechanism but not its
  verdict; K89 narrows K77. The original rows in `spec/06-decisions.md` stay as taken — a decision record
  keeps its history, on the same terms K34 is kept beside K35 — and Task 8 adds the note that points at the
  revisions.
- **Commit after every task. Never push** (CLAUDE.md §3, house rule 11).
- **What this plan does not do.** It does not give the `Rule` specialisations any shape of their own —
  `ConflictRule` and `CompletenessRule` keep exactly the text they have today, and what names the companion
  kind a `CompletenessRule` looks for stays unsettled, waiting for a session of its own alongside OQ18. No
  task may add an attribute to either specialisation.

---

### Task 1: `spec/03` §3 — `RuleSet`'s corrected cardinality (K82)

**Files:**
- Modify: `spec/03-project-lifecycle-model.md` — the `### \`RuleSet\`` subsection's opening paragraph

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: the "exactly one, possibly empty" reading that Task 8's K82 entry records.

- [ ] **Step 1: Replace the opening paragraph**

Replace:

```
A **`RuleSet`** belongs to a `RequirementDefinition` — zero or one per `RequirementDefinition` — not
to a project as a whole (K68). It gathers the `Rule`s stated over that `RequirementDefinition`
specifically. There is no reification of "everything a project has loaded" as an element of its own.
```

with:

```
**Exactly one `RuleSet` belongs to each `RequirementDefinition`, and it may be empty** (K82) — not one per
project as a whole (K68). It gathers the `Rule`s stated over that `RequirementDefinition` specifically.
There is no reification of "everything a project has loaded" as an element of its own.

Making it exactly one rather than zero-or-one removes a distinction that carries no meaning: a
`RequirementDefinition` with no rules stated over it and one holding an empty `RuleSet` are the same state
of affairs, and modelling them apart would add a null case to every reading of the structure without
buying anything. What is left is a `RuleSet` that is in effect a property of the `RequirementDefinition`,
modelled separately because K22 makes a rule-set a model in its own right.
```

- [ ] **Step 2: Confirm the rest of the subsection still reads correctly**

The two paragraphs that follow — the "natural unit, on two grounds" argument and the K69 inheritance
paragraph — are K68's and K69's own reasoning and are untouched by this change. Re-read them once to confirm
neither now contradicts "exactly one" (neither does: both argue about *where* a `RuleSet` attaches, not
whether one exists).

- [ ] **Step 3: Commit**

```bash
git add spec/03-project-lifecycle-model.md
git commit -m "$(cat <<'EOF'
Correct RuleSet's cardinality to exactly one per RequirementDefinition (K82)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: `spec/03` §3 — the `Rule`'s role and shape (K83, K84, K85)

**Files:**
- Modify: `spec/03-project-lifecycle-model.md` — the `### \`Rule\`` subsection, and §3's Mermaid diagram

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: the attribute names `when it applies` and `what to consider`, which Task 3's rewritten matching
  subsection refers to by name, and which Task 8's K84 entry records.

- [ ] **Step 1: Insert the role statement and attribute table at the head of the subsection**

Replace:

```
### `Rule`

`Rule` is **abstract**. Its specialisations divide by **mechanism** — what happens when the rule fires — not
```

with:

```
### `Rule`

**A `Rule` directs attention; it does not prescribe an outcome** (K83). It states which subjects must be
dealt with when a requirement arises under the `RequirementDefinition` it hangs on — never what the
resulting requirement should say. This is what keeps a rule-set from quietly becoming a second definition
layer: a `RequirementDefinition` says what a requirement of some kind looks like, and section 1 already
places what a requirement's wording should be outside a rule-set's territory entirely.

**A negative answer to a subject a rule raises is a full answer.** Where a project decides it needs nothing
in the subject raised, that decision appears as a `Requirement` like any other, and the `RequirementInquiry`
`discharges` to it; while no such `Requirement` exists, the question stands open, which is
`02-requirement-analysis-model.md` §11's existing rule rather than a new one (K79). That closing
`Requirement` is produced the ordinary way — a source, a `SourceNeed`, `refine` — never by the question
itself, so the commitment still enters through a source (K11) and the modeller's own instrument does not
commit the project. A subject raised and declined therefore leaves a record, and a later reader asking why
some subject carries no requirement finds an answer rather than silence.

`Rule` is **abstract**, and carries three things.

| Attribute | Carries |
|---|---|
| identity | Local to the `RequirementDefinition` that owns it; the full identifier is the composition of the two (K85) |
| when it applies | One sentence stating when this rule is relevant. Prose, not an evaluable expression, on the same terms `02-requirement-analysis-model.md` §7's own *when it applies* is prose (D20) |
| what to consider | The subject this rule raises: what has to be dealt with, never what the answer should be (K83, K84) |

**The two prose fields are split so that the judgement below reads one of them rather than the whole rule.**
Neither form is coined: a `RequirementDefinition` already carries a *when it applies* on exactly these terms,
and a *what to ask* that raises a missing parameter where *what to consider* raises a missing subject, one
level up. House rule 10 is met by adopting this collection's own established forms rather than inventing a
third.

**The identity is local because the attachment already is.** A `Rule` is reachable only through the
`RuleSet` of the `RequirementDefinition` that owns it, so an identifier unique beneath that owner is unique
in the model. **How the composition is written down is not fixed here**: spelling out a composed identifier
would be notation, which K15 excludes, and an implementation's identifier space is exactly what
`05-binding-contract.md`'s fourth declaration already leaves to it (K4).

Its specialisations divide by **mechanism** — what happens when the rule fires — not
```

- [ ] **Step 2: Update §3's Mermaid diagram to draw the three attributes**

Replace:

```
classDiagram
    class Rule {
        <<abstract>>
    }
    Rule <|-- ConflictRule
    Rule <|-- CompletenessRule
```

with:

```
classDiagram
    class Rule {
        <<abstract>>
        identity
        when it applies
        what to consider
    }
    Rule <|-- ConflictRule
    Rule <|-- CompletenessRule
```

Do not add attributes to `ConflictRule` or `CompletenessRule` — they have none, and giving them any is
explicitly outside this plan.

- [ ] **Step 3: Verify the diagram against the prose**

Confirm the diagram draws exactly what the prose states: `Rule` abstract with the three attributes from the
table, two named specialisations with nothing extra. Confirm the paragraph immediately below the diagram —
*"The diagram draws what this section states; where the two disagree, the prose wins"* — is now true of the
updated diagram.

- [ ] **Step 4: Commit**

```bash
git add spec/03-project-lifecycle-model.md
git commit -m "$(cat <<'EOF'
State what a Rule is for and the shape that follows (K83, K84, K85)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: `spec/03` §3 — rewrite the matching subsection (K86)

**Files:**
- Modify: `spec/03-project-lifecycle-model.md` — the `### Matching a \`Rule\`'s condition` subsection,
  including its heading

**Interfaces:**
- Consumes: `when it applies` (Task 2), by name.
- Produces: the corrected account of matching that Task 6's findings classification refers to.

- [ ] **Step 1: Replace the whole subsection, heading included**

Replace:

```
### Matching a `Rule`'s condition

**Whether a `Rule`'s free-text condition holds of a free-text `Requirement` is a semantic constraint (K24),
not a syntactic one** (K76). The metamodel does not guarantee this runs exhaustively or automatically; it is
carried out by judgement, human or AI, on the same terms `00-overview.md` §5 and
`02-requirement-analysis-model.md` §11 already hold extraction completeness to (K40, K41). Both a `Rule`'s
condition and a `Requirement`'s wording are prose; nothing decides whether one matches the other without
reading content.
```

with:

```
### Walking a `RuleSet`

**A `RuleSet` is a written procedure, and matching is a relevance judgement made while walking it** (K86).
When a new requirement arises in a subject, the `RuleSet`s that reach it are walked, and a reader — human or
AI — judges which entries are relevant by reading each rule's *when it applies*. This is not the evaluation
of a condition for its truth value against a requirement, which is how K76 first described it; that
description is corrected here, its verdict is not.

**The verdict stands: this is a semantic constraint (K24), not a syntactic one.** The meaning of free text is
matched against the meaning of free text, which no conventional algorithm decides. The metamodel does not
guarantee the walk runs exhaustively or automatically; it is carried out by judgement, on the same terms
`00-overview.md` §5 and `02-requirement-analysis-model.md` §11 already hold extraction completeness to (K40,
K41).

**Which `RuleSet`s reach a requirement needs no new concept.** A `RuleSet` hangs on a
`RequirementDefinition`, and the `RequirementDefinition` hierarchy is the subject hierarchy — so the rules
relevant in a subject are exactly those on that `RequirementDefinition` and its ancestors, which is the walk
the rule above already defines (K69). Nothing here adds a notion of *subject* beside the one the
specialisation tree already carries.
```

- [ ] **Step 2: Check for references to the old heading**

```bash
grep -rn "Matching a" spec/
```

Expected: no output. If any document cited the old subsection by its heading text, it must now cite
*"Walking a `RuleSet`"* instead — fix any hit found before committing.

- [ ] **Step 3: Commit**

```bash
git add spec/03-project-lifecycle-model.md
git commit -m "$(cat <<'EOF'
Correct how rule-matching actually works: a walk, not a condition evaluation (K86)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: `spec/03` §3 — what a `Rule` does not carry (K88)

**Files:**
- Modify: `spec/03-project-lifecycle-model.md` — insert a new subsection at the end of §3, immediately before
  the `## 4. What the metamodel does not do` heading

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: the provenance boundary that Task 5's `spec/02` §11 rewrite cites when it explains why
  *triggered by* is mandatory.

- [ ] **Step 1: Insert the new subsection**

Insert, after the `### Walking a \`RuleSet\`` subsection Task 3 produced and before `## 4. What the metamodel
does not do`:

```markdown
### What a `Rule` does not carry

**A `Rule` carries no provenance: this model does not record what produced a rule or who approved it**
(K88). This is K46's boundary seen from the other side. K46 stops provenance at the source because the
metamodel cannot see the procedures behind a stakeholder's words; a rule-set is one of those procedures —
an organisation's or a project manager's own way of working — and its own origin is outside what this model
can see. Recording a rule's authorship would model the organisation rather than the project.

**The chain does not break where it matters.** A `Rule` commits nothing and decides nothing; it raises a
question. When the project manager answers that question, the answer enters as a source and runs through
K11 like every other commitment, so everything that changes the requirement model still names its origin.

**Amending the procedure is the project manager's act.** A modeller who finds that no rule covers something
proposes a rule rather than working around the gap; adding one commits the project to checking it, and
anything that commits the project comes from the project manager and reaches the model as a source (K11).
The rule-set can be amended — it cannot be departed from, which is why
`02-requirement-analysis-model.md` §11 makes a `RequirementQuestion`'s *triggered by* mandatory (K87).
```

- [ ] **Step 2: Confirm §3's shape**

§3 now runs: the diagram, `### RuleSet`, `### Rule`, `### ConflictRule`, `### CompletenessRule`,
`### Walking a RuleSet`, `### What a Rule does not carry`. Confirm the new subsection sits last, before
`## 4.`, and that `### ConflictRule` and `### CompletenessRule` were not touched by any task so far.

- [ ] **Step 3: Commit**

```bash
git add spec/03-project-lifecycle-model.md
git commit -m "$(cat <<'EOF'
State that a Rule carries no provenance, on K46's boundary (K88)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: `spec/02` §11 — every `RequirementQuestion` comes from a `Rule` (K87)

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` §11 — the paragraph beginning *"**Where a
  `RequirementQuestion` comes from is now settled for two mechanisms and open for the rest.**"*

**Interfaces:**
- Consumes: `03-project-lifecycle-model.md` §3's provenance boundary (Task 4), cited by reference.
- Produces: the mandatory-origin statement that Task 7's syntactic constraint restates in one line.

- [ ] **Step 1: Replace the paragraph**

Replace:

```
**Where a `RequirementQuestion` comes from is now settled for two mechanisms and open for the rest.** A
`RequirementDefinition`'s own *what to ask* (§7) already covers a single missing parameter. A conflict
between a new `Requirement` and an existing in-force one is `03-project-lifecycle-model.md` §3's
`ConflictRule`, raising a `RequirementChoice`. A `Requirement` whose kind implies that another kind should
also exist is that section's `CompletenessRule`, raising a `RequirementInquiry` — this was OQ17's own
original case, now answered. Two further rule-set statements — whether a silent default must be owned, and
when a gap's wait becomes a decision — do not yet have a worked mechanism, and whether either raises a
`RequirementQuestion` the same way, or needs something structurally different, is recorded as OQ18 in
`06-decisions.md`.
```

with:

```
**Every `RequirementQuestion` comes from a `Rule`, without exception** (K87). Its *triggered by* is never
absent, because a rule-set is the procedure a project works to: it can be amended, but it cannot be departed
from. A modeller who finds that no rule covers something proposes a rule — adding one is the project
manager's act, since it commits the project (`03-project-lifecycle-model.md` §3) — rather than raising a
question outside the procedure. This is the strongest available reading of the rule that every event record
its cause: a `RequirementQuestion`'s cause is not merely guaranteed to exist, it is named.

**A `RequirementDefinition`'s own *what to ask* (§7) is not a second origin.** It covers a single missing
parameter through the definition's own machinery, which is why that case raises no `RequirementQuestion` at
all.

**Two mechanisms are worked out, and the rest are open.** A conflict between a new `Requirement` and an
existing in-force one is `03-project-lifecycle-model.md` §3's `ConflictRule`, raising a `RequirementChoice`.
A `Requirement` whose kind implies that another kind should also exist is that section's `CompletenessRule`,
raising a `RequirementInquiry` — this was OQ17's own original case, now answered. Two further rule-set
statements — whether a silent default must be owned, and when a gap's wait becomes a decision — do not yet
have a worked mechanism, and whether either raises a `RequirementQuestion` the same way, or needs something
structurally different, is recorded as OQ18 in `06-decisions.md`.
```

- [ ] **Step 2: Confirm the attribute table above still reads correctly**

§11's `RequirementQuestion` attribute table describes *triggered by* as *"A reference to the `Rule`
(`03-project-lifecycle-model.md` §3) that fired and produced it"*. Confirm it is untouched and now agrees
with the paragraph just written — it does; K87 states without exception what that row already implied.

- [ ] **Step 3: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
State that every RequirementQuestion names the Rule that produced it (K87)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 6: `spec/02` §11 — narrow K77's findings classification (K89)

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` §11 — the paragraph beginning *"**`RequirementQuestion` —
  both specialisations — belongs to the *review finding* family**"*

**Interfaces:**
- Consumes: Task 3's corrected account of walking a `RuleSet`, cited by reference.
- Produces: OQ23's motivating observation (that the review act is unstated), which Task 8 records as an open
  question.

- [ ] **Step 1: Replace the paragraph**

Replace:

```
**`RequirementQuestion` — both specialisations — belongs to the *review finding* family in the findings table
below, not to the *failed check* or *question* rows** (K77). It is judged by whether a `Rule`'s condition
holds (`03-project-lifecycle-model.md` §3, K76), it is modelled, and it carries state (K60) — the three
properties that table already uses to seat *review finding* apart from the other two. Nothing about
`RequirementQuestion` changes because of this: it is a naming of what it already is, for a reader who reaches
the table below looking for where it fits.
```

with:

```
**`RequirementQuestion` is not a *review finding*, and belongs to no row of the findings table below** (K89,
narrowing K77). It does share the three properties that table uses to seat a review finding apart from the
other two — it is judged, it is modelled, and it carries state (K60) — but the table classifies what a
**review** produces over this model (K10), and walking a rule-set is ordinary modelling work performed when a
requirement arises, not a separate act of review.

The table's own rules confirm the separation rather than merely failing to fit it. A review finding *"is
opened by a source"*, where a `RequirementQuestion` is raised by a `Rule` firing over the model; and
*"nothing marks a finding closed directly"*, where `discharges` does exactly that.

**Three checking modes exist, and only two had names before this.** Static model checking decides without
judgement and produces a failed check or a question, recomputed rather than modelled. Walking a `RuleSet`
takes judgement and produces a `RequirementQuestion` (`03-project-lifecycle-model.md` §3, K86). A review
takes judgement and produces a review finding. K77 saw only the first distinction — judgement or none — and
so placed `RequirementQuestion` with review findings on the strength of the three shared properties. What a
review *is*, as an act, this document still does not state; that gap is recorded as OQ23 in
`06-decisions.md`.
```

- [ ] **Step 2: Confirm the findings table below is untouched**

The three-row table (*failed check*, *question*, *review finding*) and every rule stated under it are
unchanged by this task — K89 removes `RequirementQuestion` from the family rather than altering the family.
Confirm no row and no following rule was edited.

- [ ] **Step 3: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Narrow K77: RequirementQuestion is not a review finding but a third mode's product (K89)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 7: `spec/02` §12 — the constraint over `triggered by`

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` §12 — the **"Over `RequirementQuestion`,
  `RequirementInquiry`, and `RequirementChoice`."** block

**Interfaces:**
- Consumes: K87 (Task 5).
- Produces: nothing later tasks depend on.

- [ ] **Step 1: Insert one bullet**

In that block, insert this bullet immediately after the *"No element is a `RequirementQuestion` and nothing
more..."* bullet and before the *"A `RequirementQuestion` carries exactly one of "raised" or "posed"..."*
bullet:

```markdown
- A `RequirementQuestion` names exactly one `Rule` as the origin that produced it. One naming none is not a
  well-formed element of this model, on the same footing as a `RequirementDecision` naming no `SourceDecision`
  (§11, `03-project-lifecycle-model.md` §3, K61, K87).
```

- [ ] **Step 2: Re-read the block**

Confirm the block now reads: identity uniqueness, the abstract-type constraint, the new origin constraint,
the raised/posed constraint, the `discharges` typing constraint, and the at-most-one-open-`RequirementInquiry`
constraint — six bullets, no duplication, nothing else in §12 touched.

- [ ] **Step 3: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Add the syntactic constraint over a RequirementQuestion's mandatory origin (K87)

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 8: `spec/06-decisions.md` — add K82–K89, close OQ20, open OQ21–OQ23

**Files:**
- Modify: `spec/06-decisions.md` — insert a decisions section after the existing `## Decisions K66–K81`
  section; add a revision note to that section; rewrite the `## Open question OQ20` section's heading and
  lead-in; add an open-questions section

**Interfaces:**
- Consumes: K82–K89 and OQ21–OQ23 as the design record states them.
- Produces: nothing later tasks depend on.

- [ ] **Step 1: Add the revision note to the `## Decisions K66–K81` section**

That section's lead-in currently ends with the sentence *"K74 and K78 together answer OQ17's own question;
the plan narrows OQ17 to what was only ever gathered beside it, and opens OQ18–OQ19."* Insert immediately
after it, in the same paragraph block:

```markdown
**Three of these rows have since been revised, and stand here as taken.** K68's cardinality is corrected by
K82; K76's description of the matching mechanism — not its verdict — by K86; and K77's classification is
narrowed by K89. All three are in
[the design record of 2026-09-04 on `Rule`'s shape](../docs/superpowers/specs/2026-09-04-rule-shape-design.md).
A decision record keeps its history, on the same terms K34 is kept beside K35.
```

- [ ] **Step 2: Insert the new decisions section**

Insert, immediately after the `## Decisions K66–K81` section (that is, after its table and the note Step 1
added) and before the `## Decisions K51–K54` section, which is what currently follows it — this file's
section order is already non-monotonic, and this placement keeps each decisions block beside the record it
came from, matching how K66–K81 itself sits before K51–K54:

```markdown
## Decisions K82–K89

Taken in [the design record of 2026-09-04 on `Rule`'s shape](../docs/superpowers/specs/2026-09-04-rule-shape-design.md),
which carries the full argument for each. Written into `spec/` by
[the integration plan of 2026-09-04](../docs/superpowers/plans/2026-09-04-rule-shape-integration-plan.md).
Together they close OQ20 and open OQ21–OQ23. K82 revises K68, K86 revises K76's description of the mechanism
but not its verdict, and K89 narrows K77.

| # | Decision | Reason |
|---|---|---|
| K82 | Exactly one `RuleSet` belongs to each `RequirementDefinition`, and it may be empty. Revises K68, which made it zero or one | The distinction between no `RuleSet` and an empty one carries no meaning, and removing it removes a null case from every reading of the structure |
| K83 | A `Rule` directs attention rather than prescribing an outcome: it states which subjects must be dealt with, never what the resulting requirement should say. A negative answer to a subject it raises is a full answer, appearing as a `Requirement` the `RequirementInquiry` discharges to; while none exists the question stands open (K79). That closing `Requirement` is produced the ordinary way, never by the question itself | A rule that prescribed content would state what a requirement's wording should be, which `03-project-lifecycle-model.md` §1 already puts outside a rule-set's territory and K66 places on `RequirementDefinition`. Keeping the closing `Requirement` on the ordinary route protects K11 and the actor rule at once, and leaves a record for a subject raised and declined |
| K84 | A `Rule` carries an identity, a *when it applies*, and a *what to consider*. Both prose fields, neither an evaluable expression | The split lets the relevance judgement read one field rather than the whole rule. Neither form is coined: `RequirementDefinition` already carries a *when it applies* on these terms (D20), and a *what to ask* raising a missing parameter where *what to consider* raises a missing subject |
| K85 | A `Rule`'s identity is local to the `RequirementDefinition` that owns it; the full identifier is the composition of the two. How that composition is written down is not fixed | Locality follows the structural attachment K68 establishes. Refusing to fix the spelling is K15 holding — a composed identifier written out would be notation — and an identifier space is what K4's fourth declaration leaves to an implementation |
| K86 | Rule-matching is a relevance judgement made while walking a `RuleSet`, not the evaluation of a condition against a `Requirement`. The semantic classification K76 gives is unchanged; its description of the mechanism is corrected | A rule-set is a written procedure walked when a requirement arises in its subject, and a reader judges which entries are relevant. The correction also shows *subject* needs no new concept: the `RequirementDefinition` hierarchy is the subject hierarchy, so the rules relevant in a subject are those K69's inheritance already reaches |
| K87 | A `RequirementQuestion`'s *triggered by* is mandatory, without exception. `spec/02` §11's wording, which read a `RequirementDefinition`'s *what to ask* as a second origin, is corrected | The rule-set is the procedure: amendable, but not departable-from. A modeller who finds no rule covering something proposes a rule, and proposing is as far as the modeller goes, since adding one commits the project and is the project manager's act. Every `RequirementQuestion`'s cause is therefore named, not merely guaranteed |
| K88 | A `Rule` carries no provenance. The metamodel does not record what produced a rule or who approved it | K46's boundary from the other side: a rule-set is one of the procedures K46 says the metamodel cannot see behind. The chain does not break where it matters — a rule commits nothing and decides nothing, and the project manager's answer to the question it raises enters as a source under K11 |
| K89 | A `RequirementQuestion` is not a *review finding* and belongs to no row of `spec/02` §11's findings table. It is the product of a third checking mode: static model checking needs no judgement, walking a `RuleSet` does and produces a `RequirementQuestion`, review does and produces a review finding. Narrows K77 | The table classifies what a *review* produces (K10), and walking a rule-set is ordinary modelling work, not an act of review. The table's own rules confirm it: a review finding is opened by a source where a `RequirementQuestion` is raised by a rule firing, and nothing marks a finding closed directly where `discharges` does. K77 saw only the judgement/no-judgement distinction |
```

- [ ] **Step 3: Close OQ20**

Change the heading `## Open question OQ20` to `## Open question OQ20 — answered`, and insert immediately
below it, before the existing *"Raised in this session's own whole-branch review..."* paragraph:

```markdown
**Answered by K82–K89, and no longer open.** Its three parts were answered, but not in the form they were
asked — two corrections reframed the question first. `Rule`'s attributes are K84's three plus K85's
locality, reachable only once K83 settled what a rule is *for*. *triggered by* turned out to need no
optional form at all: K87 makes it mandatory without exception, because a rule-set is a procedure that can
be amended but not departed from. And K77's classification was not qualified but narrowed — K89 finds that
`RequirementQuestion` belongs to no row of the findings table, being the product of a third checking mode
the table never named. The question is kept below as it was asked.
```

Leave the three descriptive paragraphs and the question table that follow exactly as they are — they record
the gaps as they were found, which a decision record keeps.

- [ ] **Step 4: Add the new open questions**

Insert, immediately after the OQ20 section Step 3 amended:

```markdown
## Open questions OQ21–OQ23

Raised in [the design record of 2026-09-04 on `Rule`'s shape](../docs/superpowers/specs/2026-09-04-rule-shape-design.md),
which carries the full argument for each.

| # | Question | When answerable |
|---|---|---|
| OQ21 | Should a `Rule` carry parameter criteria, letting an algorithmic filter run before the semantic judgement? The shape is a guard, as a flowchart uses the word: it could *exclude* a rule mechanically, never admit one, so the judgement K86 describes still decides everything reaching it — a narrowing of what reaches judgement, not a third category beside K24's two. Half the machinery exists: a `RequirementDefinition` declares `parameters` and a `Requirement` carries `values`. The `Rule`-side criterion and the join between them are missing | When the cost of judging every rule in a walked `RuleSet` is actually felt, which needs an implementation running the loop. Until then it is an unexercised construct and waits |
| OQ22 | How does a fired rule become a posed question? A rule detects that a subject needs dealing with; the modeller writes the question. Whether one firing yields one question or several, whether *what to consider* supplies a template for the wording, and whether K75's "at most one open `RequirementInquiry` per `Rule`" is general or specific to `CompletenessRule`, are all unstated | With the `Rule` specialisations, since the last part is a question about one of them |
| OQ23 | What is a review, as an act? `spec/02` §11 states a review finding's lifecycle — a source opens it, a later source that `replies` to it closes it — but nothing states the act producing one: who performs it, when, against what. Only static model checking and, since K86, walking a `RuleSet` are worked out | Unforced. Recorded because K89 makes the gap visible while deliberately not entering it |
```

- [ ] **Step 5: Verify K-number and OQ-number continuity**

```bash
grep -n "^| K8[2-9] " spec/06-decisions.md
grep -n "^| OQ2[0-3] " spec/06-decisions.md
```

Expected: K82 through K89 each appear exactly once; OQ20 through OQ23 each appear exactly once. Confirm also
that no K-number between K19 and K89 is missing or duplicated, other than K55, which was never transcribed
into this file and is a pre-existing gap this plan does not fix.

- [ ] **Step 6: Commit**

```bash
git add spec/06-decisions.md
git commit -m "$(cat <<'EOF'
Add K82-K89 to spec/06-decisions.md; close OQ20; open OQ21-OQ23

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 9: `CHANGELOG.md` — record the change

**Files:**
- Modify: `CHANGELOG.md`

**Interfaces:**
- Consumes: Tasks 1–8's completion.

- [ ] **Step 1: Add one new bullet**

Insert, under `## [Unreleased]` → `### Added`, immediately after the current last bullet:

```markdown
- `Rule` given a shape and, first, a purpose: it directs attention rather than prescribing an outcome,
  carrying an identity local to the `RequirementDefinition` that owns it, a *when it applies* and a *what to
  consider*. Matching is corrected to what it actually is — a relevance judgement made while walking a
  `RuleSet`, which is a written procedure — leaving K76's semantic classification intact and its description
  replaced. Every `RequirementQuestion` now names the `Rule` that produced it without exception, and
  `RequirementQuestion` is removed from the *review finding* family: it is the product of a third checking
  mode, between static checking and review. K82–K89 record the decisions; OQ20 is closed, OQ21–OQ23 opened.
  Findings are in
  [`docs/superpowers/specs/2026-09-04-rule-shape-design.md`](docs/superpowers/specs/2026-09-04-rule-shape-design.md).
```

- [ ] **Step 2: Commit**

```bash
git add CHANGELOG.md
git commit -m "$(cat <<'EOF'
Record the Rule shape integration in CHANGELOG

Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 10: Final cross-file consistency pass

**Files:**
- Read only: every file in `spec/`, `CLAUDE.md`, `CHANGELOG.md`

**Interfaces:**
- Consumes: every file Tasks 1–9 touched.
- Produces: nothing new — this task only verifies.

- [ ] **Step 1: Confirm no stale reference to the old matching subsection or the old K77 classification**

```bash
grep -rn "free-text condition" spec/
grep -rn "review finding family\|review finding\* family" spec/
```

Expected: no hit describes `Rule`-matching as a condition evaluation, and no hit places `RequirementQuestion`
in the review-finding family. Hits inside `spec/06-decisions.md`'s K76 and K77 rows are correct and must
stay — those rows record the decisions as taken, and Task 8 Step 1's note points at their revisions.

- [ ] **Step 2: Confirm §3's cross-references still resolve**

`spec/03-project-lifecycle-model.md` §3 now carries seven subsections. Search the whole repository for
citations into `03-project-lifecycle-model.md` §3 and confirm each still points at content that exists —
`spec/02` §11 cites it several times, and Task 5 and Task 6 added more.

- [ ] **Step 3: Confirm both Mermaid diagrams match their prose**

Read `spec/03` §3's diagram against the `### Rule` attribute table (three attributes, two specialisations,
nothing on the specialisations), and `spec/02` §11's `RequirementQuestion` diagram against its own prose
(unchanged by this plan — confirm no task touched it).

- [ ] **Step 4: Confirm the specialisations were not given a shape**

```bash
grep -n "ConflictRule\|CompletenessRule" spec/03-project-lifecycle-model.md
```

Read each hit. Confirm neither specialisation gained an attribute, an attribute table, or a typed reference
to an implied `RequirementDefinition` — that question is explicitly deferred, and a task drifting into it
would be scope creep this plan forbids.

- [ ] **Step 5: Read `spec/03` §3 and `spec/02` §11 in full, once, start to end**

Confirm each reads as one coherent argument rather than eight patches: that K83's role statement leads
naturally into K84's attributes, that the corrected matching subsection agrees with the attribute names, and
that §11's origin paragraph and findings paragraph do not contradict each other or the findings table below
them.

- [ ] **Step 6: Commit, only if Steps 1–5 found something to fix**

If nothing needed fixing, this task produces no commit — its deliverable is the confirmation itself. If a fix
was needed, commit it with a message naming what task and what line it corrects.
