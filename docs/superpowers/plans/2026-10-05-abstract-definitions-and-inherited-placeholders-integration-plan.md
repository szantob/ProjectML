# Integrate abstract definitions and inherited placeholders (K109–K112) into `spec/` — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Write K109–K112 into `spec/`: a definition may be abstract and states that it is; an abstract
definition carries no template, and neither a method of verification nor a wording rule applies to it; a
concrete definition's template uses every parameter it has, inherited ones included; and an inherited
parameter brings its *what to ask*. This is prose, not code: a "test" for each task is a careful re-read for
internal consistency, cross-reference correctness and conformance to `CLAUDE.md`, not a runnable suite.

**Architecture:** All of the change but the decision record lands in `spec/02-requirement-analysis-model.md`:
§7 states what an abstract definition is and carries, §9 narrows OQ9 again, §10 keeps a requirement off an
abstract definition, and §12 restates three constraints and adds two. `spec/06-decisions.md` records the
decisions; `CHANGELOG.md` the release note.

**Tech Stack:** Markdown, git.

## Global Constraints

- **English only, no exception** (CLAUDE.md §5).
- **No notation, no filled `RequirementDefinition`, nothing executable** (CLAUDE.md §1). No task shows how
  abstractness, a template or a placeholder is written down, and no task names an example kind or parameter.
- **"Attribute", not "field." Do not describe the metamodel in terms of files** (CLAUDE.md §5).
- **Everything this plan writes is already decided**, in
  [`docs/superpowers/specs/2026-10-05-abstract-definitions-and-inherited-placeholders-design.md`](../specs/2026-10-05-abstract-definitions-and-inherited-placeholders-design.md)
  (K109–K112). This plan cites decisions by number rather than re-arguing them.
- **K110 corrects the scope of three constraints; it does not revise a decision.** The constraints were written
  when `RequirementDefinition` was the only abstract definition, and it never met them. No earlier row in
  `spec/06` changes.
- **OQ30 and OQ31 are already in `spec/06`** (commit `4807cfe`). This plan neither moves nor answers them.
- **Line wrapping.** Prose wraps at 110 characters. Where a step replaces a sentence that spans lines, match it
  across the line break, and rewrap the replaced paragraph to the same width.
- **Commit after every task. Never push.** End every commit message with
  `Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>`.
- **What this plan does not do.** It does not touch OQ9's remainder (overriding or narrowing a parameter; the
  rest of the core), OQ30 or OQ31, any diagram, or `bindings/`.

---

### Task 1: `spec/02` §7 — an abstract definition, and what it carries (K109, K110)

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` — the paragraph beginning `A definition carries eight
  things.`; a new paragraph after the one ending `...written per parameter rather than per definition.`

**Interfaces:**
- Consumes: nothing.
- Produces: the statement Tasks 2–4 cite as `§7, K109` and `§7, K110`.

- [ ] **Step 1: Check the citation before writing it**

K109 cites KerML's `isAbstract`. Open the OMG KerML specification (formal/2025 or the latest beta on omg.org)
and find the description of `Type::isAbstract`. Confirm it says, in substance, that an instance of an abstract
type must also be an instance of one of its specialisations, and that SysML v2's textual notation marks a
definition with the keyword `abstract`. If the wording differs, write the paragraph in Step 3 to what the
specification actually says, and note the difference in the commit message. Quote nothing longer than a
phrase.

- [ ] **Step 2: Qualify the core**

Replace:

```
A definition carries eight things. This is the **core** — what the metamodel can interpret, or can fail on,
without reading anything an implementation supplies (K27).
```

with:

```
A definition carries eight things, three of which do not apply to an abstract definition (below, K110). This
is the **core** — what the metamodel can interpret, or can fail on, without reading anything an
implementation supplies (K27).
```

- [ ] **Step 3: Add the abstract-definition paragraphs**

After the paragraph ending `...which is why *what to ask* sits beside *parameters* and is written per
parameter rather than per definition.`, insert:

```
**A definition may be abstract** (K109). No requirement is produced under an abstract definition, only under
its specialisations: it exists so that the definitions beneath it share what it declares. Every definition
states whether it is abstract, and that it is cannot be read off the absence of anything else — an empty
template is a template nobody has written yet, not a declaration that nobody may produce a requirement under
the definition. The term is adopted rather than coined: SysML v2 marks a definition abstract, and KerML's
`isAbstract` says that whatever an abstract type classifies is also classified by one of its specialisations,
which is this reading one level down. `RequirementDefinition` itself is abstract on the same terms (K30).

**An abstract definition carries no template, and neither *how it would be verified* nor the *wording rule*
applies to it** (K110). All three speak of a requirement produced under the definition: the wording it is
produced from, how it would be shown to hold, what its wording must satisfy. None is produced under an
abstract definition. The other five apply as they do to any definition — *when it applies* still decides when
the definition comes into play, and a parameter it declares is still filled, through its specialisations, so
it still needs its *what to ask*.
```

- [ ] **Step 4: Check the section against itself**

Re-read §7 from its start. The sentence opening the section — that no element is a `RequirementDefinition`
and nothing more — must still read true: K109 makes some specialisations abstract too, and does not make
`RequirementDefinition` any less abstract. The paragraph on why *how it would be verified* sits on the
definition says a requirement inherits its definition's method; that stays true, since a requirement's
definition is never abstract (Task 3). If either now reads as contradicted, reword the new paragraph, not the
old one.

- [ ] **Step 5: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Let a definition be abstract, carrying no template, method or wording rule (K109, K110)

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: `spec/02` §9 — what a descendant's template uses, and what an inherited parameter brings (K111, K112)

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` — the paragraph beginning `**What specialisation means is
  open, except for parameters.**` at the end of §9

**Interfaces:**
- Consumes: `§7, K109` and `§7, K110` from Task 1.
- Produces: the statement Task 4 cites as `§9, K111, K112`.

- [ ] **Step 1: Replace the OQ9 paragraph**

Replace the paragraph from `**What specialisation means is open, except for parameters.**` to `...K30 chooses
the mechanism; it does not define its semantics.` with:

```
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

Everything else is not defined here: whether an inherited parameter may ever be overridden or narrowed, and
what a subtype may add to, narrow or override among the other attributes of section 7's core. That is OQ9, and
it waits for something to exercise it. K30 chooses the mechanism; it does not define its semantics.
```

- [ ] **Step 2: Check the section against itself**

Re-read §9 from its start. Nothing in it may now read as a requirement inheriting from its definition: K111
and K112 are about definitions and their specialisations, and a requirement connects to its definition only
by being produced under it. Check also that "placeholder" is used here as §7's *text* row describes the
template — a place for a parameter — and not as the name of any notation's device.

- [ ] **Step 3: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Have a concrete template use every parameter its definition has, and inherit the ask (K111, K112)

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: `spec/02` §10 — a requirement is never produced under an abstract definition (K109)

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` — the paragraph beginning `**A requirement also names the
  definition it was produced under.**`

**Interfaces:**
- Consumes: `§7, K109` from Task 1.
- Produces: nothing new.

- [ ] **Step 1: Add the sentence**

In that paragraph, after the sentence ending `...and a requirement produced under two definitions would have
two kinds or none.`, insert:

```
The definition it names is never abstract (§7, K109): an abstract definition exists to be specialised, and a
requirement is produced under one of its specialisations instead.
```

Rewrap the paragraph to 110 characters.

- [ ] **Step 2: Check the derivation**

Re-read *The derivation* above it. A `SourceNeed`'s passage *selects the definition*; the selection now ends
on a definition that is not abstract. If any sentence there says the passage may select any definition at
all, add `that is not abstract (§7, K109)` to it; otherwise change nothing.

- [ ] **Step 3: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Never produce a requirement under an abstract definition (K109)

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: `spec/02` §12 — the constraints over definitions and over the derivation

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` — under **Over `RequirementDefinition`.**, the bullets
  beginning `A definition states the template`, `**Every definition states how a requirement produced under
  it would be verified.**` and `**Every definition states a well-formedness rule`; under **Over the
  derivation, and over being no longer in force.**, the bullet beginning `A requirement in this model names
  exactly one`

**Interfaces:**
- Consumes: Tasks 1–3.
- Produces: nothing new.

- [ ] **Step 1: The template**

Replace the bullet beginning `- A definition states the template its requirements' wording is produced from
(§7).` (two lines) with:

```
- A definition that is not abstract states the template its requirements' wording is produced from (§7). A
  definition without one produces nothing, and the derivation §10 describes cannot be run against it. An
  abstract definition states none: nothing is produced under it (§7, K109, K110).
- In the template of a definition that is not abstract, every placeholder names a parameter the definition
  has, declared or inherited, and every parameter it has appears as a placeholder (§9, K107, K111).
```

- [ ] **Step 2: The method of verification**

In the bullet beginning `- **Every definition states how a requirement produced under it would be
verified.**`, replace `**Every definition states how` with `**Every definition that is not abstract states
how`. At the end of the bullet, after `...and it is the stated rule K29 rests on.`, add:

```
  An abstract definition is outside it (§7, K110): when the constraint was written,
  `RequirementDefinition` was the only abstract definition, and it never met the constraint either.
```

- [ ] **Step 3: The wording rule**

In the bullet beginning `- **Every definition states a well-formedness rule`, replace `**Every definition
states a well-formedness rule` with `**Every definition that is not abstract states a well-formedness rule`.
At the end of the bullet, after `...it is the stated rule K66 rests on.`, add:

```
  An abstract definition is outside it, on the same terms (§7, K110).
```

- [ ] **Step 4: The derivation**

Replace:

```
- A requirement in this model names exactly one `RequirementDefinition`: never none, and never two (§10, K8).
```

with:

```
- A requirement in this model names exactly one `RequirementDefinition`: never none, and never two (§10, K8).
  The definition it names is not abstract; a requirement naming an abstract definition is not a well-formed
  element of this model (§7, §10, K109).
```

- [ ] **Step 5: Check the section against itself**

Re-read **Over `RequirementDefinition`.** whole. The ask constraint (`Every parameter a definition declares
carries its own ask`) must stay as it is: it speaks of declared parameters, which K112 leaves where they were.
The redeclaration constraint must still read true beside the new placeholder constraint.

- [ ] **Step 6: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF'
Scope the template, method and wording rule constraints to concrete definitions, and add the placeholder one

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: `spec/06` — K109–K112, and OQ9 narrowed again

**Files:**
- Modify: `spec/06-decisions.md` — a new section after the `## Decisions K101–K108` table; the paragraph
  beginning `**OQ9 is narrowed by K107 and K108.**`

**Interfaces:**
- Consumes: Tasks 1–4.
- Produces: nothing new.

- [ ] **Step 1: Add the decisions**

After the last row of the `## Decisions K101–K108` table (the K108 row) and its blank line, insert:

```
## Decisions K109–K112

Taken in [the design record of 2026-10-05 on abstract definitions and inherited placeholders](../docs/superpowers/specs/2026-10-05-abstract-definitions-and-inherited-placeholders-design.md),
which carries the full argument for each. Written into `spec/` by
[the integration plan of 2026-10-05](../docs/superpowers/plans/2026-10-05-abstract-definitions-and-inherited-placeholders-integration-plan.md).
Together they narrow OQ9 again. K110 corrects the scope of three constraints in `spec/02` §12 — over the
template, the method of verification and the wording rule — which were written when `RequirementDefinition`
was the only abstract definition and which it never met; no decision is revised.

| # | Decision | Reason |
|---|---|---|
| K109 | A definition may be abstract: no requirement is produced under it, only under its specialisations. Every definition states whether it is abstract, a ninth attribute of the core, and a requirement naming an abstract definition is not well-formed | A definition that exists to be specialised, so that its descendants share what it declares, is something real material produced and the model could not state. Adopted from SysML v2 and KerML's `isAbstract`. Stated rather than inferred from an empty template, so that a definition nobody may produce a requirement under is not confused with one whose template is unwritten |
| K110 | An abstract definition carries no template, and neither *how it would be verified* nor the *wording rule* applies to it | All three speak of a requirement produced under the definition, and none is. *When it applies* and each parameter's *what to ask* still apply |
| K111 | Every parameter a definition that is not abstract has, its own and those it inherits, appears as a placeholder in its template, and every placeholder names one of them. This answers the part of OQ9 asking whether an inherited parameter must appear in a descendant's template | A template that could not name an inherited parameter would leave a filled value out of the wording; one that need not name it would let a requirement carry a value its wording never states. An abstract definition's parameters are met in its descendants' templates |
| K112 | An inherited parameter is inherited whole, its *what to ask* included; a specialisation has no ask of its own for it | K108 already makes it the same parameter with the same domain on every descendant, and the ask belongs to the parameter. A narrower ask on a narrower definition was considered and declined: every descendant is after the same value |
```

- [ ] **Step 2: Narrow OQ9 again**

Replace the paragraph from `**OQ9 is narrowed by K107 and K108.**` to `...whether an inherited parameter must
appear in a descendant's template.` with:

```
**OQ9 is narrowed by K107, K108, K111 and K112.** A specialisation has its ancestors' parameters, asks included,
and may not redeclare one; a definition that is not abstract uses every parameter it has in its template. What
stays open is whether an inherited parameter may ever be overridden or narrowed, and what specialisation means
for the rest of the core.
```

- [ ] **Step 3: Check the file against itself**

Search `spec/06-decisions.md` for `OQ9`. Every mention must agree with the paragraph of Step 2; the OQ9 row in
the `## Open questions, OQ9–OQ10` table stays as raised, as rows are kept elsewhere in this file.

- [ ] **Step 4: Commit**

```bash
git add spec/06-decisions.md
git commit -m "$(cat <<'EOF'
Add K109-K112 to spec/06-decisions.md; narrow OQ9 again

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 6: `CHANGELOG.md` — the release note

**Files:**
- Modify: `CHANGELOG.md` — under `## [Unreleased]`, `### Added`, after the entry beginning `- OQ30 and OQ31
  opened`

- [ ] **Step 1: Add the entry**

Insert, after that entry:

```
- Abstract definitions, and the placeholders a descendant inherits. A definition may be abstract — no
  requirement is produced under it, only under its specialisations — and states that it is, a ninth attribute
  of the core; it carries no
  template, and neither a method of verification nor a wording rule applies to it. A definition that is not
  abstract uses every parameter it has in its template, inherited ones included, and an inherited parameter
  brings its *what to ask*. K109–K112 record the decisions, and correct the scope of three constraints that the
  only earlier abstract definition never met; OQ9 is narrowed again. Findings are in
  [`docs/superpowers/specs/2026-10-05-abstract-definitions-and-inherited-placeholders-design.md`](docs/superpowers/specs/2026-10-05-abstract-definitions-and-inherited-placeholders-design.md).
```

- [ ] **Step 2: Commit**

```bash
git add CHANGELOG.md
git commit -m "$(cat <<'EOF'
Record abstract definitions and inherited placeholders in CHANGELOG

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 7: `spec/02` §7 and §8 — whether a definition is abstract is a ninth attribute of the core (K109)

**Added after Task 1 was reviewed, and executed directly after it, before Task 2.** Task 1 made every
definition state whether it is abstract without placing that statement among the core's attributes, so §7's
"two definitions carrying the same eight things are the same definition" became false. The owner decided on
2026-10-05 that abstractness is a ninth attribute of the core: it passes both of §8's tests, and the core is
what passes them.

**Files:**
- Modify: `spec/02-requirement-analysis-model.md` — §7's core table; the paragraph beginning `A definition
  carries eight things, three of which`; the paragraph beginning `**An abstract definition carries no
  template`; every sentence in §7 and §8 that counts the core as eight

**Interfaces:**
- Consumes: Task 1's two paragraphs.
- Produces: the core as nine attributes, which Task 5 cites.

- [ ] **Step 1: Add the row**

In §7's attribute table, after the `| name | Human-readable |` row, insert:

```
| abstract | Whether the definition is abstract: no requirement is produced under it, only under its specialisations (K109). Always stated, never read off the absence of anything else |
```

- [ ] **Step 2: Count nine**

Replace `A definition carries eight things, three of which do not apply to an abstract definition (below,
K110).` with `A definition carries nine things, three of which do not apply to an abstract definition (below,
K110).` In the paragraph beginning `**An abstract definition carries no template`, replace `The other five
apply as they do to any definition` with `The other six apply as they do to any definition`.

- [ ] **Step 3: Every other count**

Search §7 and §8 for `eight`. Each occurrence that counts the core's attributes becomes `nine` — at the time
of writing: `Two of the eight bottom out`, `any of the eight is how it is written down`, `carrying the same
eight things`, `is none of the eight` (the OQ28 paragraph), `will not be one of the eight`, `costs one of the
eight`, `Applied to the eight`. Rewrap each changed paragraph to 110 characters. Do not touch `spec/06`: its
rows are kept as taken, OQ28's included.

- [ ] **Step 4: Check §8 against the new attribute**

Re-read §8, *How the two tests relate*. If it walks the attributes one by one and gives each a verdict, add
abstractness's: it passes the seam test (it names nothing the metamodel does not define) and the record test
(§12 states a rule that fails on it — a requirement naming an abstract definition). If it does not walk them,
change nothing.

- [ ] **Step 5: Commit**

```bash
git add spec/02-requirement-analysis-model.md
git commit -m "$(cat <<'EOF2'
Make whether a definition is abstract a ninth attribute of the core (K109)

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
EOF2
)"
```
