# Website update instructions

Jekyll (academicpages theme). Content lives in `_pages/*.md`; GitHub Pages builds
on push to `master`, so a push is a publish.

Note: Jekyll cannot currently be run locally on this machine (only system Ruby
2.6 is available, and `bundle install` for `github-pages` does not resolve). To
preview a content change, fetch the live page from `https://mzhalechian.github.io/`
and inject the edit into that HTML — the surrounding rendering then matches
production exactly. Do not hand-render the markdown with another engine; its
list spacing differs from kramdown and makes unchanged content look changed.

## Updating the CV on the website

The source of truth for the CV is the Overleaf project **`MZ-CV_new3`**, synced
locally at `/Users/mzhale/Dropbox/Apps/Overleaf/+MZ - CV - new3/CV.tex`.

1. Never edit `MZ-CV_new3` directly. Work in the duplicate
   `+MZ - CV - new3 (website)` (same folder), refreshing it from `new3` first so
   it picks up any new publications, awards, etc.
2. Apply the change below, then compile. `pdflatex` is available locally at
   `/Library/TeX/texbin`. Build in a scratch directory rather than in the
   Dropbox folder, so `.aux`/`.log` files don't sync back to Overleaf.
3. **Only the compiled PDF goes to the website** — commit it to the repo root as
   `CV_MohammadZhalechian.pdf`. The `.tex` stays in Overleaf; do not add it here.
4. `_pages/cv.md` links to the repo-hosted copy at
   `https://mzhalechian.github.io/CV_MohammadZhalechian.pdf` — not a Google Drive
   link. Keep it that way; overwriting the PDF at the same path keeps the link
   stable.

After compiling, sanity-check the new PDF against the previous one (page count
and section headers) before committing.

### Website-only change: Working Papers section

The website copy differs from the Overleaf original in how a paper's status is
listed:

- **Under review** → drop the journal name. `Under Review at Operations Research`
  becomes `Under Review`.
- **Under revision** → keep the journal name, e.g.
  `Major Revision at Manufacturing & Service Operations Management`.
- **Reject & Resubmit** → treated as under review: `Under Review`, no journal
  name and no mention of the R&R status.

This is why the duplicate exists — the Overleaf original keeps the journal names
in both cases.

Known issue: as of Aug 2026 the master `new3/CV.tex` has a stray duplicate
`\end{rSection}` just before `\end{document}`, which makes `pdflatex` fail. The
duplicate has it removed. Re-drop it whenever the duplicate is refreshed from
`new3`, until it's fixed upstream.

## Keeping pages in sync

An award generally appears in three places; when adding one, check all three:

- `_pages/awards.md` — top-level list, newest first
- `_pages/research.md` — sub-bullet under the relevant paper, newest first
- the CV's Honors and Awards section (via the Overleaf flow above)
