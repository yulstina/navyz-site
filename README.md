# NAVYZ — promotional site

Static marketing site for NAVYZ business solutions (POS, online ordering, web,
payments). Hand-written HTML/CSS/JS, no build step.

**Live:** https://yulstina.github.io/navyz-site/

## What this is

The content is NAVYZ's own (from [navyz.com](https://www.navyz.com/)). The
layout, spacing rhythm, and motion follow the design system documented in
[`PEBLLA-DESIGN-SYSTEM.md`](PEBLLA-DESIGN-SYSTEM.md), which records values
measured directly off the reference site rather than eyeballed.

## Structure

```
index.html            home
products.html         POS & Kiosk
online.html           online ordering
web.html              web solution
blog.html             insights index
blog-*.html           six articles
contact.html          demo request
location.html         locations
login.html            sign in
forgot-password.html  password reset
policy.html           privacy policy
terms.html            terms of service

assets/css/style.css  all site styles, sectioned by banner comments
assets/css/util.css   utilities
assets/images/        imagery
serve.js              local static server for development
```

The header and footer are inlined in every page. **Changing a menu item means
editing every HTML file** — there is no shared include.

## Running locally

```bash
node serve.js
```

Then open http://127.0.0.1:5173.

## Notes for future edits

- Card images must share one aspect ratio (16:10). Dropping in originals at
  their natural size makes card heights uneven and stretches the page.
- The numbered 01/02/03 blocks are a CSS `position: sticky` stack, not scroll
  hijacking, and stay sticky on mobile too.
- The phone slider on the home page must have exactly one implementation. Two
  autoplay timers were once running against the same element.
- `body` still carries an old dark-theme text colour. Any new light-background
  section must set its own `color`, or headings render white on white.
- Keep images near their display size. Several were 5–20x larger than needed
  and had to be re-encoded.
