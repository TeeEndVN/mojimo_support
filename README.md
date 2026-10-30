# mojimo_support

Support for the Mojimo app: legal documents, bug reports and feature requests.

## Report a bug or suggest a feature

[Open an issue](https://github.com/TeeEndVN/mojimo_support/issues/new/choose) and pick a form:

- **Bug report**: what happened, steps to reproduce, the part of the app, the app version and your device.
- **Feature request**: the problem you want solved, what Mojimo should do, and how important it is to you.

Issues are public. Don't post your email, passwords or other personal information, and blur private content in screenshots. For account, privacy or payment questions, email us instead (see [Contact](#contact)).

## Legal documents

| Page | File |
|---|---|
| Landing page | [`legal/index.html`](legal/index.html) |
| Privacy Policy | [`legal/privacy.html`](legal/privacy.html) |
| Terms of Use | [`legal/terms.html`](legal/terms.html) |
| Delete account and data | [`legal/delete-account.html`](legal/delete-account.html) |

Static HTML with a shared [`legal/style.css`](legal/style.css) (light and dark mode). There is no build step.

## Contact

nghiaht275.work@gmail.com

---

## Maintaining this repo

### Hosting the legal pages

**GitHub Pages** (repository must be public on a free plan):

1. Settings → Pages → Source: *Deploy from a branch*, branch `main`, folder `/ (root)`.
2. Pages are served at `https://teeendvn.github.io/mojimo_support/legal/privacy.html` (and so on).

The empty `.nojekyll` file makes GitHub Pages serve the files as they are.

**Cloudflare Pages** (works with a private repository, but issues need the repository to be public):

1. Create a project from this repository, production branch `main`.
2. Leave the build command empty and set the output directory to `legal`.
3. Pages are served at `https://<project>.pages.dev/privacy.html`.

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
