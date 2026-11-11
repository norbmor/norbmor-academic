# Project Context & Instructions for Antigravity (`norbmor-academic`)

This repository contains the source code and content for **Norberto Moreno-Quibén's** personal academic website, built with **Hugo** and deployed automatically to **Netlify** at **https://www.quiben.net/** (Netlify subdomain: **https://norbmor.netlify.app/**).

## 1. Architecture, Domain & Deployment
- **GitHub Repository**: `https://github.com/norbmor/norbmor-academic` (`main` branch, SSH remote `git@github.com:norbmor/norbmor-academic.git`).
- **Netlify Project**: `norbmor` (Site ID: `440e1472-ec4a-4259-9aac-df0dcec5528c`).
- **Primary Custom Domain**: `https://www.quiben.net/` (`baseURL` in `hugo.yml`), with `https://norbmor.netlify.app/` as the underlying Netlify site URL.
  - **Custom Domain & DNS Configuration (`quiben.net` via DreamHost / Netlify)**:
    - In Netlify (**Domain management**), `www.quiben.net` and `quiben.net` are registered as custom domains with automatic Let's Encrypt HTTPS provisioning.
    - **Option A (Netlify DNS)**: Delegate nameservers in DreamHost (**Nameservers → Change**) from `ns1–ns3.dreamhost.com` to the 4 Netlify `nsone.net` nameservers.
    - **Option B (DreamHost DNS Only)**: Switch `quiben.net` from *Fully Hosted* to *DNS Only* in DreamHost (**Websites → Manage Websites**) to unlock `@` and `www` while preserving DreamHost email records, then add an `A` record on `@` pointing to `75.2.60.5` and a `CNAME` record on `www` pointing to `norbmor.netlify.app`.
- **Build Configuration** (`netlify.toml`):
  - Runs `hugo --gc --minify` with `HUGO_VERSION = "0.154.5"` (`HUGO_EXTENDED = "true"`).
  - Pushing to `origin main` automatically triggers a production build and deploy on Netlify (`https://www.quiben.net/`).
  - Note: Do not add a GitHub Pages Actions workflow (`.github/workflows/`) unless GitHub Pages is explicitly enabled in the repo settings.

## 2. Design & Styling (`consequently.org` + UAH Corporate Blue)
- The layout and typography are modeled after Greg Restall's `https://consequently.org/` using Bootstrap 5 and Google Fonts (**Alegreya**, **Alegreya SC**, **Alegreya Sans**, **Alegreya Sans SC**) in `static/css/style.css`.
- **Color Palette**:
  - Primary color (`--bs-primary`): `#002C91` (Universidad de Alcalá official corporate blue, Pantone 661 C).
  - Secondary color (`--bs-secondary`): `#5A76B0` (muted slate blue for borders, dates, pilcrows, and badges).
- **Conventions**:
  - Homepage introduction (`content/_index.md`) uses inline pilcrow separators (`<span class="text-secondary">&para;</span>`) between sentences/topics and a left-floated portrait thumbnail (`/img/foto-NMQ-2018.jpg`).
  - Email address (`norberto.morenoquibe@uah.es`) in `layouts/partials/footer.html` must remain **non-clickable** plain text styled in UAH blue (`<span class="text-primary">norberto.morenoquibe@uah.es</span>`).

## 3. Content Structure & How Pages Update
The homepage (`layouts/index.html`) dynamically pulls content from the `content/` folder:
- **Homepage Bio**: Edit `content/_index.md`.
- **Curriculum Vitae**: Edit `content/cv/_index.md` (plain Markdown) and replace `static/files/my-cv.pdf` as needed. Updates both `/cv/` and the left column of the homepage.
- **Research Papers**: Each paper is a Markdown file in `content/research/<slug>.md` with frontmatter:
  ```yaml
  ---
  title: "Paper Title"
  date: YYYY-MM-DD
  kind: "paper"                        # "paper" (document icon, default), "chapter" or "thesis" (book icon)
  citation: "Author (Year). “Title.” *Journal*..."
  file: "filename-in-static-files.pdf" # optional
  doi: "https://doi.org/..."           # optional
  external_url: "https://..."          # optional
  status: "unpublished"                # optional ("unpublished" shows tools icon)
  keywords: "kw1; kw2"                 # optional
  ---
  Abstract text here...
  ```
  Adding a `.md` file in `content/research/` automatically updates the **Recent Research** list on the homepage, the year-grouped archive at `/research/`, and creates a dedicated abstract page.
- **Conferences**: Edit `content/conferences/_index.md` using plain Markdown. Each conference entry starts with the microphone icon `<i class="text-primary bi bi-mic me-1"></i>` and bold year (`**YYYY**.`). Links written as `[PDF](/files/<filename>.pdf)` automatically render as a rounded `PDF` pill badge. Adding a new entry at the top of `content/conferences/_index.md` automatically updates both `/conferences/` and the **Recent Conferences** section (top 3 entries) on the homepage.
- **Teaching Materials**: Edit `content/teaching/_index.md` using plain Markdown. Any link ending in `.pdf` or pointing to `github.com` is automatically styled with an icon and a rounded pill badge (`PDF` or `GitHub`) by `layouts/_default/_markup/render-link.html`.
- **Static Files / PDFs**: Place downloadable PDFs in `static/files/` and reference them as `/files/<filename>.pdf` (or `file: "<filename>.pdf"` in research paper frontmatter).
