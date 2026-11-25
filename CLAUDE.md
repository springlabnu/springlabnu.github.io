# CLAUDE.md — Spring Lab website

Guide for Claude Code sessions working on this repo. Read it fully before you edit anything. The person you're helping may not be technical, and this file is the only project context you get.

## What this is

The public website of the **Spring Lab** (photomedicine and biophysical microscopy of cancer) at Northeastern University, PI Bryan Q. Spring. It is a static **Hugo** site deployed to **GitHub Pages** from the `springlabnu/springlabnu.github.io` repo, which is owned by the `springlabnu` GitHub org.

- **Live URL:** <https://springlabnu.github.io/>. A custom domain (`springlabnu.com`) will be configured later; see "Domain" below.
- **Replaces:** the old Google Site at <https://sites.google.com/view/springlabnu>. What moved where, and the open TODOs, are in [MIGRATION.md](MIGRATION.md).

### Audiences, in priority order

1. **Undergraduate volunteers.** This is the top priority: the Join Us page and its call to action must always work. After them, graduate students from physics and bioengineering.
2. **Researchers, funders and collaborators.** They come for the research, the people and the news.
3. **Public and press.** Every major page opens with a short plain-language paragraph.

### Hard requirement: low maintenance

- No "by the numbers" stats.
- News is one flat chronological page.
- Collaborators is a simple list.

Don't add features that need regular upkeep.

## Hugo version (pinned)

**Hugo v0.170.1, standard edition (not extended).** The site uses plain CSS, so the extended edition isn't needed. WebP image encoding works in the standard edition.

The same version is pinned in three places. Change all of them together:

1. `.github/workflows/hugo.yaml` → `HUGO_VERSION`
2. This file
3. `README.md`

Hugo 0.170 deprecated some older names; use the new ones:

- `locale`, not `languageCode`
- `hugo.Data`, not `site.Data`
- `imaging.webp.quality`, not `imaging.quality`
- template folders `layouts/_partials/` and the `home.html` / `section.html` / `page.html` names

## Local preview

```powershell
hugo server
```

Then open <http://localhost:1313/>; it live-reloads on save. In Claude Code desktop you can also use the `hugo` entry in `.claude/launch.json`.

To run a full production build exactly like CI:

```powershell
hugo --gc --minify
```

The output goes to `public/`, which is git-ignored. **Always run this and check that it finishes with no `ERROR` or `WARN` before committing.**

**Git on the original admin's Windows machine:** `git` is not on PATH there. Use GitHub Desktop's bundled git, `%LOCALAPPDATA%\GitHubDesktop\app-<version>\resources\app\git\cmd\git.exe`, or install Git for Windows.

## Deploy

- **Pushes to `main`:** run `.github/workflows/hugo.yaml`, which builds and deploys to Pages (about 1–2 minutes). Check progress in the repo's **Actions** tab.
- **Pull requests:** build only (as a check); they don't deploy.
- **One-time setting:** Settings → Pages → Source = **GitHub Actions**. Don't add a `gh-pages` branch.
- **Recommended flow:** work on a branch, open a PR, let the build check pass, merge to `main`.

## Directory map

```
hugo.toml                  ← site config: baseURL (only here), theme line, nav menu, [params] contact/social/join form
content/                   ← ALL page text (Markdown). Theme-agnostic.
  _index.md                ← Home: headline, plain-language intro, hero_video (YouTube/Vimeo URL)
  research/_index.md       ← intro only; pillars/projects come from data/research.yaml
  platforms/_index.md      ← intro only; entries from data/platforms.yaml
  publications/_index.md   ← intro only; entries from data/publications.yaml + data/patents.yaml
  people/_index.md         ← intro only; entries from data/people.yaml
  collaborators/_index.md  ← intro only; entries from data/collaborators.yaml
  news/_index.md           ← News intro; news items are content/news/YYYY-MM-DD-slug.md
  news/*.md                ← one file per news item (no individual pages; listed on /news/)
  older-news.md            ← frozen archive of pre-2021 news at /older-news/ (linked from News)
  join/_index.md           ← Join Us: undergrad volunteers (+ learning opportunities + form), grads/postdocs
data/*.yaml                ← ALL structured content (schemas below)
assets/images/             ← original images (people/, research/, platforms/, news/); Hugo makes WebP at build time
static/                    ← files copied as-is (favicon, PDFs, etc.) — currently empty
layouts/shortcodes/        ← PROJECT shortcodes (survive a theme swap): video, google-form, contact-pi
themes/springlab-minimal/  ← the theme (layouts + assets/css/main.css). Replaceable.
.github/workflows/hugo.yaml← CI build + deploy, pinned Hugo version
scripts/                   ← place for helper scripts (e.g. a future BibTeX → publications.yaml importer)
MIGRATION.md               ← migration log + open TODO list for the site owner
README.md                  ← human-admin guide
```

## Conventions and rules (must follow)

1. **No invented facts.** Never make up people, papers, dates, affiliations, awards, grant numbers or numbers of any kind. If something is missing or uncertain, write `[PLACEHOLDER] …` and add a line to the TODO list in `MIGRATION.md`. To find every placeholder:
   ```powershell
   Select-String -Path content\*,content\*\*,data\* -Pattern 'PLACEHOLDER'
   ```
2. **No verbatim copying** from other websites, including the old Google Site. Extract facts and write new wording. Titles of papers, theses and grants are facts and are kept exactly.
3. **Nothing theme-specific in content.** Markdown in `content/` and YAML in `data/` must not contain HTML, CSS classes or theme names. If you need an embed or a widget, write a shortcode in **`layouts/shortcodes/`** (project level), never inside the theme. Raw HTML in Markdown is disabled (`unsafe = false`).
4. **Images:**
   - Put originals in `assets/images/<section>/` with descriptive kebab-case names, e.g. `people/jane-doe.jpg`.
   - Keep each original at **1280 px or less on the long side and about 400 KB or less**. Resize before committing if needed.
   - The theme outputs WebP at about 200 KB or less automatically.
   - Always provide alt text: `image_alt` fields in YAML or front matter. Headshots get "Photo of <name>" automatically.
   - Only commit images the lab owns or has permission to use.
   - **Never commit video files.** Use a YouTube or Vimeo link.
5. **Privacy:** the PI's email address is **deliberately not published** anywhere on the site. Don't add it back unless the owner asks. Contact goes through his faculty profile (`[params.contact].profile`, the `contact-pi` shortcode).
6. **`baseURL` lives only in `hugo.toml`.** Use relative links like `/people/` in content.
7. **Nav order is fixed:** Home · Research · Platforms · Publications · People · Collaborators · News · Join Us (`[[menus.main]]` in `hugo.toml`).
8. Keep the plain-language intro paragraph at the top of each major page.
9. After any change, run `hugo --gc --minify` and look at the affected page in `hugo server` before committing. Write clear commit messages.

## Content schemas

All files are YAML lists unless noted. Optional fields can be omitted or left as `""`.

### `data/people.yaml`

| field | required | notes |
|---|---|---|
| `name` | yes | display name, e.g. `"Kai Zhang, Ph.D."` |
| `group` | yes | `pi` \| `postdoc` \| `staff` \| `grad` \| `undergrad`. Sets the section heading for current members. |
| `status` | yes | `current` \| `alumni` |
| `role` | no | shown under the name, e.g. `"Ph.D. student, Physics"` |
| `education` | no | list of degree strings, newest first |
| `photo` | no | path under `assets/images/`, e.g. `people/kai-zhang.jpg` (square-cropped, face-aware) |
| `bio` | no | 2–4 plain sentences for current members, 1 sentence for alumni |
| `years` | no | alumni: `"2017–2021"` or `"joined 2019"` |
| `now` | no | alumni: next position |
| `links` | no | map with any of `website`, `profile`, `scholar`, `orcid`, `linkedin`, `github`, `cv` (and `email`, but see the privacy rule) |

Within a group, people show in file order. Current members appear 2 per row, alumni 3 per row.

```yaml
- name: "Jane Doe"
  group: grad
  status: current
  role: "Ph.D. student, Bioengineering"
  photo: people/jane-doe.jpg
  education:
    - "B.S. Physics, Example University, 2024"
  bio: >-
    Jane builds 3D tumor models to test light-activated drugs.
  links:
    linkedin: "https://www.linkedin.com/in/…"
```

### `data/publications.yaml`

Field names follow **BibTeX**, so a future script can generate this file from a `.bib` export. The mapping is `key`←citation key, `type`←entry type, `venue`←`journal`/`booktitle`, and `number`←`number`/issue.

| field | required | notes |
|---|---|---|
| `key` | yes | unique id, `firstauthorYEARword`. Other files reference it. |
| `type` | yes | `article` \| `inproceedings` \| `incollection` \| `book` \| `thesis` \| `preprint` \| `misc` |
| `title` | yes | exact title |
| `authors` | yes | list of `"Lastname Initials"` |
| `venue` | no | journal or book; book chapters use `"In: Book (series), editors. Publisher"` |
| `year` | no | omit it when there's no year yet; the entry then shows under **Forthcoming** |
| `volume`, `number`, `pages` | no | strings |
| `doi` | no | bare DOI (`10.1364/OE.475175`). Preferred. Add only if certain. |
| `url` | no | publisher page when there's no DOI |
| `pdf` | no | link to a PDF copy (Google Drive) |
| `status` | yes | `published` \| `accepted` \| `submitted` \| `embargoed`. Anything but `published` shows a label. |
| `note` | no | short text, e.g. `"Early view."` |

The page groups entries by year, newest first, keeping file order within a year. New papers go at the top of their year block.

```yaml
- key: doe2027example
  type: article
  title: "An example title"
  authors: ["Doe J", "Spring BQ"]
  venue: "Biomedical Optics Express"
  year: 2027
  volume: "18"
  pages: "100–110"
  doi: "10.1364/BOE.000000"
  pdf: ""
  status: published
```

### `data/patents.yaml`

Fields: `title`, `inventors` (list), `assignees` (list), `number`, `status` (`issued` \| `provisional` \| `pending`), `date` (YYYY-MM-DD, unquoted; the issue date for patents, the filing date for applications), and `url`. The page sorts entries by date, newest first.

### `data/collaborators.yaml`

Fields: `name` (lab name), `pi`, `institution`, `description` (1–3 sentences in new wording, ideally the partner's own description of its work plus what we do together), and `url` (the partner lab's own site). Entries show in file order.

```yaml
- name: "Example Lab"
  pi: "A. Person, Ph.D."
  institution: "Example University"
  description: >-
    Develops X. Together we study Y.
  url: "https://example.edu/lab"
```

### `data/platforms.yaml`

Fields:

- `name`
- `kind`: `software` \| `hardware` \| `instrument`. This sets the section.
- `summary`: Markdown allowed.
- `repo`: main source link.
- `docs`
- `links`: list of `{label, url}`.
- `license`
- `image` and `image_alt`
- `publications`: list of `publications.yaml` keys, shown as "How to cite".

### `data/research.yaml` (a map, not a list)

- **`pillars`:** list of `{id, title, summary, image, image_alt}`. The three are cell-culture, optics and computational.
- **`projects`:** list of `{title, pillars: [ids], summary (Markdown), image, image_alt, links: [{label,url}], publications: [keys]}`.
- **`funding`:** list of `{name, note, url}`.

### `data/home.yaml` (a map)

`teasers`: list of `{title, text, link, image}`. These are the poster-style cards on the home page.

### News items: `content/news/YYYY-MM-DD-short-slug.md`

Front matter fields:

- `title`
- `date` (YYYY-MM-DD)
- optional `link`: makes the title clickable
- optional `image`: path under `assets/images/`
- optional `image_alt`

The body is 1–3 short sentences. News items never get their own pages; the `cascade` in `news/_index.md` handles that.

## How-to recipes

### Add a person

1. Put a photo at `assets/images/people/firstname-lastname.jpg`: roughly square, face centered, 1280 px or less, about 400 KB or less.
2. Add an entry to `data/people.yaml` in the right place. Current members go above the `# ----- Alumni` comment line, in the order you want them shown.
3. Run `hugo server` and check `/people/`.

### Move someone to alumni

Set `status: alumni`. Add `years` and, if known, `now`. Shorten `bio` to one sentence. Move the entry below the alumni comment line.

### Add a news item

Create `content/news/2027-01-15-short-slug.md`:

```markdown
---
title: "Jane Doe wins the Example Award"
date: 2027-01-15
link: "https://…"          # optional
---

One or two sentences in plain language.
```

The newest three items also show on the home page automatically. Don't edit `older-news.md`; it's a frozen archive.

### Add a publication

Add an entry at the top of the right year block in `data/publications.yaml`. When an accepted or embargoed paper comes out, add `year` and `doi`, and set `status: published`.

### Add a collaborator

Append an entry to `data/collaborators.yaml`.

### Add a platform

Append to `data/platforms.yaml`. If the platform has a paper, add the paper to `publications.yaml` first and reference its `key`.

### Replace the home video

In `content/_index.md`, set `hero_video: "https://www.youtube.com/watch?v=XXXXXXXXXXX"` (or a `youtu.be/…` or `vimeo.com/…` link) and optionally `hero_video_title`. YouTube is embedded through youtube-nocookie.com. To embed a video anywhere else, use the shortcode `{{< video url="…" title="…" >}}`.

### Change the volunteer form

In `hugo.toml` → `[params.join]`:

- `formURL`: the share link.
- `formEmbedURL`: the form's `…/viewform?embedded=true` URL. In Google Forms: Send → `<>` → copy the `src` URL.

The Join Us page uses `{{< google-form >}}`.

### Change contact info or social links

Edit `hugo.toml` → `[params.contact]` and `[params.social]`. The footer updates on every page.

### Swap the theme

1. Put the new theme in `themes/<new-name>/`. No Hugo Modules, no npm.
2. Change `theme = "springlab-minimal"` in `hugo.toml`.
3. The new theme must implement this **data contract** (copy from `themes/springlab-minimal/layouts/` as a reference):
   - A base layout that renders `site.Menus.main` and a footer from `site.Params.contact` / `site.Params.social`.
   - `home.html`: `.Params.headline`, `.Content`, `.Params.hero_video` (YouTube/Vimeo URL → embed, empty → placeholder), `hugo.Data.home.teasers`, plus the latest 3 news items.
   - Section templates for `research`, `platforms`, `publications`, `people`, `collaborators` and `news`, each reading the matching `hugo.Data.*` file with the schemas above. News uses `.Pages.ByDate.Reverse` and links `.Params.older_news`.
   - A generic `section.html` / `page.html` for `join` and `older-news` that renders `.Content`.
   - CSS for the shortcode output classes `.embed`, `.embed-video`, `.embed-form`, `.embed-fallback` and `.embed-placeholder`.
4. Don't move shortcodes into the theme, and don't change `content/` or `data/` to suit a theme.

### Upgrade Hugo

Change `HUGO_VERSION` in the workflow, this file and `README.md`. Install the same version locally, run `hugo --gc --minify`, and fix any deprecation warnings before merging.

## Domain

Planned custom domain: **springlabnu.com**, which currently still points to the old Google Site. DNS is managed by `[PLACEHOLDER: registrar / DNS provider and who has access]`.

**Do not change DNS, add a `CNAME` file, or change the Pages custom-domain setting unless the owner explicitly asks.** When it's time:

1. Set `baseURL = "https://springlabnu.com/"` in `hugo.toml`.
2. Set the custom domain in Settings → Pages.
3. Update DNS at the registrar, following GitHub's instructions.
4. Enable "Enforce HTTPS".

## Theme notes (springlab-minimal)

- Plain CSS in `themes/springlab-minimal/assets/css/main.css`. Colors are tokens on `:root`. It's **light-only** by the owner's choice, with a 4 px corner radius.
- `_partials/img.html` turns `assets/images/…` into resized WebP files at 1x and 2x. A missing file logs a warning instead of breaking the build.
- `_partials/person-card.html` renders the two-row people card: photo and name/role/education on top, bio below.
- Accessibility: skip link, landmarks, `aria-current` in the nav, visible focus outlines, alt text on every image.
