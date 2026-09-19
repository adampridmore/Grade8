# Grade 8 Guitar Practice

A small set of standalone HTML pages for practicing Grade 8 guitar theory and ear-training: chord recognition, fretboard notes, harmony (chord-sequence) dictation, and pitch-memory. No install, no build — just open a page in your browser.

## Pages

- **`index.html`** — home page with a menu linking to the exercises below.
- **`chord-test.html`** — click a chord button to hear it strummed on a sampled acoustic guitar.
- **`fretboard.html`** — interactive fretboard (12 frets, 6 strings); click a fret to hear that note, or toggle note names on.
- **`harmony-test.html`** — ear-training test: plays a random 4-bar chord progression, you type the chord sequence you heard (e.g. `G C D Am`) and get instant right/wrong feedback per chord, the correct sequence with roman-numeral degrees (e.g. `C (I) F (IV) G (V) Dm (ii)`), and a running correct/wrong score for the page (resets on refresh).
- **`pitch-test.html`** — ABRSM-style pitch-memory test: plays a random 4-bar melodic phrase (bar 3 always repeats bar 1) in a random major key and time signature (3/4, 4/4, 6/8); press Play to hear it (and again as many times as you like), then Reveal shows the answer as a 6-line guitar tab with a rhythm line above it, fingered within a single 5-fret position with no open strings.
- **`index-samples.html`** — an earlier standalone variant of the chord player, kept for reference (not linked from the home page).

## Running it

No dependencies to install. Either:

- Open any `.html` file directly in your browser, or
- Serve the folder as static files, e.g.:

  ```
  python3 -m http.server 8000
  ```

  then visit `http://localhost:8000/index.html`.

Audio samples are fetched from a CDN on page load, so an internet connection is required to hear sound.
