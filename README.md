# Bla Bli Blu — The Words Studio

A working prototype of the personalisation engine for Case-O-Nova 8.0 (Team MU).

Everyone has their own way of saying "I love you". Pick a fragrance, pick a size, pick who it's for,
tell us about them — and the studio turns what you wrote into the three words that become their
version of it, printed on the bottle where BLA BLI BLU normally goes.

![Hero](preview-hero.png)
![Studio](preview-studio.png)

## Live demo

`https://sharmasambhav25.github.io/Bla-Bli-Blu/`

## Running it

It's a single file with no build step and no dependencies. Open `index.html` in a browser, or:

```
python3 -m http.server 8000
```

## Publishing changes

Push an updated `index.html` to the `main` branch of this repo — GitHub Pages redeploys
automatically within a minute or two.

## How the three words work

There's no invented gibberish anymore. The customer writes a few sentences about the person
they're gifting the bottle to, and gets back three real words that read like something you'd
actually say instead of "I love you" — pulled from what they wrote, not built from sound patterns.

Two engines run behind it:

- **Live model** — when the page is opened as a Claude Artifact, it reads the whole message and
  picks the three words that best capture the person, plus a one-line reason why.
- **Local engine** (`localWords()`) — a deterministic fallback with no model involved. It strips
  filler words (the, and, still, never, …) out of the message and keeps three distinct real words
  from what was actually written, spread across the message rather than clumped together. This is
  what runs on GitHub Pages, and it's also the fallback if the live model is unavailable, so a demo
  never breaks — it's just honest extraction instead of true understanding.

## Product photography

`images/` holds one real product photo per fragrance plus `hero-set.jpg` for the header — Bla Bli
Blu's own photography, used here for the concept demo only. `mountShot()` places these photos in
the hero, the interactive stage, each fragrance's picker thumbnail, and the wall tiles.

The customer's three words are printed on top of the photo through a small label patch
(`.shot .label`) positioned over each bottle's real printed-name area — its position is hand-tuned
per shot via the `lab` property on each `FRAGS`/`WALL` entry (`LAB_CAN` for the two-bottle studio
shots, `LAB_SOLO` for single-bottle shots), and its colour matches the bottle (cream label / red ink
for parfum, red label / cream ink for oud).

Dragging the stage tilts the photo in 3D (`rotateY`, via `applyTilt()`); arrow keys nudge it ±3°.
If a photo ever fails to load, `mountShot()` automatically falls back to the original hand-drawn SVG
bottle (`renderBottle()`), so the demo never breaks even if `images/` is missing or a filename typo
sneaks in.

## Structure

Everything is in `index.html`:

- `FRAGS` — the 15 live fragrances with notes, prices, product photo (`img`), label position (`lab`)
  and thumbnail crop (`op`), and whether they're in the oud range (oud ships in the red bottle,
  parfum in cream).
- `SIZES` — 30 / 75 / 100 ml with the price multipliers.
- `RELS` — the relationships, each with an example message about that person.
- `WALL` — the sample bottles on the "Everyone's got one" wall, each with its own photo.
- `localWords()` — the offline three-words extraction engine.
- `mountShot()` / `renderBottle()` — the bottle: the real photo plus label overlay, with the drawn
  SVG bottle kept as an automatic fallback.

## Note

This is a student concept prototype, not an official Bla Bli Blu property. Fragrance names, notes and
prices reflect the live range; bottle photography is Bla Bli Blu's own product photography, used here
for the concept demo.
