# What specialisation does to the rest of the core, and where verification lies — Design record

**Status: settled, and written into `spec/` in the same pass.** This record carries decisions K113 and K114,
narrows OQ9 again, closes OQ10, and opens OQ32. The change is small enough that it needs no separate plan:
`spec/02-requirement-analysis-model.md` §9, `spec/05-binding-contract.md` §5 and `spec/06-decisions.md` change,
and `CHANGELOG.md` records it.

**Date:** 2026-10-05
**Follows:** [`2026-10-05-abstract-definitions-and-inherited-placeholders-design.md`](2026-10-05-abstract-definitions-and-inherited-placeholders-design.md),
which left OQ9 holding two things: whether an inherited parameter may be overridden or narrowed, and what
specialisation means for the rest of the core.
**Began as:** the owner going through that remainder attribute by attribute.

The next numbers free were K113 and OQ32.

---

## 1. Two attributes of the core are not inherited

The owner answered two of the core's attributes directly.

| # | Decision | Reason |
|---|---|---|
| K113 | **A definition's template is not inherited.** A specialisation that is not abstract states its own (K110, K111) | A template is the sentence a requirement of exactly this definition is produced from. A specialisation handed its ancestor's sentence would produce requirements that say what the ancestor's say, and the specialisation would lose what makes it one. K111 already has a concrete specialisation's template use the parameters it inherits; what it inherits is the parameters, never the sentence |
| K114 | **A definition's *when it applies* is not inherited** | *When it applies* guides the modeller — a person or an agent — while a new requirement is being classified: it says which branch of the tree is worth following. Each definition states when it, and not its ancestor, comes into play; an inherited statement would point every branch the same way and guide nothing |

**Abstractness is not inherited either, and needs no decision of its own.** K109 makes a definition abstract
only where it says so, so a specialisation of an abstract definition is not abstract unless it says it is.

## 2. What OQ9 still holds, with the evidence so far

Three questions remain, and the owner is not yet convinced either way on any of them.

- **The wording rule.** Whether it is inherited, and whether it belongs to the definition at all. The first
  package to use parameter inheritance wrote, on its abstract root, a wording rule that is about the parameter
  that root declares — how the shared parameter's value must be phrased — not about any sentence. That
  suggests two kinds of rule under one attribute: one about how a parameter's value is written, which could
  travel with the parameter as its *what to ask* does (K112), and one about the whole sentence, which belongs
  to the definition. The owner's concern is the opposite case: a rule inherited from an ancestor that is too
  narrow would contradict a specialisation's own rule or its template. A rule that travels with a parameter
  cannot do that, since it constrains no sentence; a rule about the sentence could.
- **How it would be verified.** Whether it is inherited. The owner's observation runs against the obvious
  reading: the more abstract a definition, the harder it is to show that a requirement under it holds, so an
  ancestor's method is more complicated than a specialisation's, not more general. Whether the attribute is
  needed on a definition at all is a separate question, OQ32.
- **An inherited parameter.** Whether it may ever be overridden or narrowed. The one candidate so far, a
  narrower *what to ask*, was declined (K112); nothing else has asked for it.

## 3. Where verification lies

OQ10 asked whether a second edge kind, for verification, joins the seam. **The owner's answer is that it does
not, because verification is not on the seam but beyond it.** A baseline holds the client's requirements and
the derivations between them, and nothing else. What satisfies a requirement, and what verifies it, are design
decisions: they belong to the requirement model a design language builds after the project model, beyond the
seam, on that language's own terms. This is the same answer K56 gave for a requirement's subject — the design
language's affair, with nothing on the kernel's side to supply or need — and it agrees with `spec/05` §2,
which already says that whether a requirement is actually met is verification, and that this metamodel does
not undertake it (K7, K40).

**OQ10 is closed.** It is answered by the owner rather than by a binding, as it said it would be, because the
answer does not depend on any one design language: every one of them verifies on its own side.

## 4. What this opens

| # | Question | When answerable |
|---|---|---|
| OQ32 | Does a definition need *how it would be verified* at all? K29 made it required, and `spec/02` §12 makes its absence on a definition that is not abstract a failed check. Its reason was that a verification method is generic to a kind. But if verification is a design decision taken beyond the seam (OQ10, closed), the method a definition states is at most guidance to whoever designs, and for a definition far up the tree it would have to be the most complicated of all (OQ9). Three answers are open: required as now; kept but optional, so that its absence is a gap rather than a failed check; or removed from the core | When a baseline is carried into a design language's requirement model, and it shows whether anything there reads the method a definition stated |

## 5. Notes for the change

- `spec/02` §9: the closing OQ9 paragraph states K113 and K114, that abstractness is not inherited, and what
  OQ9 still holds.
- `spec/05` §5: the OQ10 bullet becomes a closed one, pointing at `spec/06`.
- `spec/06`: K113–K114 after K109–K112; the OQ9 narrowing paragraph updated; a closing paragraph under the
  OQ9–OQ10 table for OQ10; OQ32 opened after OQ30–OQ31.
- The editor and the skills repository change nothing: they never inherited a template or a *when it applies*.
