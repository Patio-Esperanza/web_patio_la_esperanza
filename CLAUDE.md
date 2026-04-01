# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static website clone of **Patio La Esperanza** (patiolaesperanza.com.mx) — a logistics and container storage company in Lázaro Cárdenas, Michoacán. No build system, framework, or package manager. Pure HTML/CSS/JS.

## Running the Site

Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080
```

## Architecture

Single-page site (`index.html`) with four anchor-linked sections: `#inicio`, `#nosotros`, `#servicios`, `#contacto`.

| File | Role |
|------|------|
| `index.html` | All markup and content |
| `assets/css/style.css` | All styles — CSS custom properties at top (`:root`) define the design tokens |
| `assets/js/main.js` | Navbar scroll effect, hamburger menu, active link tracking, contact form feedback, scroll-triggered fade-in animations |
| `assets/img/` | All images downloaded from the original site |

## Design Tokens (CSS variables)

Defined in `:root` at the top of `style.css`:

- `--primary: #1a5276` — navy blue (navbar, headings, cards)
- `--accent: #e67e22` — orange (CTAs, highlights, hover borders)
- `--font-head: 'Montserrat'` — headings and labels
- `--font-body: 'Open Sans'` — body text

## Content Source

All text and images were cloned from:
- `patiolaesperanza.com.mx` → Hero, stats, contact info
- `patiolaesperanza.com.mx/about.html` → Misión, Visión, Valores
- `patiolaesperanza.com.mx/services.html` → 9 service cards + services gallery
