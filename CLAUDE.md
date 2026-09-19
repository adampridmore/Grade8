# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A set of standalone HTML pages for practicing Grade 8 guitar theory/ear-training (chord recognition, fretboard notes, harmony/chord-sequence dictation, pitch-memory). There is no build system, package manager, or test suite — each page is a single self-contained `.html` file with inline `<style>` and `<script>`, opened directly in a browser or served as static files, sharing only `style.css`.

- `index.html` — home page with a menu linking to the exercises.
- `chord-test.html` — grid of major chord buttons that strum a sampled guitar chord on click.
- `fretboard.html` — interactive fretboard (12 frets, 6 strings) where clicking a fret plays that note; toggle to reveal note names.
- `harmony-test.html` — ear-training test: plays a random 4-bar diatonic chord progression (I–IV–V plus a diatonic minor), user types the chord sequence and gets per-chord right/wrong feedback, the correct sequence with roman-numeral degrees, and a running per-page correct/wrong score (in-memory only, resets on reload).
- `pitch-test.html` — ABRSM-style pitch-memory test: generates a random 4-bar melodic phrase (bar 3 always equal to bar 1) in a random major key/time signature, plays it on demand, and reveals the answer as an ASCII guitar tab + rhythm line, fingered within a single 5-fret position.
- `index-samples.html` — earlier/alternate standalone version of the chord player (not linked from `index.html`); kept as a reference variant, not part of the main navigation flow.
- `style.css` — shared dark-theme stylesheet (reset, body base, `h1`, `.subtitle`, `.home-link`, `#status`, disabled-button styling) linked by every page above; each page's own `<style>` block holds only its page-specific rules.

## Running

No build/install step. Open any `.html` file directly in a browser, or serve the directory statically, e.g.:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000/index.html`.

## Architecture notes

- **Audio**: all sound comes from `soundfont-player` (loaded via CDN `<script>` tag, no local dependency) using the `acoustic_guitar_steel` instrument from the `FluidR3_GM` soundfont, fetched from `https://gleitz.github.io/midi-js-soundfonts/...`. Samples load asynchronously after `window.load`; UI controls stay disabled (and a status line shows "Loading samples…") until the `Soundfont.instrument(...)` promise resolves. Each page falls back to loading samples on first click if the initial load fails (autoplay-policy workaround).
- **Notes/chords are MIDI numbers**, not note names — e.g. standard tuning open strings are E2=40, A2=45, D3=50, G3=55, B3=59, E4=64. Chords are hardcoded arrays of MIDI note numbers representing specific open/barre voicings, strummed with a small per-string delay (`STRUM_DELAY`) via sequential `guitar.play(midi, ac.currentTime + i * STRUM_DELAY, ...)` calls.
- **Shared styling lives in `style.css`**; audio-loading boilerplate (`loadSamples()`, the soundfont init/fallback pattern) is still duplicated per page rather than factored out. When editing shared visual style, `style.css` is the one place to change; when editing audio-loading logic, changes must be applied to each page individually.
- **harmony-test.html** encodes music theory directly in data: `SEQUENCES` is a fixed list of 18 four-chord progressions across three keys (C/G/D major), each pairing a I–IV–V (or I–V–IV) skeleton with a diatonic minor fourth chord (ii/iii/vi). Answer checking (`checkAnswer`) does a case-insensitive string compare per position against the user's space-separated input; `ROMAN_NUMERALS` is a per-key lookup used only to annotate the revealed answer. The score counters (`correctCount`/`wrongCount`) are page-level state, not stored on the sequence objects themselves — `pickSequence()` returns a reference into the shared `SEQUENCES` array, so writing an "already scored" flag onto that object would corrupt it permanently the next time it's picked; a separate `currentScored` variable is used instead.
- **fretboard.html** derives note names from MIDI pitch-class (`midi % 12` indexed into `NOTE_NAMES`) rather than storing them, so fret/string layout and note labeling stay in sync automatically.
- **pitch-test.html** builds each phrase from a scale degree/rhythm generator, then separately solves for a guitar fingering: `buildDegreeMap` searches for the lowest 5-fret window (skipping open strings) from which every scale degree (tonic..octave) is reachable on some string, so the whole phrase can be played from a single hand position. The revealed tab and its rhythm line are built column-by-column together (`buildAnswer`) so symbols stay aligned with fret numbers, and the tab's font auto-shrinks (`fitTabToWidth`) to fit its box rather than overflow.
