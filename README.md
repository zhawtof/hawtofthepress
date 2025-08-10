# hawtofthepress

Monospace brutalist Ghost theme with first‑class Ghost Portal and built‑in search support.

![Theme preview](docs/screenshot.png)

## Features

- **Header & navigation**: sticky header with brand and primary navigation; responsive mobile drawer
- **Secondary nav (tags/categories)**: optional bar under header using Ghost’s Secondary Navigation
- **Search**: integrates Ghost’s built‑in `{{search}}` helper in the header
- **Dark mode**: system‑aware dark theme with a header toggle; preference is persisted
- **Feed**: vertical DATE — Title layout with reading time, tags, featured badge, zebra striping
- **Featured posts**: compact featured section on the homepage
- **Accessibility**: skip‑to‑content link, focus styles, semantic landmarks
- **Members**: Portal subscribe/sign‑in in header; post‑page subscribe CTA
- **Typography**: system monospace by default; optional serif toggles via CSS utility classes
- **SEO‑native pages**: `index.hbs`, `post.hbs`, `page.hbs`, `tag.hbs`, `author.hbs`

## Navigation setup

- **Primary navigation (pages menu)**: Managed in Ghost Admin → Settings → Navigation. Links appear in the header. The "Home" item is automatically hidden (`.nav-home`) because the logo/title links to home.
- **Secondary navigation (tags/categories)**: Managed in Ghost Admin → Settings → Navigation (Secondary). Appears as a thin bar under the header. You can link tags (e.g. `/tag/news/`, `/tag/guides/`) or any URLs you want to surface.

## File structure

- `default.hbs`: base template; includes `partials/header.hbs`, `partials/footer.hbs`; injects RSS link and icons
- `partials/header.hbs`: brand, `{{navigation}}` (primary), `{{search}}`, Portal links, mobile drawer, and `partials/components/tag-nav.hbs` (secondary nav)
- `partials/components/theme-toggle.hbs`: reusable dark‑mode toggle button component
- `partials/footer.hbs`: copyright, "Built with" line, and RSS link (`partials/components/rss-link.hbs`)
- `index.hbs`: homepage with Featured section and feed list + pagination
- `post.hbs`: post template with feature image, content, subscribe CTA, and prev/next nav
- `page.hbs`: page template with optional title/feature image
- `tag.hbs`, `author.hbs`: archive templates with compact headers
- `assets/css/screen.css`: authored theme styles (source)
- `assets/built/screen.css`: compiled/minified output from the build (not linked by default)
- `assets/js/theme-toggle.js`: client script that applies and persists the theme

## Dark mode

- The theme is system‑aware and applies dark mode by default when the OS prefers it. A header toggle lets visitors switch modes, and the choice is saved to `localStorage`.
- The toggle component lives at `partials/components/theme-toggle.hbs`. To render it elsewhere, include:

  ```hbs
  {{> "components/theme-toggle"}}
  ```

- An inline script in `default.hbs` sets the theme class early to avoid flash of incorrect theme and updates the `<meta name="theme-color">` for mobile address bars.
- Colors are driven by CSS variables. Key tokens include: `--c-bg`, `--c-fg`, `--c-border`, `--c-subtle`, `--c-muted`, `--c-link`, `--c-link-visited`, `--c-surface`, `--c-surface-hover`, `--c-surface-alt`, and icon colors `--c-icon-sun`, `--c-icon-moon`. Override these in your own CSS if you want a different palette.
- The icon glyphs can be changed by editing `partials/components/theme-toggle.hbs`; colors will follow the tokens above.

## Development

Gulp + PostCSS pipeline builds CSS, live‑reloads with BrowserSync, validates with gscan, and creates a zip for upload.

### Prereqs

- Node >= 18 and pnpm
- Local Ghost at `http://localhost:2368`

### Install deps

```bash
pnpm install
```

### Start dev (Ghost + theme watcher)

```bash
pnpm dev
```

- Proxies Ghost at `http://localhost:2368` for live reload
- Watches `assets/css/**/*.css` and all `.hbs` templates

### Build once (production)

```bash
pnpm build
```

- Processes `assets/css/screen.css` with PostCSS (`postcss-import`, `postcss-nested`, `autoprefixer`)
- Minifies with `cssnano` in production
- Writes output to `assets/built/screen.css`

### Lint theme with gscan

```bash
pnpm lint
```

### Create distributable zip (in `dist/`)

```bash
pnpm zip
```

### Running Ghost locally from the parent directory

From the repository’s parent directory, install Ghost once if needed:

```bash
ghost install local
```

Then use these package scripts from this theme folder:

- `pnpm run ghost:dev` — run Ghost in foreground
- `pnpm run ghost:start` — start Ghost in background
- `pnpm run ghost:stop` — stop Ghost

In Ghost Admin → Settings → Design, upload the zip from `dist/` or symlink this theme for development.

## Notes on CSS

- The theme links `assets/css/screen.css` directly for readability during development. The build also emits a minified copy to `assets/built/screen.css`. If you prefer, you can change the stylesheet link in `default.hbs` to point to the built file for production.

## Compatibility

- Requires **Ghost >= 5.130.2**
- Tested with Ghost Portal members enabled/disabled

## License

MIT

## Releases

- Releases are managed by a GitHub Actions workflow using release-please.
- On pushes to `main`, a release PR may be opened/updated with a changelog and version bump based on conventional commits (e.g., `feat:`, `fix:`).
- When the release PR is merged, a Git tag and GitHub Release are created automatically.
- A separate workflow builds the theme zip and uploads it to the GitHub Release as an asset.

### Triggering a release manually

- Go to Actions → "Release (manage via release-please)" → Run workflow.
- Or merge a conventional commits PR to `main` and the workflow will handle it automatically.

### Downloading the theme zip

- After a release is published, the built zip (e.g. `hawtofthepress-<version>.zip`) is attached to that release.
