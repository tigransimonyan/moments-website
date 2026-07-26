# Moments — website

Minimal static site for the **Moments** mindfulness app, hosted on GitHub Pages.
It exists mainly to provide the **Support URL** and **Privacy Policy URL** that
the App Store and Google Play require.

Plain HTML and one CSS file — no build step, no dependencies, no Jekyll.

## Pages

| File           | Purpose                                              |
| -------------- | ---------------------------------------------------- |
| `index.html`   | Home — what the app is, features, privacy summary    |
| `support/index.html` | Support — contact email and FAQ                |
| `privacy/index.html` | Privacy policy — App Store / Play submission URL |
| `assets/style.css` | Shared styles; tokens mirror the app's `src/theme.ts` |
| `assets/icon.png`  | App icon, used as favicon and hero image         |

`.nojekyll` tells GitHub Pages to serve the files as-is instead of running them
through Jekyll.

## Local preview

No tooling needed — open `index.html` directly, or serve the folder:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Publishing to GitHub Pages

1. Create a new **public** repo on GitHub named `moments-site`.
2. Push this folder:

   ```sh
   git remote add origin git@github.com:tigransimonyan/moments-site.git
   git push -u origin main
   ```

3. In the repo: **Settings → Pages → Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: `main`, folder: `/ (root)`
4. Wait for the first deploy, then confirm the URLs resolve:

   - Home — `https://tigransimonyan.github.io/moments-site/`
   - Support — `https://tigransimonyan.github.io/moments-site/support/`
   - Privacy — `https://tigransimonyan.github.io/moments-site/privacy/`

Links between pages are relative, so the site works unchanged under the
`/moments-site/` subpath or on a custom domain later.

## Where these URLs go

- **App Store Connect** → App Information → *Support URL* and *Privacy Policy URL*
- **Google Play Console** → Store listing → *Privacy policy*

## Before publishing

- [ ] **App Store link** — `index.html` currently shows a "Coming soon to the
      App Store" pill. Once the app is live, replace that `<span class="badge">`
      with the commented-out `<a class="badge" href="…">` right above it and drop
      in the real App Store ID.
- [ ] **Privacy policy review** — the policy was written to match the app's
      actual behavior as of the current build (local-only storage, no network
      requests, opt-in write-only Apple Health). It is not legal advice; give it
      a read before submitting.
- [ ] **Keep it honest** — if the app ever gains analytics, a backend, or Apple
      Health *read* access, `privacy/index.html` must be updated to match.

## Content sources

Copy is derived from `store-listing.md` in the app repo, and the factual claims
were checked against the app source:

- no network calls or analytics dependencies anywhere in `App.tsx` / `src/`
- local storage only — `expo-sqlite` for moments, AsyncStorage for preferences
- `src/health.ts` requests `toShare: [MindfulSession]` with `toRead: []`, i.e.
  write-only Apple Health access, off by default
- streak logic in `src/storage.ts` — consecutive calendar days with at least one
  moment, with today's grace period
