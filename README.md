# BRAHM 2026 — Founders' Day Fest

Official companion web app for **BRAHM 2026**, the Founders' Day Fest at
Amrita Vishwa Vidyapeetham, Delhi NCR Campus, Faridabad.

**19 – 23 September 2026**

## About

An installable, offline-friendly progressive web app that carries the full
fest programme: the five-day schedule, nine inter-college competitions with
their rules, venue directions, organiser contacts and a personal saved-events
list. Visitors can add individual events — or the whole fest — to their own
calendar as `.ics` downloads.

## Stack

A static site with no build step. The UI is authored as a declarative
template rendered at runtime by `support.js`, which pulls React 18 from a CDN
with subresource-integrity pinning. Styling comes from the design-system
bundle in `_ds/` plus Cormorant Garamond and Phosphor Icons.

## Layout

| Path            | Purpose                                          |
| --------------- | ------------------------------------------------ |
| `index.html`    | Entry point — the app template and its logic     |
| `support.js`    | Runtime that renders the template                |
| `_ds/`          | Design-system stylesheet and bundle              |
| `assets/`       | Images, icons and the registration QR code       |
| `manifest.json` | PWA manifest — name, icons, theme colours        |
| `vercel.json`   | Hosting config — caching and security headers    |

`*.dc.html` are the editable design-canvas sources. `index.html` is the
deployed copy of `Brahm 2026 Fest App.dc.html`; regenerate it after edits
with:

```sh
cp "Brahm 2026 Fest App.dc.html" index.html
```

## Running locally

Any static file server works — the app needs to be served over HTTP rather
than opened from the filesystem, so that the manifest and relative asset
paths resolve:

```sh
npx serve .
```

Then open the printed URL.

## Deploying

Hosted on Vercel as a static site. There is no build command and no output
directory to set — pushes to `main` deploy automatically. `scratch/` and
`uploads/` hold working material and are excluded from both the repository
and the deployment.
