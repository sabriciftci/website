# Migration Memo: Hugo Academic → Quarto Website
**For:** Claude Code  
**Project:** sabriciftci.com  
**Date:** May 2026

---

## Project Context

Migrating a personal academic website from **Hugo Academic (Wowchemy)** to **Quarto** in R. The site is currently built with Hugo/Wowchemy, stored in a **Dropbox folder** on the local machine, pushed to **GitHub**, and deployed via **Netlify**. The deployment pipeline (GitHub → Netlify) stays the same — only the site engine changes.

Current live site: https://www.sabriciftci.com  
Current stack: Wowchemy 5.0.0-beta.3 for Hugo

---

## Goal: Simple 3-Page Quarto Website

Keep only these pages:

| Page | Status |
|------|--------|
| Home (front page) | Keep — redesigned |
| Books | Keep — as-is or lightly restyled |
| Articles | Keep — restyled (see below) |
| Teaching | **Drop** |
| Contact | **Drop** (contact info goes in footer only) |

---

## Page 1: Home Page

### Layout
- Profile photo centered at top (same photo: `avatar_hu25d333fcded103db2f52f7476cb1420a_220002_270x270_fill_q75_lanczos_center.jpg`)
- Name and title below photo
- **5 icons only** — in a horizontal row below the title:

| Icon | Link |
|------|------|
| Email | ciftci@ksu.edu |
| Google Scholar | https://scholar.google.com/citations?user=TQOlhZQAAAAJ&hl=en&oi=ao |
| ResearchGate | https://www.researchgate.net/profile/Sabri-Ciftci |
| CV (download) | Dropbox PDF link (update to current CV path) |
| LinkedIn | https://www.linkedin.com/in/sabri-ciftci-7309b9306/ |

> ❌ **Remove:** Twitter/X icon entirely  
> ❌ **Remove:** All the bottom-heavy widgets (experience timeline, education list, interests section, contact form) that Wowchemy generates automatically

### Bio
Keep the biography paragraph text. Place it directly below the icons — clean, no widget boxes.

---

## Page 2: Articles

### Design Reference
Model after: https://blaydes.people.stanford.edu/articles

### Style
- Plain, clean, academic list
- Each entry = one line (or short block): **Title** (with co-authors if any), *Journal Name*, Volume/Year, \[pdf\] link
- No cards, no abstracts, no thumbnails
- Reverse chronological order (newest first)
- Simple `##` year-group headers optional, or just a flat list

### Content
Carry over all existing articles from the current Research page. Confirm article entries and PDF links during migration.

---

## Page 3: Books

Keep existing books page. Lightly restyle to match the clean aesthetic of the new site (no Wowchemy card widgets). Simple list or minimal card layout is fine.

---

## Navigation Bar

```
[Home]   [Books]   [Articles]
```

No Teaching. No Contact in nav (contact email can go in footer).

---

## Quarto Configuration Notes

### `_quarto.yml` structure
```yaml
project:
  type: website

website:
  title: "Sabri Ciftci"
  navbar:
    left:
      - href: index.qmd
        text: Home
      - href: books.qmd
        text: Books
      - href: articles.qmd
        text: Articles

format:
  html:
    theme: [cosmo, custom.scss]   # or flatly — pick a clean academic theme
    css: styles.css
```

### File structure to create
```
/
├── _quarto.yml
├── index.qmd          # Home page
├── books.qmd          # Books page
├── articles.qmd       # Articles page
├── styles.css         # Custom overrides
├── custom.scss        # Theme tweaks if needed
└── media/
    └── avatar.jpg     # Profile photo
```

---

## Deployment Pipeline (Unchanged)

- Files live in **Dropbox folder** on local machine
- Push to **GitHub** repo (same repo, just replace Hugo files with Quarto files)
- **Netlify** auto-deploys on push

### Netlify build settings to update:
```
Build command:  quarto render
Publish directory: _site
```

> Note: Netlify will need R + Quarto installed or use a Docker-based build. Simplest option is to **render locally** (`quarto render` in terminal) and push the `_site/` folder, or set up a `netlify.toml` with the right build image.

---

## What to Clean Up From Hugo

When removing Hugo files, delete or ignore:
- `config.toml` / `config/_default/` (Hugo config)
- `content/` folder structure (Hugo content)
- `themes/` folder (Wowchemy theme)
- `static/` — keep any images/PDFs you want to carry over
- `layouts/` — Hugo-specific, not needed in Quarto

---

## Summary Checklist for Claude Code

- [ ] Initialize Quarto website project in the Dropbox folder
- [ ] Create `_quarto.yml` with 3-page nav
- [ ] Build `index.qmd` — photo, title, 5 icons (Email, Scholar, ResearchGate, CV, LinkedIn), bio paragraph
- [ ] Build `articles.qmd` — flat academic list, Blaydes-style, all existing articles
- [ ] Build `books.qmd` — clean list of existing books
- [ ] Add `styles.css` for any custom styling
- [ ] Remove Hugo/Wowchemy files
- [ ] Update Netlify build settings (`quarto render` / `_site`)
- [ ] Test locally with `quarto preview`
- [ ] Push to GitHub and confirm Netlify deploys correctly
