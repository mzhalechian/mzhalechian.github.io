# Website update instructions

Personal academic site for Mohammad Zhalechian. Jekyll (academicpages theme).
Content lives in `_pages/*.md`; GitHub Pages builds on push to `master`, so
**a push to `master` is a publish**. There is no staging branch.

Repo: `https://github.com/mzhalechian/mzhalechian.github.io.git`
Live: `https://mzhalechian.github.io/`

## Previewing changes

Jekyll cannot currently be run on this machine — only system Ruby 2.6 is
available, and `bundle install` for the `github-pages` gem never resolves (it
spins at multi-GB memory). Don't retry it; it wastes several minutes.

To preview instead: fetch the live page from `https://mzhalechian.github.io/`,
inject the edit into that HTML, and serve the result on `127.0.0.1:4000`. The
surrounding rendering is then byte-identical to production.

**Do not hand-render the markdown with another engine** (e.g. Python-Markdown).
Its list spacing differs from kramdown's — loose vs. tight `<li>` — which makes
untouched content look restyled and sends the review off on a false trail.

## The two CV files

The CV exists as two LaTeX projects in `/Users/mzhale/Dropbox/Apps/Overleaf/`
(this folder syncs to Overleaf, so edits there appear in the Overleaf account):

| Project | Role |
|---|---|
| `+MZ - CV - new3/CV.tex` | **Master.** The real CV, used everywhere else. |
| `+MZ - CV - new3 (website)/CV.tex` | **Website duplicate.** Only this one is compiled and published to the site. |

There is also an older `+MZ - CV - website` project built on a previous template
(`old.tex`). It is stale and unused — ignore it, don't edit or delete it.

### What differs between master and duplicate

The duplicate is a copy of the master with exactly two deviations. Everything
else must stay identical.

1. **Working Papers status lines** (the reason the duplicate exists):
   - *Under review* → drop the journal name.
     `Under Review at Operations Research` becomes `Under Review`.
   - *Under revision* → keep the journal name, e.g.
     `Major Revision at Manufacturing & Service Operations Management`.
   - *Reject & Resubmit* → treated as under review: `Under Review`, with neither
     the journal name nor the R&R status shown.
2. **A stray `\end{rSection}`** just before `\end{document}` is removed. See
   Known issues.

### Which file to edit

- **Real CV content** (new paper, new award, new talk) → edit **both** files, so
  they don't drift. Same edit, same place in each.
- **The status-line rule above** → duplicate only.

### Publishing the CV

1. Refresh the duplicate from the master if the master has moved on, then
   re-apply the two deviations.
2. Compile the duplicate. `pdflatex` is at `/Library/TeX/texbin`; run it 2–3
   times so page refs settle. **Build in a scratch directory**, not in the
   Dropbox folder — otherwise `.aux`/`.log` files sync into Overleaf.
3. Sanity-check the new PDF against the previous one: page count and section
   headers should match unless the change was meant to alter them. (Section
   headers extract as uppercase, e.g. `HONORS AND AWARDS`.)
4. **Only the compiled PDF goes into this repo** — copy it to the repo root as
   `CV_MohammadZhalechian.pdf` and commit. The `.tex` stays in Overleaf; never
   add `.tex`, `.cls`, or build artifacts here.
5. `_pages/cv.md` links to `https://mzhalechian.github.io/CV_MohammadZhalechian.pdf`.
   It used to be a Google Drive link — don't go back to that. Overwriting the PDF
   at this same path keeps the link stable, so nothing needs re-sharing.

GitHub Pages takes roughly 1–2 minutes to rebuild; verify the live URL rather
than assuming the push took effect.

## Keeping content in sync

A paper award appears in **four** places. When adding one, do all four:

- `_pages/awards.md` — top-level list, newest first
- `_pages/research.md` — sub-bullet under the relevant paper, newest first
- CV Honors and Awards section — both `.tex` files
- CV entry for that specific paper — both `.tex` files, newest bullet first

Site navigation is `_data/navigation.yml`. As of Aug 2026 the menu is
Research / Teaching / CV, with two deliberate choices — don't "helpfully" undo
either:

- **The Awards tab was removed.** `_pages/awards.md` still exists and `/awards/`
  is still live, just unlinked. Keep maintaining it as above.
- **The CV tab points straight at the PDF** (`/CV_MohammadZhalechian.pdf`), not
  at `/cv/`. The intermediate "You can find my CV here" page was redundant.
  `_pages/cv.md` still exists and `/cv/` still works, also unlinked.

## Known issues

- The master `new3/CV.tex` has a **stray duplicate `\end{rSection}`** just before
  `\end{document}` (as of Aug 2026). It makes `pdflatex` fail with
  `\begin{document} ended by \end{list}` and produce no PDF. The duplicate has it
  removed; the master still has it and does not compile as-is. Re-remove it
  whenever the duplicate is refreshed from the master, until it's fixed upstream.
- `.DS_Store` shows as modified constantly (Google Drive folder). Leave it out of
  commits.

## Change log

- **Aug 2026** — Added "2026 Finalist, MIF Paper Competition" to `awards.md`, as
  a sub-bullet under the *Contextual Learning with Online Convex Optimization*
  paper in `research.md`, and to both CV files (Honors and Awards + that paper's
  bullet list). Moved the CV off Google Drive to the repo-hosted PDF. Created the
  `+MZ - CV - new3 (website)` duplicate and applied the status-line rule to it.
- **Aug 2026** — Removed the Awards tab from the menu and pointed the CV tab
  directly at the PDF. Both pages kept, unlinked.
