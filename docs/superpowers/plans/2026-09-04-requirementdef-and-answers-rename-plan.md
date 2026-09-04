# Rename `RequirementDef`→`RequirementDefinition` and the `answers` edge→`replies` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Execute two already-decided renames, tracked outside `spec/` in this session's own memory rather
than as open design questions: `RequirementDef` → `RequirementDefinition` (to match SysML v2's own term rather
than an abbreviation of it), and the `Source` `answers` edge → `replies` (a likely translation artefact —
Hungarian *"válaszol"* renders more accurately as *"replies to"* than *"answers,"* which implies a posed
question where the edge in fact covers any later source responding to an earlier one, disagreement included).
Neither is a design decision — no new `K` number is taken by this plan — both are pure terminology passes,
on the scale of the `Need`→`SourceNeed` rename already done once.

**Architecture:** Two independent rename sweeps, each touching a different, precisely-scoped set of files.
`RequirementDef`→`RequirementDefinition` is a lossless, position-preserving token substitution — every
occurrence of the type name changes the same way, with one exception (two places already read
`RequirementDefinition`, quoting SysML v2's own vocabulary directly, and must not be touched). The `answers`
edge rename is not a blind substitution: the English word "answer(s)/answered/answering" appears constantly
in this corpus with no relation to the edge (open-question bookkeeping, generic prose), and only a small,
precisely identified set of occurrences actually name or describe the `Source`↔`Source` edge itself. Each of
those gets an exact, individually-judged replacement.

**Tech Stack:** Markdown, git. No code, no notation, no filled definitions — this plan writes and rewrites
prose only.

## Global Constraints

- **English only, no exception** (CLAUDE.md §5).
- **Scope boundary, matching this repository's own established precedent**: `docs/2026-08-26-kernel-brainstorm.md`
  (the founding record), `docs/eventml-decisions.md` (a standing reference, grouped with the founding record
  in CLAUDE.md §4 as "not one release's paperwork"), every file under `docs/superpowers/plans/` and
  `docs/superpowers/specs/` (historical design records and plans, snapshots of what was true when each was
  written), and `CHANGELOG.md`'s **existing** bullets are **not touched by this plan**. Confirmed by checking
  git history: neither the founding record nor `docs/eventml-decisions.md` was touched by the prior
  `Need`→`SourceNeed` rename pass, despite both containing the old term at the time. This plan follows the
  same precedent. Only `spec/*.md`, `spec/06-decisions.md`, and `CLAUDE.md` are in scope for the rename
  itself; Task 7 adds one **new** `CHANGELOG.md` bullet describing the rename, without touching any existing
  one.
- **`RequirementDefinition` already appears twice** — `spec/02-requirement-analysis-model.md:410` and
  `spec/06-decisions.md`'s K67 row — both quoting SysML v2's own primary specification text (*"the base type
  of all `RequirementDefinition`s"*). These are not instances of `RequirementDef` and must not be touched;
  after this rename they will coincidentally read as the same name ProjectML's own type now uses, which is
  correct and intentional (K67 already argues this is SysML v2's own mechanism).
- **The new edge name is `replies`, one word** — not `replies to`. Every other coined edge in this collection
  (`poses`, `refine`, `retires`, `discharges`, `satisfies`) is a single backticked verb used directly as the
  edge's label; `replies` matches that convention. Where the surrounding sentence needs the preposition
  ("a later source replies **to** an earlier one"), "to" is ordinary prose outside the backticks, never part
  of the edge name itself.
- **No new `K` decision, no `spec/06-decisions.md` entry for the rename itself.** This is execution of an
  already-decided TODO, not a new design decision — nothing here is added to the decisions table.
- **Commit after every task. Never push** (CLAUDE.md §3, house rule 11).
- **What this plan does not do:** it does not touch the two files it explicitly excludes above; it does not
  revisit K67, K37–K41, or any other decision's substance — only the literal tokens named.

---

### Task 1: `RequirementDef`→`RequirementDefinition` — the four small files

**Files:**
- Modify: `spec/00-overview.md` (3 occurrences)
- Modify: `spec/01-requirement-model.md` (5 occurrences)
- Modify: `spec/05-binding-contract.md` (1 occurrence)
- Modify: `CLAUDE.md` (4 occurrences)

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: nothing later tasks depend on — each file in this rename is independent of the others.

**The rule for every step in this task:** find every occurrence of the literal token `RequirementDef`
(always backtick-quoted in these files, sometimes with a trailing `s` for the plural, e.g. `` `RequirementDef`s
``) and insert `inition` immediately before the closing backtick, turning it into `` `RequirementDefinition` ``
or `` `RequirementDefinition`s ``. Do not touch any occurrence that already reads `RequirementDefinition` —
there are none in these four files, but the check in Step 5 below confirms that.

- [ ] **Step 1: `spec/00-overview.md`** — three occurrences. Search the file for the literal string
  `RequirementDef` and rename each hit (append `inition` before the closing backtick). They appear in:
  *"metamodel says what a `RequirementDef` is; an implementation is a filled set of them"*, *"because the
  metamodel says what a `RequirementDef` is and holds none itself"*, and *"`RequirementDef` as an abstract
  type with placeholder subtypes named after no real kind"*.

- [ ] **Step 2: `spec/01-requirement-model.md`** — five occurrences. Search the file for the literal string
  `RequirementDef` and rename each hit (append `inition` before the closing backtick). They appear in: the
  K33 subsection heading's surrounding prose (*"defines `RequirementDef`: the definition whose"*), the
  question *"name the `RequirementDef` it was produced"*, *"`RequirementDef`, and the specialisation
  hierarchy K30 gives it, are defined only in"*, the decision sentence *"does not name the `RequirementDef`
  it came from, nor"*, and *"`RequirementDef` — even by a bare identifier — asks the reader"*.

- [ ] **Step 3: `spec/05-binding-contract.md`** — one occurrence. Search the file for the literal string
  `RequirementDef` and rename the one hit.

- [ ] **Step 4: `CLAUDE.md`** — four occurrences. Search the file for the literal string `RequirementDef` and
  rename each hit. They appear in: §1's opening paragraph (*"what a `RequirementDef` is, which edges"*), the
  "do not write" table's *"A filled-in `RequirementDef`"* row (both cells — the row header and *"The
  metamodel defines what a `RequirementDef` *is* and contains none"*), the implementation-definition
  paragraph (*"a filled set of `RequirementDef`s, and a rule-set"*), and locked decision #3 (*"It defines the
  `RequirementDef` type; it declares"*).

- [ ] **Step 5: Verify**

```bash
grep -n "RequirementDef\b" spec/00-overview.md spec/01-requirement-model.md spec/05-binding-contract.md CLAUDE.md
```

Expected: every match shown is immediately followed by `inition` (i.e. reads `RequirementDefinition`) — no
bare `RequirementDef` (not followed by `inition`) remains in any of the four files. If a match is bare, fix
it before continuing.

- [ ] **Step 6: Commit**

```bash
git add spec/00-overview.md spec/01-requirement-model.md spec/05-binding-contract.md CLAUDE.md
git commit -m "$(cat <<'EOF'
Rename RequirementDef to RequirementDefinition in spec/00, spec/01, spec/05, and CLAUDE.md

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: `RequirementDef`→`RequirementDefinition` — `spec/02-requirement-analysis-model.md`

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` (24 occurrences to rename; 1 existing
  `RequirementDefinition` at line 410 must NOT be touched)

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: nothing later tasks depend on.

- [ ] **Step 1: Rename every occurrence except the one at line 410**

Same rule as Task 1: find every literal `RequirementDef` token (backtick-quoted, singular or with a trailing
`s`) and insert `inition` before the closing backtick.

**Skip line 410 entirely** — it reads *"specialises `RequirementCheck`, stated as *the base type of all
`RequirementDefinition`s*, entirely on the"* and already says `RequirementDefinition`; do not touch it, and
do not turn it into `RequirementDefinitionition`.

- [ ] **Step 2: Verify**

```bash
grep -n "RequirementDef\b" spec/02-requirement-analysis-model.md | grep -v "RequirementDefinition"
```

Expected: no output. Every remaining `RequirementDef` occurrence in the file should be immediately followed
by `inition`.

```bash
grep -c "RequirementDefinition" spec/02-requirement-analysis-model.md
```

Expected: 25 (24 renamed occurrences + the 1 pre-existing SysML quote at line 410, untouched).

- [ ] **Step 3: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Rename RequirementDef to RequirementDefinition in spec/02

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: `RequirementDef`→`RequirementDefinition` — `spec/03-project-lifecycle-model.md`

**Files:**
- Modify: `spec/03-project-lifecycle-model.md` (13 occurrences)

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: nothing later tasks depend on.

- [ ] **Step 1: Rename every occurrence**

Same rule as Task 1. Search the file for the literal string `RequirementDef` and rename each of the 13 hits
(append `inition` before the closing backtick). No exceptions in this file — `RequirementDefinition` does not
already appear anywhere in it.

- [ ] **Step 2: Verify**

```bash
grep -n "RequirementDef\b" spec/03-project-lifecycle-model.md | grep -v "RequirementDefinition"
```

Expected: no output.

- [ ] **Step 3: Commit**

```bash
git add spec/03-project-lifecycle-model.md
git commit -m "$(cat <<'EOF'
Rename RequirementDef to RequirementDefinition in spec/03

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: `RequirementDef`→`RequirementDefinition` — `spec/06-decisions.md`

**Files:**
- Modify: `spec/06-decisions.md` (15 occurrences to rename; 1 existing `RequirementDefinition`, inside K67's
  own reason cell, must NOT be touched)

**Interfaces:**
- Consumes: nothing from an earlier task.
- Produces: nothing later tasks depend on.

- [ ] **Step 1: Rename every occurrence except the one inside K67's reason cell**

Same rule as Task 1. **Skip the occurrence inside the K67 row's Reason cell** — it reads *"stated as 'the base
type of all `RequirementDefinition`s,' entirely on the definition side"* and already says
`RequirementDefinition`; do not touch it.

- [ ] **Step 2: Verify**

```bash
grep -n "RequirementDef\b" spec/06-decisions.md | grep -v "RequirementDefinition"
```

Expected: no output.

```bash
grep -c "RequirementDefinition" spec/06-decisions.md
```

Expected: 16 (15 renamed + the 1 pre-existing SysML quote in K67, untouched).

- [ ] **Step 3: Commit**

```bash
git add spec/06-decisions.md
git commit -m "$(cat <<'EOF'
Rename RequirementDef to RequirementDefinition in spec/06-decisions.md

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: The `answers` edge → `replies` — `spec/02-requirement-analysis-model.md`

**Files:**
- Modify: `spec/02-requirement-analysis-model.md`

**Interfaces:**
- Consumes: nothing from an earlier task (independent of Tasks 1–4's `RequirementDef` rename, but run after
  Task 2 so both passes over this file don't collide in one diff).
- Produces: nothing later tasks depend on.

**This is not a blind substitution.** The English word "answer(s)/answered/answering" appears in this file
with no relation to the `Source`↔`Source` edge (e.g. "the honest answer is to check the reasoning," "whether
and how the question is answered" describing `RequirementQuestion`'s own discharge, not this edge). Only the
occurrences below name or describe the edge itself. Every other "answer"-containing sentence in this file is
correctly left untouched — do not search-and-replace the bare word.

- [ ] **Step 1: §3, the edge's own definition (around line 64)**

Replace:
```
Sources connect to each other through exactly one edge: a later source **`answers`** an earlier one. Four
properties hold of it, each already settled:
```
with:
```
Sources connect to each other through exactly one edge: a later source **`replies`** to an earlier one. Four
properties hold of it, each already settled:
```

- [ ] **Step 2: §3, the four properties (around lines 67–72)**

Replace:
```
- It points backward in time: a source can only answer something that came before it (D38).
```
with:
```
- It points backward in time: a source can only reply to something that came before it (D38).
```

Replace:
```
- One edge relates exactly one source to exactly one source, but a source may carry any number of them —
  answering several earlier sources, or being answered by several later ones (D29).
```
with:
```
- One edge relates exactly one source to exactly one source, but a source may carry any number of them —
  replying to several earlier sources, or being replied to by several later ones (D29).
```

- [ ] **Step 3: §3, the closing-edge finding (around lines 74–76)**

Replace:
```
The founding record's section 5 makes a further finding about this edge worth carrying forward here: `answers`
is the natural closing edge for a review finding. A finding is opened by a source and closed by a later one
that answers it, so closing a finding is not a tick somebody applies to a record — it is itself evidence,
```
with:
```
The founding record's section 5 makes a further finding about this edge worth carrying forward here: `replies`
is the natural closing edge for a review finding. A finding is opened by a source and closed by a later one
that replies to it, so closing a finding is not a tick somebody applies to a record — it is itself evidence,
```

- [ ] **Step 4: §4, `SourceQuestion`'s discharge (around lines 147–150)**

Replace:
```
`answers` edge (§3): a later source `answers` the source the question's passage sits in.

**How `SourceQuestion` subdivides, and whether it names the party expected to answer it, is not settled
here.**
```
with:
```
`replies` edge (§3): a later source `replies` to the source the question's passage sits in.

**How `SourceQuestion` subdivides, and whether it names the party expected to reply to it, is not settled
here.**
```

- [ ] **Step 5: §10, the projection's drop list (around line 530)**

Replace:
```
**What it drops** is everything this model adds, and every requirement no longer in force. Sources and the
`answers` edge between them; `SourceNeed`s, `SourceDecision`s and the `refine` edge that names them;
```
with:
```
**What it drops** is everything this model adds, and every requirement no longer in force. Sources and the
`replies` edge between them; `SourceNeed`s, `SourceDecision`s and the `refine` edge that names them;
```

- [ ] **Step 6: §11, `discharges` vs. the edge it is not reusing (around lines 689–691)**

Replace:
```
`discharges` is a coined edge rather than a reuse of `answers`: `answers` is a `Source`↔`Source`, evidentiary
edge — one passage of material responding to another — where `discharges` names, on the model's own side,
what closed a question, a different kind of relationship entirely. Reusing `answers` here would blur exactly
```
with:
```
`discharges` is a coined edge rather than a reuse of `replies`: `replies` is a `Source`↔`Source`, evidentiary
edge — one passage of material responding to another — where `discharges` names, on the model's own side,
what closed a question, a different kind of relationship entirely. Reusing `replies` here would blur exactly
```

- [ ] **Step 7: §11, the traceable chain (around lines 708–710)**

Replace:
```
machinery this document has independently of `discharges`: `RequirementQuestion` --poses--> `SourceQuestion`,
whose source a later source `answers` (§3); if that answering source carries a `SourceDecision`, it `refine`s
into the `RequirementDecision` that answers the question (K61). `discharges` sits *beside* that traceable
```
with:
```
machinery this document has independently of `discharges`: `RequirementQuestion` --poses--> `SourceQuestion`,
whose source a later source `replies` to (§3); if that replying source carries a `SourceDecision`, it `refine`s
into the `RequirementDecision` that answers the question (K61). `discharges` sits *beside* that traceable
```

**Leave *"the `RequirementDecision` that answers the question"* unchanged** — this describes
`RequirementDecision` resolving a `RequirementQuestion`, a different relationship from the `Source`↔`Source`
edge, and is not part of this rename.

- [ ] **Step 8: §11, findings closure prose (two occurrences, around lines 759–760 and 815–817)**

Replace:
```
**Closure is evidence, not a tick.** A review finding is opened by a source and closed by a later source that
`answers` it, on exactly the terms section 3 sets out for that edge. Nothing marks a finding closed directly;
```
with:
```
**Closure is evidence, not a tick.** A review finding is opened by a source and closed by a later source that
`replies` to it, on exactly the terms section 3 sets out for that edge. Nothing marks a finding closed directly;
```

Replace:
```
one of the three kinds above decided by judgement, and the only one carrying a state. It is opened by a
source and closed by a later source that `answers` it, on the terms this section has already set out, and
```
with:
```
one of the three kinds above decided by judgement, and the only one carrying a state. It is opened by a
source and closed by a later source that `replies` to it, on the terms this section has already set out, and
```

- [ ] **Step 9: §12, the syntactic constraints over `Source` (around lines 855–858)**

Replace:
```
- A source's identity is unique among every source in the model (§2).
- The `answers` edge points backward in time: the source carrying it is dated later than the source it names
  (§3, D38).
- One `answers` edge names exactly one earlier source; a source may carry any number of them (§3, D29).
```
with:
```
- A source's identity is unique among every source in the model (§2).
- The `replies` edge points backward in time: the source carrying it is dated later than the source it names
  (§3, D38).
- One `replies` edge names exactly one earlier source; a source may carry any number of them (§3, D29).
```

- [ ] **Step 10: §12, the syntactic constraints over findings (around lines 943–945)**

Replace:
```
- A review finding names at least two elements (§11, K10).
- A review finding recorded as closed names the later source that closed it, and that source `answers` the
  source the finding was opened by. Nothing marks a finding closed directly (§11, K11).
```
with:
```
- A review finding names at least two elements (§11, K10).
- A review finding recorded as closed names the later source that closed it, and that source `replies` to the
  source the finding was opened by. Nothing marks a finding closed directly (§11, K11).
```

- [ ] **Step 11: Verify**

```bash
grep -n "\`answers\`" spec/02-requirement-analysis-model.md
```

Expected: no output — every backticked `` `answers` `` occurrence is now `` `replies` ``.

```bash
grep -n "\banswer" spec/02-requirement-analysis-model.md
```

Expected: every remaining hit is one of the deliberately-untouched generic-English occurrences (e.g. *"the
honest answer is to check the reasoning"*, *"whether and how the question is answered"*, *"the `RequirementDecision`
that answers the question"*) — none should be a bare, unbackticked description of the `Source`↔`Source` edge
itself.

- [ ] **Step 12: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Rename the Source answers edge to replies in spec/02

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 6: The `answers` edge → `replies` — `spec/06-decisions.md`

**Files:**
- Modify: `spec/06-decisions.md`

**Interfaces:**
- Consumes: nothing from an earlier task (run after Task 4 so both passes over this file don't collide in
  one diff).
- Produces: nothing later tasks depend on.

**Same caution as Task 5:** this file uses "answer(s)/answered/answerable" constantly for open-question
bookkeeping (*"OQ12 — answered"*, *"is answered by K56"*, *"not answered, but shaped"*) — none of that is the
edge, and none of it is touched by this task. Only the four occurrences below are.

- [ ] **Step 1: K61's row**

Find the K61 row (its Reason cell ends *"...the connection already traces through `poses` + `answers` +
`refine`"*). Replace, within that cell only:

```
the connection already traces through `poses` + `answers` + `refine`
```
with:
```
the connection already traces through `poses` + `replies` + `refine`
```

- [ ] **Step 2: K79's row**

Find the K79 row (its Reason cell contains *"`discharges` is coined rather than reusing `answers`: `answers`
is a `Source`↔`Source` evidentiary edge"*). Replace, within that cell only:

```
`discharges` is coined rather than reusing `answers`: `answers` is a `Source`↔`Source` evidentiary edge
```
with:
```
`discharges` is coined rather than reusing `replies`: `replies` is a `Source`↔`Source` evidentiary edge
```

- [ ] **Step 3: The OQ13 row**

Find the OQ13 row (its Question cell contains *"it is opened by a source and closed by a later source that
`answers` it (K10, K11"*). Replace, within that cell only:

```
it is opened by a source and closed by a later source that `answers` it (K10, K11
```
with:
```
it is opened by a source and closed by a later source that `replies` to it (K10, K11
```

- [ ] **Step 4: The OQ16 row**

Find the OQ16 row (its Question cell reads *"How does `SourceQuestion` subdivide, and does it name the party
expected to answer?"*). Replace, within that cell only:

```
How does `SourceQuestion` subdivide, and does it name the party expected to answer?
```
with:
```
How does `SourceQuestion` subdivide, and does it name the party expected to reply?
```

- [ ] **Step 5: Verify**

```bash
grep -n "\`answers\`" spec/06-decisions.md
```

Expected: no output.

```bash
grep -c "\banswer" spec/06-decisions.md
```

Expected: a large number, unchanged from before this task — every other occurrence is open-question
bookkeeping and was correctly left alone.

- [ ] **Step 6: Commit**

```bash
git add spec/06-decisions.md
git commit -m "$(cat <<'EOF'
Rename the Source answers edge to replies in spec/06-decisions.md

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 7: `CHANGELOG.md` — record both renames

**Files:**
- Modify: `CHANGELOG.md`

**Interfaces:**
- Consumes: both renames' completion (Tasks 1–6).
- Produces: nothing later tasks depend on.

- [ ] **Step 1: Add one new bullet**

Insert, under `## [Unreleased]` → `### Added`, immediately after the current last bullet (the one ending
*"...Findings are in [`docs/superpowers/specs/2026-09-04-project-lifecycle-model-design.md`]..."*):

```markdown
- Two corpus-wide renames, both mechanical — no design change: `RequirementDef` is now `RequirementDefinition`,
  matching SysML v2's own term rather than an abbreviation of it; the `Source` edge previously named `answers`
  is now `replies`, correcting a likely translation artefact (the edge covers any later source responding to
  an earlier one, disagreement included, not only a question being answered).
```

- [ ] **Step 2: Commit**

```bash
git add CHANGELOG.md
git commit -m "$(cat <<'EOF'
Record the RequirementDefinition and replies renames in CHANGELOG

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 8: Final cross-file consistency pass

**Files:**
- Read only: every file in `spec/`, `CLAUDE.md`, `CHANGELOG.md`

**Interfaces:**
- Consumes: every file Tasks 1–7 touched.
- Produces: nothing new — this task only verifies.

- [ ] **Step 1: Corpus-wide `RequirementDef` check**

```bash
grep -rn "RequirementDef\b" spec/ CLAUDE.md | grep -v "RequirementDefinition"
```

Expected: no output. Every occurrence of `RequirementDef` anywhere in the in-scope files is now
`RequirementDefinition`.

- [ ] **Step 2: Corpus-wide backticked `answers` check**

```bash
grep -rn "\`answers\`" spec/
```

Expected: no output. Every backticked `` `answers` `` — the edge itself — is now `` `replies` ``.

- [ ] **Step 3: Confirm the two SysML-quote exceptions survived untouched**

```bash
grep -n "RequirementDefinition" spec/02-requirement-analysis-model.md | grep -i "base type"
grep -n "RequirementDefinition" spec/06-decisions.md | grep -i "base type"
```

Expected: both commands return the original SysML v2 quotation, reading `RequirementDefinition` exactly
once each, not `RequirementDefinitionition` or any other double-rename artefact.

- [ ] **Step 4: Confirm the excluded files were not touched**

Find the commit immediately before this plan's Task 1 began — the commit that added this plan file itself
(`git log --oneline -- docs/superpowers/plans/2026-09-04-requirementdef-and-answers-rename-plan.md` gives it)
is one commit after that base; use its parent as `BASE`:

```bash
BASE=$(git log --format=%H -- docs/superpowers/plans/2026-09-04-requirementdef-and-answers-rename-plan.md | tail -1)^
git diff --stat "$BASE"..HEAD -- docs/2026-08-26-kernel-brainstorm.md docs/eventml-decisions.md docs/superpowers/plans docs/superpowers/specs
```

Expected: no output for `docs/2026-08-26-kernel-brainstorm.md` and `docs/eventml-decisions.md` — neither
should appear at all. `docs/superpowers/plans` should show only this plan file itself (already committed
before Task 1, not modified since). Nothing under `docs/superpowers/specs` should appear.

- [ ] **Step 5: Read `spec/02-requirement-analysis-model.md` and `spec/06-decisions.md` in full, once, start
  to end**

Confirm every remaining "answer"-family word reads correctly as ordinary English (open-question bookkeeping,
`RequirementDecision`/`RequirementQuestion` prose, the value-state discussion) and that neither file has a
sentence left grammatically broken by Tasks 5–6's edits (in particular Task 5 Step 7's fronted relative
clause — *"whose source a later source `replies` to (§3)"* — read it in context to confirm it parses).

- [ ] **Step 6: Commit, only if Steps 1–5 found something to fix**

If nothing needed fixing, this task produces no commit — its deliverable is the confirmation itself. If a fix
was needed, commit it with a message naming what Task and what line it corrects.
