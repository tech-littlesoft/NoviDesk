# NoviDesk Docs

Source of the official NoviDesk documentation site, built with Jekyll and served by GitHub Pages.

No third-party theme, no CSS framework, no CDN — the entire site is a few Markdown files plus one stylesheet, so it loads fast and stays reachable from mainland China.

## Live site

| Page | URL |
| --- | --- |
| Overview | https://tech-littlesoft.github.io/NoviDesk/ |
| User Guide | https://tech-littlesoft.github.io/NoviDesk/guide/ |
| Upgrade to Pro | https://tech-littlesoft.github.io/NoviDesk/buy/ |
| Privacy Policy | https://tech-littlesoft.github.io/NoviDesk/privacy/ |

Every page is independently addressable. The left sidebar highlights the current page; the right rail generates an on-page table of contents and tracks scroll position.

## Layout

```
.
├── _config.yml            # Jekyll / GitHub Pages settings
├── _layouts/
│   └── default.html       # Top bar, sidebar, content, TOC rail, footer, TOC script
├── _includes/
│   └── sidebar.html       # Left navigation
├── assets/
│   ├── css/style.css      # All styles: layout, typography, dark mode, print
│   └── img/               # Illustrations (.svg) and demo clips (.gif)
├── index.md               # Overview          ->  /
├── guide.md               # User Guide        ->  /guide/
├── buy.md                 # Upgrade to Pro    ->  /buy/
├── privacy.md             # Privacy Policy    ->  /privacy/
└── README.md
```

## Editing

| I want to change | Edit this |
| --- | --- |
| Page copy | `index.md` / `guide.md` / `buy.md` / `privacy.md` |
| Left navigation | `_includes/sidebar.html` |
| Header, footer, TOC behavior | `_layouts/default.html` |
| Styles, colors, spacing | `assets/css/style.css` |
| Illustrations | Replace files under `assets/img/` (`.png` / `.jpg` work too) |
| Site title, description, baseurl | `_config.yml` |

Bilingual pages use two helper classes: `.zh` for the Chinese line, `.en` for the English line beneath it.

The on-page table of contents is opt-in. Add `toc: true` to a page's front matter to enable it; omit it and the right rail disappears.

## Two conventions you must follow

**1. Static assets use relative paths.**

```html
<img src="assets/img/01-float.svg">
```

`_layouts/default.html` declares `<base href="{{ site.baseurl }}/">`, which resolves these against the repository root on every page, at any depth. No paths need updating when a page moves.

**2. Internal links must go through `relative_url`.**

```liquid
<a href="{{ '/buy/' | relative_url }}">Upgrade to Pro</a>
```

Do not write `href="/buy/"`. Because of the `<base>` tag above, a leading `/` resolves against the domain root and 404s (`.../buy/` instead of `.../NoviDesk/buy/`).

## Deployment

Pushing to `main` is the entire release process — GitHub Pages builds the site server-side.

```bash
git add .
git commit -m "Update docs"
git push origin main
```

Configure Pages once under **Settings → Pages → Source: `main` / `/ (root)`**.

## Local preview

Not required. GitHub builds the site remotely and there is nothing to compile.

To preview changes locally, install Ruby with Jekyll and run:

```bash
bundle init      # then add: gem "github-pages", group: :jekyll_plugins
bundle install
bundle exec jekyll serve --livereload
```

Open http://127.0.0.1:4000/NoviDesk/ — the `/NoviDesk/` prefix comes from `baseurl` and cannot be omitted.

## Custom domain

1. Add your domain under **Settings → Pages → Custom domain** and enable Enforce HTTPS.
2. Commit a `CNAME` file at the repository root containing the bare domain.
3. Update `_config.yml`:

```yaml
url: https://novidesk.com
baseurl: ""
```

Nothing else changes — links, stylesheets, and images follow automatically.
