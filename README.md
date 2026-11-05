# Earbop website

Static site for Earbop: Jazz Ear Training, served by GitHub Pages at <https://earbop.github.io/>.
Its first job is the privacy policy at a permanent URL for the Google Play Console and App Store Connect.

Plain HTML and one CSS file. No build step, no JavaScript, no third-party requests, no cookies.

## Pages

| URL | File |
| --- | --- |
| `/` | `index.html` (placeholder home page) |
| `/privacy/` | `privacy/index.html` (**permanent URL: never move it once it is in a store form**) |
| `/support/` | `support/index.html` |
| `/privacy-policy/` | `privacy-policy/index.html` (redirects to `/privacy/`) |
| not found | `404.html` |

## The base URL lives in these places

GitHub Pages has no templating, so if the site moves to a custom domain, replace `https://earbop.github.io` in:

- the `<link rel="canonical">` in `index.html`, `privacy/index.html`, `support/index.html`, `privacy-policy/index.html`
- the app's `PRIVACY_POLICY_URL` / `SUPPORT_URL` constants, once they exist (needs an app update)
- the store listings

Search with `grep -rn "earbop.github.io" .`. When a custom domain is added to the same Pages site, GitHub redirects the old `github.io` addresses to it.

## Editing the policy

Last updated 5 November 2026. When the policy changes, update the date at the top of `privacy/index.html`, and update the Google Play Data safety and App Store privacy answers to match. Crash reports are kept 90 days (Sentry's event retention setting); change the policy if that setting changes.

## Publishing

Pushing to `main` publishes the site (user site, deployed from `main`). Preview locally first:

```bash
python3 -m http.server 8099
```

Then open <http://localhost:8099/>. Do not edit the policy without also updating the stores' Data safety / App Privacy answers.

## Rules the site follows

- No cookies, analytics, embeds, third-party fonts or any request to another domain, so no cookie banner is needed.
- No contact form: contact is by email link only.
- Each page sets a strict Content-Security-Policy through a `<meta>` tag, because GitHub Pages cannot set response headers.
