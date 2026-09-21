# dascorpus.xyz landing page

Static, dependency-free landing page for Corpus. No build step: every file in
this folder is served as-is by GitHub Pages.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The page. Self-contained (inline CSS, no JS). |
| `404.html` | GitHub Pages picks this up automatically. |
| `privacy/index.html` | Privacy policy, served at `/privacy`. Required by Google OAuth verification. |
| `terms/index.html` | Terms of service, served at `/terms`. Required by Google OAuth verification. |
| `CNAME` | Custom domain, `dascorpus.xyz`. Required, do not delete. |
| `robots.txt` | Allows `/` only; disallows `/admin`, `/internal`, `/mcp`. |
| `sitemap.xml` | Single-URL sitemap. Bump `lastmod` when the copy changes. |
| `og.png` | 1200×630 social card, referenced by the OG/Twitter meta tags. |
| `logo.svg` | Corpus logo (mark + wordmark) for reuse elsewhere. |
| `favicon.svg` | Corpus mark, monochrome. |

## Deploy

Branch-based Pages (recommended, so the landing page stays off `main`):

```sh
git checkout -b gh-pages
git rm -r --cached . -q            # gh-pages holds only the site
# copy this folder's contents to the repo root, then:
git add index.html 404.html privacy terms CNAME robots.txt sitemap.xml og.png favicon.svg
git commit -m "Landing page for dascorpus.xyz"
git push -u origin gh-pages
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch →
`gh-pages` / `/ (root)`**, and set the custom domain to `dascorpus.xyz`
(this writes/keeps `CNAME`). Tick **Enforce HTTPS** once the certificate is
issued.

Alternative, if you'd rather keep it on `main`: commit this folder as `docs/`
and set Pages source to `main` / `/docs`.

## DNS at the registrar for dascorpus.xyz

Apex `A` records → GitHub Pages:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

Plus `AAAA` (optional, IPv6):

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

And `CNAME www → <org>.github.io.`

## Google OAuth verification

`/privacy` and `/terms` exist so the Corpus OAuth client can be switched from
Internal to External in the Google Cloud console. Before submitting:

1. **Create the `privacy@dascorpus.xyz` alias.** Both pages name it as the
   contact address and reviewers do check that it resolves.
2. **Verify `dascorpus.xyz` in Google Search Console** under the same Google
   account that owns the Cloud project. The consent screen rejects homepage,
   privacy, and terms URLs on an unverified domain.
3. **Check the app domain matches.** The consent screen's authorized domain has
   to cover wherever the app actually runs. If `/admin` is served from Railway
   or another host rather than from `dascorpus.xyz`, resolve that mismatch first
   or review will fail regardless of these two pages.
4. **Keep the scopes to `openid`, `email`, and `profile`.** The privacy policy
   states those three and nothing else. Adding Drive, Gmail, or Calendar means
   rewriting the Google account data section and a much heavier review.

Both pages are crawlable: `robots.txt` disallows only `/admin`, `/internal`, and
`/mcp`.

## Before going live: three things to decide

1. **`/admin` links.** Both "Sign in" buttons point at `/admin`, which only
   resolves if the app is served from this same domain. If the app lives
   elsewhere (Railway URL or a subdomain like `app.dascorpus.xyz`), replace the
   two `href="/admin"` values in `index.html` and the one in `404.html`.
2. **Indexing.** The page is currently `index, follow`. If you'd rather stay
   out of search for now, switch the `robots` meta in `index.html` to
   `noindex, nofollow` and set `robots.txt` to `Disallow: /`.
3. **Contact form.** GitHub Pages serves static files only, so the form posts to
   a third-party handler. Create a form at [formspree.io](https://formspree.io)
   (free tier is fine) and replace `YOUR_FORM_ID` in the form's `action` in
   `index.html`. Also replace the fallback address `hello@dascorpus.xyz` in the
   hint under the button. Until both are set the mailto link works, but the
   submit button will error.
