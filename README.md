# Bitcoin Point Cloud

A single-file canvas toy: a cloud of ink specks drifts on sepia paper, and a blurred glyph
presses into it like a stamp. Every speck walks down the stamp's pressure gradient until it
sits outside the letterform, so the word appears as **negative space**. Clones of nearby
specks then fly into a raster inside the letters and lift by the local relief height,
revealed as a top-to-bottom line scan.

Type any text, pick the paper and ink colour, drag size and density. No build step, no
dependencies, no network calls — one HTML file.

## Run it

Open `index.html` in a browser, or serve the folder statically:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Controls

| Control | Effect |
| --- | --- |
| **Text** | The stamped word. A `^` turns the rest into a superscript, e.g. `2^256`. |
| **Hintergrund** | Paper colour. |
| **Punkte** | Ink colour. Four tones are derived from it, keeping the printed texture. |
| **Grösse** | Glyph scale. |
| **Dichte** | Square pixels of paper per speck — lower means denser. |
| **Neu abspielen** | Replay the reveal (also the space bar). |
| **Zurücksetzen** | Back to defaults. |

The mouse gently displaces specks it passes over. `prefers-reduced-motion` skips the
animation and shows the finished drawing.

## How it works

1. The glyph is rendered to an offscreen canvas. A sharp alpha mask marks where a letter
   ends; several blur radii are stacked into one pressure field, so the gradient still
   points outward deep inside a thick stroke.
2. Each speck walks down that gradient until it leaves the mask, plus a couple of rim
   steps. On a plateau it wanders in one fixed direction.
3. The relief is built from *clones* of nearby specks — picked through a bucket grid — so
   the background keeps its density instead of thinning out where the letters are.
4. Springs are integrated per frame; when frames arrive far apart the scene snaps to the
   state the timeline asks for instead of falling behind.

## Credit

The idea and the sepia look are inspired by an interactive "how big is 2^256"
visualisation: <https://tlausz.github.io/keyspace/>. This is an independent
implementation, not a copy of its code.

## Licence

MIT — see [LICENSE](LICENSE).
