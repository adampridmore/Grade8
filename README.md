# Grade 8 Guitar Practice

A small set of standalone HTML pages for practicing Grade 8 guitar theory and ear-training: chord recognition, fretboard notes, and harmony (chord-sequence) dictation. No install, no build — just open a page in your browser.

## Pages

- **`index.html`** — home page with a menu linking to the exercises below.
- **`chord-test.html`** — click a chord button to hear it strummed on a sampled acoustic guitar.
- **`fretboard.html`** — interactive fretboard (12 frets, 6 strings); click a fret to hear that note, or toggle note names on.
- **`harmony-test.html`** — ear-training test: plays a random 4-bar chord progression, you type the chord sequence you heard (e.g. `G C D Am`) and get instant right/wrong feedback per chord.
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
