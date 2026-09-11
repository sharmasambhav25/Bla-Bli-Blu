# Bla Bli Blu — The Gibberish Studio

A working prototype of the personalisation engine for Case-O-Nova 8.0 (Team MU).

Everyone has their own way of saying "I love you". Pick a fragrance, pick a size, pick who it's for,
write the one line only the two of you understand — and the studio turns it into three gibberish
words that get printed on the bottle where BLA BLI BLU normally goes.

![Hero](preview-hero.png)
![Studio](preview-studio.png)

## Live demo

Once this repo has GitHub Pages turned on (see below), put the live link here so the jury can open
it directly: `https://<username>.github.io/<repo>/`

## Running it

It's a single file with no build step and no dependencies. Open `index.html` in a browser, or:

```
python3 -m http.server 8000
```

## Publishing on GitHub Pages

This folder is already its own git repo (separate from any other project) with the first commit
made. To push it up:

1. On GitHub, create a new **empty** repository (no README/license/gitignore) — e.g. `bla-bli-blu-studio`.
2. In this folder, point it at that repo and push:
   ```
   git remote add origin https://github.com/<username>/<repo>.git
   git branch -M main
   git push -u origin main
   ```
3. Settings → Pages → Source: `Deploy from a branch` → `main` / `root`.
4. The site goes live at `https://<username>.github.io/<repo>/` in about a minute — drop that link
   into the "Live demo" section above before you send it to the jury.

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

## Swapping in real product photography

The bottle is currently drawn in SVG, which is what lets the label text wrap around the glass and
change live. To use real photographs instead, drop transparent PNGs into `images/` and replace the
`renderBottle()` call in the stage with an `<img>` plus an absolutely-positioned text overlay. The
label text is generated in one place (`renderBottle`), so this is a contained change.

## Structure

Everything is in `index.html`:

- `FRAGS` — the 15 live fragrances with notes, prices and whether they're in the oud range (oud ships
  in the red bottle, parfum in cream).
- `SIZES` — 30 / 75 / 100 ml with the price multipliers.
- `RELS` — the relationships, each with a suggested line.
- `WALL` — the sample bottles on the "Everyone's got one" wall.
- `localGib()` — the offline gibberish engine.
- `renderBottle()` — the bottle, including the cylindrical label wrap.

## Note

This is a student concept prototype, not an official Bla Bli Blu property. Fragrance names, notes and
prices reflect the live range; the bottle artwork is an illustration.
