# constructor

Static one-page website for constructor, a holding company. Brand, one sentence, an address.

No framework, no build step. The only JavaScript: the brand beats once, a second after the page opens, the visible spectrum sliding through the word and leaving it black. After that only by hand: a tap is one beat, a hold keeps the word lit until it is let go. No timers beyond the first, no network requests. IBM Plex Sans is self-hosted from `public/fonts/` (SIL OFL 1.1, licence included). Everything served lives in `public/`.

```
public/
  index.html        the page (brand, one sentence, contact + SEO metadata / JSON-LD)
  styles.css        all styling (light grey ground, black type, lowercase, the brand's pulse)
  og-image.png      social share image (1200×630)
  fonts/            IBM Plex Sans regular + semi-bold (woff2)
  assets/           not used on the page, kept for later: mark.svg (a geometric c) and wordmark.svg ("constructor" outlined from Plex Sans
                    semi-bold); the spectrum covers the first 38.2 % (golden section) of each, the rest is ink. -light variants for dark grounds, -ink variants in one colour
  robots.txt, sitemap.xml, site.webmanifest, favicon.ico, apple-touch-icon, android-chrome icons
tools/              sources for the generated images, not served (see tools/README.md)
```

## Develop

Any static file server works:

```shell
python3 -m http.server 8000 -d public
# → http://localhost:8000
```

## Deploy

GitHub Pages, from `.github/workflows/pages.yml`: every push to `main` uploads `public/` as-is. Enable once in the
repository settings (Pages → Source → GitHub Actions), or `gh api -X POST repos/<owner>/<repo>/pages -f build_type=workflow`.

For the custom domain, add a `public/CNAME` file containing the domain and point DNS at GitHub Pages (four A records
for the apex, a CNAME for `www`), then set the domain under Pages → Custom domain and enforce HTTPS.

`netlify.toml` is kept so the same folder deploys unchanged on Netlify if that is ever preferred.

## Editing content

- **Sentence** — the `.tagline` paragraph in `index.html`. Mirror it in the `description` metas, the Open Graph tags and the JSON-LD block, then regenerate `og-image.png` from `tools/og.html`.
- **Domain** — `https://constructor.art/` in `index.html`, `robots.txt`, `sitemap.xml`.
- **Type scale** — one unit `--u` (the address size) and the golden ratio `--phi`; the sentence, brand, margins and page padding are powers of φ from it, at the top of `styles.css`.
- **Colors** — `--bg`, `--ink`, `--dim` at the top of `styles.css`, plus the `theme-color` meta and `site.webmanifest`.
- **Spectrum** — `--spectrum` in `styles.css` is the visible range 380–700 nm as sRGB stops (CIE 1931 fit); `--pulse` wraps it in ink so it can slide through the word. The beat is .9 s and ends the instant the word is black again, so a new press lands right away; the hold transition is .5 s; both off under `prefers-reduced-motion`.
