# Bla Bli Blu — The Gibberish Studio

A working prototype of the personalisation engine for Case-O-Nova 8.0 (Team MU).

Everyone has their own way of saying "I love you". Pick a fragrance, pick a size, pick who it's for,
write the one line only the two of you understand — and the studio turns it into three gibberish
words that get printed on the bottle where BLA BLI BLU normally goes.

## Live demo

Once GitHub Pages is turned on for this repo (see below), the live link goes here so the jury can
open it directly: `https://sharmasambhav25.github.io/Bla-Bli-Blu/`

## Running it

It's a single file with no build step and no dependencies. Open `index.html` in a browser, or:

```
python3 -m http.server 8000
```

## Publishing on GitHub Pages

The files already live in this repo (`sharmasambhav25/Bla-Bli-Blu`). Two steps left, both in
Settings:

1. **Settings → General → Danger Zone → Change visibility → make it Public.** GitHub Pages on the
   free plan only serves public repos, so this repo needs to switch from Private to Public before
   Pages will turn on. (Nothing sensitive lives here — it's a UI prototype and a README.)
2. **Settings → Pages → Source: `Deploy from a branch` → `main` / `root` → Save.**

The site goes live at `https://sharmasambhav25.github.io/Bla-Bli-Blu/` within a minute or two —
that's the link for the jury.

## How the gibberish works

The generator follows the brand's own logic: it takes a consonant cluster out of the user's line and
shifts the vowel across it three times — exactly how **Bla / Bli / Blu** is built from one cluster.
So "kuch khaaya?" from a mother comes back as **KHAA KHII KHUU**. Every custom bottle is visibly a
sibling of the brand name rather than an unrelated word.

Two engines run behind it:

- **Live model** — when the page is opened as a Claude Artifact it calls Claude for a richer word set
  plus a one-line meaning.
- **Local engine** — a deterministic phonetic generator in `index.html`. This is what runs on GitHub
  Pages, and it's also the fallback if the model is unavailable, so a demo never breaks.

## Product photography

The site now uses Bla Bli Blu's **real product photography** — the same studio shots that appear on
their own website (`blabliblulife.com`) — instead of the old drawn bottle:

- `images/` holds one photo per fragrance (plus `hero-set.jpg` for the hero), saved from the brand's
  product pages and their retailer listings. Photography © Bla Bli Blu; used here for this concept
  demo only.
- `mountShot()` in `index.html` places each photo in the hero, the studio stage, the fragrance
  picker (as a thumbnail) and the wall tiles. The three gibberish words are printed over the
  photographed bottle on a label patch matched to the bottle's own colour, sitting exactly where the
  printed fragrance name is (`lab` zone per photo, in percent of the image).
- Dragging the stage now tilts the photo in 3D instead of spinning the drawn bottle.
- `renderBottle()` (the SVG bottle with the cylindrical label wrap) is kept as an automatic fallback:
  if a photo ever fails to load, the drawn bottle appears in its place so a demo never breaks.

## Structure

Everything is in `index.html`:

- `FRAGS` — the 15 live fragrances with notes, prices and whether they're in the oud range (oud ships
  in the red bottle, parfum in cream).
- `SIZES` — 30 / 75 / 100 ml with the price multipliers.
- `RELS` — the relationships, each with a suggested line.
- `WALL` — the sample bottles on the "Everyone's got one" wall.
- `localGib()` — the offline gibberish engine.
- `mountShot()` — the real product photo plus the printed label patch.
- `renderBottle()` — the fallback SVG bottle, including the cylindrical label wrap.

## Note

This is a student concept prototype, not an official Bla Bli Blu property. Fragrance names, notes and
prices reflect the live range; the bottle photography is Bla Bli Blu's own product photography.
