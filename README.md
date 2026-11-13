# Autoclave Operation Dashboard — Odisha NHM (PWA)

Installable, offline-capable mobile web app. Hosted on GitHub Pages.

- `index.html` – the dashboard
- `manifest.webmanifest`, `sw.js`, `icons/` – PWA layer
- `.github/workflows/pages.yml` – auto-deploy on push to `main`

After changing `index.html`, bump `VERSION` in `sw.js` so installed apps refresh.

## Login (v6)
Three sign-in tabs, all passcode based (block login removed):
- **District / State** – district password, or "State admin" for all districts
- **Facility** – district → block → facility type → facility + passcode
- **SNCU / NBSU** – district → unit type → unit (from Odisha_SNCU_NBSU_List.xlsx) + passcode

Account usernames the app sends to the server: `fac_<block>_<facility>` and `sncu_/nbsu_<district>_<unit>`.
The server (Code.gs) must hold a hashed passcode and enforce the same scope for each.

## New entry (v7)
Facility and SNCU/NBSU logins get a **➕ New entry** tab (plus a floating button) — District/State logins do not.
The form is generated from `aUuCY6dcPgkNfCP337b9XZ.xlsx` (survey + choices): facility equipment, department equipment
(Labour Room / SNCU / OT), declaration, sterilization steps with time capture and photos. Facility/district/block are locked to the login.
Entries are sent with the existing `push` action (flat XLSForm field names, `_uuid` as key); photos with `photoPut`.
**Code.gs must allow `push` and `photoPut` for `facility` and `unit` roles, restricted to that role's own facility scope.**
