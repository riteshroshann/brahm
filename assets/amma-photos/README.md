# Amma — photographs

The photographs of Sri Mata Amritanandamayi Devi. Everything that shows her
reads from this folder, so there are three files to keep current rather than
one per placement.

| File          | What it is                                   | Where it appears |
| ------------- | -------------------------------------------- | ---------------- |
| `amma-1.jpg`  | 1200×630, landscape, seated among blossoms    | `og:image` link-preview card · band at the head of the home closer · band above the pull-quote on the About screen |
| `amma-2.webp` | 1000×1314, portrait cut-out, alpha            | The hero figure on the home screen |
| `amma-3.png`  | 1086×1448, portrait cut-out, alpha            | The 4:5 portrait in **About the Founder** |

## Replacing one

Each path is named in a handful of places in `index.html` (and in
`Brahm 2026 Fest App.dc.html`). Keep the filenames and nothing needs touching;
change one and update all of its references:

- `amma-1.jpg` — the `og:image` meta tag, `.closer-art img`, `.amma-band img`
- `amma-2.webp` — the `<link rel="preload">` in the head, and `.hero-figure`
- `amma-3.png` — the `AMMA_PORTRAIT` constant near the top of the script block

`AMMA_PORTRAIT` is probed with a `HEAD` request on load, the same contract the
sponsor logos and the Wardrobe media use, so if that file ever goes missing the
founder frame falls back to the *73 Years of Pure Love & Light* emblem on navy
rather than leaving a broken frame. The hero figure has no such fallback — it is
a plain `<img>`, so `amma-2.webp` must be present.

## What to supply

- **`amma-1`** — landscape, 1200×630 or the same 1.9:1 ratio. It is also the
  social card, so keep her clear of the extreme edges.
- **`amma-2` / `amma-3`** — portrait, 3:4 to 4:5, **with a transparent
  background**. The hero masks her away at the hem and stands her in front of a
  blurred crowd; a rectangular photo with its own background will not work
  there. The founder frame lays a warm radial behind the cut-out.

Both cut-outs are placed so nothing is lost off the top of the head. `amma-2`
has almost no headroom of its own — her crown sits 0.3% from the top edge of
the file — which is why the hero caps the figure's height rather than sizing it
by width alone. Leave a little space above the head in a replacement and that
constraint relaxes.

## Weight

`amma-3.png` is 1.9 MB, by far the heaviest asset here. It is lazy-loaded, so it
costs nothing until the founder section scrolls into view, but the same picture
as WebP would be roughly a tenth of that. Worth converting if the page is ever
measured on a slow connection.
