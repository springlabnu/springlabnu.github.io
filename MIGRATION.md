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
