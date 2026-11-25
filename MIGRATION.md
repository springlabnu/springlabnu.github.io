# Migration log: Google Site → Hugo

Source: <https://sites.google.com/view/springlabnu>. The old site's footer said "Site last updated: 3/27/2026". It was read on 2026-11-25.

Rules followed: facts only, all text reworded, no old layout or styling, nothing invented. Anything missing or uncertain is marked `[PLACEHOLDER]` in the repo and listed under **Open TODOs** below.

Find every remaining placeholder with:

```powershell
Select-String -Path content\*,content\*\*,data\* -Pattern 'PLACEHOLDER'
```

---

## Page map

| Old page | New location | Notes |
|---|---|---|
| People `/people` | `data/people.yaml`, `content/people/_index.md`, photos in `assets/images/people/` | see below |
| Publications `/publications` | `data/publications.yaml` (49), `data/patents.yaml` (5), `content/publications/_index.md` | see below |
| Research `/research` | `data/research.yaml` (3 pillars, 7 projects, funding), `content/research/_index.md`, images in `assets/images/research/` | see below |
| Software `/software` | **Platforms**: `data/platforms.yaml`, `content/platforms/_index.md`, images in `assets/images/platforms/` | renamed page |
| Collaborators `/collaborators` | `data/collaborators.yaml` (7), `content/collaborators/_index.md` | see below |
| Home `/home`: intro, photos | `content/_index.md`, `data/home.yaml` (teasers), `assets/images/research/home-photo-*.jpg` | see below |
| Home `/home`: News, 2021 onward | `content/news/*.md` (23 items) | see below |
| Home `/home`: News before 2021 | `content/older-news.md` (one archive page at `/older-news/`, linked from the bottom of News) | see below |
| Contact Us `/contact-us` | **Join Us**: `content/join/_index.md`. Contact details are in `hugo.toml` → `[params.contact]` and shown in the footer on every page. | see below |

### People

- **Migrated:** 37 people: the PI, 2 postdocs, 5 grad students, 3 undergrads and 26 alumni. There are 35 photos (Jiasen Hou and Julia Tatz had none). Each photo was matched to its person by its position next to the name on the old page.
- **Grouping:** I followed the old page's layout. Everyone shown under the old "Alumni" heading is marked `alumni`. Several of those bios are still written in the present tense (Gabriel Hill, Nikolas Sanderson and others), but the page grouping was used.
- **Education:** degrees now go in a separate `education` list instead of the bio text.
- **Alumni bios:** shortened to one sentence each.
- **Dropped:**
  - most hobby details for alumni;
  - stale year-in-program phrases ("first-year", "second-year…");
  - the PI's office address, which is already in the site footer;
  - the "cleans the lab space" line;
  - the large photo at the bottom of the old People page (it looked like a group or campus photo; see TODO);
  - the Northeastern Physics logo link.
- **Name used:** "Ivonne Lozano-Pope", the form in her bio. The old heading said "Ivonne Lozano".

### Publications

- **Migrated:** all 49 entries plus 5 patents and applications. The 5 entries without a year (1 submitted, 2 accepted, 2 accepted and under embargo) are listed under "Forthcoming", each with a status label. The Google Scholar link moved into the page intro.
- **The old numbering (1–49) was dropped.** Entries are now grouped by year, newest first.
- **DOIs:** taken only where the old link contained one explicitly (doi.org, `/doi/…`, nature.com, Springer and Frontiers paths). Everything else keeps the old publisher link as `url`. Google Drive PDFs are kept as `pdf`.
- **Small fixes:**
  - "long-wavelenth" → "long-wavelength" (Lang & Spring 2021).
  - Missing comma "Ilday FO Spring BQ" → "Ilday FO, Spring BQ".
  - Broken DOI link on Park et al. 2026 cleaned up to `10.1111/php.70149`.
  - Patent title "Photoimmunotherapy-diven" → "Photoimmunotherapy-driven".
- **Journal names** were expanded from abbreviations, e.g. "Photochem Photobiol" → "Photochemistry and Photobiology".

### Research

- **The 7 old project sections became `projects`:**
  - "Optical image-guided photomedicine" → "Light-activated, tumor-targeted therapy"
  - "Multiphoton microendoscopy"
  - "Femtosecond fiber lasers"
  - "GPU-accelerated hyperspectral image analysis" → "Real-time hyperspectral image analysis"
  - "Low-cost custom LED-arrays"
  - "Fluorescent probes and biosensors"
  - "Cancer biology and physics" → "Models of chemoresistant cancer"

  Each project is tagged with one or more of the three new pillars and linked to related papers in `publications.yaml`. I chose those links; please review them.
- **Funding:** the old section was logos only. They became a text list naming NCI (including the K22), the Chan Zuckerberg Initiative, Scialog 2021, the Smith Family Awards Program, and Northeastern's Office of Research Development. The logo images were not migrated.
- **Images kept** (the lab's own, or adapted from the lab's own papers):
  - the taPIT schematic and in vivo microendoscopy figure (adapted from Spring et al., PNAS 2014);
  - the ANDi fiber-laser photos;
  - the activatable-probe figure (adapted from Obaid, Spring, Hasan et al., JBO 2017);
  - two lab photos (cell culture, optics) credited on the old site to photographer Spencer A. Cohen;
  - the two LED-array photos, which went to `assets/images/platforms/`.
- **Dropped:**
  - figures from other groups: the Chris Xu (Cornell) excitation diagram, the imaging-depth plot from Kobat et al. 2009, the Ducourthial/Louradour Sci Rep 2015 probe schematic, and the Cellaria Bio cell-line micrograph;
  - external "read more" links (MicroscopyU, the Hallmarks of Cancer PDF, the Cellaria white paper, a 1999 ovarian-cancer news clipping);
  - the long physics explanation of multiphoton excitation, now reduced to a plain-language summary.
- **Updated wording:** the old page called the micrometastasis technology "patent-pending". It is now US 11,547,300 (per the Publications page), so the text says "issued patent".

### Software → Platforms

- **Migrated:** the 4 tools became platform entries. The LED-array PDT controller (pdtV4) is merged with the LED hardware into one **hardware** entry, illustrated with the old Research page's LED photo. The mosaicking tool keeps both repos (Multi-Channel-Mosaicking and MultiMosaic) plus the bioRxiv preprint link.
- **Citations** now point to `publications.yaml` keys, so each one exists in only one place.
- **Added a placeholder** "instrument" entry for the multiphoton microendoscopy setup, as requested. It has no content yet.
- **Dropped:**
  - the LabVIEW controller screenshot;
  - links to Wikipedia and the AGE-II paper;
  - technical NNLS references, kept only as a short summary.

### Collaborators

- **Migrated:** the Translational Biophotonics Cluster plus 6 external collaborators, as a simple list. Each entry has the lab name, PI, institution, a short reworded description and its URL.
- **Dropped:**
  - collaborator headshots (not the lab's photos);
  - long biographical details (positions held, dates);
  - the Boston Globe 1999 clipping link;
  - the Wired article link;
  - the Knox / Förster frisbee anecdote and the photo adapted from Knox, JBO 2012;
  - the "5 degrees from Einstein" trivia.

### Home and News

- **Intro:** the two mission paragraphs became one plain-language paragraph. The headline is kept.
- **Buttons:** the old buttons ("Learn more about our research", "List of Publications", "Contact Professor Spring") are replaced by the hero buttons plus 6 teaser cards (`data/home.yaml`).
- **Social icons** moved to the footer.
- **Photos:** the two home photos (credited to Bryan Spring and Nima Davoudzadeh) are now used on teaser cards.
- **News since 2021:** 23 items, one Markdown file each, all reworded. That includes the 4 items from Jan–Aug 2021 that sat in the old "Older News" section.
- **News before 2021:** about 10 items on `/older-news/`, reworded and grouped by year. Two Northeastern article links had no date on the old page; the one whose URL contains 2016/02 is listed as Feb 2016, the other as "Undated".
- **News images kept:** the three thesis-defense photos (Harman, Timilsina, Price) and two figures from the lab's own papers (Lang 2021, Harman 2022). The Electro Optics item reuses the lab's LED-platform photo.
- **News images dropped:**
  - graphics owned by others: Scialog, the Electro Optics cover, the IPA podcast banner, the Smith Family event collage, the Northeastern Global News photo, and two Northeastern-news thumbnails;
  - individual headshots attached to news items (Yuchen Cheng, Annabel Devault, Rami Taha, Jvalanti Prasad).
- **Facts from news applied to other pages:**
  - Sophia Renauld's next step (Harvard–MIT M.D.–Ph.D.) → `people.yaml`.
  - Eric Kercher as the lab's first Ph.D. graduate → `people.yaml`.
  - Heiko Enderling's affiliation: the June 2023 news names MD Anderson, so `collaborators.yaml` now says MD Anderson and links his MD Anderson profile.
- **Dropped from news text:** hashtags and Twitter handles, bit.ly links, the claim that the denoising code "will be released soon at springlabnu.com/software", and long praise passages, which were shortened.

### Contact Us → Join Us and footer

- **Contact details:** the address (ISEC Room 314, 805 Columbus Ave, Boston MA 02120; the old page had the typo "Colombus"), "Department of Physics" and the email now live in `hugo.toml` and appear in the footer.
- **Undergraduate volunteering:** the four areas (cell culture, electronics, optics, computational biophysics) became the "Learning opportunities" list on Join Us.
- **Volunteer form:** the existing Google Form (`forms.gle/XB4iHnKQyjsTBsheA`) is now embedded through the `google-form` shortcode. The embed URL was taken from the short link's redirect. A fallback "open in new tab" link sits below it.
- **Grads and postdocs:** a new "email the PI" section, as specified in the brief.
- **Dropped:** two banner images on the Contact page and the Northeastern Physics logo.

---

## Open TODOs (for the site owner)

### People
- [ ] Confirm the current/alumni status of everyone listed as alumni. Gabriel Hill (class of 2026) and Nikolas Sanderson (class of 2026) in particular have bios that read as current.
- [ ] **Aryan Mehrotra:** the old page listed "B.A. in Biochemistry, Earlham College, 2020" for him. That is identical to Aden Johnson-Shoucair's degree and contradicts his bio (B.S. Computer Engineering, Northeastern, 2023). I used the bio's degree. Please confirm.
- [ ] **Guillaume Ducourthial** and **Matthew Waguespack:** the old page gave no title or position, so `role` is empty. Add one if known.
- [ ] **Rebecca Harman:** the year of her Ph.D. isn't given.
- [ ] Alumni `years` are often only "joined YYYY". Add end years and `now` (next position) where known.
- [ ] The PI's email is shown as `b.spring@northeastern.edu`, decoded from the old site's "b dot spring at northeastern dot edu". Please confirm.
- [ ] The large photo at the bottom of the old People page was not migrated. Tell me if it's a group photo you want on the site.

### Join Us
- [ ] **The volunteer form requires Google sign-in.** In the preview the embedded form showed "Sign in to your Google Account". Google often blocks sign-in inside embeds, so visitors may only be able to use the "open in new tab" link. To fix it, in Google Forms → Settings → Responses, turn off "Restrict to users in …" (and "Collect email addresses: Verified" if you can use "Responder input" instead). Or keep the restriction and accept the link-only behaviour.
- [ ] **Undergraduate welcome text:** I wrote it from facts elsewhere on the site. Please review the tone, and add any requirements such as hours per week or minimum semesters.

### Home and News
- [ ] **Home video:** add a YouTube or Vimeo URL as `hero_video:` in `content/_index.md`. A placeholder box shows until then.
- [ ] **Two people not on the People page:** Yuchen Cheng (2024 news) and Annabel Devault (2023 news) appear in news but weren't listed as members or alumni. Add them to `people.yaml` as alumni if appropriate.
- [ ] **June 2020 statement on racial injustice:** it's summarized, not quoted, in `content/older-news.md`. Decide whether to keep the summary, restore the full original wording (I can't copy it verbatim under the migration rules, but you can paste it), or remove it.
- [ ] **Denoising code:** the July 2022 news said it would be released. If it's public now (e.g. inside HyperViewer), add a link.
- [ ] **Home photos:** check that the teaser images on the home page are the ones you want.

### Research
- [ ] **The old Research text may be years out of date.** Check that the 7 projects still describe current work. Two things to check in particular: whether the Cellaria Bio collaboration is still active, and the target cancers listed for microendoscopy (brain, pancreas, ovary).
- [ ] **Computation pillar:** it has no image. Add one (e.g. a screenshot of the hyperspectral software) at `assets/images/research/…`.
- [ ] **Funding list:** confirm it's complete and current. For example, the Aug 2026 news mentions a new NCI R01 (CA296584).
- [ ] **Photo license:** confirm the lab may keep using Spencer A. Cohen's photos. If credit is required, we can add a credit line.

### Platforms
- [ ] **Endoscopy setup entry:** it's a placeholder. Say what is published (CAD files, parts list, code?) and give a link. Rename it, or delete it if nothing is public yet.
- [ ] **Licenses:** none are stated on the old site. Check each GitHub repo and fill in `license`.
- [ ] **HyperViewer:** add an image (e.g. a screenshot) if you have one.
- [ ] **Compound prism designer:** it lives in a personal GitHub account (`jdtatz`), not the lab's. Consider forking or transferring it to the springlabnu org.
- [ ] **Possible new entry:** the femtosecond fiber laser build has a published step-by-step protocol (JoVE 2019). Add it as a platform if you'd like.

### Collaborators
- [ ] **Affiliations and URLs may be stale.** They come from an old page. Please check each one. Heiko Enderling was moved to MD Anderson based on the 2023 news item; also check his lab's name and the Hasan Lab URL.
- [ ] **Brain Tumor Stem Cell Lab (Wakimoto):** no URL was given. Add one.
- [ ] **Robert S. Knox:** he's an individual (emeritus), not a lab. Keep him here, or drop him.
- [ ] **Partner descriptions:** ideally each partner reviews or supplies its own text.

### Publications
- [ ] **DOIs are missing** for 14 entries that link only to a publisher page:
  - Kercher (JID, accepted)
  - Price 2026 (BOE 17:202)
  - Zhang 2024 (BOE 15:6324)
  - Zhang 2019 (JoVE e60160)
  - Davoudzadeh 2019 (Sci Rep 9:4233)
  - Tangutoori 2016
  - Spring 2015 (PPS)
  - Mallidi 2015
  - Spring 2014 (PNAS)
  - Zhong 2009 (BJC)
  - Knox 2003
  - the 3 book chapters (Chen 2012, Spring 2009, Chen 2009)

  Five more have no link at all: Saad (Nat Commun, accepted), the 2 embargoed papers, Spring 2014 (JBO 19:066006) and Spring 2013 (JBO 18:096015).

  Add a `doi:` to each when you can; it's the most durable link.
- [ ] Kercher et al. "Cancer cell-targeted and activatable photoimmunotherapy spares T cells…" is listed as 2019, but its volume is 96(2), which is the 2020 issue. Please confirm the year.
- [ ] Check whether the 2 embargoed papers (Kercher, Sci Rep; Lang, J Biomed Opt) and the 2 accepted ones (Saad, Nat Commun; Kercher, JID) are out now. If so, add `year`, `doi`, and set `status: published`.
- [ ] **Copyright:** several PDFs are publisher versions. Confirm that sharing them on Google Drive is allowed under each journal's policy.
