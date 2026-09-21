# BRAHM 2026 — Founders' Day Fest

Official companion web app for **BRAHM 2026**, the Founders' Day Fest at
Amrita Vishwa Vidyapeetham, Delhi NCR Campus, Faridabad.

**19th – 23rd September 2026**

## About

An installable, offline-friendly progressive web app that carries the full
fest programme: the five-day schedule, the inter-college competitions with
their rules, the three in-house sports, venue directions, organiser contacts
and a personal saved-events list. Visitors can add individual events — or the
whole fest — to their own calendar as `.ics` downloads.

The home screen reads top to bottom as one story, and each turn in it is
marked by the same three-part chapter head (hairline, label, title):

| # | Section | Where it lives in the template |
| - | ------- | ------------------------------ |
| 1 | Brahm — the cover | `.hero` |
| 2 | About Brahm | `.spirit` |
| 3 | Founders’ Day | `.fday` |
| 4 | About our Founder | `.founder` |
| 5 | Brahm 2026 | the callout, the up-next card and the day picker |
| 6 | Explore the events | `.catgrid` — six tiles, each opening its own screen |
| 7 | Founders’ Day 2025 | `.memgrid` |
| 8 | Follow Brahm | `.insta` |

Then the partners band and the closing slab, as before.

The home screen deliberately does **not** list every event. Chapter six is six
category tiles; the detail lives on the screen behind whichever one is chosen,
so the story keeps its shape and nobody scrolls past the whole competition
list to reach the photographs from last year.

Each chapter carries the class `reveal`, which is one scroll-driven
animation — a 26px rise, tied to the element's travel through the scrollport
with `animation-timeline: view()` rather than an observer, so nothing runs on
the scroll path and nothing can fall out of step when the app re-renders. It
is wrapped in `@supports` and in `prefers-reduced-motion: no-preference`;
where either fails the rule never applies and every section is simply visible.
One movement, one distance, used everywhere.

## Stack

A static site with no build step. The UI is authored as a declarative
template rendered at runtime by `support.js`, which loads React 18 from
`vendor/` on this origin rather than a CDN — the page makes **no third-party
requests at all**, so there are no extra DNS lookups or TLS handshakes on a
cold load.

Typography is Cormorant Garamond (display) with Source Serif 4 (body), both
self-hosted as latin-subset woff2. Icons are Phosphor Duotone, subset from
1000+ glyphs down to the 37 the app actually uses — 164 KB to 7 KB.

The layout is responsive across three breakpoints: a single column on phones
with a tab bar docked to the bottom edge; two-column grids on tablets; and on screens
1080px and wider a two-panel editorial spread — a fixed portrait rail beside
the content, with the tab bar becoming a thin bar docked to the top and marigold petals drifting
in the background.

## Layout

| Path            | Purpose                                          |
| --------------- | ------------------------------------------------ |
| `index.html`    | Entry point — the app template and its logic     |
| `support.js`    | Runtime that renders the template                |
| `_ds/`          | Design-system stylesheet and bundle              |
| `vendor/`       | Self-hosted React, fonts and subset icon font    |
| `assets/`       | Images, icons and the registration QR code       |
| `assets/amma-photos/` | The three photographs of Amma — see its own README |
| `assets/founders-2025/` | Founders’ Day 2025 photographs — see its own README |
| `assets/wardrobe/` | Drop folder for films and for fests before 2025 |
| `manifest.json` | PWA manifest — name, icons, theme colours        |
| `vercel.json`   | Hosting config — caching and security headers    |

`*.dc.html` are the editable design-canvas sources. `index.html` is the
deployed copy of `Brahm 2026 Fest App.dc.html`; regenerate it after edits
with:

```sh
cp "Brahm 2026 Fest App.dc.html" index.html
```

## Five things you will want to change

**Registration.** Entries are currently closed. One flag near the top of the
script block in `index.html` drives every registration call-to-action in the
app — the home callout, the QR promo, the competitions page and both buttons on
each competition:

```js
const REG_CLOSED = true;   // false brings all of them back
```

**Amrita's Wardrobe.** The archive of earlier fests is driven by
`WARDROBE_PHOTOS` and `WARDROBE_VIDEOS` near the top of the script block. The
Founders' Day 2025 rows point at real files in `assets/founders-2025/`; the
2024 and 2023 rows, and both films, are empty slots waiting on
`assets/wardrobe/` under the names listed in that folder's README. The app
probes each path first, so anything not yet supplied is quietly skipped.

**The sports fixtures.** The whole Sports page (`#/sports`) is driven by
`SPORTS` near the top of the script block. Venue and timing are settled and
written in. `fixtures` starts empty, and while it is empty the page prints one
honest line in its place rather than an empty frame — the draw not having been
made is information too. Publishing a fixture is pushing a row:

```js
{ id:"cricket", name:"Cricket", kind:"Field · Limited overs", accent:28,
  venue:"Amrita Ground", timing:"6:00 AM – 2:30 PM", date:null,
  note:"First ball at dawn; the ground runs all morning.",
  fixtures:[
    { round:"League · Match 1", a:"Amrita XI", b:"MVN XI",
      time:"6:00 AM", note:"Toss at 5:45" }
  ] }
```

`note` on a fixture is optional and `round` is just the label printed above the
pairing, so `"Semi-final"`, `"Final"` and `"League · Match 3"` all work. Filling
`date` replaces the "Across 21st – 23rd September" line. `accent` is the hue, in
degrees, that the sport's panel is tinted with — it drives the panel wash, the
rule under the name and the fixture heading, so one number gives a new sport its
own identity. A fourth sport is a fourth row.

The same venues and timings are repeated in the 21st and 22nd September
schedule entries and in `VENUES`, so change those too if a ground moves.

**The event categories.** `CATEGORIES` decides the six tiles on the home screen
and on the Events hub. A category either gathers competitions by id from
`COMPS`, or names schedule items by id from `DAYS`, or — for Sports — sets
`sports:true` and hands off to the Sports page. Adding one means adding a row
and letting `SCREENS` pick up its `screen` id, which it does automatically for
any id beginning `cat-`.

**Founders' Day 2025 photographs.** The look-back grid draws from the
`Founders’ Day 2025` rows of `WARDROBE_PHOTOS` — the same probed list the
Wardrobe screen uses, so adding a file to `assets/founders-2025/` and a row to
that array puts it in both places. Nothing is held open for photographs that do
not exist yet: a tile appears when its file does, and the whole grid is skipped
if none of them are there.

**The founder portrait.** *About the Founder* on the home screen holds a 4:5
portrait of Sri Mata Amritanandamayi Devi, taken from `amma-3.png` in
`assets/amma-photos/`:

```js
const AMMA_PORTRAIT = "assets/amma-photos/amma-3.png";
```

To use a different photograph, point that constant at it — nothing else needs
changing. The app checks the path on load, the same way the sponsor logos and
the Wardrobe media are checked, so a path with nothing behind it falls back to
the *73 Years of Pure Love & Light* emblem on navy rather than leaving a broken
frame.

A replacement wants to be portrait, roughly 4:5 (1200x1500 is ample). The frame
crops to 4:5 and anchors to the top of the image, so nothing is taken off the
head. A cut-out with alpha sits best: `.founder-frame::before` lays a warm
radial behind the figure so she is lit by the frame rather than flat on it.

## The hero photograph

The home screen is led by Amma (Sri Mata Amritanandamayi Devi). She is a cut-out
with a transparent background, so she is not a background image but a real
`<img class="hero-figure">` standing in front of the frame:

| Layer            | What it is                                                  |
| ---------------- | ----------------------------------------------------------- |
| `.hero-art`      | `hero-crowd.jpg` / `hero-crowd-wide.jpg`, blurred and held down in tone — the room she is standing in |
| `.hero-wash`     | The scrim, plus a warm radial where her head falls, so she is lit by the frame rather than pasted onto it |
| `.hero-figure`   | `assets/amma-photos/amma-2.webp` (1000x1314, 118 KB, alpha)           |
| `.hero-glow`     | Marigold at the foot, and the veil that guarantees the lockup its contrast — this one sits *above* her |

She is masked away at the hem with a `mask-image` gradient rather than cropped,
so there is no cut edge where she meets the type. The fest name is set in
Cormorant over her; the neon `brahm-logo.png` poster wordmark is no longer used
by any screen, and is kept only in case it is wanted back.

**The lockup.** `AVVP_Emblem-new-white.png` sits directly above the fest name as
`.hero-brandmark`, so the institutional association reads before Brahm does and
the header is left with nothing but the emblem. It carries a tight dark halo as
well as a cast shadow, because at tablet width it falls on the bright of the
sari and white on white needs the separation. The `73 Years` mark keeps the
opposite corner at every width.

**Sizing.** Below 1080px the cover is a flex column: `.hero-figwrap` takes
whatever height is left once `.hero-lock` has what it needs, and she is capped
at 160% of that track and pushed down by 38% of her own height. Because both
numbers are shares of her rendered size rather than fixed distances, the lockup
always meets her at the same point on the robe and her face — which sits in the
top 40% of the cut-out — always clears it, at every window size. She shrinks
instead of colliding.

She used to be positioned against the top of the cover and capped at a share of
*its* height, with the lockup anchored to the foot. On a short or square window
the two met in the middle and the emblem and the fest name landed across her
face. Two `min-height` floors, 790px in the base rule and 780px in the tablet
one, made it worse by holding the cover taller than the window, so the
countdown fell behind the tab bar and all that was left on screen was a cropped
portrait. The cover is now `min(100dvh,920px)` — `min(92dvh,1000px)` from 700px
up — so it is exactly the window and never more, and from 780px of window
height down the lockup gives room back rather than squeezing her out: the
tagline goes first, then the type steps down and the countdown compacts.

From 1080px the cover is a row again — the wrapper goes `display:contents`, the
figure returns to being positioned against the hero itself, the lockup holds the
left and she stands to its right at `width:min(46vw,620px)`.

**Adjusting.** `--hero-focus-x` / `--hero-focus-y` on `.hero` place the crowd
frame behind her. If a future photograph needs more or less shade, change the
stops on `.hero-wash` and `.hero-glow`, not the image.

**Replacing her.** Export a cut-out with a transparent background, trim to the
alpha bounding box, resize to ~1000px wide and save as WebP q86. The originals
supplied for this pass are kept in `uploads/` (excluded from both the repository
and the deployment).

`assets/amma-photos/amma-1.jpg` is the landscape frame of Amma among the
blossoms. It does three jobs from the one file: the `og:image` link-preview
card, the band at the head of the home closer, and the band above the pull-quote
on the About screen.

## Following the fest

The official account is a single constant, `INSTAGRAM`, with `INSTAGRAM_HANDLE`
beside it for the label. Two places read them: the **Follow Brahm** section that
closes the home story, and a row on the *More* screen. Change the constants and
both follow.

```js
const INSTAGRAM = "https://www.instagram.com/brahmamritafest/?hl=en";
```

The glyph on the button is inline SVG rather than an icon-font character —
the subset font carries only the 37 glyphs the app already used, and adding one
would mean rebuilding it.

## Navigation

Each screen owns a hash — `#/schedule`, `#/comp/yukti`, `#/sports`,
`#/cat-dance`, `#/wardrobe` — pushed
with `history.pushState`. The device back button, the browser's back arrow and
the in-app **Back** pill therefore all walk the same stack, and a screen can be
linked to or reloaded directly. The hash rather than the path keeps that true
on a static host with no rewrite rules.

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
