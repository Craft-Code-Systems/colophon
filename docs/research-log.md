# Research Log — Colophon

Append-only notes from the exploration that led to Colophon.
Dated, rough, not rewritten after the fact.

---

## 2026-04-22 — Origin

Observation: traditional project documentation (plan of approach,
requirements, design, test plan) works well for waterfall and large
teams, but for solo and small-team work it creates overhead without
value. The documents are long, repetitive across files, written when
knowledge is lowest, and rarely re-read.

Four felt criteria for a replacement:
1. Preserves overview of the project.
2. Avoids long, meandering documents.
3. Easy and fast to set up, easy to read without losing focus.
4. Still covers enough detail to count as real documentation.

---

## 2026-04-22 — Survey of existing approaches

Things that already exist and solve parts of this:

- **Architecture Decision Records** (Michael Nygard, 2011). Short
  markdown files, one per decision. Solid pedigree (ThoughtWorks
  "Adopt" on Tech Radar). Covers "the memory" slot.
- **Docs-as-Code** (Write the Docs, Anne Gentle). Philosophy of
  treating docs like code: in git, reviewed, in CI. Covers the
  "where" question.
- **README-Driven Development** (Tom Preston-Werner, 2010). Write the
  README first. Argues the README is the entry point. Covers "the
  door" slot.
- **Diátaxis** (Daniele Procida). Four-quadrant framework for
  user-facing docs. Strong, but different scope from project-internal.
- **Keep a Changelog** (Olivier Lacan). Changelog as a human-readable
  history file. Covers "the history" slot.
- **Living Documentation** (Gojko Adzic, 2011). BDD-flavoured idea
  that tests are executable specs. Related but orthogonal.

None of these alone cover what a solo developer needs. The synthesis
below assembles them into a single minimal kit.

---

## 2026-04-22 — Synthesis: five files

Mapped the four user criteria to five files:

- `README.md` — overview (criterion 1)
- `docs/adr/` — decisions (criterion 4)
- `docs/research-log.md` — exploration (criterion 4)
- `docs/runbook.md` — operational (criterion 3, 4)
- `CHANGELOG.md` — history (criterion 1, 4)

All short, all focused, all in git. Each file has one job. Together
they cover what phase documents tried to.

---

## 2026-04-22 — Naming

Considered: JED (Just Enough Docs), Margin, Vellum, Folio, Stanza,
Cairn, Marginalia. Landed on **Colophon** — a colophon is the short
note at the back of a book describing how it was made. The metaphor
is almost too on-the-nose: Colophon files are the colophon of a
codebase.

Domain `colophon.dev` unavailable; `usecolophon.dev` follows the
`useanvil.com` / `usefathom.com` pattern and is acceptable.

---

## 2026-04-22 — Scope boundary

Clear decision: Colophon covers *project-internal* documentation.
User-facing documentation (tutorials, API reference) is out of scope
and is better served by Diátaxis. Organisational knowledge
(multi-project, business) belongs in a wiki. Regulatory compliance
documents belong in whatever system the regulator requires.

This keeps Colophon small enough to actually do one job well.

---

## 2026-04-22 — Vocabulary iteration

First draft used metaphors ("the door", "the memory", "the notebook",
"the lifeline", "the history"). Good for marketing copy, bad as
working vocabulary — no one wants to say "add a Memory to your project".

Second draft considered `-doc` suffixes for uniformity: `maindoc`,
`designdoc`, `researchdoc`, `rundoc`, `changedoc`. Rejected:
- `designdoc` collides with the existing software term for long
  upfront design documents — exactly what Colophon replaces.
- Uniform suffixes read as bureaucratic, not purposeful.
- Ignores strong existing conventions (runbook, changelog).

Landed on a hybrid: reuse industry conventions where they are strong
(**Log**, **Runbook**, **Changelog**); coin short, collision-free
terms for the rest (**Brief**, **Decision**). Filenames stay standard
(`README.md`, `CHANGELOG.md`) so tooling keeps working. Full reasoning
in Decision 0004.

---

## 2026-04-22 — Gap: adoption for existing projects

Drafted methodology assumes a fresh project ("use this template").
Missed the larger case: adopting Colophon in an existing codebase.
Also missed guidance on what to do when structural choices change
over time (supersession mechanics).

Added `docs/adoption.md` with:
- 30-minute bootstrap for existing projects
- 30-day rule (triggers, not marathons)
- Supersession worked example
- Table of what does and does not deserve a Decision
- Anti-patterns

This probably should have been in v0.1. Added for v0.2.
