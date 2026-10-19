# Complex Tunes

An interactive browser instrument for exploring the relationship between music and complex systems.

- **Music → paths:** Load a mono or stereo recording and visualize frequency-band energy or channel energy as a rotatable 3D delay-coordinate portrait. Adjust delay, smoothing, and history; scrub playback or show the whole recording. Includes a generated demo.
- **Lorenz → sound:** Listen to the Lorenz system with x mapped to pitch, y to stereo position, and z to brightness. Add nearby trajectories with independent voices and colored paths.

## Static hosting

The application is plain HTML and JavaScript. No build, npm install, backend, API keys, database, or ChatGPT services are needed. Audio processing runs locally in a Web Worker and playback uses the Web Audio API. Selected recordings are never uploaded.

Deploy these four files together, preserving their names:

```
index.html
app.js
music.js
music-analysis.js
```

All runtime asset URLs are relative, so the site works at a domain root or under a project path such as `/complex-tunes/`. Serve over HTTPS in production. Opening `index.html` directly with a `file://` URL is not supported because browser security can prevent the analysis worker from loading.

### GitHub Pages

In this repository, open **Settings → Pages** and choose **Deploy from a branch**, **main**, **/ (root)**, then save. The `.nojekyll` file lets GitHub Pages serve the files directly. No workflow or build command is required. Use the URL shown by GitHub when deployment finishes.

### Local use

From the repository directory, run:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000/`. On Windows, `py -m http.server 8000` is an alternative. Python is only a convenient local static server, not an application backend.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | UI, styles, controls, and explanatory content |
| `app.js` | Lorenz integration, traces, and synthesizer |
| `music.js` | File loading, playback, controls, and 3D projection |
| `music-analysis.js` | Worker for FFT and energy-feature extraction |

## How the music portrait works

For each measured energy feature E, plot `[E(t), E(t − τ), E(t − 2τ)]`. The frequency bands are 20–250 Hz, 250–2,000 Hz, and 2,000–12,000 Hz, limited by the recording's sample rate. These are bands of sound, not isolated instruments.

Analysis uses centered 2,048-sample Hann windows at 50 feature samples per second. Band powers are averaged across channels before conversion to decibels. Features are smoothed, centered, and independently scaled using whole-recording statistics. The portrait is an exploratory representation; its shape alone does not establish chaos or identify the generating equations.

## Browser support and limits

Use a modern browser with Web Audio, Web Workers, Canvas 2D, and Pointer Events. Playback starts after a user action. Audio decoding support varies by browser; MP3 and WAV are useful first choices. The file loader accepts mono or stereo recordings up to 10 minutes and 80 MB. Large recordings require memory for decoded audio and analysis copies.

The plot supports mouse/touch rotation, scroll zoom, and keyboard rotation (arrow keys) and zoom (+/−) when focused. Browser playback and touch behavior should be checked on the devices you intend to support.

## Development checks

No dependencies are required for JavaScript syntax checks if Node.js is installed:

```sh
node --check app.js
node --check music.js
node --check music-analysis.js
```

Smoke test: open the demo, play/pause, scrub, rotate, switch between frequency bands and stereo, then switch to Lorenz mode and add an offset path. Switching modes stops the previous mode's audio.

This repository contains the standalone static app. Hosting-specific metadata and deployment archives from the original Sites workspace are not required and are excluded.
