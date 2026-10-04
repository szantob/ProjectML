# Abstract definitions, and the placeholders a descendant inherits — Design record

**Status: settled, and not yet written into `spec/`.** This record carries decisions K109–K112 and answers
two further parts of OQ9. It also stands behind OQ30 and OQ31, which were opened in `spec/06-decisions.md`
ahead of it, on the same day, so that they would not wait for the rest. `spec/` has not otherwise been
changed: the change touches `spec/02-requirement-analysis-model.md` §7, §9, §10 and §12, and
`spec/06-decisions.md`, and gets a plan of its own.

**Date:** 2026-10-05
**Follows:** [`2026-09-28-value-domain-comparability-and-guard-design.md`](2026-09-28-value-domain-comparability-and-guard-design.md),
whose K107 and K108 settled what a descendant has of its ancestors' parameters and left the rest of OQ9 open.
**Began as:** the first implementation package to use parameter inheritance, which could not use it.

The next numbers free were K109 and OQ30.

---

## 1. Where this record came from

K107 gives a descendant every parameter its ancestors declare, and K108 forbids it declaring one again. The
first implementation package to rely on that wanted a parameter declared once, high in its tree, and used by
the descendants' templates — the same parameter, filled the same way, in every requirement beneath it.

It ran into two things the metamodel left open. **Whether a descendant's template may name an inherited
parameter at all** is OQ9's own question, and the checking an implementation built in its absence measured
each definition alone: a template could name only the definition's own parameters, and every parameter a
definition declared had to appear in its own template. And the definition at the top of such a tree **was
never meant to produce a requirement**. It existed so that its descendants could share what it declared, and
nothing in the metamodel could say so: its empty template read as a template not yet written.

The package worked around both by declaring the shared parameter again on every definition that used it,
which K108 forbids. The workaround is the evidence: the package had to break a decided rule to say what the
decisions were for.

## 2. A definition may be abstract

`RequirementDefinition` is abstract (K30), and so far it has been the only abstract definition the metamodel
admits. Every specialisation of it has been assumed to be one a requirement can be produced under.

| # | Decision | Reason |
|---|---|---|
| K109 | **A definition may be abstract: no requirement is produced under it, only under its specialisations.** Every definition states whether it is abstract — a ninth attribute of the core — and that it is cannot be read off the absence of anything else. A requirement naming an abstract definition is not a well-formed element of this model | A definition that exists to be specialised, so that its descendants share what it declares, is a thing real material produces and the model could not state. The term is adopted, not coined: SysML v2 marks a definition `abstract`, and KerML's `isAbstract` says that whatever a type classifies must also be classified by one of its specialisations — the same reading, one level down. Stating it rather than inferring it from an empty template keeps two things apart that an inference would merge: a definition nobody may produce a requirement under, and one whose template nobody has written yet |
| K110 | **An abstract definition carries no template, and neither *how it would be verified* nor the *wording rule* applies to it.** The other attributes of the core apply as they do to any definition | All three speak of a requirement produced under the definition — the wording it is produced from, how it would be shown to hold, what its wording must satisfy — and no requirement is produced under an abstract definition. *When it applies* still decides when the definition comes into play, and a parameter declared on it is still filled, through its descendants, so each still needs its *what to ask* |

**What K110 does to `spec/02` §12, and why it is a correction rather than a revision.** Three constraints there
say that *every* definition states a template, a method of verification and a wording rule. They were written
when the only abstract definition was `RequirementDefinition` itself, and it never satisfied them either: an
abstract type with no template, no method and no rule was already in the model the constraints claimed to
cover, and the contradiction was overlooked rather than resolved. K110 states the scope they always needed.
Each applies to a definition that is not abstract. One of them also anticipated, in its own text, a definition
whose requirements cannot be verified independently, and asked it to say so in the attribute; that remains
right for a definition requirements are produced under, and is not what an abstract definition is.

**A `CompletenessRule` may imply an abstract definition** without any change. Its test is already satisfied by
a requirement produced under the implied definition "or under any specialisation of it"
(`03-project-lifecycle-model.md` §3, K95), and under K109 a specialisation is the only way it can be satisfied.

## 3. What a descendant's template uses

| # | Decision | Reason |
|---|---|---|
| K111 | **Every parameter a definition that is not abstract has — its own and those it inherits — appears as a placeholder in its template, and every placeholder in its template names one of them.** An abstract definition's parameters are therefore met in the templates of its descendants, never in its own. This answers the part of OQ9 that asked whether an inherited parameter must appear as a placeholder | K107 already gives a descendant the inherited parameter, and the derivation fills it like any other (§10). A template that could not name it would leave a filled value nowhere in the requirement's wording; a template that need not name it would let a requirement carry a value its wording never states. Both directions follow from the template being the place a requirement's wording is produced from (§7) |
| K112 | **An inherited parameter is inherited whole: its identity, its domain and its *what to ask*.** A descendant has no ask of its own for a parameter it inherits | K108 already makes an inherited parameter the same parameter on every descendant, with the same domain. The ask belongs to the parameter (§7), so it travels with it. The owner considered letting a descendant restate the ask, so that a narrower definition could ask a narrower question, and decided against it: a well-written ask on the declaring definition serves every descendant, because every descendant is after the same value |

**What OQ9 still holds.** Whether an inherited parameter may ever be overridden or narrowed, and what
specialisation means for the rest of the core. K112 does not settle narrowing: a narrower ask was the only
narrowing considered, and it was declined, not ruled out for the parameter itself.

## 4. The syntactic constraints this changes

Each is decidable without judgement. Each failing is a failed check, except the fourth, whose failure leaves an
element that is not well-formed, on the terms K9 and K61 set. They belong to
`02-requirement-analysis-model.md` §12.

1. **Changed.** A definition that is not abstract states its template, how a requirement produced under it
   would be verified, and its wording rule (K110). This replaces the three constraints that said *every*
   definition does.
2. **New.** An abstract definition carries no template (K110).
3. **New.** In the template of a definition that is not abstract, every placeholder names a parameter the
   definition has, declared or inherited, and every parameter it has appears as a placeholder (K111).
4. **Changed.** A requirement names exactly one definition, and that definition is not abstract (K109). This
   extends the existing constraint over the derivation.

## 5. What came out of this, and is not settled here

Working out what an inherited parameter carries raised two questions bigger than this record. The owner judged
that either may restructure part of the metamodel, and that the model has developed, where they touch it,
differently from what was originally intended. Both were opened at once, at high priority, in
`spec/06-decisions.md`, and neither is entered here:

- **OQ30.** What a definition's *what to ask* produces when a parameter's value is missing. Today the answer is
  a value in the unknown state and no question (§11, K87), and by the owner's principle — only what
  rule-raised questions and conflicts carry reaches the project manager — that is a silent failure, not an
  answer.
- **OQ31.** Whether one requirement can be an aspect of another. The package arranged aspects of a thing
  beneath it in the specialisation tree, whose relation is *is a kind of*, and asked in prose that an aspect's
  value follow the value of the requirement it qualifies. K107 inherits a parameter, never a value.

Nothing in K109–K112 depends on how either is answered.

## 6. Notes for the plan

- `02-requirement-analysis-model.md` §7: an abstract definition (K109) and what it carries (K110), beside the
  core table; the sentence that the core applies to every definition qualified accordingly.
  **Amended during integration, 2026-10-05:** the owner placed abstractness in the core as its ninth attribute,
  since it passes both of §8's tests; §7 and §8 count nine.
- §9: K111 and K112 in the paragraph that narrows OQ9, and what OQ9 still holds.
- §10: a requirement is never produced under an abstract definition (K109).
- §12: the four constraints of section 4.
- `06-decisions.md`: K109–K112, stating that K110 corrects the scope of three §12 constraints; OQ9 narrowed
  again.
- The §9 class diagram needs no change. If an abstract kind is ever drawn, it uses the diagram language's own
  abstract marking (K32), on a placeholder named after no real kind.
- Check K109's citation of KerML's `isAbstract` against the primary specification before it is written in, as
  the binding's citations were.
- The editor whose package raised this follows in its own repository: a new schema version that states a kind
  is abstract, and the contract's checks for section 4. That is implementation, and gets its own design there.
