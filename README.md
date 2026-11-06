# Earbop website

Static site for Earbop: Jazz Ear Training, served by GitHub Pages at <https://earbop.github.io/>.
Its first job is the privacy policy at a permanent URL for the Google Play Console and App Store Connect.

Plain HTML and one CSS file. No build step, no JavaScript, no third-party requests, no cookies, no inline styles (the
CSP `<meta>` tag blocks them, so every style lives in `css/site.css`).

## Pages

| URL | File |
| --- | --- |
| `/` | `index.html` (landing page) |
| `/beta/` | `beta/index.html` (how to join the beta; **hidden and `noindex` until stage 2**, see below) |
| `/support/` | `support/index.html` |
| `/privacy/` | `privacy/index.html` (**permanent URL: never move it once it is in a store form**) |
| `/privacy-policy/` | `privacy-policy/index.html` (redirects to `/privacy/`) |
| not found | `404.html` |

Images are in `img/` (self-hosted WebP screenshots, and `og-feature.png` for link previews). Icons are in `assets/`.

## Navigation and footer lists

Every page repeats the same header and footer markup (there is no build step), so a change to the links is a find and
replace across the six HTML files. The lists:

- **Header (desktop nav and phone menu, same links in both):** Home, Beta, Support, Privacy. The current page has
  `aria-current="page"`. Only the Home page also shows the "Coming soon" pill.
- **Footer links (the current page's own link is left out):** Join the beta, Support, Privacy policy.

### Stage 1 and stage 2 for the Beta page

- **Stage 1 (now):** `/beta/` is built but not linked from the header, the footers or `sitemap.xml`, and carries
  `<meta name="robots" content="noindex">`. Its two link pills still read `[PLAY OPT-IN LINK]` and `[TESTFLIGHT LINK]`.
- **Stage 2 (when both links exist):** replace the two bracketed pills with real `<a class="link-pill" href="...">` links
  (text "Become a tester on Google Play" and "Open the invite in TestFlight"), remove the `noindex` line, add "Beta" to the
  header nav (and the phone menu) and "Join the beta" to the footers, and add `/beta/` to `sitemap.xml`.
  Check: `grep -rnE "\[[A-Z][A-Z ,.-]+\]" --include="*.html" .` must find nothing.

## The base URL lives in these places

GitHub Pages has no templating, so if the site moves to a custom domain, replace `https://earbop.github.io` in:

- the `<link rel="canonical">`, `og:url` and `og:image` tags in every page (not the 404, which has none)
- `robots.txt` and `sitemap.xml`
- the app's `PRIVACY_POLICY_URL` / `SUPPORT_URL` constants (`constants/links.ts`; needs an app update)
- the store listings

Search with `grep -rn "earbop.github.io" .`. When a custom domain is added to the same Pages site, GitHub redirects the
old `github.io` addresses to it.

## Editing the policy

Last updated 6 November 2026. When the policy changes, update the date at the top of `privacy/index.html`, and update the
Google Play Data safety and App Store privacy answers to match. Crash reports are kept 90 days (Sentry's event retention
setting); change the policy if that setting changes. The policy's "beta" paragraph (under Contact) says tester emails are
deleted when the beta ends: keep to that.

## Design

Dark only, system font stack. The tokens (colours, type scale) are the `:root` custom properties at the top of
`css/site.css`. The four accents carry meaning wherever a drill is named: cyan = intervals, amber = chords, pink =
enclosures, periwinkle = progressions.

## Publishing

Pushing to `main` publishes the site (user site, deployed from `main`). Preview locally first:

```bash
python3 -m http.server 8099
```

Then open <http://localhost:8099/>. Do not edit the policy without also updating the stores' Data safety / App Privacy answers.

## Rules the site follows

- No cookies, analytics, embeds, third-party fonts or any request to another domain, so no cookie banner is needed.
- No contact form: contact is by email link only.
- No JavaScript. The phone menu is a `<details>` element.
- Each page sets a strict Content-Security-Policy through a `<meta>` tag, because GitHub Pages cannot set response headers.
