# mojimo_support

Support for the Mojimo app: legal documents, bug reports and feature requests.

## Report a bug or suggest a feature

[Open an issue](https://github.com/TeeEndVN/mojimo_support/issues/new/choose) and pick a form:

- **Bug report**: what happened, steps to reproduce, the part of the app, the app version and your device.
- **Feature request**: the problem you want solved, what Mojimo should do, and how important it is to you.

Issues are public. Don't post your email, passwords or other personal information, and blur private content in screenshots. For account, privacy or payment questions, email us instead (see [Contact](#contact)).

## Website

Landing page: https://mojimo.teeendvn.com/

| Page | File |
|---|---|
| Mojimo landing page | [`index.html`](index.html) |
| Support hub | [`legal/index.html`](legal/index.html) |
| Privacy Policy | [`legal/privacy.html`](legal/privacy.html) |
| Terms of Use | [`legal/terms.html`](legal/terms.html) |
| Delete account and data | [`legal/delete-account.html`](legal/delete-account.html) |
| Android App Links | [`.well-known/assetlinks.json`](.well-known/assetlinks.json) |
| Not found page | [`404.html`](404.html) |
| Crawler rules and sitemap | [`robots.txt`](robots.txt), [`sitemap.xml`](sitemap.xml) |
| Link preview image | [`assets/og-image.jpg`](assets/og-image.jpg) (source [`assets/og-image.svg`](assets/og-image.svg)) |

Static HTML, no build step. The landing page has its styles inline; the legal pages share [`legal/style.css`](legal/style.css). Both use the same look as https://teeendvn.com/ (Inter font, same colours, light and dark mode). Icons live in [`assets/`](assets).

## Contact

support@teeendvn.com

---

## Maintaining this repo

### Hosting the legal pages

**GitHub Pages** (repository must be public on a free plan):

1. Settings → Pages → Source: *Deploy from a branch*, branch `main`, folder `/ (root)`.
2. The landing page is served at `https://mojimo.teeendvn.com/` and the legal pages at `https://mojimo.teeendvn.com/legal/privacy.html` (and so on).

The empty `.nojekyll` file makes GitHub Pages serve the files as they are.

The custom domain comes from the [`CNAME`](CNAME) file (`mojimo.teeendvn.com`). DNS is on Cloudflare: a `CNAME` record `mojimo` → `teeendvn.github.io`, set to *DNS only*, so GitHub can issue the HTTPS certificate. Old `teeendvn.github.io/mojimo_support/` links redirect to the new domain.

**Cloudflare Pages** (works with a private repository, but issues need the repository to be public):

1. Create a project from this repository, production branch `main`.
2. Leave the build command empty and set the output directory to `legal`.
3. Pages are served at `https://<project>.pages.dev/privacy.html`. With `legal` as the output directory the landing page, the header icon and the "Mojimo home" links are not deployed; set the output directory to the repository root to include them.

### App Links

[`.well-known/assetlinks.json`](.well-known/assetlinks.json) verifies `mojimo.teeendvn.com` for the Android app `com.teeendvn.mojimo.prod`, so Play Console's Deep links page lists the domain as verified.

- It lists two SHA-256 fingerprints: the app signing key and the upload key, both from Play Console → Test and release → App integrity → App signing. Update it if a key changes.
- The app claims only `https://mojimo.teeendvn.com/scan`, so the landing and legal pages still open in the browser.
- Keep the file at this path. Android reads it from the domain root and doesn't follow redirects. `.nojekyll` makes GitHub Pages serve the dot-folder.

Check it:

```sh
curl -sI https://mojimo.teeendvn.com/.well-known/assetlinks.json
curl -s "https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://mojimo.teeendvn.com&relation=delegate_permission/common.handle_all_urls"
adb shell pm verify-app-links --re-verify com.teeendvn.mojimo.prod
adb shell pm get-app-links com.teeendvn.mojimo.prod
```

### Search and link previews

- Every page has a `<title>`, a meta description, a `rel="canonical"` URL on `https://mojimo.teeendvn.com/...` and Open Graph tags. Give a new page the same set.
- When you add a page or change its content, add it to [`sitemap.xml`](sitemap.xml) or update its `lastmod`.
- The landing page has a `MobileApplication` JSON-LD block. Keep its description and Play URL in line with the page, and don't add ratings without real data.
- [`assets/og-image.jpg`](assets/og-image.jpg) (1200×630) is the link preview image. It is built from [`assets/og-image.svg`](assets/og-image.svg), which embeds the app icon from `mojimo_flutter/assets/svgs/app_logo.svg`. To regenerate it on macOS:

  ```sh
  sed '1s/viewBox="0 0 1200 630"/viewBox="0 -285 1200 1200"/; 1s/height="630"/height="1200"/' assets/og-image.svg > /tmp/og-square.svg
  qlmanage -t -s 1200 -o /tmp /tmp/og-square.svg
  sips --cropToHeightWidth 630 1200 /tmp/og-square.svg.png -s format jpeg -s formatOptions 90 --out assets/og-image.jpg
  ```

- Search Console: the domain property `mojimo.teeendvn.com` is verified with a DNS TXT record on Cloudflare. Submit `https://mojimo.teeendvn.com/sitemap.xml` there.

### Where the links go

- Google Play Console: Privacy policy URL, Data safety form, Account deletion URL (`delete-account.html`).
- App Store Connect: Privacy Policy URL, Support URL (this repository or `legal/index.html`); Terms of Use in the description or as a custom EULA.
- In the app: Settings and the Pro paywall link the legal pages. Settings → **Send feedback** opens [`issues/new/choose`](https://github.com/TeeEndVN/mojimo_support/issues/new/choose). Profile → **Delete account** deletes the account after two confirmations, so keep `delete-account.html` in line with what the app deletes.

### Issue forms

- [`.github/ISSUE_TEMPLATE/bug_report.yml`](.github/ISSUE_TEMPLATE/bug_report.yml) adds the `bug` label.
- [`.github/ISSUE_TEMPLATE/feature_request.yml`](.github/ISSUE_TEMPLATE/feature_request.yml) adds the `enhancement` label.
- [`.github/ISSUE_TEMPLATE/config.yml`](.github/ISSUE_TEMPLATE/config.yml) turns off blank issues and points private questions to email.
- Keep the "Part of the app" lists in both forms in line with the app's features.

### Editing the legal pages

- Update the effective date at the top of `privacy.html` and `terms.html` whenever their content changes.
- Keep the Privacy Policy in sync with the Google Play Data safety form and the App Store privacy labels.
