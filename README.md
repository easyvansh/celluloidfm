[site]: https://rainnerlins.com/
[twitter]: https://twitter.com/raintek_
[mit]: https://www.opensource.org/licenses/mit-license.php
[repo]: https://github.com/rainner/soma-fm-player/
[demo]: https://somafm.loudfusion.com/
[somafm]: https://somafm.com/
[audioapi]: https://developer.mozilla.org/en-US/docs/Web/API/AudioContext
[vue]: https://vuejs.org/
[webpack]: https://webpack.js.org/
[three]: https://threejs.org/

# Nightwave SomaFM Player

![SomaFM Player preview](https://raw.githubusercontent.com/rainner/soma-fm-player/master/thumb.jpg)

Nightwave SomaFM Player is a cinematic single-page web app for browsing and streaming radio stations from [SomaFM][somafm]. It keeps the original player architecture intact while reframing the interface as a late-night listening room with dark editorial typography, warm projector-gold accents, glass panels, atmospheric station cards, favorites, search, recently played tracks, and a Three.js audio visualizer driven by the Web Audio API.

Live demo: [somafm.loudfusion.com][demo]

## Features

- Browse SomaFM stations from `https://somafm.com/channels.json`.
- Nightwave visual treatment with a noir palette, cinematic panels, and editorial typography.
- Search stations and sort by station name, listener count, favorites, or genre.
- Play and stop streams with persistent volume control.
- View current and recent tracks for the selected station.
- Save favorite stations in `localStorage`.
- Export saved favorites as an `.m3u` playlist.
- Open station info, Twitter, and PLS links when SomaFM provides them.
- Deep-link to a station with hash routes such as `#/channel/groovesalad`.
- Animated Three.js visualizer using browser audio frequency data.
- Keyboard shortcuts: `Enter` opens stations, `Escape` closes stations, and `Space` toggles playback when a station is selected.

## Tech Stack

- [Vue 2][vue] through the global CDN build loaded in `index.html`.
- [Three.js][three] through the global CDN build loaded in `index.html`.
- Axios through the global CDN build loaded in `index.html`.
- [Webpack 3][webpack] and Babel for bundling the app entry point.
- Sass compiled through `sass-loader` during the Webpack build.
- `public/css/nightwave.css` provides the current visible Nightwave override while the legacy build stack is being maintained.
- Static assets served from `public/`.

## Project Structure

```text
.
|-- index.html              # Main HTML shell and Vue template markup
|-- package.json            # npm scripts and build dependencies
|-- webpack.config.js       # Webpack 3 dev/build configuration
|-- public/
|   |-- bundles/            # Built JS and CSS bundles
|   |-- css/                # Font stylesheet and Nightwave override
|   |-- fonts/              # Local font and icon assets
|   |-- img/                # Background images
|   `-- audio/              # Small bundled audio assets
`-- src/
    |-- app.js              # Main Vue app, state, lifecycle, and UI actions
    |-- js/
    |   |-- audio.js        # HTML audio element and Web Audio API setup
    |   |-- soma.js         # SomaFM API fetch and response normalization
    |   |-- scene.js        # Three.js canvas setup and render loop
    |   |-- sphere.js       # Visualizer object
    |   |-- store.js        # localStorage wrapper
    |   |-- favorite.js     # Vue favorite-button component
    |   |-- filters.js      # Vue formatting filters
    |   `-- utils.js        # Search/sort helpers
    `-- scss/               # Application styles
```

## Getting Started

Install dependencies:

```bash
npm install
```

Start the Webpack development server:

```bash
npm run dev
```

The development server settings live in `webpack.config.js`. By default this project is configured with:

```js
const serverHost = '192.168.1.152';
const serverPort = 8000;
```

Update `serverHost` to `localhost` or your local network IP before running the dev server if needed.

Build production assets:

```bash
npm run build
```

The production build writes:

- `public/bundles/app.min.js`
- `public/bundles/app.min.css`

You can also serve the current static files with:

```bash
npm run init
```

## Important Compatibility Note

This project uses older build dependencies, including Webpack 3 and `node-sass@4`. If install or build fails on a modern Node.js version, use an older Node.js runtime that supports `node-sass@4`, or upgrade the build tooling before continuing. A typical maintenance upgrade would replace `node-sass` with `sass`, update `sass-loader`, and move the Webpack config to a modern Webpack version.

## How the App Works

1. `index.html` loads the static HTML shell, Vue template, CDN libraries, and compiled bundle.
2. `src/app.js` creates the Vue app and manages player state, station selection, favorites, volume, routing, and lifecycle events.
3. `src/js/soma.js` fetches SomaFM channels and recently played songs, then normalizes each channel with stream URLs, song URLs, info URLs, and route data.
4. `src/js/audio.js` creates the browser `Audio` element, connects it to `AudioContext`, manages playback, applies volume through a gain node, and exposes frequency data.
5. `src/js/scene.js` and `src/js/sphere.js` render the Three.js visualizer and update it every animation frame.
6. `src/js/store.js` persists user preferences in `localStorage`.

## What to Update

Use this section as a quick map for common changes.

| Goal | Update |
| --- | --- |
| Change page markup or Vue template structure | `index.html` |
| Change player behavior, station selection, routing, timers, favorites, or keyboard controls | `src/app.js` |
| Change SomaFM API URLs or channel normalization | `src/js/soma.js` |
| Change stream playback, audio events, volume behavior, or visualizer frequency data | `src/js/audio.js` |
| Change the visualizer scene setup | `src/js/scene.js` |
| Change the visualizer object shape or animation | `src/js/sphere.js` |
| Change sorting/search helpers | `src/js/utils.js` |
| Change persisted data keys or local storage behavior | `src/js/store.js` |
| Change favorite button markup | `src/js/favorite.js` |
| Change layout, colors, animation, responsive rules, or component styling | `src/scss/*.scss` |
| Change the currently loaded Nightwave override without rebuilding | `public/css/nightwave.css` |
| Change bundled image, audio, font, or icon assets | `public/` |
| Change dev server host, port, bundle output path, or build loaders | `webpack.config.js` |
| Change custom deployment domain | `CNAME` |

## How to Update Safely

1. Make changes in `src/` or `index.html`.
2. Run the development server with `npm run dev`.
3. Test these core flows in a browser:
   - Station list loads.
   - Search and sorting work.
   - Selecting a station updates the URL hash.
   - Playback starts after a user click.
   - Volume changes apply and persist after refresh.
   - Favorites can be toggled and exported.
   - Recent tracks load for the selected station.
   - The visualizer renders and responds during playback.
4. Run `npm run build`.
5. Commit the updated source files and generated bundle files in `public/bundles/` if your deployment serves the repository directly.

## Deployment

The app is static after building, so it can be hosted on GitHub Pages, Netlify, Vercel, or any static web server. The current repository includes a `CNAME` file for:

```text
somafm.loudfusion.com
```

For a custom domain, update `CNAME` and configure the DNS settings with your hosting provider.

## Known Limitations

- Streams and song metadata depend on the public SomaFM endpoints being available.
- Browser autoplay policies require the user to interact with the page before audio can start.
- Audio frequency analysis can behave differently across browsers; `audio.js` includes a fallback animation counter for browsers that do not provide usable frequency data.
- The current build stack is legacy and may require an older Node.js version unless upgraded.

## Author

[Rainner Lins][site]  
[@raintek_][twitter]

## License

Licensed under the [MIT License][mit].
