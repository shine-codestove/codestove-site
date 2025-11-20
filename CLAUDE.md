# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static website for CodeStove (코드스토브), an IT solutions company. The site is built using the OneFlow Jekyll theme, which is designed for creating one-page websites with multiple sections. The site is primarily in Korean (locale: ko-KR) and showcases services in Enterprise Software, AIoT, and IoT Business.

**Live URL**: https://codestove.io
**Repository**: ryoostar/codestove.io

## Development Commands

### Local Development with Docker
```bash
docker compose up
```
The site will be available at `http://0.0.0.0:4000/`

### JavaScript Build Commands
```bash
# Minify and build JavaScript
npm run build:js

# Watch JavaScript files for changes and rebuild
npm run watch:js

# Uglify JavaScript manually
npm run uglify
```

### Jekyll/Ruby Setup (if not using Docker)
```bash
bundle install
bundle exec jekyll serve
```

## Architecture and Structure

### Site Configuration
- **_config.yml**: Main Jekyll configuration file containing:
  - Theme skin selection (default, air, aqua, contrast, dark, dirt, neon, mint, plum, sunrise)
  - Site metadata (title, locale, URL, repository)
  - Masthead settings (sticky navigation, opacity, background images)
  - Footer links and copyright
  - Jekyll plugins configuration

### Content Structure
- **_pages/**: Contains main page content
  - `index.html`: Homepage with multiple sections (Enterprise Software, AIoT, IoT Business, Contact Us)
  - `imprint.md`: Legal/imprint information
  - Sections are defined inline using `<section>` tags with IDs matching navigation anchors

- **_data/navigation.yml**: Navigation menu configuration
  - Defines anchor links to sections on the homepage
  - Each entry has title (Korean), URL (anchor link), and description

### Layout System
- **_layouts/**: Jekyll layout templates
  - `default.html`: Base layout wrapper
  - `page.html`: Extends default layout for pages

- **_includes/**: Reusable components
  - `masthead.html`: Site header and navigation
  - `footer.html`: Site footer with links
  - `page__hero.html`: Hero section with overlay images
  - `image-text-row`: Component for image+text rows
  - `gallery`: Gallery component for showcasing images
  - `popup.html`: Cookie/CDN consent banner (controlled by `show-popup` in _config.yml)
  - `editables/`: Custom editable components

### Styling
- **_sass/oneflow/**: SCSS source files organized by component
  - `_variables.scss`: Theme variables and colors
  - `_sections.scss`: Section-specific styles
  - `_boxes.scss`: Box/card component styles
  - `skins/`: Color scheme variations (10 themes available)
  - Main entry point: `oneflow.scss`

- **assets/css/custom-styles.css**: For custom overrides without modifying theme files

### JavaScript
- **assets/js/**:
  - `_main.js`: Main JavaScript source (unminified)
  - `main.min.js`: Production minified bundle (generated)
  - `plugins/`: jQuery plugins (fitvids, magnific-popup, greedy-navigation, smooth-scroll, gumshoe, etc.)
  - `vendor/jquery/`: jQuery library
  - Build process concatenates and minifies all JS files into `main.min.js`

### Assets
- **assets/images/**: Site images (header backgrounds, logos, content images)
- **assets/fonts/**: Custom web fonts
- **assets/webfonts/**: Font Awesome icon fonts

## Content Management

### Adding New Sections
1. Add section markup in `_pages/index.html` with unique ID
2. Add navigation link in `_data/navigation.yml` pointing to section ID
3. Use front matter variables for reusable content (gallery, image-text-row, etc.)

### Image Text Rows
Define in page front matter:
```yaml
image-text-row:
  - header: "Section Title"
    excerpt: "Description text"
    image-url: "/assets/images/example.webp"
    image-width: "500px"
    image-right: false  # or true to position image on right
```
Include in page: `{% include image-text-row %}`

### Galleries
Define in page front matter:
```yaml
gallery:
  - url: /assets/images/full-size.webp
    image_path: /assets/images/thumb.webp
    alt: "Alt text"
    title: "Title"
```
Include in page: `{% include gallery %}`

### Theme Customization
- Change color scheme: Edit `oneflow-skin` in `_config.yml`
- Modify masthead behavior: Set `sticky-masthead` and `masthead-opacity` in `_config.yml`
- Add/remove footer links: Edit `footer.links` array in `_config.yml`

## Jekyll Plugins

Active plugins (github-pages compatible):
- jekyll-sitemap: Generates sitemap.xml
- jekyll-gist: Embed GitHub gists
- jekyll-feed: Generates RSS/Atom feed
- jekyll-include-cache: Caches include files for performance
- jekyll-algolia: Search integration (configured but may not be active)
- jemoji: GitHub-style emoji support

## Deployment Configuration

### Git Remotes
This repository has two remotes configured:
```bash
origin           https://github.com/shine-codestove/codestove-site.git  # Development repo
codestove.io     https://github.com/ryoostar/codestove.io               # Production repo
```

### Deployment Process
```bash
# 1. Test locally with Docker
docker compose up

# 2. Commit changes
git add -A
git commit -m "Your commit message"

# 3. Push to production repository
git push codestove.io main

# 4. GitHub Pages will automatically build and deploy (1-2 minutes)
```

### _config.yml Production Settings
```yaml
url: "https://codestove.io"
baseurl: ""  # Empty for root domain
repository: "ryoostar/codestove.io"
show-popup: false  # Cookie banner disabled
```

### GitHub Pages Configuration
- **Custom Domain**: www.codestove.io (defined in CNAME file)
- **HTTPS Enforcement**: Enabled
- **Source Branch**: main
- **Build**: Automatic via GitHub Actions

### DNS Configuration
Domain registrar settings for codestove.io:

**Apex Domain (A Records):**
```
Type: A
Name: @ (or leave empty)
Values:
  185.199.108.153
  185.199.109.153
  185.199.110.153
  185.199.111.153
```

**www Subdomain (CNAME):**
```
Type: CNAME
Name: www
Value: ryoostar.github.io
```

**URL Behavior:**
- codestove.io → redirects to → https://www.codestove.io
- www.codestove.io → https://www.codestove.io
- All HTTP requests → redirects to → HTTPS

### Custom Design Features (Added 2025-11-20)

**CSS Customizations** (`assets/css/custom-styles.css`):
- Fade-in animations for sections (opacity-based to avoid transform conflicts)
- Enhanced gallery card hover effects with shadows and scale
- Professional contact cards with icons and hover animations
- AI & Tech highlight boxes with gradient backgrounds
- Section header underlines with gradient effect
- Smooth scroll behavior
- Responsive design for mobile/tablet/desktop

**Design Components:**
- **AI Highlight Box**: Purple gradient, brain icon, AI development message
- **Tech Highlight Box**: Cyan gradient, layer icon, full-stack technology message
- **Contact Cards**: Email, Phone, Location with Font Awesome icons
- **Gallery Enhancements**: Border radius, shadow effects, hover transformations

**Important Design Notes:**
- All animations use `opacity` only (NOT `transform`) to avoid conflicts with theme's `translateX(-50%)` positioning
- Images use `.webp` format for optimal performance
- Color palette follows professional blue/purple gradient scheme
- All text content is in Korean

### Troubleshooting

**Images not loading:**
- Ensure images are pushed to the repository
- Verify paths start with `/assets/images/`
- Check file extensions (.webp, .png, etc.)
- Wait for GitHub Pages build to complete

**HTTPS not working:**
1. Check CNAME file exists and contains correct domain
2. Verify DNS settings point to GitHub Pages IPs
3. In GitHub Settings → Pages, remove and re-add custom domain
4. Wait up to 24 hours for SSL certificate provisioning

**Changes not appearing:**
- Remember to push to `codestove.io` remote (not just `origin`)
- Check GitHub Actions for build status
- Clear browser cache (Cmd+Shift+R on Mac, Ctrl+Shift+R on Windows)
- _config.yml changes require Docker restart: `docker compose restart`

## Important Notes

- This is a fork of the OneFlow theme, not a gem-based theme, so theme files are directly editable
- Site uses webp image format for performance
- Korean language content throughout
- Background images and overlays configured in both `_config.yml` and page front matter
- Masthead is sticky by default with 0.85 opacity
- HTML compression is enabled for production builds (disabled in development env)
- Docker is the recommended development environment (no local Ruby/Jekyll setup needed)
- Cookie consent banner is disabled (`show-popup: false` in _config.yml)
