# wholify-site

Static site for [Wholify](https://github.com/) hosting the public pages App Store Connect needs a URL for:

- **[index.html](index.html)** — marketing landing page (ASC Marketing URL)
- **[privacy.html](privacy.html)** — Privacy Policy (ASC Privacy Policy URL, required)
- **[terms.html](terms.html)** — Terms of Use (referenced from the in-app PaywallSheet)
- **[support.html](support.html)** — Support page with FAQ (ASC Support URL, required)
- **[about.html](about.html)** — About the publisher and the app
- **[404.html](404.html)** — Friendly 404 (GitHub Pages serves this automatically)
- **[assets/styles.css](assets/styles.css)** — shared layout, brand palette
- **[assets/favicon.svg](assets/favicon.svg)** — minimal brand mark

Plain HTML. No build step, no Jekyll, no dependencies. Open `index.html` in a browser and you're looking at the site exactly as it'll render in production.

---

## Publish to GitHub Pages

The assumption is a new repository named `wholify-site` under the `appsbyai` org (or your personal account). Adjust as needed.

```bash
# 1. Create an empty public repo on GitHub named "wholify-site".
#    Do this via the GitHub UI — no README, no .gitignore, no license.

# 2. From the KitchenKit repo root, copy this folder into a new directory
#    outside the Xcode workspace so it can live in its own repo:
cp -R docs/wholify-site ../wholify-site
cd ../wholify-site

# 3. Initialize git and push.
git init
git add .
git commit -m "Initial publish"
git branch -M main
git remote add origin git@github.com:appsbyai/wholify-site.git
git push -u origin main

# 4. Enable Pages:
#    GitHub → wholify-site repo → Settings → Pages
#    Source: "Deploy from a branch"
#    Branch: main / (root)
#    Click Save. Wait ~1 minute for the first deploy.

# 5. Verify:
open https://appsbyai.github.io/wholify-site/
open https://appsbyai.github.io/wholify-site/privacy.html
open https://appsbyai.github.io/wholify-site/terms.html
open https://appsbyai.github.io/wholify-site/support.html
```

### URLs to paste into App Store Connect

| ASC field | URL |
|---|---|
| Marketing URL | `https://appsbyai.github.io/wholify-site/` |
| Privacy Policy URL | `https://appsbyai.github.io/wholify-site/privacy.html` |
| Support URL | `https://appsbyai.github.io/wholify-site/support.html` |

The Terms link is already in the iOS binary ([TermsSheet.swift](../../Wholify/Features/Pro/TermsSheet.swift)) as an in-app sheet, so it doesn't need to be in ASC — but the public web page gives reviewers somewhere to link when inspecting your listing, and users can share it.

---

## Custom domain (optional)

If you own `wholify.app` or similar, you can swap the `.github.io` URL for a branded one:

1. In the GitHub repo → Settings → Pages → Custom domain — enter `wholify.app`
2. Add a `CNAME` file to the repo root containing `wholify.app`
3. At your DNS provider, add a CNAME record: `wholify.app → appsbyai.github.io`
4. Wait for DNS propagation (~10 minutes typical)
5. Check the "Enforce HTTPS" box in GitHub Pages settings

Once live, update the ASC URLs to the custom domain.

---

## Editing

Content is plain HTML. Make changes, preview locally by opening the `.html` file in a browser, commit, push. GitHub Pages re-deploys within a minute.

**Canonical content lives in the main KitchenKit repo** at:

- Privacy Policy body → [`docs/PRIVACY_POLICY.md`](../PRIVACY_POLICY.md) (markdown source — keep both in sync)
- Terms body → [`Wholify/Features/Pro/TermsSheet.swift`](../../Wholify/Features/Pro/TermsSheet.swift) (in-app sheet copy — keep both in sync)
- Support FAQ → [`docs/APP_STORE_METADATA.md §Support URL`](../APP_STORE_METADATA.md)

If the in-app disclaimer, TOS, or Privacy text changes, update the web version here too. A version bump in either place without the other produces a reviewer-visible inconsistency.

---

## Brand

- **Terracotta** `#D97757` — primary / CTA / accent dots
- **Sage** `#87A878` — callout cards, muted accents
- **Cream** `#FAF5EC` — page background
- **Ink** `#2E2A24` / **Ink soft** `#5A524A` / **Ink faint** `#8A8178` — text tiers
- **Divider** `#E4DCC9` — horizontal rules and card borders

Typography: system font stack (no webfont dependency, fastest first render). Headings use `ui-rounded` / `SF Pro Rounded` where available; body is regular system sans.

Layout: single 720px column, one consistent nav/footer, no carousels or modals. The goal is "legal-grade clarity with a bit of warmth," not "marketing site."
