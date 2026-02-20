# FrankJamison.com v2016

Static multi-page resume website (HTML/CSS/JavaScript). No build step; deploy by serving the files.

**Live preview:** https://frankjamison2016.fcjamison.com/

## What this demonstrates

### Design & UX

- **Consistent layout system**: shared header/nav/banner/content/aside/footer structure across all pages.
- **Branding and visual system**: an explicit color palette in CSS and a repeated “cube” motif used in headings and the header.
- **Page-specific hero banners**: each section uses a dedicated banner image (Employment/Education/Skills/Awards/Organizations) for fast orientation.
- **Readable typography**: custom webfonts loaded via `@font-face` with a serif display face for headings.
- **Information hierarchy**: content is grouped into scannable categories and nested sections; long resume content is made navigable with accordions.

### Front-end development

- **Progressive enhancement**: the site renders as plain HTML, then adds interactivity through JavaScript.
- **Two accordion implementations**:
  - jQuery UI accordion for nested list sections (Employment, Skills, Awards, Affiliations).
  - Checkbox-driven accordion markup for Education (`.cd-accordion-menu`) with animated open/close behavior.
- **Animated navigation**: Animsition adds fade-in/fade-out transitions between internal pages.
- **Asset strategy**:
  - Local images for banners/icons.
  - Social icons embedded as base64 data URIs in CSS to reduce extra HTTP requests.
- **Practical HTML details**: `mailto:` and `tel:` links, `alt` text for the profile image, and a straightforward folder layout.

## Pages

- `index.html` — Home / summary + contact info
- `employment.html` — Employment, military, and volunteer history (accordion)
- `education.html` — Formal education and coursework (checkbox accordion)
- `skills.html` — Skills grouped by category (accordion)
- `awards.html` — Awards grouped by category (accordion)
- `affiliations.html` — Organizations/affiliations grouped by category (accordion)

## Tech stack

- HTML5
- CSS (single shared stylesheet)
- JavaScript
- External libraries (loaded via CDN in each HTML page):
  - jQuery 2.2.4
  - jQuery UI 1.11.4
  - Animsition 4.0.2

## Project structure

- `_css/style2.css` — site-wide styles: palette, typography, layout, banners, accordion styling, contact/social styling
- `_javascript/script.js` — jQuery UI accordion initialization + Animsition configuration
- `_javascript/main.js` — Education page checkbox-accordion animation
- `_images/` — banners, icons, profile image
- `_fonts/` — Cantarell, Cardo, Ostrich webfont files + included license texts
- `.vscode/tasks.json` — optional VS Code task to open a local URL in Chrome

## Run locally

This is a static site, so you can preview it a few different ways depending on what you want to validate.

### Option A: open directly (fastest)

Open `index.html` in your browser.

This is usually fine for visual/layout work. If you run into browser restrictions around local files, use Option B.

### Option B: run a tiny static server (recommended)

Running a local server avoids browser restrictions and ensures relative assets behave consistently.

- Python (from the repo root):
  - `python -m http.server 8080`
- Node:
  - `npx serve .`

Then open the printed URL (typically `http://localhost:8080/`).

Tip: if you want the URL to look more like the VS Code task / production-style hostnames, you can also open `http://frankjamisoncomv2016.localhost:8080/` while the server is running.

### VS Code shortcut (optional)

There’s a VS Code task named **Open in Browser** (see `.vscode/tasks.json`) that opens `http://frankjamisoncomv2016.localhost/` in Chrome.

Notes:

- The task URL does **not** include a port, so it assumes something is serving the site on port 80 for that hostname (for example, an IIS/Apache/Nginx vhost). If you’re using Option B on port 8080, update the task URL to `http://frankjamisoncomv2016.localhost:8080/`.
- `*.localhost` commonly resolves to `127.0.0.1` in modern browsers/OSes. If your environment does not resolve it, change the URL in `.vscode/tasks.json` or just use `http://localhost:8080/` from Option B.
- The task only opens the URL; it does not start a server. Use Option A or B to actually serve the site.

## Troubleshooting

- **Blank styling / missing images:** confirm you’re serving the repo root (so `_css/`, `_images/`, `_javascript/` resolve) and that you didn’t change folder names.
- **VS Code task opens a site that doesn’t load:** you likely don’t have anything serving `http://frankjamisoncomv2016.localhost/` on port 80. Either stand up a vhost on port 80 or change the task URL to include your dev server port (commonly `:8080`).
- **Animations/accordions don’t work:** check that the CDN scripts are reachable (corporate proxy/offline). If you need offline operation, vendor the libraries locally and update each HTML file.
- **File URL quirks (`file:///...`)**: use Option B to avoid browser security restrictions around local files.

## How it’s built

### Shared layout pattern

Each page follows the same major sections:

- `header#pageHeader` (name + subtitle)
- `nav#mainNav` (section navigation)
- `#banner` (page-specific banner class, e.g. `#banner.skills`)
- `article` (main content)
- `aside` (contact card)
- `footer#pageFooter`

### Interactive content

- List-based accordion sections use `.trackAccordion` and are initialized in `_javascript/script.js`.
- Education uses `.cd-accordion-menu` markup styled in `_css/style2.css` and animated in `_javascript/main.js`.

### Animated page transitions

Animsition is applied to the `.animsition` wrapper and triggers when following links with `.animsition-link`.

## Notes for maintainers

- Navigation is duplicated in each HTML file; if you add/remove a page, update the `<nav>` block everywhere.
- The layout is **desktop-first and fixed-width** (e.g., `body { width: 1280px; }`). If you want modern mobile support, the quickest path is adding a `meta viewport` tag and converting layout to a responsive grid/flex approach.
- This project depends on CDNs for its JS/CSS libraries; for offline deployments, vendor those assets locally and update the `<script>`/`<link>` tags.

## Making changes

### Content edits

- Each page is a standalone HTML file; there is no templating.
- Navigation is duplicated across pages. If you add/remove/rename a page, update the `<nav id="mainNav">` block in every HTML file.

### Styling

- Global styles live in `_css/style2.css`.
- Page banners are controlled by `#banner` + a page-specific class (e.g., `.home`, `.skills`), with background images defined in CSS.

### JavaScript behavior

- `_javascript/script.js` initializes jQuery UI accordions (`ul.trackAccordion`) and configures Animsition page transitions.
- `_javascript/main.js` adds the checkbox-driven accordion animation used by `education.html`.

If you remove or replace any of these patterns, also remove the associated `<script>`/`<link>` tags in each HTML file.

## Deployment

This site can be hosted on any static host (IIS, Apache, Nginx, S3/static hosting, etc.). Publish by copying the repository contents (at minimum the `*.html` files plus `_css/`, `_javascript/`, `_images/`, `_fonts/`).

CDN dependencies:

- jQuery 2.2.4
- jQuery UI 1.11.4
- Animsition 4.0.2

If you need fully offline hosting, download/vendor those assets into the repo and update the HTML references.

## Attribution

- Font license files are included in `_fonts/`.
