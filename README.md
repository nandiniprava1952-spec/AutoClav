# Autoclave Operation Dashboard — Odisha NHM (PWA)

Installable, offline-capable mobile web app. Hosted on GitHub Pages.

- `index.html` – the dashboard
- `manifest.webmanifest`, `sw.js`, `icons/` – PWA layer
- `.github/workflows/pages.yml` – auto-deploy on push to `main`

After changing `index.html`, just commit it: installed apps detect the new file and offer it through the in-app update flow below (no `VERSION` bump needed).

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

## Cover / login page
- Top-right **Online** (green dot) / **Offline** (yellow dot) pill; switches the moment the browser reports the connection dropped or came back.
- **Install app** button at the bottom of the login card (hidden once installed). Chrome/Edge/Android open the native install prompt; iPhone/Safari shows "Tap Share, then Add to Home Screen".
- `sw.js` was empty in the uploaded zip; it is now a working worker that also caches the cdnjs libraries for offline use.

## In-app update flow
1. **Update found** – the app checks when it opens, when you return to it, when the phone comes back online, and every 30 minutes. A card appears at the top with a progress ring and percentage: *Downloading update…*.
2. **Update ready** – at 100% it becomes *Update available* with **Update** and **Later**. Later hides the card until the next check (the downloaded copy is kept).
3. **Installing** – **Update** shows a spinning ring, *Installing update…*, then the app restarts on its own.
4. **After the restart** – *✔ Updated to the latest version* appears briefly.

`sw.js` is cache-first (cache `autoclave-v2`): a new `index.html` is not picked up silently. The page compares the `ETag` of `index.html` on the host with its cached copy, downloads the new file into a staging cache, and swaps it in only when **Update** is tapped. Saved entries and the login session live in the browser's own storage and are not touched.
