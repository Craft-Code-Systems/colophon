# Changelog

All notable changes to the Colophon methodology are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
Colophon adheres to [Semantic Versioning](https://semver.org).

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
