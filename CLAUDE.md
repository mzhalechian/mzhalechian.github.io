# Website and CV instructions

Personal academic site for Mohammad Zhalechian, Assistant Professor of Operations
and Decision Technologies, Kelley School of Business, Indiana University.
Jekyll (academicpages theme). Content lives in `_pages/*.md`; GitHub Pages builds
on push to `master`, so **a push to `master` is a publish**. No staging branch.

Repo: `https://github.com/mzhalechian/mzhalechian.github.io.git`
Live: `https://mzhalechian.github.io/`

---

## 1. Previewing changes

Jekyll **cannot** be run on this machine — only system Ruby 2.6 is available, and
`bundle install` for the `github-pages` gem never resolves (it spins at multi-GB
memory for 10+ minutes). Don't retry it.

Preview instead by fetching the live page and patching it:

1. `curl` the live page (e.g. `https://mzhalechian.github.io/research/`) to a file
   in the scratch directory.
2. Inject the edit into that HTML with a small Python build script.
3. Rewrite root-relative links to absolute `https://mzhalechian.github.io/...` so
   the file opens standalone, then `open` it. **Use `file://`, not a local
   server** — background servers get reaped mid-session and the link dies.

The surrounding rendering is then byte-identical to production.

**Never hand-render the markdown with another engine** (e.g. Python-Markdown).
Its list spacing differs from kramdown's — loose vs. tight `<li>` — which makes
untouched content look restyled and sends the review down a false trail. This
wasted a round trip once; don't repeat it.

Scratch previews are disposable: the scratch directory has been wiped
mid-session before. Anything worth keeping goes into the repo.

---

## 2. The two CV files

Two LaTeX projects in `/Users/mzhale/Dropbox/Apps/Overleaf/` (this folder syncs
to Overleaf, so edits appear in the Overleaf account):

| Project | Role |
|---|---|
| `+MZ - CV - new3/CV.tex` | **Master.** The real CV, used everywhere else. |
| `+MZ - CV - new3 (website)/CV.tex` | **Website duplicate.** Only this one is compiled and published to the site. |

An older `+MZ - CV - website` project (built on a previous `old.tex` template) is
stale and unused — ignore it; don't edit or delete it.

### What differs between master and duplicate

The duplicate is the master with **two** deviations. Everything else stays identical.

**1. Working Papers status lines** (the reason the duplicate exists):
- *Under review* → drop the journal name. `Under Review at Operations Research`
  becomes `Under Review`.
- *Under revision* → keep the journal name, e.g.
  `Major Revision at Manufacturing & Service Operations Management`.
- *Reject & Resubmit* → treated as under review: `Under Review`, showing neither
  the journal name nor the R&R status.

**2. A stray duplicate `\end{rSection}`** just before `\end{document}` is removed.
See Known issues — it is still present in the master.

### Which file to edit

- **Real CV content** (new paper, award, talk, layout fix) → edit **both** files
  so they don't drift. Same edit, same place in each.
- **The status-line rule above** → duplicate only.

### Page-break protection (both files — preserve it)

Both `.tex` files load `\usepackage{needspace}` and reserve space before headings
so a heading never lands alone at the foot of a page:

- `\Needspace*{12\baselineskip}` at the top of `\newcommand{\pubgroup}`
- `\Needspace*{8\baselineskip}` at the top of `\renewenvironment{rSection}`

The two values differ deliberately. Entries are wrapped in `keeptogether`
minipages that cannot split, so a tall entry jumps the page and strands its
heading. 8 was enough for `rSection` but `pubgroup` needed 12; using 12 for both
opened a ~209pt gap on page 3. **If you refresh the duplicate from the master,
check these survive.**

### Publishing the CV

1. Refresh the duplicate from the master if the master moved on, then re-apply
   the status-line rule.
2. Compile the duplicate. `pdflatex` is at `/Library/TeX/texbin`; run it 2–3
   times so page refs settle. **Build in a scratch directory**, never in the
   Dropbox folder, or `.aux`/`.log` files sync into Overleaf.
3. Sanity-check against the previous PDF: page count (currently **6**), section
   headers (they extract as uppercase, e.g. `HONORS AND AWARDS`), and that no
   heading is the last line on its page.
4. **Only the compiled PDF goes into this repo** — copy to the repo root as
   `CV_MohammadZhalechian.pdf` and commit. The `.tex` stays in Overleaf; never
   add `.tex`, `.cls`, or build artifacts here.
5. `_pages/cv.md` and the nav both point at
   `https://mzhalechian.github.io/CV_MohammadZhalechian.pdf`. It used to be a
   Google Drive link — don't go back. Overwriting the PDF at the same path keeps
   the link stable, so nothing needs re-sharing.
6. **Verify the deployed PDF by hash**, not by eye — the URL never changes, so a
   stale copy looks identical:
   `shasum -a 256 CV_MohammadZhalechian.pdf` vs. the same on the downloaded live
   file. See §6 on failed builds.

---

## 3. Page-by-page layout notes

### Home (`_pages/about.md`, permalink `/`)

- **`author_profile: false`** — this page has no sidebar; it carries its own
  masthead (circular photo left, name/title/affiliations right, then
  `mzhale@iu.edu · Google Scholar · LinkedIn`).
- The theme's `.page` rule is a fixed 83% width with a right float regardless of
  sidebar, so **the layout override CSS must stay inside the page**:
  `.page { width:100% !important; float:none !important; padding-left/right:0 !important }`.
  It is scoped to this page; other pages keep their sidebars.
- Sections: Research Interests (blue-label rows) → breadth paragraph → Bio (three
  paragraphs) → Selected Publications (venue in a 12.5em left column) → Recent
  Recognition (three items + link to the full list).
- The pre-redesign version is kept at `_backup/about-2026-08-17-pre-redesign.md`.

### Research (`_pages/research.md`)

- Shares typography with `/teaching/`: uppercase letter-spaced `h3`, hairline
  rules, `.88em` body, muted sub-bullets.
- **Honors and Awards** lives at the bottom of this page — all 12 awards with
  their gray institution sub-lines. Trimming it was tried and reverted; keep it
  complete unless asked again.
- The award rows must sit **inside** `<div class="archive">`. Inserting after the
  `</div></div>` that closes it renders the block full-bleed, outside the layout.

### Teaching (`_pages/teaching.md`)

Sections: Courses → Teaching Recognition → Mentoring. The TA list was removed
deliberately (reads as junior on a tenure-track page); the full history stays on
the CV. Mentoring lists Undergraduate first — it matters for teaching awards.

### Navigation (`_data/navigation.yml`, `_includes/masthead.html`)

Menu is **Home | Research | Teaching | CV**. Deliberate choices — don't undo:

- `masthead.html` hardcodes `Home` instead of `{{ site.title }}`. Do **not**
  change `site.title` in `_config.yml` — it also feeds page titles, SEO meta, and
  the feed name.
- **The Awards tab was removed.** `_pages/awards.md` still exists and `/awards/`
  is live but unlinked and now duplicated by the research page.
- **The CV tab points straight at the PDF**, not `/cv/`. `_pages/cv.md` still
  exists, unlinked.

### Sidebar (all pages except home)

`author.bio` in `_config.yml` holds HTML: a small gray two-line block
(`Assistant Professor` / `Operations and Decision Technologies`) then the email.
Use single quotes inside so the double-quoted YAML string stays valid.
`author.twitter` is deliberately blank — only LinkedIn and Google Scholar show.

---

## 4. Content conventions

**Award names are spelled out in full**, everywhere (site, both CVs, anything
sent to IU). Abbreviations alone are opaque to outside readers:

- `INFORMS Minority Issues Forum (MIF) Paper Competition`
- `POMS College of Healthcare Operations Management (CHOM) Best Paper Competition`
- `Operations and Decision Technologies Faculty Scholar for Research Excellence`
  (never "ODT")
- Still unexpanded and arguably should be: `IOE Richard C. Wilson Prize`

**An award appears in five places.** When adding one, do all five:

1. `_pages/awards.md` — top-level list, newest first
2. `_pages/research.md` — sub-bullet under the relevant paper, newest first
3. `_pages/research.md` — the Honors and Awards section at the bottom
4. `_pages/about.md` — Recent Recognition, if it belongs in the top three
5. Both `.tex` files — Honors and Awards **and** that paper's bullet list

`*` after a year means the award was received by a **student**, not by Mohammad.
Never present those as his own on a faculty profile.

**Style preferences observed over many rounds:**

- No badges, chips, pills, or symbol markers on award lines. Two attempts were
  rejected as "too shiny." Distinctions go in the wording, not the styling.
- No bold for emphasis in body prose on the home page.
- No em-dashes in the home page's opening statement.
- Prefers plain, formal academic phrasing over punchy copy.

---

## 5. Known data issues

- **The master `new3/CV.tex` is unbalanced**: 10 `\begin{rSection}` against 11
  `\end{rSection}`, the extra one immediately before `\end{document}`. With
  `-halt-on-error` it fails outright (`\begin{document} ended by \end{list}`);
  in plain `nonstopmode` LaTeX logs the error and still emits a PDF, so it can
  look fine. The website duplicate has it removed and is balanced. Re-remove it
  whenever the duplicate is refreshed from the master, until it's fixed upstream
  in Overleaf.
- **`Ophthalmology Science` paper is mis-cited on the site and CV.** The published
  title is *"Augmenting Kalman Filter Machine Learning Models with Data from OCT
  to Predict Future Visual Field Loss"* — no "Optical Coherence Tomography" spelled
  out, and **no** "The African Descent and Glaucoma Evaluation Study" subtitle.
  The real author list is 12 names (De Moraes, Girkin, Fazio, Weinreb, Bowd,
  Liebmann, Zangwill, Andrews, Stein…), not "et al." Verified against Crossref
  (`10.1016/j.xops.2021.100097`). Not yet corrected.
- **`_pages/teaching.html`** also declares `permalink: /teaching/`, conflicting
  with `teaching.md`. The markdown wins today, but that's Jekyll's tiebreak, not a
  guarantee.
- **Placeholder pages are live**: `/teaching/2014-spring-teaching-1` and `-2`
  ("Teaching experience 1", venue "University 1, Department") from
  `_teaching/*.md` academicpages boilerplate.
- **`_pages/about-beforeKelley.md` and `_pages/about-OnMarket.md`** are stale
  drafts still carrying old award wording.
- **Heading typo**: "Selected Papers Published Prior my Ph.D." is missing "to".
- **`images/AcademyPic.jpg` is actually a PNG** (RGBA, 807×790, 784KB) with a
  `.jpg` extension. It is a full upper-body portrait, so in the home page's 165px
  circle the face renders small. A head-and-shoulders crop (560×560 from
  ~(152,20)) was previewed and looked much better but was never applied.

Use Crossref for citation metadata rather than the CV:
`curl -s "https://api.crossref.org/works/<DOI>"`.

---

## 6. Deployment gotchas

- **GitHub Pages builds fail silently and often.** Three times in one day the
  build errored while downloading the `jekyll-build-pages` action (429 / 503 from
  codeload), and the site kept serving the previous version — indistinguishable
  from a slow build. If a change doesn't appear after ~3 minutes, it probably
  failed. Fix: `git commit --allow-empty` and push to retrigger. The Actions tab
  shows the real status.
- `gh` CLI is **not installed**, so failed runs can't be re-run from here.
- Pages sends `cache-control: max-age=600`, so the browser can lag 10 minutes
  behind. Verify with `curl -H 'Cache-Control: no-cache'` and a cache-busting
  query string, not by reloading the browser.
- `.DS_Store` shows as modified constantly (Google Drive folder). Keep it out of
  commits; don't commit it "for tidiness."
- Always confirm a change is actually live before reporting it done.

---

## 7. Change log

- **Aug 2026** — Added the 2026 INFORMS Minority Issues Forum finalist across the
  site and both CVs. Moved the CV off Google Drive to the repo-hosted PDF and
  created the `+MZ - CV - new3 (website)` duplicate.
- **Aug 2026** — Removed the Awards tab; pointed the CV tab at the PDF directly.
- **Aug 2026** — Rebuilt `/teaching/` (statement, recognition, mentoring; TA list
  dropped). Gave `/research/` matching typography and a Honors and Awards section.
- **Aug 2026** — Redesigned the home page: no sidebar, masthead header, research
  interests, bio, selected publications, recent recognition. Nav title → "Home".
- **Aug 2026** — Spelled out MIF, CHOM, and Operations and Decision Technologies
  everywhere. Dropped Twitter from the sidebar; shrank the sidebar title.
- **Aug 2026** — Added `needspace` page-break protection to both CVs.
- **Aug 2026** — Prepared an IU Kelley faculty-profile update (awards 6→11,
  publications 3→7, citations verified against Crossref). Delivered to the user's
  Desktop; the repo copy was removed at their request.
