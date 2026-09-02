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
