# Changelog

All notable changes to the Colophon methodology are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
Colophon adheres to [Semantic Versioning](https://semver.org).

## [0.3.0] — 2026-04-23

### Added
- Brief gains a documented ten-section structure (Why, Scope,
  Success criteria, Capabilities, Non-functional baseline,
  Stakeholders & roles, Risks, Stack, Status & milestones, Getting
  started). Previously vague; this clarifies what the Brief should
  cover.
- Optional sixth document type: **Spec** (`docs/spec.md`), opt-in,
  for projects that genuinely need richer requirements documentation.
  Template in `templates/SPEC.template.md`.
- `templates/SPEC.template.md` with functional requirements,
  non-functional requirements, constraints, out-of-scope, open
  questions, and glossary sections.
- Coverage-mapping table in README and adoption.md ("A place for
  everything"): where each kind of project content lives in Colophon.
- Decision 0005 documenting the coverage approach and Spec opt-in.
- FAQ entries on requirements coverage, roles, planning, and the
  optional Spec.

### Changed
- README expanded with "What the Brief should cover" and "A place for
  everything" sections.
- README template updated to demonstrate the full Brief structure.
- Adoption guide now leads with the coverage mapping before the
  bootstrap section.

## [0.2.0] — 2026-04-22

### Added
- Role-name vocabulary: **Brief**, **Decision**, **Log**, **Runbook**,
  **Changelog**. Filenames unchanged (see Decision 0004).
- `docs/adoption.md`: guide for adopting Colophon in existing projects
  and handling structural design changes via supersession.
- Decision 0004 documenting the naming conventions.
- Supersession mechanics: explicit `Supersedes:` field in the
  Decision template, and a worked example in the adoption guide.
- FAQ entries on existing projects, structural changes, and the
  vocabulary choice.

### Changed
- All internal documentation updated to use the new role-names.
- Runbook now references `adoption.md` for the existing-project case.
- Decision template includes optional `Supersedes:` metadata.

## [0.1.0] — 2026-04-22

### Added
- Five principles and five file types defined.
- Template repository published.
- Landing page at usecolophon.dev.
- Initial Decisions documenting the methodology's own choices
  (0001–0003).
