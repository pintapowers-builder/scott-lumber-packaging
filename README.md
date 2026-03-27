# Scott Lumber Packaging Website

Static website for Scott Lumber Packaging — custom lumber wrap and industrial packaging since 2005.

## Live Site

Hosted on Netlify: [scottlumberpackaging.com](https://scottlumberpackaging.com)

## Setup

This is a single-file static site. No build tools, frameworks, or dependencies required.

- `index.html` — the entire site (HTML, CSS, and JS in one file)

To run locally, just open `index.html` in a browser.

## Deployment

The site auto-deploys to Netlify when changes are pushed to the `main` branch.

## Images

Product images go in an `images/` folder. Replace the placeholder comments in `index.html` with actual `<img>` tags pointing to the image files. Example:

```html
<!-- Replace this: -->
<div class="placeholder">[ Lumber Wrap Image ]</div>

<!-- With this: -->
<img src="images/lumber-wrap.jpg" alt="Lumber Wrap">
```

## Forms

The quote form uses Netlify Forms. No configuration needed — Netlify detects the `data-netlify="true"` attribute automatically. Submissions appear in the Netlify dashboard under Forms.

## Updates

To update the site, edit `index.html` and commit the changes. Netlify will redeploy automatically within seconds.
