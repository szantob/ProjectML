# How phase 2 is organised, and what a first reading of SysML v2 found — Design record

**Status: the approach is settled; the binding is not written.** This record carries K55 and the findings of
a first reading against each of K4's four declarations. It exists so that writing the binding does not have
to rediscover them.

**Date:** 2026-09-03
**Written against:** [`spec/05-binding-contract.md`](../../../spec/05-binding-contract.md), which is the
only document the binding is written against and is meant to be sufficient on its own.
**Findings are preliminary and rest on secondary sources.** The normative check against the OMG
specification is part of writing the binding.

---

## 1. How phase 2's output is organised

| # | Decision | Rationale |
|---|---|---|
| K55 | **The binding and the findings are two documents.** The binding lives in `bindings/` and states K4's four declarations and nothing more, as K18 requires. What writing it discovers about the metamodel — where SysML resists, and what a declaration turns out to buy or not buy — goes in a design record beside it | They answer different questions for different readers. The binding's reader owns a design language and wants to attach; the findings' reader is this project, asking whether K2 holds. K18's *nothing more* exists so that a binding does not swell into an implementation, and honouring it costs nothing once the findings have somewhere else to go. Folding them together would blur the distinction K18 draws; leaving the findings unwritten would discard phase 2's most valuable output, since the binding is the first place K2 is tested rather than asserted |

## 2. What a first reading found, declaration by declaration

### 4.4 — how far it takes the value model

**The answer is that it does not, and this is the declaration working rather than failing.** SysML gives an
attribute a value or no value; it has no notion of a value being assumed rather than stated, or conflicting
because two sources disagree. The prior art's own mapping records that a translation **flattens** the five
states away. The binding says so plainly, and what it says is exactly the information a future resolution of
OQ1 would work from.

### 4.3 — the identifier space, and the map

**This looked like the hard one and has a good answer.** SysML v2 gives every element more than one name: a
declared name, a qualified name that navigates the package hierarchy, and a **short name**, written between
angle brackets, whose purpose is concise identifiers of exactly the kind a requirement carries.

So the map the declaration demands can be carried element by element, in the model itself: **a requirement's
identity from this metamodel becomes the SysML element's short name, while its qualified name remains the
design language's own.** The pairing sits on each element and resolves in both directions, which is what the
declaration asks for and what a naming scheme alone would not supply.

**Why it holds is worth recording, because it is not obvious.** Representing a baseline's requirements
inside a SysML model would ordinarily raise a synchronisation problem — two copies drifting apart. It does
not here, because **a baseline is frozen**: the copy cannot drift within the baseline it was taken from, and
moving to a later baseline means re-importing rather than reconciling. K21's argument for binding to a
baseline rather than to the live projection pays off concretely at this point, which is the first
independent confirmation it has had.

The prior art's own worked comparison already documents the problem this solves: a catalogue identifier in
one space becomes a qualified name in the other, and *"a round trip needs an ID map, or the stable IDs that
every model depends on are lost."*

### 4.2 — its own internal refinement chain

**Answerable, with a wording mismatch worth noting.** SysML v2 has not one such relation but several — part
containment, specialisation, redefinition, allocation. The declaration is phrased in the singular, *its own
internal refinement chain*. Nothing is blocked: the binding states what SysML has. But a design language
richer than the declaration's phrasing expects is a small sign that the phrasing, not the language, is what
needs adjusting.

### 4.1 — which of its elements may carry the seam edge

**This is where a problem may be, and it is not the problem that was expected.** In SysML v2 a requirement
must have a subject, and `satisfy` binds that subject to the element the usage is nested in. Secondary
sources say the subject is a model element *such as a part* and do not enumerate what else may be one.

If the honest answer turns out to be *essentially any usage*, the declaration is **satisfied on paper and
buys nothing**: its stated purpose is to turn a check over the seam from a search into a lookup, and a
declaration naming every kind there is has turned nothing.

**That would not make K2 false.** SysML would still attach on the same terms as anything else. It would mean
one of the four declarations is worth less to the most important case than the contract claims — which is a
finding about the contract rather than about SysML, and exactly the kind phase 2 exists to produce.
Settling it needs the specification itself, not a secondary source.

## 3. Where to resume

The binding is not started. `bindings/` does not exist.

Work in declaration order 4.3, 4.1, 4.2, 4.4 rather than 1 to 4: the third has the answer that shapes the
document, the first has the only open risk, and the remaining two are largely written already by the
findings above.

**Two things to settle against the OMG specification rather than a secondary source**, because both bear on
findings rather than on wording: what may be the subject of a requirement in SysML v2, which decides 4.1;
and whether a short name is free text or constrained, which decides whether 4.3's map can be carried the way
§2 proposes.

**Sources for this record**, all secondary:
[Visual Paradigm — Packages, Namespaces and Libraries](https://sysml.visual-paradigm.com/docs/sysml-v2-studio-kick-start-guide/cohesive-system-model-in-8-views/step-1-building-the-foundation-packages-namespaces-and-libraries/),
[Sensmetry — Requirements](https://sensmetry.com/advent-of-sysml-v2-lesson-22-requirements/),
[Sensmetry — Requirement Satisfaction and Verification](https://sensmetry.com/advent-of-sysml-v2-lesson-24-requirement-satisfaction-and-verification/),
[PTC — SysML v2 Requirements](https://support.ptc.com/help/modeler/r10.1/en/Modeler/sysml2/requirements.html),
[OMG SysML v2](https://www.omg.org/sysml/sysmlv2/).
