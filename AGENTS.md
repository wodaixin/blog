# AGENTS.md

## Hugo Blog — Wodaixin

A Hugo-based personal blog (Stack theme), deployed to GitHub Pages.

## Dev Commands

```bash
hugo server -D        # Serve drafts locally (VSCode task: "Serve Drafts")
hugo                  # Build for production (outputs to ./public)
```

## Build Requirements

- **Hugo** (extended edition required for SCSS/Sass)
- **Go** 1.17+
- **Node.js** (for optional npm dependencies)
- **Dart Sass** (for SCSS compilation)

CI installs these automatically. Local dev needs: `hugo extended`.

## Content Structure

```
content/
├── post/          # Blog posts (main content)
├── page/          # Static pages (archives, links, search, etc.)
└── categories/    # Category index pages
```

Posts use standard Hugo markdown with frontmatter.

## Deployment

GitHub Actions workflow (`.github/workflows/deploy.yml`) builds on push to `main`/`master` and deploys to GitHub Pages. Theme is loaded as a Hugo module from `github.com/CaiJimmy/hugo-theme-stack/v4`.

## Config

- `config/_default/config.toml` — Base URL, language, title
- `config/_default/params.toml` — Theme params (sidebar, widgets, colors)
- `config/_default/menu.toml`, `markup.toml`, etc. — Additional config

## Important Notes

- `public/` is generated and gitignored — do not edit
- Theme is vendored in `_vendor/` (git submodule)
- `resources/` is generated and gitignored
- Run `hugo --gc --minify` for production builds (as CI does)
