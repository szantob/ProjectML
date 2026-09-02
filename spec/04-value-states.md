# Value states

This is the value-state model, one member of the collection ProjectML metamodels (K19).

## 1. Incompleteness is data

A value in the model records not only what it is, but what is known about it: where it came from, whether
it follows from something else, or whether it is missing outright. This second dimension is the value's
**value state**, and it is carried alongside the value rather than inferred from it.

The material a project starts from is incomplete by nature — some of it is stated outright, some of it has
to be inferred, some of it is missing and known to be missing, some of it is supplied provisionally so work
can continue, and some of it is contested between sources that disagree. The metamodel records this
incompleteness rather than requiring it to be resolved before modelling begins. K1 puts the value-state
model in the kernel on that ground: incomplete information is not an exceptional state a project passes
through on its way to something better, it is the normal condition of the material the requirement model is
built from.

## 2. The five states

A value carries exactly one of five states.

| State | Meaning | Requires |
|---|---|---|
| stated | Somebody said it | The source it came from |
| derived | It follows from a rule or another value | The reasoning |
| assumed | It was supplied to keep moving | The reasoning |
| unknown | Missing, and known to be missing | Nothing |
| conflicting | Two sources disagree | The competing values, each with its source |

Separately from which of these five states a value carries, a value may be marked as one to ask about. This
marking is independent of state: a stated value can still need a stakeholder's confirmation, an assumed
value can be exactly the kind of thing that marking exists for, and even a derived or conflicting value can
be worth raising with somebody, even though nothing about the marking is fixed to any one of the five rows
above. The marking says a value is open for enquiry; the state says what kind of thing the value currently
is.

## 3. Assumed against derived

The distinction between assumed and derived is the one that does the most work in this model. An assumption
is a choice made in the absence of information: something had to be supplied so work could continue, it may
turn out to be wrong, and somebody with standing to know may need to confirm or correct it. A derived value
follows necessarily from something already established in the model — a rule applied to values already
present — and asking a stakeholder to confirm it is asking them to re-verify arithmetic they had no part in;
what would actually change a derived value is a change to its inputs, not a conversation. Conflating the two
produces a question list clogged with things nobody can usefully answer, because the honest answer to "is
this derived value right" is always "check the rule and the inputs," never a fact only a person holds.

## 4. Reach

A value state applies to any value in any member of the collection, not to one kind of element within it: a
value carried by a need, a parameter of a requirement, a value belonging to a design language's own elements
beyond the seam all carry a value state on the same terms. It is a property of values wherever they occur,
not a property confined to one member of the collection. `D27` records this for a need's own value
specifically — a need's value is optional, and the value-state model applies to it exactly as it applies
everywhere else, untouched by that optionality. The founding record's section 5 finding is the general form:
the value-state model attaches to every value or to none, and it is this reach, set against the evidence
chain's single seam, that forces the collection into the two-piece structure standing behind `OQ1`.

## 5. What is deliberately not here

**Progressive wrapping is notation and is not here.** How a notation tells a plain value apart from one
carrying a state — a bare scalar sitting next to an object, or any other device — is a decision a notation
makes, and notation belongs to an implementation (`K15`). The five states are what is portable; the device
that marks their presence in a written model is not. The founding record's section 7 puts the line exactly
where this document holds it: *"the value states are portable where progressive wrapping is notation and is
not."*

**The metamodel enumerates no value domains.** A value has a domain — the range of things it could be — but
which domains exist, and what they are called, is declared by an implementation rather than fixed here. This
is the same move `K30` makes for requirement kinds: the metamodel provides the slot a domain fills without
naming what goes into it.
