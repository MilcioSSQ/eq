# eq

A 5-band browser equalizer. Drop in an audio file, adjust the bands, pick a preset. No dependencies, no install, no account.

**[Live demo →](https://milciossq.github.io/eq)**

---

## What it does

- 5 parametric bands: Sub (60 Hz), Bass (250 Hz), Mid (1 kHz), Presence (4 kHz), Air (16 kHz)
- Each band: −12 dB to +12 dB
- Master volume up to 150%
- Live frequency curve that redraws as you move sliders
- 24 presets (Flat, Bass Boost, Gaming, RnB, Rock, Jazz, …)
- Drag & drop or click-to-browse for audio files
- Play / pause with a live time counter
- Works with MP3, WAV, FLAC, OGG, and anything else your browser can decode

## What it doesn't do

It can't touch audio from other tabs (YouTube, Spotify, etc.) — browsers block that by design. If you want a system-wide EQ that affects everything, look at [Equalizer APO](https://sourceforge.net/projects/equalizerapo/) on Windows or [EQMac](https://eqmac.app/) on macOS.

---

## Usage

Open `index.html` in Chrome, Firefox, Edge, or Safari. That's it.

```
git clone https://github.com/MilcioSSQ/eq
cd eq
# open index.html in your browser
```

No build step. No npm install. One file.

---

## How it works

Each band is a [`BiquadFilterNode`](https://developer.mozilla.org/en-US/docs/Web/API/BiquadFilterNode) chained in series through the Web Audio API:

```
AudioBufferSource
  → LowShelf  (Sub, 60 Hz)
  → Peaking   (Bass, 250 Hz)
  → Peaking   (Mid, 1 kHz)
  → Peaking   (Presence, 4 kHz)
  → HighShelf (Air, 16 kHz)
  → GainNode  (volume)
  → AnalyserNode
  → speakers
```

The frequency curve is calculated by calling `getFrequencyResponse()` on a matching set of temporary filter nodes, multiplying the magnitude responses, converting to dB, and drawing the result to a canvas element on every slider move.

---

## Presets

| Preset | Character |
|--------|-----------|
| Flat | No EQ — reference point |
| Bass Boost | Heavy low-end push |
| Gaming | Enhanced highs and sub for spatial awareness |
| RnB | Warm lows, clear vocals, airy top |
| Rock | Tight bass, scooped mids, bright highs |
| Vocal Boost | Cut sub, lift mids and presence |
| Small Speakers | Compensates for thin laptop / phone speakers |
| … | 24 total |

---

## License

[MIT](LICENSE) © MilcioSSQ
