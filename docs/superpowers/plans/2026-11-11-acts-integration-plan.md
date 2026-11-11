# Integrate the closed list of acts (K150–K160) into `spec/` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Add `spec/07-acts.md`, which states that a project model changes only through fourteen listed acts,
each defined by who performs it, the cause it carries, its condition and its effect; record K150–K160 in
`spec/06`; and point `spec/00`, `spec/01`, `spec/02` and `spec/03` at it where they meet it. This is prose,
not code: a "test" for each task is a re-read for internal consistency, cross-reference correctness and
conformance to `CLAUDE.md`, plus the grep checks the task names.

**Architecture:** One new normative document carries the whole of it. It cites the rules each act rests on
rather than restating them, so no rule is stated twice. The other documents change by a sentence or a
paragraph each. `spec/06` takes the decisions in the same commit as `spec/07`, by its own convention.

**Tech Stack:** Markdown, Mermaid (GitHub-rendered), git.

## Read first — the decision this plan writes

[`docs/superpowers/specs/2026-11-11-acts-design.md`](../specs/2026-11-11-acts-design.md) is the only
authority for what follows. Read it whole before Task 1. Where this plan and the record disagree, the record
wins; where the record and a decision already in `spec/06` disagree, stop and report, do not resolve it.

## Global Constraints

- **Branch:** `claude/projectml-mcp-server-b94b1c`, whose last commits are the design record. Work on it; do
  not rebase it. **Commit after every task. Never push.**
- **English only. No notation, no filled `RequirementDefinition`, nothing executable. "Attribute", not
  "field". Do not describe the metamodel in terms of files** (`CLAUDE.md` §1, §5).
- **An act names no operation, argument, call or order of calls** (K152). No sentence in `spec/07` mentions
  a tool, a server, an agent's interface or a protocol. "The modeller, a person or an agent" is the furthest
  the prose goes, as `spec/02` §9 already does.
- **Every entry passes `00-overview.md` §4's test:** true of a project modelled in YAML, in SysML v2 textual
  notation, and in a spreadsheet, without change.
- **Cite, do not restate.** An act's condition names the decision and section it rests on (for example
  "K129, `02-requirement-analysis-model.md` §10"). A rule already stated elsewhere is never reworded in
  `spec/07`.
- **The number 04 stays retired** (K131). The new document is `07`.
- **Historical records are not edited** (`CLAUDE.md` §4): no file under `docs/superpowers/specs/` or
  `docs/superpowers/plans/`, the founding record, or `docs/eventml-decisions.md` changes.
- **Before Task 1, re-check the highest K and OQ on `main`:** `git fetch` and
  `git show origin/main:spec/06-decisions.md | grep -oE "K1[5-9][0-9]|OQ[4-9][0-9]" | sort -u | tail`. If
  anything at or above K150 or OQ49 exists there, stop and report: the record's numbers would collide.

---

### Task 1: `spec/07-acts.md` and `spec/06`'s block K150–K160

**Files:**
- Create: `spec/07-acts.md`
- Modify: `spec/06-decisions.md` — a new section after `## Decisions K136–K149`'s table and before
  `## Decisions K51–K54`

**Interfaces:**
- Consumes: the record's §2–§5.
- Produces: the section numbers Tasks 2 and 3 cite — §1 scope and closure, §2 what an act is, §3 who acts,
  §4 what an act leaves behind, §5 the acts, §6 inseparable acts, §7 what is not an act, §8 what no act does,
  §9 what is open. Act identifiers A1–A14 exactly as the record numbers them.

- [ ] **Step 1: Write `spec/07-acts.md`, title `# How the project model changes`, with these sections.**

  **§1 What this document is.** It states the acts through which a project model changes, and that there are
  no others (K150). It is normative and not a member of the collection, as `05-binding-contract.md` and
  `06-decisions.md` are not (`00-overview.md` §2). Its scope is the project model — sources,
  `SourceElement`s, requirements, `RequirementQuestion`s, `RequirementDecision`s, review findings — and not a
  `RequirementDefinition` or a `RuleSet` (K151, citing K15, K16, K22, K88, and OQ46 for what their change does
  to requirements already complete). Close with the two measurements K150 states — safety and completeness —
  phrased over "whatever changes a model on a modeller's behalf", without naming a tool.

  **§2 What an act is.** K152: defined by its effect; the four entries; several acts as one step, whole or
  not at all, each condition met. K157: the term is this collection's own, and W3C PROV's `Activity` is not
  adopted because it is a recorded element (cite `CLAUDE.md` §3 rule 10 as the reason the refusal is
  recorded).

  **§3 Who acts.** K153 in full, including that the modeller's judgements — extraction, the flag, relevance —
  commit nothing, and that who performs a review is OQ23.

  **§4 What an act leaves behind.** K154: the product carries its cause, listed per product as the record
  lists it; the model records no act; who, when and in what order is an implementation's service. The
  revisit condition on OQ44 and OQ45, verbatim in substance. The one traceless act, A3, with K158 and K41.
  End on the sentence the owner's position gives: the model carries the derivability of accountability, not
  a record of the modeller's steps.

  **§5 The acts.** Three tables — over sources (A1–A3), over requirements (A4–A7), over questions, decisions
  and findings (A8–A14) — with the columns *Act*, *Cause carried*, *Condition*, *Effect*, rows exactly as the
  record's §4 gives them. After the tables, one Mermaid `flowchart LR` drawing the usual path and nothing
  else: A1 → A2 → A4 → A6 → (complete) → A8 → A10 → A11/A12, with A5 branching from A2 when the element is a
  `SourceUpdate`. Follow it with "The diagram draws what this section states; where the two disagree, the
  prose wins."

  **§6 Inseparable acts.** K155's three pairings, each with the rules it follows from.

  **§7 What is not an act.** Reading — querying, checking, the projection (K156). Cutting a baseline, as an
  export: no source states it, a new one is cut whenever later information calls for it, and K142's condition
  holds on it (K156, citing `01-requirement-model.md` §4). Creating a project model (K160). A walk leaves no
  trace beyond what it raises (K159), citing `03-project-lifecycle-model.md` §3's refusal to guarantee an
  exhaustive walk.

  **§8 What no act does.** The seven items of the record's §4 closing paragraph, each with its decision, as a
  list. One sentence first: these are stated so that a reader proposing one finds the refusal rather than an
  absence.

  **§9 What is open.** A table *Act* / *Waits on*: A5 OQ44; A7 OQ42; A8 and A9 OQ41, OQ45, OQ48; A13 OQ23;
  §7's baseline OQ35; §1's scope OQ46; and, as its own row, closing a `RequirementDecision`, which no act
  does because K63 leaves the criterion unstated. One sentence: the list closes over what is decided, and each act marked
  here keeps the condition or cause the open question leaves.

- [ ] **Step 2: Add `spec/06`'s block.** Heading `## Decisions K150–K160`. An opening paragraph in the shape
  of `## Decisions K136–K149`'s: taken in the design record of 2026-11-11 (link it), written in by this plan
  (link it); K39 is extended and narrowed by K158; no open question is answered or opened. Then one table,
  columns `| # | Decision | Reason |`, rows K150–K160 copied verbatim from the record's §2, §3 and §5, in
  numeric order (K160 last, although the record places it among §3's rows).

- [ ] **Step 3: Mark K39's row.** In `spec/06`, append to K39's *Reason* cell: " Extended to every
  `SourceElement`, and narrowed to one nothing refers to, by K158."

- [ ] **Step 4: Check.**

  ```bash
  grep -n -i -E "field|yaml|json|xml|\bmcp\b|server|tool call|argument|\bAPI\b" spec/07-acts.md
  ```
  Expected: no output.

  ```bash
  grep -c -E "^\| K1(5[0-9]|60) " spec/06-decisions.md
  ```
  Expected: `11`.

  ```bash
  grep -o -E "\bA(1[0-4]|[1-9])\b" spec/07-acts.md | sort -u | wc -l
  ```
  Expected: `14`.

  ```bash
  grep -n -E "04-|[őűáéíóöüú]" spec/07-acts.md
  ```
  Expected: no output.

  Re-read `spec/07` against the record's §4, act by act: every condition cites, none restates.

- [ ] **Step 5: Commit.**

  ```bash
  git add spec/07-acts.md spec/06-decisions.md
  git commit -m "Add spec/07: the closed list of acts through which a project model changes (K150-K160)"
  ```
  End the message with the attribution line the session gives.

### Task 2: `spec/00` — the further documents, and what keeps the guarantee

**Files:**
- Modify: `spec/00-overview.md` §2 (the paragraph beginning "Two further documents round out `spec/`") and
  §5 (after the paragraph beginning "Two decisions record it.")

**Interfaces:**
- Consumes: `spec/07`'s §1 and §4 from Task 1.

- [ ] **Step 1: Replace §2's paragraph** with:

  > Three further documents round out `spec/`, beyond the collection itself: `05-binding-contract.md`, which
  > states what attaching underneath the collection requires, `06-decisions.md`, the normative record of
  > every decision the collection rests on (K31), and `07-acts.md`, which states the acts through which a
  > project model changes, and that there are no others (K150).

- [ ] **Step 2: Add a paragraph at the end of §5:**

  > The guarantee is kept by a closure. A project model changes only through the acts `07-acts.md` lists,
  > and whatever an act produces names, on itself, what it came from (K150, K154). That is the extent to
  > which the model records its own history: accountability is derivable from it, back to the source that
  > states each thing. Who performed an act, and when, the model does not record; an implementation may.

- [ ] **Step 3: Check.** `grep -n "07-acts" spec/00-overview.md` shows two lines. Re-read §2 and §5 whole.

- [ ] **Step 4: Commit.**

  ```bash
  git add spec/00-overview.md
  git commit -m "Point spec/00 at spec/07: the further documents, and the closure that keeps the guarantee"
  ```

### Task 3: `spec/01`, `spec/02`, `spec/03` — the baseline, deletion, and the walk

**Files:**
- Modify: `spec/01-requirement-model.md` §4, after the paragraph beginning "A baseline's condition is
  losslessness"
- Modify: `spec/02-requirement-analysis-model.md` §11, the paragraph beginning "**Deleting a `SourceNeed`
  loses nothing of record"
- Modify: `spec/03-project-lifecycle-model.md` §3, *Walking a `RuleSet`*, at the end of the paragraph
  beginning "**The walk runs once, and a `ValueRule` is not part of it.**"

**Interfaces:**
- Consumes: `spec/07` §1, §7 from Task 1.

- [ ] **Step 1: `spec/01` §4.** Add:

  > Cutting a baseline is not an act on the working model (`07-acts.md` §7, K156). It changes nothing there:
  > a baseline is the projection given identity, an export, and a new one is cut whenever later information
  > calls for it. No source states a cut.

- [ ] **Step 2: `spec/02` §11.** At the end of the paragraph on deleting a `SourceNeed`, add:

  > The same holds of every `SourceElement`, since each is a pointer into a passage: any one extracted in
  > error may be deleted, and only while nothing refers to it, since deleting one that a requirement or a
  > decision names would leave that element without its origin at once (K158, K145, K61). Deletion is the
  > one act that leaves no trace (`07-acts.md` §4).

- [ ] **Step 3: `spec/03` §3.** At the end of the named paragraph, add:

  > A walk leaves no trace beyond the questions it raises. Whether it ran is not recorded: it is the reaction
  > to an event the model does record, a requirement becoming complete, and that every such reaction was
  > carried out is the modeller's (K159, `07-acts.md` §7).

- [ ] **Step 4: Check.**

  ```bash
  grep -n "07-acts" spec/01-requirement-model.md spec/02-requirement-analysis-model.md spec/03-project-lifecycle-model.md
  ```
  Expected: one line in each of the three files.

  Re-read each paragraph around the insertion: no sentence before or after it now contradicts it. In
  `spec/02` §12, confirm no constraint says deletion is limited to a `SourceNeed`; if one does, report it
  rather than edit it.

- [ ] **Step 5: Commit.**

  ```bash
  git add spec/01-requirement-model.md spec/02-requirement-analysis-model.md spec/03-project-lifecycle-model.md
  git commit -m "Point spec/01-03 at spec/07: a baseline is an export, deletion of any source element, the walk's trace"
  ```

### Task 4: `CHANGELOG.md`, and the sweep

**Files:**
- Modify: `CHANGELOG.md`, `## [Unreleased]`, `### Added`, as the last entry

- [ ] **Step 1: Add the entry:**

  > - The closed list of acts. A project model changes only through fourteen acts, each defined by who
  >   performs it, the cause it carries, its condition and its effect, in a new `spec/07-acts.md`. Every act
  >   is the modeller's; one that commits the project names the source in which the project manager stated
  >   it. What an act produces carries its cause, and the model records no act of its own. Reading, cutting
  >   a baseline and creating a model are not acts; a walk leaves no trace beyond what it raises. Any source
  >   element extracted in error may be deleted while nothing refers to it, extending K39. K150–K160 record
  >   the decisions; no open question is opened or answered. Findings are in
  >   [`docs/superpowers/specs/2026-11-11-acts-design.md`](docs/superpowers/specs/2026-11-11-acts-design.md).

- [ ] **Step 2: Sweep.**

  ```bash
  grep -rn -E "Two further documents|two further documents" spec/
  ```
  Expected: no output.

  ```bash
  grep -rn -E "K15[0-9]|K160" spec/ | grep -v "06-decisions.md" | grep -o -E "K1(5[0-9]|60)" | sort -u
  ```
  Expected: every one of K150–K160 that `spec/07` cites appears; none is cited that `spec/06` lacks.

  Read `spec/07` once more whole, as a reader who has read only `spec/00`–`spec/03`: every citation resolves,
  and nothing in it names a tool.

- [ ] **Step 3: Commit.**

  ```bash
  git add CHANGELOG.md
  git commit -m "Record the closed list of acts in the changelog"
  ```
