# Decision 0006: Generated views are tooling, not documents

**Status:** Proposed
**Date:**   2026-10-27

## Context

A recurring request is a single overview page of the project: status,
Decisions, recent changes, progress, the state of the docs themselves.
It would help readers who do not browse markdown in a repository —
stakeholders, newcomers, future-you after a month away.

Nobody maintains such a page by hand, and a hand-kept one would be a
second copy of facts that already live in the Brief, the Decisions,
the Changelog, and the issue tracker. Copies drift.
[Decision 0001](./0001-minimal-by-default.md) rejects optional extra
file types for this kind of reason. Yet
[Decision 0002](./0002-markdown-in-git.md) already allows a rendered
documentation site on top of the same markdown files.

Three options were considered:

1. **Add an optional overview document**, kept current by hand or by
   an AI agent. Cost: a second source of truth; breaks Decision 0001.
2. **Leave it out of Colophon entirely.** Cost: teams build one anyway,
   without guidance, and the page becomes the place facts get edited.
3. **Treat generated views as tooling.** Allow them, under rules that
   keep the Colophon files the only source.

## Decision

Adopt option 3. Colophon permits generated views — overview pages,
dashboards, documentation sites — as tooling, not as documents.
A generated view:

- **Is derived only.** Every fact on it comes from the Colophon files,
  git history, or the issue tracker. A view never introduces information.
- **Is never edited by hand.** A wrong view is fixed at its source.
- **Can be deleted without loss.** Rebuilding it from the same sources
  restores it.
- **Extracts rather than infers.** Facts are read from the files, not
  composed by a language model. A model-written summary, if any, is
  labelled as generated, links to its sources, and never claims a
  status the sources do not state.
- **Is built, not committed, by default.** Build it in CI and publish
  it (for example to GitHub Pages or as a build artefact). If it must
  live in the repo, only automation regenerates it, on the main
  branch, and it is marked as generated.

## Considerations

**Pro:**
- The five-file promise and Decision 0001 stand: a view is not a file type.
- Readers get one page; writers keep writing in one place.
- A view can surface drift — a Runbook untouched while the deploy code
  changed weekly — which is the problem Colophon exists to solve.
- A script reading markdown and git is cheap, offline, and reproducible.

**Con:**
- Someone has to write and maintain the script or CI job.
- A view is stale between builds unless it is rebuilt on every change.
- "Extract, don't infer" limits what an AI-written view may say.

Rejected: option 1. A hand- or agent-maintained overview is a second
source of truth, and its first stale status line costs it the readers'
trust. Rejected: option 2. Silence leaves the likeliest mistake —
editing the overview instead of the Brief — unaddressed.

## Consequences

- Generated views get no role-name
  ([Decision 0004](./0004-naming-conventions.md)): they are not
  documents, so they are not part of the vocabulary.
- The Runbook describes what a view may show and how to publish it.
- Colophon prescribes no tool; a shell script and a CI job are enough.
- An AI agent that keeps documentation current edits the Colophon
  files themselves, through a pull request like any other change —
  never the view.
- Status and progress stay where
  [Decision 0005](./0005-coverage-mapping.md) put them: the Brief and
  the issue tracker. A view displays them; it does not own them.
