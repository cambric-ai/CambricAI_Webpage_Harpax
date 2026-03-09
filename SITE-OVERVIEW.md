# Cambric AI — Site Repository Overview

Static marketing site for **Cambric AI** and its flagship product **Harpax** (AI session security). Hosted on GitHub Pages. No JavaScript — pure HTML + CSS.

---

## File Structure

```
getACGS-main/
├── .claude/launch.json          # Dev server config (Python HTTP on :3000)
├── .gitignore
├── .nojekyll                    # Tells GitHub Pages to skip Jekyll
├── robots.txt                   # SEO — allows all crawlers, points to sitemap
├── sitemap.xml                  # 9 canonical URLs for search engines
├── site.webmanifest             # PWA manifest (Cambric AI, purple theme)
├── favicon.svg                  # Brand icon
├── README.md                    # Setup notes
│
├── assets/
│   └── logo.svg                 # Cambric AI logo
│
├── css/
│   ├── base.css                 # Design tokens, reset, typography
│   └── components.css           # All component styles (~460 lines)
│
├── index.html                   # Homepage — Harpax product marketing
├── harpax.html                  # Full product docs + install guide
├── about.html                   # Company + team
├── contact.html                 # Contact form (Formspree)
├── research.html                # Research + governance framework
├── products.html                # Meta-redirect → harpax.html
├── blog.html                    # Blog index
├── blog/
│   └── what-does-claude-code-do.html  # First blog post
├── 404.html                     # Error page
├── privacy.html                 # Privacy policy
└── terms.html                   # Terms of service
```

---

## How It Works

### Design System (2-file CSS)

**`css/base.css`** — Foundation layer:
- Google Fonts import: EB Garamond (serif headings) + JetBrains Mono (monospace body)
- CSS custom properties for the entire color palette:
  - Backgrounds: `--bg` (#0D1020), `--bg2` (#111428), `--surface` (#161930)
  - Accents: `--purple` (#4A3C7A), `--purple2` (#6050A0), `--purple3` (#A090C8)
  - Text: `--cream` (#DDD9D0), `--cream2` (#A8A49C), `--cream3` (#6A6878)
  - Status: `--green`/`--green2`/`--green3`, `--warn` (#D07070)
- Global reset, smooth scroll, antialiased fonts
- H1 uses responsive `clamp(44px, 5.6vw, 72px)`, italic `<em>` renders in purple3
- Fade-in keyframe animation used by the session trace

**`css/components.css`** — All UI components:
- **Nav**: Fixed top bar, blur backdrop, logo + links + download button
- **Hero**: Full-viewport with grid overlay pattern + radial purple glow
- **Two-Lane CTA**: 2-column grid — consumer lane (GUI, $9/mo) + developer lane (CLI, free)
- **Buttons**: `.btn-download` (cream), `.btn-cli` (ghost purple), `.btn-p` (primary), `.btn-g` (ghost)
- **Section Labels**: `// LABEL` eyebrow pattern with horizontal rule line
- **Consumer Grid**: 3-column card grid with icons, titles, descriptions
- **Pricing**: Row-based layout with green (free) and purple (paid) values
- **Trace Window**: macOS-style terminal with color-coded action badges + cascading fade-in animation
- **Features Grid**: 3-column developer feature cards
- **Detection Tiers**: 2-column layout — tier descriptions on left, sticky config card on right
- **CTA Section**: Centered heading + dual buttons over radial gradient
- **Footer**: Logo, nav links, copyright
- **Responsive**: Single breakpoint at 860px collapses everything to single-column

### Page Template

Every page follows the same skeleton:

```
<head>
  meta + title + OG tags
  favicon
  Google Fonts import
  css/base.css
  css/components.css
  <style> (page-specific overrides)
</head>
<body>
  <nav> — Cambric AI logo | Harpax | Research | Docs | Download
  <sections> — page content
  <footer> — logo | Harpax | Docs | Research | Blog | About | Contact | copyright
</body>
```

Page-specific styles are scoped in inline `<style>` blocks in each file's `<head>`.

### Page Descriptions

| Page | Purpose |
|------|---------|
| `index.html` | Homepage — hero, two-lane CTA, consumer features, pricing, session trace, dev features, detection tiers, final CTA |
| `harpax.html` | Deep product page — hero, quick install (GUI + CLI), how it works (3 steps), full feature breakdown (6 cards), expanded detection tiers, 3 config examples, CLI command reference, CTA |
| `about.html` | Company story — problem statement, what we built, what's next, team grid (Paul Montgomery CEO, Chris Guffey CPO) |
| `contact.html` | Two-column — Formspree form (name, email, type, message) + direct contact info |
| `research.html` | Research areas — conference presentations, AI governance framework, writing section |
| `blog.html` | Blog index — post listings with dates and descriptions |
| `blog/what-does-claude-code-do.html` | Blog post — what AI coding assistants actually do to your codebase |
| `products.html` | Meta-refresh redirect to `harpax.html` |
| `404.html` | Centered "404 / Page not found" with back-to-home link |
| `privacy.html` | Privacy policy — website (no tracking), Harpax (local-only), GUI (payments) |
| `terms.html` | Terms of service — CLI (MIT), GUI (subscription), liability |

### Navigation Map

```
index.html (home)
├── harpax.html (product docs)
│   └── github.com/nullevent/harpax (external docs)
├── about.html
│   ├── harpax.html (CTA)
│   └── contact.html (CTA)
├── contact.html (Formspree form)
├── research.html
├── blog.html
│   └── blog/what-does-claude-code-do.html
│       ├── ../harpax.html (CTA)
│       └── ../blog.html (back link)
├── privacy.html
├── terms.html
├── products.html → harpax.html (redirect)
└── 404.html → index.html (back home)
```

### Deployment

- **Host**: GitHub Pages (zeppelin150/getACGS)
- **Branch**: `main` (source for Pages)
- **Archive**: Old site preserved on `archive/v1` branch
- **Build trigger**: POST to `api.github.com/repos/zeppelin150/getACGS/pages/builds`
- **Domain**: cambricai.io (configured in robots.txt + sitemap.xml)

### Local Development

```bash
cd getACGS-main
python -m http.server 3000
# Open http://localhost:3000
```

Or via Claude Code preview: uses `.claude/launch.json` to auto-start the server.

### Design Rules

- Border-radius: 2px everywhere (no pills, no rounded corners)
- No drop shadows
- 1px solid borders using `var(--border)`
- Hover states transition to `var(--surface)` background
- Section eyebrows: `// LABEL` pattern in 8.5px uppercase purple3
- All text in JetBrains Mono except h1/h2/h3 which use EB Garamond
- Single responsive breakpoint at 860px
