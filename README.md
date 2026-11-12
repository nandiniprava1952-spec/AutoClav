# Autoclave Operation Dashboard — Odisha NHM (PWA)

Installable, offline-capable mobile web app. Hosted on GitHub Pages.

- `index.html` – the dashboard
- `manifest.webmanifest`, `sw.js`, `icons/` – PWA layer
- `.github/workflows/pages.yml` – auto-deploy on push to `main`

After changing `index.html`, bump `VERSION` in `sw.js` so installed apps refresh.
