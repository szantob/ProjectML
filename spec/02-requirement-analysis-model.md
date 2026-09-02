# The requirement analysis model

## 1. What this model is

This is the requirement analysis model, one member of the collection ProjectML metamodels (K19). It is the
**working model**: the model in which a requirement system is actually built, rather than the model handed
to somebody who was not in the room while it was assembled. Six things make it up, and naming all six here
lets a reader see the shape before meeting the parts: `Source`, the material a project starts from; `Need`,
a passage of a source anchored so it can be worked with; `RequirementDef`, the definition a requirement is
produced under; `Requirement`, the bound statement that definition yields; `Decision`, the record of why one
outcome was chosen over another; and the findings a review produces over all of it. This document defines
the first three — `Source`, the edge between sources, and `Need` — in the sections that follow.
`RequirementDef`, the derivation it governs, `Decision` and the findings follow after.

This model **projects** to the requirement model: the product model defined in `01-requirement-model.md`,
which a reader can adopt on its own, without ever having read this document (K20). The projection itself —
what it keeps, what it drops, and on what condition it may drop anything — is defined in section 8 of this
document, once everything the projection draws from has been introduced.

One rule governs everything that follows, and it is stated here because every section after this one assumes
it: **the requirement model changes only through sources** (K11). A review does not update a need, a
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
   source — a need's passage, a citation in a review — points at words that stay exactly as given; if the
   source could change after the fact, an anchor into it would drift from what was actually said, and a
   later reader could no longer tell whether a quotation still means what it meant when it was captured.
2. **A source is never decomposed.** The raw material stays raw rather than being broken into parts and
   classified as it is captured, which would impose a classification taxonomy on it before anyone has asked
   what the material is for (D25).
3. **The kind and origin attributes are open.** The metamodel names them and leaves their vocabularies to an
   implementation. The founding record's section 5 records why: EventML's own enumerations for these two
   attributes carry a domain leak — a kind of material and two origins that exist only in that domain — and
   a vocabulary fixed here would carry the same leak into every project that adopts this metamodel. This is
   the same move K30 makes for a requirement's kind: the metamodel provides the attribute a vocabulary fills
   without naming what goes into it.

## 3. The edge between sources

Sources connect to each other through exactly one edge: a later source **`answers`** an earlier one. Four
properties hold of it, each already settled:

- It sits on the later source and names the earlier one it responds to, not the reverse (D35).
- It points backward in time: a source can only answer something that came before it (D38).
- It changes no value on its own. Both the earlier statement and the later one stand as made; which of them
  prevails, if they disagree, is not decided by the edge but by a decision recorded separately (D37).
- One edge relates exactly one source to exactly one source, but a source may carry any number of them —
  answering several earlier sources, or being answered by several later ones (D29).

The founding record's section 5 makes a further finding about this edge worth carrying forward here: `answers`
is the natural closing edge for a review finding. A finding is opened by a source and closed by a later one
that answers it, so closing a finding is not a tick somebody applies to a record — it is itself evidence,
carrying the same source that closes it as everything else in this model does. Section 7 uses the edge on
exactly these terms.

## 4. `Need`

A need carries three things.

| Attribute | Carries |
|---|---|
| identity | A stable identifier, distinct from every other need's |
| passage | The passage of the source the need anchors into |
| value | An optional value, carrying a value state on the same terms as any other value in the collection (see `04-value-states.md`) |

The name is adopted rather than coined: *stakeholder need* is ISO/IEC/IEEE 29148's term (D23).

Three rules govern a need.

1. **A need anchors into exactly one passage of exactly one source.** A need with no anchor fails a
   syntactic check: there is nothing in it to ask a stakeholder about, and nothing for a reviewer to weigh —
   only an omission to fix (D34, D26).
2. **A need carries no lifecycle state.** It belongs to its source, a source is material of record, and a
   quotation cannot cease to be true — there is no state for a need to hold that would ever change while the
   source behind it stays what it was (K6).
3. **A need's value is optional**, and where present it carries a value state like any other value in the
   collection: stated, derived, assumed, unknown, or conflicting (D27).

Passage anchoring adopts the W3C Web Annotation Data Model (D26). No requirements standard was adopted for
it instead, because none serves here: a need anchors into a source before any requirement exists, at a stage
SysML v2 places outside itself and has nothing to say about.
