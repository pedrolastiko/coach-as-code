# Coach as Code — Marathon Plan

*[Version française](README.md)*

An installable web app (PWA) that displays the weekly training plan for the **P'tit Train du Nord Marathon** (10/04/2026, goal 3h55–3h59). It's a single page, no backend, no build step, designed to be checked from a phone, including offline.

**Live demo:** [DEMO](https://pedrolastiko.github.io/coach-as-code/).

## Features

- **16 weeks of plan** (S1 to S16), grouped by week, expandable on click.
- Each session shows: effort type (EF, threshold, intervals, long run…), status (done / upcoming / substituted), distance, weather, segment breakdown, metrics (HR, cadence, elevation gain…) and an analysis comment.
- **"Expand all / Collapse all"** button to quickly browse the whole plan.
- Responsive interface, optimized for mobile.
- **Works offline** thanks to a service worker that caches the app after the first visit.
- **Installable** on the home screen (Android and iOS) like a native app, no app store required.

## Project structure

```
.
├── index.html          # Full application (structure, style, plan data, display logic)
├── manifest.json        # PWA manifest (name, icons, colors, standalone mode)
├── service-worker.js    # Asset caching for offline mode
└── icons/
    ├── icon-192.png
    ├── icon-512.png
    └── icon-512-maskable.png
```

No dependencies, no build: everything is native HTML/CSS/JS in `index.html`.

## Deploy to GitHub Pages

Deployment is automated by the [`.github/workflows/deploy-pages.yml`](.github/workflows/deploy-pages.yml) workflow: on every push to `main`, GitHub Actions publishes the repository's content to GitHub Pages (no build, the page is served as-is).

- On the first deployment, GitHub automatically configures the Pages source to **"GitHub Actions"** (visible afterward under **Settings → Pages**).
- If it doesn't, just go to **Settings → Pages → Build and deployment → Source** and select **GitHub Actions**.
- Deployment progress can be tracked in the repo's **Actions** tab.
- Once finished, the app is available at:
  `https://<your-github-username>.github.io/coach-as-code/`

## Install the app on your phone

Once the page is published, open the URL from your phone's browser, then:

**Android (Chrome):**
Menu ⋮ → **"Install app"** (or **"Add to Home screen"**).

**iPhone / iPad (Safari):**
**Share** button (square icon with an arrow) → **"Add to Home Screen"**.

The icon then appears like a native app, opens fullscreen (no address bar), and stays available even offline thanks to the service worker's cache.

> ⚠️ A PWA must be served over HTTPS to be installable — that's the default with GitHub Pages.

## Local development / testing

Since a service worker requires a server (no `file://`), start a small local server at the project root:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Updating the plan

Each week's and session's data is defined directly in the `weeks` JavaScript array, in `index.html`. To log a completed session or adjust an upcoming week, just edit the corresponding entries (status `done`/`todo`/`warn`, distance, metrics, comment), then commit/push — GitHub Pages automatically republishes the change.

Remember to bump `CACHE_NAME` in `service-worker.js` (e.g. `plan-marathon-v2`) after a content update, to force the cache to refresh on devices that already installed the app.

## License

MIT — see [LICENSE](LICENSE).
