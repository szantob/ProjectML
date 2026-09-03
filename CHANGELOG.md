# Changelog

All notable changes to this project are documented in this file.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

`projectml-core` is the metamodel, and it is the only thing versioned here. Implementations — a notation, a
set of requirement definitions, a rule-set — version separately in their own repositories, except the
SysML v2 binding, which lives in `bindings/` and moves with the metamodel.

**Nothing has been released.** The metamodel is a draft, and under `CLAUDE.md` §6 it cannot reach 1.0 until
an implementation has been built on it and has carried a project end to end. Until then this file records
work, not releases.

## [Unreleased]

### Added

- Repository initialised: conventions in `CLAUDE.md`, the founding record in
  `docs/2026-08-26-kernel-brainstorm.md` — eighteen decisions, eight open questions, and the four-phase
  order of work carried over from the session that started this project.
- `spec/`, the metamodel's first complete draft: an overview, one document per member of the collection —
  the requirement model, the requirement analysis model, the Project Lifecycle Model and the value states —
  a binding contract, and a decision record continuing the K series. Prose and diagrams; no notation and no
  filled definitions. OQ2 and OQ4 are answered; OQ9, OQ10 and OQ11 are opened.
- The binding contract's seam finished: cardinality, the check that runs over it, and the far end's naming
  (K51–K54), closing OQ12.
- `bindings/sysml-v2.md`, phase 2's binding — the four declarations K4 asks of a design language, stated for
  SysML v2 and checked against the OMG SysML v2 and KerML specifications directly rather than secondary
  sources. Its findings, including where a declaration turns out to buy less than the binding contract
  claims for it, are recorded in
  [`docs/superpowers/specs/2026-09-03-sysml-binding-approach-design.md`](docs/superpowers/specs/2026-09-03-sysml-binding-approach-design.md).
- K56, closing OQ11: the metamodel needs no subject, checked against both SysML v2 (which carries the
  concept richly, independent of `satisfy`) and EventML (which carries none).
- The binding contract's seam clarified: a binding may give the requirement a native element of its own, or
  a bare reference, and both are symmetric under K2 — the identifier map is what does the work of getting a
  requirement into a design language's own model, not `satisfy`. OQ10's premise is corrected accordingly.
