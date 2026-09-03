# The SysML v2 binding

This is a binding, in the sense `spec/05-binding-contract.md` defines the word: it states the four things
that document's §4 asks of a design language attaching underneath ProjectML, for SysML v2, and nothing more
(K18). It is written against that document alone.

**Specification cited.** OMG Systems Modeling Language (SysML®) v2.0, Part 1: Language Specification, OMG
Document Number formal/2026-03-02 (<https://www.omg.org/spec/SysML/2.0/>), and the OMG Kernel Modeling
Language (KerML) v1.0, which SysML v2 is built on and which several terms cited below — `Feature`,
`Namespace`, `NAME` — belong to rather than to SysML itself.

## 1. Which of its elements may carry the seam edge

**Any `Feature` may be bound as the satisfying element.** SysML v2's `satisfy` is `05-binding-contract.md`
§2's `satisfies`, adopted by name and by direction (K3). The element that carries it is not one SysML kind
among several: `checkSatisfyRequirementUsageBindingConnector` requires a `SatisfyRequirementUsage` to hold
"a `BindingConnector` between its `subjectParameter` and some `Feature` other than the `subjectParameter`"
(§8.3.21.10), and every usage SysML defines — a part usage, an action usage, an item usage, and every other
kind — is a `Feature`. Nothing about the requirement side narrows this further at the base case: the subject
of `Requirements::RequirementCheck`, the most general requirement definition, is typed `Anything`
(§8.4.17.2), the most general type there is.

**That base case is the floor, not the whole answer.** SysML v2's own Systems Model Library ships several
requirement kinds that already narrow it: `FunctionalRequirementCheck` redefines the subject to `Action`,
`InterfaceRequirementCheck` to `BinaryInterface`, `PerformanceRequirementCheck` to `AttributeValue`, and
`PhysicalRequirementCheck` and `DesignConstraintCheck` to `Part` (§9.2.14.2.3–9.2.14.2.7). A requirement kind
is a specialization of `RequirementCheck` on the same terms as any other, so a model built on one of these —
or on a further specialization of one — inherits the narrower subject, and whatever satisfies that
requirement must be typed to conform to it. What this declaration can promise across every SysML model
without exception is *any `Feature`*; what a given requirement actually accepts is fixed by which kind it
specializes from, and can be narrower than that.

The relationship is written in either of two shapes, and both are open to whichever element carries it.
Standalone, naming both ends:

```
satisfy requirement sr : R by f;
```

or nested inside the satisfying element itself, which is then bound to it implicitly:

```
part p {
    satisfy requirement sr1 : R1;
}
```

Which shape a model uses is notation, and this binding does not choose between them (K52).

## 2. Its own internal refinement chain

SysML v2 does not have one such relation; it has several, and each is named here rather than one being
chosen to stand for the rest:

- **Containment.** A definition or usage nests other usages as its own features, most directly a part
  definition nesting part usages (§7.11).
- **Specialization, subsetting and redefinition.** A definition specializes another definition
  (`specializes`, or `:>`); a usage of a specialized definition subsets or redefines the corresponding usage
  of the definition it specializes (`subsets`/`redefines`, or `:>`/`:>>`) (KerML §8.2.3).
- **Allocation.** An `AllocationUsage` of an `AllocationDefinition` asserts that responsibility carried by a
  source feature is discharged by a target feature; an `AllocationDefinition` may itself be refined by
  nested `AllocationUsage`s that give a finer-grained mapping (§8.3.15.2–8.3.15.3).

Any one of these, or several together, is what a SysML v2 model offers where `05-binding-contract.md` §4.2
asks for a design language's own internal refinement chain.

## 3. Its identifier space, and the map

SysML v2 gives every element two names that serve different purposes: a **qualified name**, which places
the element in the package and namespace structure SysML itself builds, and an optional **short name**
(`declaredShortName`), written between angle brackets — `<name>` — ahead of the element's declaration
(KerML §8.2.2.3, §8.2.3.1). On a `RequirementDefinition` and on a `RequirementUsage`, the short name is
additionally exposed as `reqId`, described as "an optional modeler-specified identifier for this
`RequirementUsage` (used, e.g., to link it to an original requirement text in some source document), which
is the `declaredShortName` for the `RequirementUsage`" (§8.3.21.8, §8.3.21.9).

**The map:** a requirement's identity, in this metamodel's sense, is the `reqId` of the `RequirementUsage`
that names it. The usage's qualified name remains SysML's own and is not read as the requirement's
identity. The pairing sits on the element itself, in both directions — given a requirement identifier, the
element carrying it is the usage whose `reqId` equals that identifier; given the usage, its `reqId` is read
directly off it — which is what `05-binding-contract.md` §4.3 asks a naming scheme alone not to be trusted
to supply on its own.

`declaredShortName` is not confined to identifier-shaped text. SysML's `NAME` production is either a
`BASIC_NAME` — letters, digits and underscore — or, written in single quotes, an `UNRESTRICTED_NAME`, which
admits any printable character (KerML §8.2.2.3). A requirement identifier that is not a legal `BASIC_NAME`
on its own — one containing a hyphen, for instance — is written quoted, and still has a `reqId` to occupy.

## 4. How far it takes the value model

Not at all. A SysML attribute either carries a value or it does not; nothing in the language distinguishes
a value that was stated from one that was assumed, derived from other values, missing and known to be
missing, or contested between two sources that disagree. `04-value-states.md`'s five states have no
counterpart in SysML v2's own constructs: a value crossing the seam into a SysML element's attribute is
written as a bare value, and the state that stood beside it in the requirement model does not cross with
it.
