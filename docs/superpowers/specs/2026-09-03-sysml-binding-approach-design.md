# How phase 2 is organised, and what a first reading of SysML v2 found — Design record

**Status: the approach is settled, and [`bindings/sysml-v2.md`](../../../bindings/sysml-v2.md) is written.**
This record carries K55 and the findings behind each of K4's four declarations, checked against the OMG
specification itself rather than left on the secondary sources §2 first drew on. It exists so that a later
reader does not have to rediscover them, and so that the binding, which by K18 states the four declarations
and nothing more, has somewhere to carry what writing it found.

**Date:** 2026-09-03
**Written against:** [`spec/05-binding-contract.md`](../../../spec/05-binding-contract.md), which is the
only document the binding is written against and is meant to be sufficient on its own.
**Findings below are confirmed against the OMG specification**: OMG Systems Modeling Language (SysML®) v2.0,
Part 1: Language Specification, formal/2026-03-02, and OMG Kernel Modeling Language (KerML) v1.0, which
SysML v2 is built on. Section numbers cited are from those two documents. The secondary sources §3 lists
were where this record started; they are superseded by the primary source, not corrected by it — nothing
they said turned out to be wrong.

---

## 1. How phase 2's output is organised

| # | Decision | Rationale |
|---|---|---|
| K55 | **The binding and the findings are two documents.** The binding lives in `bindings/` and states K4's four declarations and nothing more, as K18 requires. What writing it discovers about the metamodel — where SysML resists, and what a declaration turns out to buy or not buy — goes in a design record beside it | They answer different questions for different readers. The binding's reader owns a design language and wants to attach; the findings' reader is this project, asking whether K2 holds. K18's *nothing more* exists so that a binding does not swell into an implementation, and honouring it costs nothing once the findings have somewhere else to go. Folding them together would blur the distinction K18 draws; leaving the findings unwritten would discard phase 2's most valuable output, since the binding is the first place K2 is tested rather than asserted |

## 2. What checking against the specification found, declaration by declaration

### 4.4 — how far it takes the value model

**The answer is that it does not, and this is the declaration working rather than failing — confirmed by
the absence rather than by a statement.** Nothing in the specification's attribute semantics carries a
notion of a value being assumed rather than stated, or conflicting because two sources disagree; an
attribute has a value or it has none. The prior art's own mapping records that a translation **flattens**
the five states away, and the primary source gives no reason to expect otherwise — there is no construct to
have missed. The binding says so plainly, and what it says is exactly the information a future resolution of
OQ1 would work from.

### 4.3 — the identifier space, and the map

**This looked like the hard one and has a good answer, now confirmed.** SysML v2 gives every element more
than one name: a declared name, a qualified name that navigates the package hierarchy, and a **short name**
(`declaredShortName`), written between angle brackets, whose purpose is concise identifiers of exactly the
kind a requirement carries. On a `RequirementDefinition` and a `RequirementUsage` specifically, that short
name is additionally exposed as `reqId`, described in the specification itself as existing to "link it to an
original requirement text in some source document" (SysML §8.3.21.8, §8.3.21.9) — which is this metamodel's
requirement identity, named almost exactly.

So the map the declaration demands can be carried element by element, in the model itself: **a requirement's
identity from this metamodel becomes the SysML element's `reqId`, while its qualified name remains the
design language's own.** The pairing sits on each element and resolves in both directions, which is what the
declaration asks for and what a naming scheme alone would not supply.

**The second half of the risk — whether a short name is free text — is also confirmed, and favourably.**
SysML's `NAME` production is either a `BASIC_NAME` (letters, digits, underscore) or, written in single
quotes, an `UNRESTRICTED_NAME`, which "shall consist of the characters within the single quotes... any
printable character other than backslash or single_quote" (KerML §8.2.2.3). A requirement identifier that is
not a legal `BASIC_NAME` on its own is written quoted rather than refused, so no identifier scheme this
metamodel might use is unrepresentable as a `reqId`.

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

**Answerable, with a wording mismatch worth noting, and confirmed as four named mechanisms rather than
one.** SysML v2 has not one such relation but several: containment, where a definition or usage nests other
usages as its own features (§7.11); specialization, subsetting and redefinition, at the definition and usage
levels respectively (KerML §8.2.3); and allocation, where an `AllocationUsage` asserts that a source
feature's responsibility is discharged by a target feature, and may itself be refined by nested allocations
(§8.3.15.2–8.3.15.3). The declaration is phrased in the singular, *its own internal refinement chain*.
Nothing is blocked: the binding states what SysML has. But a design language richer than the declaration's
phrasing expects is a small sign that the phrasing, not the language, is what needs adjusting.

### 4.1 — which of its elements may carry the seam edge

**Confirmed, and it is the problem that was suspected rather than a different one.** The specification is
direct about it: `checkSatisfyRequirementUsageBindingConnector` requires a `BindingConnector` between the
requirement's `subjectParameter` and "some `Feature` other than the `subjectParameter`" (SysML §8.3.21.10) —
`Feature`, not `PartUsage` or any narrower kind. A requirement's own subject parameter carries no narrowing
either: the base subject in the Systems Model Library is typed `Anything` (§8.4.17.2), the most general type
KerML has, and is narrowed only by whatever a particular requirement definition's author chooses to
specialize it to. Every usage SysML defines — a part usage, an action usage, an item usage, an allocation
usage, and everything else — is a `Feature`, so every one of them is eligible on the same terms.

The honest answer is not merely *essentially any usage*, which is what the secondary sources' silence had
left open; it is *any `Feature`* — a KerML-level concept broader than "usage" itself. The declaration is
**satisfied on paper and buys nothing for SysML v2's own most important case**: its stated purpose in
`05-binding-contract.md` §4.1 is to turn a check over the seam from a search into a lookup, and naming every
kind of `Feature` there is turns nothing. A binding that reported this faithfully — which
[`bindings/sysml-v2.md`](../../../bindings/sysml-v2.md) §1 does — is not thereby wrong or incomplete; it is
reporting SysML v2 accurately, and the declaration's purchase is what the specification actually gives.

**This does not make K2 false.** SysML attaches on the same terms as anything else would; nothing about this
finding treats SysML differently from a hypothetical stricter design language. What it means is that one of
K4's four declarations is worth less, for the specific case phase 2 exists to test, than
`05-binding-contract.md` §4.1 claims for it in general — a finding about the contract rather than about
SysML, produced by exactly the mechanism phase 2 was built to produce it: writing a real binding rather than
reasoning about one in the abstract. It is recorded here rather than acted on; changing §4.1 is not this
binding's decision to take on its own, and nothing yet says the declaration is wrong for a design language
that does narrow its own subject types, only that SysML v2, as specified, does not.

## 3. Where this landed

The binding is written, at [`bindings/sysml-v2.md`](../../../bindings/sysml-v2.md), in the order this
section originally recommended — 4.3 first, then 4.1, then 4.2 and 4.4 — though the document itself presents
the four declarations in the contract's own order, 4.1 through 4.4, since that is the order its reader
already knows from `05-binding-contract.md`.

Both things this record left to settle against the primary source rather than a secondary one — what may be
the subject of a requirement, and whether a short name is free text — are settled above, in §2, and neither
overturned what the secondary sources had suggested.

**Sources.** OMG Systems Modeling Language (SysML®) v2.0, Part 1: Language Specification, OMG Document
Number formal/2026-03-02 (<https://www.omg.org/spec/SysML/2.0/Language/PDF>); OMG Kernel Modeling Language
(KerML) v1.0 (<https://www.omg.org/spec/KerML/1.0/PDF>). The secondary sources the first reading started
from, kept for their own record: [Visual Paradigm — Packages, Namespaces and
Libraries](https://sysml.visual-paradigm.com/docs/sysml-v2-studio-kick-start-guide/cohesive-system-model-in-8-views/step-1-building-the-foundation-packages-namespaces-and-libraries/),
[Sensmetry — Requirements](https://sensmetry.com/advent-of-sysml-v2-lesson-22-requirements/),
[Sensmetry — Requirement Satisfaction and Verification](https://sensmetry.com/advent-of-sysml-v2-lesson-24-requirement-satisfaction-and-verification/),
[PTC — SysML v2 Requirements](https://support.ptc.com/help/modeler/r10.1/en/Modeler/sysml2/requirements.html),
[OMG SysML v2](https://www.omg.org/sysml/sysmlv2/).
