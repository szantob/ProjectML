# Contributing

## 1. Scope

This repository is in phase 1: the metamodel is a draft and nothing is published. It accepts corrections and
conceptual proposals. **It does not accept code, notation, schemas or requirement templates** — this
repository ships prose and diagrams only, and that is a locked decision rather than a stage it will grow out
of. See [`CLAUDE.md`](CLAUDE.md) §1 and §3.

The most useful contribution at this stage is an argument against something in
[`docs/2026-08-26-kernel-brainstorm.md`](docs/2026-08-26-kernel-brainstorm.md) — either against a decision
that turns out not to hold, or an answer to one of the open questions.

## 2. What belongs where

| You want to propose | Where it goes |
|---|---|
| A concept, type, edge or state the metamodel is missing | `spec/`, once it exists — open an issue first |
| How a design language attaches | `bindings/`, one document per language |
| A file format, a schema, a validator | Not here. That is an implementation |
| A requirement template, or a set of requirement kinds | Not here. Implementations declare their own |
| A domain vocabulary | Not here, for the same reason |

## 3. The test for whether something belongs

Could the sentence be true of a project modelled in YAML, in SysML v2 textual notation, and in a
spreadsheet, without change? If yes, it is metamodel. If it assumes one of them, it is implementation.

## 4. Style

- English throughout.
- Adopt terms from ISO/IEC/IEEE 29148, ISO/IEC/IEEE 42010, SysML v2 and W3C PROV rather than coining
  synonyms, and cite where a term came from. Coin only where no standard has one.
- Say what an element *is* in the model, never which file or header key carries it.
- "Attribute", not "field".
- Diagrams in Mermaid, so they render on GitHub and stay diffable. Where a diagram and the prose disagree,
  the prose wins.

## 5. Proposing a change

Open an issue describing the problem before writing a change. A proposal that adds a construct should say
what would exercise it — a construct nothing will ever exercise is one to leave out.
