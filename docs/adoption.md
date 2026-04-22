# Adoption

How to apply Colophon to an existing project, and how to handle
structural design changes once you are using it.

---

## Adopting Colophon in an existing project

Most projects that could benefit from Colophon already exist.
Trying to retroactively document everything at once is the fastest
way to abandon the effort. Colophon is adopted *forward*, not
backward.

### The 30-minute bootstrap

**Step 1 — Triage the existing README (10 min).**
Open the current `README.md`. Does it answer these within two
screens?

- Why does this project exist?
- What is in and out of scope?
- How do I run it locally?
- What is its current status?

If not, fill in the gaps. At the same time, remove cruft: outdated
screenshots, dead links, stale "TODO for v2" blocks. This is not a
rewrite — it is a cleanup.

**Step 2 — Add the skeleton (5 min).**
```
docs/
├── adr/
│   └── 0000-template.md
├── research-log.md
└── runbook.md
CHANGELOG.md
```
Create the files empty except for headers.

**Step 3 — Write your first Decision: "0001-adopting-colophon.md" (10 min).**
This is a Decision *about* adopting Colophon. Its Context explains
that the project has been running for a while, that documentation
was previously scattered or absent, and that you are now moving to
this structure. It is meta but it is also your first working
example — and it gives future contributors the reason why this
structure suddenly exists.

**Step 4 — Snapshot in the Log (5 min).**
One entry in `docs/research-log.md`:

```
## YYYY-MM-DD — Pre-Colophon snapshot
This project has existed since [date]. Decisions made before
Colophon adoption that may be retroactively recorded later if
they become relevant:
  - Choice of stack X (approx. date)
  - Database design Y (around Q2 2025)
  - API versioning approach Z
```

This is *not* a commitment to write those Decisions now. It is a
parking list: if someone asks "why was X done this way?" and you
have to explain it, write the Decision then. Not before.

### The 30-day rule

Over the next thirty days, when any of these triggers fire, write
the matching file:

| Trigger                                           | Action                |
|---------------------------------------------------|-----------------------|
| You explain a historical decision to someone      | Add a Decision        |
| You make a new design choice                      | Add a Decision        |
| You fix a bug better docs would have prevented    | Update the Runbook    |
| You spend time investigating something            | Append to the Log     |
| You ship something                                | Line in the Changelog |

After thirty days you will have five to ten Decisions that are
actually relevant — not the full history, but the parts still in use.

---

## When a structural design choice changes

This is where Colophon shines. Two rules keep the system honest
over time.

### Rule 1: Decisions are immutable. New Decisions supersede old ones.

You never edit an accepted Decision in place. You write a new one
that supersedes it. This sounds strict, but it is essential: the
history of *why* you once chose X is often more valuable than the
current choice. Without it, you will re-litigate the same trade-offs
two years from now.

### Rule 2: Four files to consider on a major change.

On a structural change, walk these four files:

1. **New Decision** — the new choice and why.
2. **Old Decision(s)** — update only the Status line.
3. **Runbook** — has the run/deploy/fix procedure changed? Update now.
4. **Changelog** — add under `[Unreleased]` with `### Changed`.

The Brief usually does not need updating unless the project's
fundamental purpose has shifted. The Log is optional — only append
if you did investigation before the change.

---

## Supersession worked example

Suppose `0003-sveltekit-for-pwa.md` was written a year ago, and you
now need to switch to Next.js for SSR support.

### First, edit only the Status line of the old Decision:

```markdown
# Decision 0003: SvelteKit for the PWA

**Status:** Superseded by [Decision 0012](./0012-nextjs-for-ssr.md)
**Date:**   2026-11-05

## Context
...original text, unchanged...
```

No other part of `0003` is touched. The original Context, Decision,
Considerations, and Consequences remain as a historical record.

### Then, write the new Decision:

```markdown
# Decision 0012: Switch to Next.js for SSR support

**Status:**      Accepted
**Date:**        2027-03-15
**Supersedes:**  [Decision 0003](./0003-sveltekit-for-pwa.md)

## Context
Decision 0003 chose SvelteKit on the assumption that static rendering
was sufficient. In practice: B2B customers require deep-linking of
dashboard views with SEO rendering (see Log entry 2027-02-20).
Switching adapters within SvelteKit turned out to be more work than
a full migration.

## Decision
Migrate to Next.js 15 with the App Router.

## Considerations
**Pro:** SSR out-of-the-box, larger talent pool, React ecosystem.
**Con:** migration work (~3 weeks), larger client bundle, loss of
Svelte developer experience.

Rejected alternatives: Remix (smaller community), SvelteKit with
the Node adapter (SSR still immature).

## Consequences
- Frontend codebase rewritten during Q2.
- `svelte-check` CI job removed, ESLint + TypeScript strict added.
- PWA configuration changes (workbox instead of @vite-pwa).
- Performance budget must be revised (see Decision 0013).
```

Two years from now, someone asks "why Next.js?" and finds Decision
0012, which links back to Decision 0003. They see the full
evolutionary trail: not "we just picked Next", but "we picked Next
*because* SvelteKit fell short in specific ways". This prevents
future contributors from suggesting a revert based on incomplete
memory.

---

## Supersede, replace, or deprecate?

| Situation                                          | Action                              |
|----------------------------------------------------|-------------------------------------|
| New choice directly replaces the old one           | **Supersede** — link both ways      |
| New choice adds to or extends existing scope       | **New Decision**, no supersede      |
| Old choice is stopped without a replacement        | **Deprecate** the old Decision      |

---

## What does *not* deserve a Decision

Not every change gets a Decision. Reserve them for *choices* — things
where a reasonable alternative existed.

| Change                      | Decision?                              |
|-----------------------------|----------------------------------------|
| Bug fix                     | No                                     |
| Refactor (same design)      | No                                     |
| Minor dependency bump       | No                                     |
| Major version with breaks   | Usually yes                            |
| New feature implementation  | Only if the approach itself was chosen |
| Stack swap                  | Yes                                    |
| Architecture change         | Yes                                    |

A Decision that runs to 200 lines is doing too much. Keep the
*choice* and the *reasoning*. Implementation details belong in
code, tickets, or pull request descriptions.

---

## Anti-patterns to avoid

**Retrospective Decision-writing marathon.**
The urge to capture "all historical decisions" in a single weekend
produces shallow Decisions that no one reads, and burnout. Let
triggers hold the pen.

**Decision inflation on changes.**
Not every change deserves a Decision. See the table above. If in
doubt, a Changelog line plus a Runbook update is enough.

**Treating Decisions as living documents.**
They are not. The Brief lives, the Runbook lives, the Changelog
grows — but Decisions are snapshots. Editing an accepted Decision
breaks the system.

**Decision as implementation plan.**
A Decision answers *which* and *why*. Not *how*. Implementation
details are code and PR territory.

---

## When Colophon may be overkill

If, six weeks after adopting Colophon in an existing project, no
Decision has been written and no bugs or onboarding events have
triggered any Runbook or Log entries, that is information. The
project may be stable enough that Colophon adds more overhead than
it repays. Scale back to just a well-maintained Brief and Changelog
and call it done. Dogma is also waste.
