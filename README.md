# Wipezy website

The marketing site for [Wipezy](https://wipezy.web.app), served by Firebase Hosting.

It's a plain static site (HTML, CSS and a little JavaScript) with no build step, so every page is fully readable by search engines and link previews.

## Structure

```
site/
  index.html              Landing page (/)
  privacy-policy.html     /privacy-policy
  terms-of-service.html   /terms-of-service
  404.html                Shown for unknown URLs
  assets/css/styles.css   Shared styles for every page
  assets/js/main.js       Small enhancements (sticky nav border, scroll reveal); the site works without JS
  assets/img/             Logo, social icons, og-image.png (1200x630 share image)
  assets/img/app/         App screenshots (WebP); the phone frames around them are drawn in CSS
  assets/fonts/           Self-hosted Inter (SIL Open Font License)
  favicons, site.webmanifest, robots.txt, sitemap.xml
```

`cleanUrls` in `firebase.json` serves `privacy-policy.html` at `/privacy-policy`.

The header and footer are repeated in each HTML page, between the `HEADER:START/END` and `FOOTER:START/END` comments. If you change one, copy it to the other pages.

## Preview locally

```sh
cd site && python3 -m http.server 8000
# or: firebase emulators:start --only hosting
```

## Deploy

- **Pull requests** deploy to a temporary preview channel; the bot comments the preview URL on the PR.
- **Merging to `master`** deploys to production (Wipezy.in).
- Manual deploy: `firebase deploy --only hosting`

## When updating content

- Change the effective date at the top of the privacy policy or terms when their content changes, and update `lastmod` in `sitemap.xml`.
- Images in `assets/img/` are cached for 7 days. When you replace one with a file of the same name, bump its `?v=` tag where it's linked (or give it a new name), or visitors and the CDN may keep the old one. CSS and JS are revalidated on every load, and their links also carry a `?v=` tag.
- Replace screenshots in `assets/img/app/` with WebP files of the same names to keep the HTML unchanged, then bump the `?v=` tag as above.

