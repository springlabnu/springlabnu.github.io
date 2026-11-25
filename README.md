# Spring Lab website

The public website of the **Spring Lab** at Northeastern University (photomedicine and biophysical microscopy of cancer, PI Bryan Q. Spring).

- **Live site:** <https://springlabnu.github.io/> (custom domain coming later; see [Domain](#domain-and-dns))
- **Built with:** [Hugo](https://gohugo.io/), a static site generator. Pages are plain Markdown and YAML files, and the site is hosted for free on GitHub Pages.
- **Owner:** the `springlabnu` GitHub organization. Repository: `springlabnu/springlabnu.github.io`.

Using Claude Code? It reads [CLAUDE.md](CLAUDE.md), which has the full schemas, rules and step-by-step recipes. [MIGRATION.md](MIGRATION.md) records how content moved from the old Google Site and lists open TODOs.

---

## Quick edits in the browser (no install needed)

Most updates are small text changes. You can make them directly on GitHub:

1. Open the file on github.com. Common ones:

   | To change… | Edit this file |
   |---|---|
   | People (members, alumni, photos, bios) | `data/people.yaml` |
   | Publications | `data/publications.yaml` (patents: `data/patents.yaml`) |
   | A news item | add a file in `content/news/` (copy an existing one) |
   | Collaborators | `data/collaborators.yaml` |
   | Platforms / software | `data/platforms.yaml` |
   | Research projects and funding | `data/research.yaml` |
   | Page intro text | `content/<page>/_index.md` |
   | Contact info, social links, volunteer form | `hugo.toml` |
   | Home-page video | `hero_video:` in `content/_index.md` |

2. Click the **pencil icon** (Edit). Or press `.` anywhere in the repo to open the full web editor.
3. Make your change, then click **Commit changes…**. Choose **"Create a new branch and start a pull request"** (recommended), or commit directly to `main` for a tiny fix.
4. On a pull request, wait for the green ✓ build check, then **Merge**. The live site updates about 1–2 minutes after anything reaches `main`.

**To add an image:** open the folder (e.g. `assets/images/people/`) → **Add file → Upload files**. Keep images at 1280 px or less on the long side and about 400 KB or less. Use lower-case names with hyphens (`jane-doe.jpg`), then reference the path in the YAML (`photo: people/jane-doe.jpg`). Don't upload videos; link YouTube or Vimeo instead.

YAML is picky about indentation. Copy an existing entry and change the values. If the build check turns red, open it to see the error line.

---

## Local preview (recommended for bigger changes)

### 1. Install Hugo, the exact pinned version

**Pinned version: Hugo v0.170.1, standard edition** (the "extended" edition isn't needed). Use the same version the build server uses, so what you see locally matches the live site.

- **Windows:** download `hugo_0.170.1_windows-amd64.zip` from <https://github.com/gohugoio/hugo/releases/tag/v0.170.1>, unzip `hugo.exe` into a folder (e.g. `C:\Tools\hugo\`), and add that folder to your PATH. Or try winget (it only works if winget offers this exact version; check that `hugo version` shows 0.170.1 afterwards):
  ```powershell
  winget install Hugo.Hugo --version 0.170.1
  ```
- **macOS:** download and run `hugo_0.170.1_darwin-universal.pkg` from the same release page.
- **Linux:** install `hugo_0.170.1_linux-amd64.deb` from the same release page (`sudo dpkg -i …`).

Check it:

```powershell
hugo version
```

The output should start with `hugo v0.170.1`.

You'll also need **git**. Install [Git for Windows](https://git-scm.com/download/win), or use [GitHub Desktop](https://desktop.github.com/).

### 2. Run the preview

```powershell
git clone https://github.com/springlabnu/springlabnu.github.io.git
```

```powershell
cd springlabnu.github.io
```

```powershell
hugo server
```

Open <http://localhost:1313/>. Pages reload automatically when you save a file. Press `Ctrl+C` to stop.

Before you push, run a full production build and make sure it shows no `ERROR` or `WARN` lines:

```powershell
hugo --gc --minify
```

---

## How deployment works

- `.github/workflows/hugo.yaml` runs on GitHub Actions. It installs Hugo 0.170.1, builds the site, and publishes it to GitHub Pages.
- **Push or merge to `main`:** build and deploy.
- **Pull request:** build only (a check), with no deploy.
- **Manual run:** Actions tab → "Build and deploy site" → **Run workflow**.
- Progress and errors show in the repo's **Actions** tab.

### One-time repository settings (already needed once; listed here for reference)

1. **Visibility:** the repo must be **Public**, unless the org has a paid plan that allows Pages for private repos.
2. **Settings → Pages → Build and deployment → Source:** choose **GitHub Actions**.
3. **Settings → Actions → General:** allow actions (at least "Allow GitHub-authored actions"). If the org restricts Actions, an org owner has to allow this at **Organization → Settings → Actions → General**.
4. **Settings → Environments → `github-pages`** is created automatically on the first deploy. It should allow deployments from the `main` branch.

### Upgrading Hugo

Change `HUGO_VERSION` in `.github/workflows/hugo.yaml`, and update the version in this README and in `CLAUDE.md`. Install the same version locally and check the site builds cleanly before merging.

---

## Repository layout

```
hugo.toml              site settings, navigation, contact info, volunteer form
content/               page text (Markdown); content/news/ = one file per news item
data/                  people, publications, patents, collaborators, platforms, research, home cards (YAML)
assets/images/         photos and figures (converted to WebP automatically)
layouts/shortcodes/    small embeds usable in Markdown (video, Google Form, PI contact link)
themes/springlab-minimal/  the look and feel (HTML templates + one CSS file); can be swapped
.github/workflows/     the build + deploy pipeline
```

The design is kept separate from the content, so a different theme can be dropped in later without rewriting any pages. See "Swap the theme" in [CLAUDE.md](CLAUDE.md).

---

## Ownership and access

- The repository belongs to the **`springlabnu` GitHub organization**. Org owners control who can edit. To add an editor, go to Organization → People → Invite member, then give them write access to this repo.
- Keep at least two org owners, so access isn't lost when someone leaves the lab.

## Domain and DNS

- **Current address:** <https://springlabnu.github.io/>
- **Planned custom domain:** `springlabnu.com`. It currently still points to the old Google Site.
- **DNS managed at:** `[PLACEHOLDER: registrar / DNS provider, account owner]`

When switching the domain (this hasn't been done yet):

1. Set `baseURL = "https://springlabnu.com/"` in `hugo.toml`.
2. Add the domain in **Settings → Pages → Custom domain**.
3. Update the DNS records at the registrar, following [GitHub's custom domain guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
4. Tick **Enforce HTTPS** once the certificate is issued.

You don't need to add a `CNAME` file in this repo; with Actions-based deploys, the Pages setting is what counts.
