# Runbook — Adopting Colophon

How to turn this template into a working Colophon project, and how
to keep it healthy over time.

For applying Colophon to an existing project, see
[adoption.md](./adoption.md).

---

## Setting up a new project

1. **Create from template**
   ```
   gh repo create my-project --template Craft-Code-Systems/colophon --public
   cd my-project
   ```
   Or click **Use this template** on GitHub.

2. **Rewrite the Brief (`README.md`)**
   - Replace the Colophon description with your project's purpose.
   - Fill in scope, non-scope, success criteria, stack.
   - Keep it to one or two screens.

3. **Clean the Decisions folder**
   - Delete Colophon's own Decisions (`0001-` through `0006-`).
   - Keep `0000-template.md` — it is the template for your first
     Decision.

4. **Rewrite the Runbook**
   - This file. Replace with how *your* project runs and deploys.
   - Include "how to unbreak at 3am" notes.

5. **Reset the Log and Changelog**
   - Empty `docs/research-log.md` — keep the header only.
   - Empty `CHANGELOG.md` down to the header and an `[Unreleased]`
     section.

6. **Optional: add a Spec**
   - If your project genuinely needs richer requirements documentation
     (prototyping many capabilities, formal stakeholder requirement),
     copy `templates/SPEC.template.md` to `docs/spec.md`.
     Otherwise skip — most projects do not need one. See
     [adoption.md](./adoption.md#when-to-add-the-optional-spec).

Total time: around ten minutes. You now have a working Colophon setup.

---

## Daily use

- **Made a real design choice?** → add a Decision in `docs/adr/`.
  Copy `0000-template.md`, increment the number, fill it in.
  Keep it to roughly one screen.
- **Explored something, found facts worth keeping?** → append a
  dated entry to the Log. Do not rewrite older entries.
- **Shipped something?** → add a line to the Changelog under
  `[Unreleased]`. Group by *Added / Changed / Fixed / Removed*.
- **Changed how you run or deploy?** → update the Runbook.
- **Project direction shifted?** → update the Brief. It should
  always reflect current reality.

---

## Writing a Decision (the ritual)

1. Copy `docs/adr/0000-template.md` to
   `docs/adr/NNNN-short-title.md`.
2. Increment `NNNN` by one over the highest existing number.
3. Fill in *Context*, *Decision*, *Considerations*, *Consequences*.
4. Commit in the same pull request as the code change, where possible.
5. Never edit an accepted Decision's *Decision* section. If you
   change your mind, write a new Decision that supersedes it, and
   update the old one's Status to `Superseded by Decision NNNN`.
   See [adoption.md](./adoption.md#when-a-structural-design-choice-changes)
   for a full worked example.

---

## Keeping it healthy

Signs your Colophon setup is drifting:

- The Brief no longer reflects what the project does → update it.
- `docs/adr/` has design choices you never wrote down → write them
  retroactively if still relevant; skip if not.
- The Runbook is out of date → fix the next time it bites you,
  not before.
- The Changelog has not been touched in months → you are either not
  shipping, or not recording. Both worth attention.

Rule of thumb: if a file has not been touched in a long time *and*
nobody has needed it in a long time, consider deleting it.
Colophon files earn their place or leave.

---

## Generated views (optional)

Some teams want one page that shows the whole project at a glance.
Generate it; never write it. The rules are in
[Decision 0006](./adr/0006-generated-views.md). In short: every fact
comes from the Colophon files, git, or the issue tracker, and nobody
edits the page by hand.

What a view can show, and where each part comes from:

| On the page                | Source                                     |
|----------------------------|--------------------------------------------|
| Status & milestones        | The Brief's section, shown verbatim        |
| Decisions and their status | The `**Status:**` line of each Decision    |
| Latest version and changes | The top section of the Changelog           |
| Latest exploration         | The newest dated entry in the Log          |
| Milestone progress         | The issue tracker's API                    |
| Doc freshness              | `git log` per Colophon file                |

Most of it is a few lines of shell. For example:

```
grep -H '^\*\*Status:\*\*' docs/adr/*.md | grep -v 0000-template
for f in README.md CHANGELOG.md docs/runbook.md docs/research-log.md; do
  printf '%s  %s\n' "$(git log -1 --format=%cs -- "$f")" "$f"
done
```

Colophon ships a reference generator built on these sources in
`templates/overview/`. It needs Python 3.8+ and git, nothing else:

1. Copy `templates/overview/overview.py` to `scripts/overview.py`.
2. Copy `templates/overview/overview.yml` to
   `.github/workflows/overview.yml`.
3. In the repository's *Settings → Pages*, set *Source* to
   *GitHub Actions*.
4. Push to the main branch.

The page then rebuilds on every push and once a day. To build it
locally, run `python3 scripts/overview.py --out _site` and add `_site/`
to `.gitignore`. Until copied, the files in `templates/` do nothing, so
the overview stays opt-in.

Running it:

- Keep the script and the workflow in the project's own repository;
  only the generated page stays out of git.
- Build the page in CI on every push to the main branch and publish it
  (GitHub Pages, GitLab Pages, or a build artefact). Do not commit it.
- If it must be committed, let only the CI job write it, and mark it
  as generated in `.gitattributes` so diffs and reviews skip it:
  ```
  docs/overview.html linguist-generated=true
  ```
- If a language model writes a summary paragraph, label it as
  generated and link it to the files it read. Let it paraphrase the
  Brief, not judge the project.
- Doc freshness is the most useful signal: a Runbook untouched for
  months while the deploy code changed weekly is drift you can see.

---

## Integration notes

- **With Diátaxis**: Colophon covers project-internal docs. If your
  project needs user-facing tutorials and reference, put those in a
  separate `user-docs/` folder (or a dedicated site) structured per
  Diátaxis. The two do not overlap.
- **With Doxygen / Sphinx / JSDoc**: these generate API reference
  from source comments. That is different from Colophon and
  complementary. Link to the generated output from the Brief.
- **With compliance documents**: keep those in whatever system the
  regulator requires. Do not try to fold them into Colophon.
