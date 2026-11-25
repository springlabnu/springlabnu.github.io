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
