[repo]: https://github.com/easyvansh/celluloidfm
[original]: https://github.com/rainner/soma-fm-player
[somafm]: https://somafm.com/
[audioapi]: https://developer.mozilla.org/en-US/docs/Web/API/AudioContext
[vue]: https://vuejs.org/
[webpack]: https://webpack.js.org/
[three]: https://threejs.org/
[mit]: https://www.opensource.org/licenses/mit-license.php

# CelluloidFM

![CelluloidFM preview](thumb.png)

CelluloidFM is a cinematic SomaFM web radio player redesigned by **Vansh**. It transforms a classic browser radio app into a late-night listening room with dark film-inspired visuals, warm projector-gold accents, editorial typography, atmospheric station cards, favorites, station search, recently played tracks, and a Web Audio powered visualizer.

The project is based on the original MIT-licensed [SomaFM Music Player][original] by Rainner Lins, with a new Nightwave visual direction and project identity.

Repository: [github.com/easyvansh/celluloidfm][repo]

## What It Does

- Streams live radio stations from [SomaFM][somafm].
- Loads station data from `https://somafm.com/channels.json`.
- Lets users search stations and sort by name, listener count, genre, or favorites.
- Plays MP3 streams directly in the browser.
- Shows current and recent track metadata when SomaFM provides it.
- Saves favorite stations in `localStorage`.
- Exports favorites as an `.m3u` playlist.
- Supports station deep links such as `#/channel/groovesalad`.
- Renders a Three.js audio visualizer using browser audio frequency data.
- Adds a Nightwave UI layer with glass panels, cinematic spacing, glow states, and modern typography.

## Design Direction

CelluloidFM uses a **Nightwave** theme: a cinematic late-night radio room inspired by rainy city windows, CRT glow, noir interiors, deep work playlists, and old film UI.

Core palette:

```scss
$bg: #090909;
$panel: #111111;
$text: #f5f1ea;
$muted: #8b8b8b;
$accent: #d4a574;
$accent2: #ff5e5e;
$border: #1f1f1f;
```

Typography:

- Headings: Cormorant Garamond
- UI/body: Inter
- Accent labels: Space Grotesk

## Tech Stack

- [Vue 2][vue] loaded from CDN.
- [Three.js][three] loaded from CDN.
- Axios loaded from CDN.
- [Webpack 3][webpack] and Babel for the legacy source build.
- Sass source styles in `src/scss/`.
- `public/css/nightwave.css` as the currently loaded visible Nightwave override.
- Static hosting friendly: no backend is required.

## Project Structure

```text
.
|-- index.html              # App shell and Vue template
|-- README.md               # Project documentation
|-- CNAME                   # Custom domain config, if used
|-- package.json            # Legacy npm scripts and dependencies
|-- webpack.config.js       # Legacy Webpack 3 build config
|-- public/
|   |-- bundles/            # Existing compiled JS/CSS bundle
|   |-- css/
|   |   |-- fonts.css
|   |   `-- nightwave.css   # Current visible Nightwave theme override
|   |-- fonts/              # Local font and icon assets
|   |-- img/                # Background images
|   `-- audio/              # Small audio assets
`-- src/
    |-- app.js              # Vue state, routing, controls, favorites, UI actions
    |-- js/
    |   |-- audio.js        # HTML audio and Web Audio API logic
    |   |-- soma.js         # SomaFM API fetch and channel normalization
    |   |-- scene.js        # Three.js visualizer scene
    |   |-- sphere.js       # Visualizer object
    |   |-- store.js        # localStorage helper
    |   |-- favorite.js     # Favorite button component
    |   |-- filters.js      # Vue filters
    |   `-- utils.js        # Search and sort helpers
    `-- scss/               # Source Sass theme files
```

## Run Locally

The fastest way to run the current app is with a static server:

```bash
python -m http.server 8000
```

Open:

```text
http://localhost:8000
```

This works because the app is already static and loads the current Nightwave CSS override directly from `public/css/nightwave.css`.

## Legacy Build Commands

Install dependencies:

```bash
npm install
```

Run the Webpack dev server:

```bash
npm run dev
```

Build production bundles:

```bash
npm run build
```

## Compatibility Note

This project still contains a legacy build stack: Webpack 3, Vue 2, and `node-sass@4`. On modern Node.js versions, `npm install` may fail because `node-sass@4` depends on old native build tooling.

For now, the practical workflow is:

1. Run the app with `python -m http.server 8000`.
2. Edit `index.html` and `public/css/nightwave.css` for visible UI changes.
3. Treat `src/scss/` as the source direction for a future build-tool upgrade.

A future modernization pass should replace `node-sass` with `sass`, update Webpack or migrate to Vite, and then remove the need for a standalone CSS override.

## Key Files To Update

| Goal | File |
| --- | --- |
| App structure and Vue template | `index.html` |
| Current visible Nightwave styling | `public/css/nightwave.css` |
| Source theme tokens and Sass styles | `src/scss/` |
| Player state, favorites, routing, and controls | `src/app.js` |
| SomaFM API handling | `src/js/soma.js` |
| Audio playback engine | `src/js/audio.js` |
| Visualizer scene | `src/js/scene.js`, `src/js/sphere.js` |
| Custom domain | `CNAME` |

## Update Checklist

Before pushing changes, test these flows:

- Station list loads.
- Search works.
- Sorting works.
- Selecting a station updates the URL hash.
- Playback starts after a user click.
- Volume control works.
- Favorites can be toggled.
- Favorites can be exported.
- Recent tracks load.
- The visualizer appears during playback.
- Mobile layout remains usable.

## Git Workflow

Stage and commit your changes:

```bash
git add .
git commit -m "Update CelluloidFM README and Nightwave UI"
```

Push to your GitHub repository:

```bash
git push -u origin main
```

If the remote is not set correctly:

```bash
git remote set-url origin https://github.com/easyvansh/celluloidfm.git
```

## Credits

- Maintainer and redesign: **Vansh**


## License

This project keeps the original MIT license. See [MIT License][mit].
