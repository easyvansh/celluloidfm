# CelluloidFM UI/UX Report

This file is the living UI/UX source of truth for CelluloidFM. Update it whenever the visual design, layout, component behavior, copy, controls, color system, or interaction model changes.

## Product Summary

CelluloidFM is a cinematic SomaFM web radio player. The UI presents live internet radio as a late-night listening room with atmospheric visuals, warm projector-gold accents, glass panels, station cards, favorites, search, sorting, recent tracks, and an audio visualizer.

Primary user goals:

- Browse SomaFM stations quickly.
- Start playback with one click after choosing a station.
- Understand what is currently playing.
- Save favorite stations.
- Export saved favorites as an `.m3u` playlist.
- Adjust volume and monitor listening time.
- Recover clearly from loading or stream errors.

## Design Direction

Product name: CelluloidFM.

Current theme name: Nightwave.

Mood:

- Cinematic
- Late-night
- Noir-inspired
- Warm glow over deep black
- Radio-room / listening-room atmosphere
- Premium but still simple and usable

Visual references:

- Rainy city windows
- Film projection glow
- CRT signal texture
- Dark music-control surfaces
- Glassy editorial panels

## Main Screens And States

### 1. Loading State

Shown before the Vue app initializes.

UI elements:

- Full-screen black background.
- Centered circular spinner.
- Spinner ring uses dark border and projector-gold active stroke.
- `noscript` message appears if JavaScript is disabled.

Colors:

- Background: `#090909`
- Spinner base stroke: `#1f1f1f`
- Spinner active stroke: `#d4a574`
- Text: `#f5f1ea`
- Link: `#d4a574`

Behavior:

- App removes the initial loader after initialization.
- Main player fades in by changing `#player-wrap` opacity to `1`.

### 2. Welcome / Empty State

Shown when no station is selected and there are no blocking errors.

Content:

- Kicker: `Late-night radio room`
- Hero heading: `Tune into the midnight signal.`
- Supporting copy: `Browse SomaFM stations as moods, save the ones that fit, and let the visualizer turn the room into a soft cinematic glow.`
- Primary CTA: `Enter Stations`

Primary action:

- `Enter Stations` opens the station drawer.

UX purpose:

- Gives a clear starting point.
- Avoids an empty dashboard before the user selects a station.
- Establishes the Nightwave tone immediately.

### 3. Now Playing State

Shown after a station is selected.

Main content areas:

- Now Playing panel
- Recent Tracks panel

Now Playing panel contains:

- Station artwork, `104px x 104px`.
- Station title.
- Kicker: `Now broadcasting`.
- Favorite toggle.
- DJ, station title, genre, and description.
- Optional station webpage button.
- Optional PLS download button.
- Optional Twitter button.
- Now Playing heading.
- Listener count.
- Current track card.

Current track card fields:

- Track title
- Album
- Artist

Recent Tracks panel contains:

- Heading: `Recent Tracks`
- Up to 3 recent track cards.
- Empty message: `There are no songs loaded yet for this station.`

Behavior:

- Selecting a station updates the URL hash.
- Selecting from the station drawer attempts playback immediately.
- Current and recent track metadata refreshes every 30 seconds.
- Active station is highlighted in the drawer.

### 4. Station Drawer

Opened from the top-right menu button, the welcome CTA, or the `Enter` keyboard shortcut.

Layout:

- Full-screen overlay.
- Right-side drawer.
- Drawer width is `100vw` on small screens.
- Drawer width is `420px` from the small breakpoint upward.
- Overlay background darkens the app.

Header:

- Search input with search icon.
- Close button.

Station list item contains:

- Station artwork, `78px x 78px`.
- Station title.
- Listener count.
- Favorite toggle.
- Genre kicker.
- Station description.

Footer:

- Sort direction toggle icon.
- Current sort label.
- Sort popover.
- Favorites export button.

Behavior:

- Clicking overlay closes drawer.
- Clicking inside drawer does not close it.
- `Escape` closes drawer.
- Active station card receives active styling.
- Search filters station names after the user enters more than one cleaned character.
- Sort settings are persisted in `localStorage`.

### 5. Error State

Shown when audio support, stream loading, or initial channel loading fails.

Content:

- Unplugged icon.
- Heading: `Oops, there's a problem!`
- One or more error messages.
- `Try again` CTA.

Behavior:

- If browser audio support failed, `Try again` re-runs audio setup.
- Otherwise, `Try again` retries playback for the current station.

### 6. Footer Player Controls

Always visible inside the player layout.

Controls:

- Play / stop / loading button.
- Volume icon.
- Volume range slider.
- Playback timer.
- Selected station title.
- GitHub profile link.

Behavior:

- Play button is disabled until a station is selected and audio is not loading.
- Icon changes:
  - Loading: loader spinner.
  - Playing: stop icon.
  - Idle: play icon.
- Volume slider updates audio volume live.
- Volume value is persisted in `localStorage`.
- Timer starts when playback starts and stops when playback stops.
- `Space` toggles playback when a station is selected.

## Functional Inventory

### Station Browsing

- Loads stations from `https://somafm.com/channels.json`.
- Normalizes station data into playable MP3 URL, PLS URL, songs URL, info URL, Twitter URL, route, listener count, favorite state, and active state.
- Displays stations in the drawer.
- Supports station deep links like `#/channel/groovesalad`.

### Search

- Located in the station drawer header.
- Placeholder: `Search the signal...`
- Searches by station title.
- Sanitizes punctuation and whitespace before filtering.

### Sorting

Sort options:

- Station Name
- Listeners Count
- Saved Favorites
- Music Genre

Sort order:

- Ascending icon: `ico-sort-asc`
- Descending icon: `ico-sort-desc`

Default app state:

- Sort parameter: `listeners`
- Sort order: `desc`

Persistence:

- Stored as `sorting_data` in `localStorage`.

### Playback

- Plays MP3 streams from `https://ice1.somafm.com/{id}-128-mp3`.
- Uses browser audio APIs through the app audio module.
- Audio can only be started from a user action.
- Playback state controls timer and visualizer behavior.

### Favorites

- Favorite button appears on the station detail panel and every station card.
- Inactive icon: `ico-favs-add`
- Active icon: `ico-favs-check`
- Favorites are stored as `favorites_data` in `localStorage`.
- Favorite stations can be exported as `somafm_favorites.m3u`.

### Recent Tracks

- Track metadata loads from `https://somafm.com/songs/{id}.json`.
- First song becomes the current track.
- Next 3 songs become recent tracks.
- Refreshes every 30 seconds.

### Visualizer

- Uses a full-player canvas.
- Built with Three.js and Web Audio frequency data.
- Canvas opacity is `0.55`.
- Canvas blend mode is `screen`.
- Animation pauses visual updates when the document is not visible.

### Keyboard Shortcuts

- `Space`: toggle playback when a station is selected.
- `Enter`: open station drawer.
- `Escape`: close station drawer.

## Component And Button Inventory

### Header Menu Button

Location: top-right header.

Icon:

- `ico-menu`

Purpose:

- Opens the station drawer.

Style:

- Circular icon button.
- Text color: `#f5f1ea`
- Hover color: `#d4a574`
- Hover glow: `0 0 18px rgba(212, 165, 116, 0.35)`

### Enter Stations CTA

Location: welcome state.

Icon:

- `ico-headphones`

Label:

- `Enter Stations`

Purpose:

- Opens the station drawer.

Style:

- Pill shape.
- Background: `linear-gradient(135deg, rgba(212, 165, 116, 0.95), rgba(255, 94, 94, 0.62))`
- Text: `#fff6e8`
- Border radius: `100px`
- Font: Space Grotesk
- Uppercase
- Hover moves up `-1px`.

### Play / Stop Button

Location: footer.

Icons:

- Idle: `ico-play`
- Playing: `ico-stop`
- Loading: `ico-loader` with spin animation

Purpose:

- Starts or stops current station stream.

Style:

- Circular button.
- Size: `2.2em x 2.2em`
- Border: `1px solid rgba(245, 241, 234, 0.12)`
- Background: `rgba(245, 241, 234, 0.06)`
- Outer glow: `0 0 0 6px rgba(212, 165, 116, 0.04)`
- Disabled state: opacity `0.5`, pointer events disabled.

### Volume Slider

Location: footer.

Icons:

- `ico-volume-4` for volume `>= 75`
- `ico-volume-3` for volume `>= 50`
- `ico-volume-2` for volume `>= 25`
- `ico-volume-1` for volume below `25`

Purpose:

- Adjusts player volume from `0` to `100`.

Style:

- Track height: `3px`
- Track color: `rgba(245, 241, 234, 0.12)`
- Thumb: `1em` circular
- Thumb color: `#d4a574`
- Hover thumb color: lighter projector gold.

### Favorite Button

Locations:

- Station detail panel.
- Each station drawer card.

Icons:

- Inactive: `ico-favs-add`
- Active: `ico-favs-check`

Optional label:

- `Favorite` on the station detail panel.

Purpose:

- Saves or removes a station from favorites.

Style:

- Icon/text button.
- Active icon color: `#d4a574`
- Hover color: `#d4a574`
- Focus uses text glow.

### Webpage Button

Location: station detail panel.

Icon:

- `ico-earth`

Label:

- `Webpage`

Purpose:

- Opens the SomaFM station page in a new tab.

Style:

- CTA pill button.

### PLS Button

Location: station detail panel.

Icon:

- `ico-download`

Label:

- `PLS`

Purpose:

- Opens/downloads the station PLS playlist.

Style:

- CTA pill button.

### Twitter Button

Location: station detail panel.

Icon:

- `ico-twitter`

Purpose:

- Opens station Twitter profile when provided by SomaFM.

Style:

- CTA pill icon button.

### Close Drawer Button

Location: station drawer header.

Icon:

- `ico-close`

Purpose:

- Closes station drawer.

Style:

- Icon button matching common button hover/focus behavior.

### Sort Order Toggle

Location: station drawer footer.

Icons:

- `ico-sort-desc`
- `ico-sort-asc`

Purpose:

- Toggles current sort order.

Style:

- Inline clickable icon.
- Uses bright/faded text context.

### Sort Popover Buttons

Location: station drawer footer.

Buttons:

- Station Name
- Listeners Count
- Saved Favorites
- Music Genre

Purpose:

- Changes station list sort parameter.

Style:

- Popover background: `#161616`
- Border: `1px solid rgba(245, 241, 234, 0.08)`
- Button padding: `0.5em 1em`
- Hover background: subtle dark overlay.

### Export Favorites Button

Location: station drawer footer.

Icon:

- `ico-download`

Purpose:

- Downloads all saved favorites as `somafm_favorites.m3u`.

Style:

- Simple icon button.

### GitHub Link Button

Location: footer right.

Icon:

- `ico-github`

Purpose:

- Opens GitHub profile.

Style:

- Common icon button with faded text.

### Try Again Button

Location: error state.

Icon:

- `ico-waveform`

Label:

- `Try again`

Purpose:

- Retries audio setup or current station playback.

Style:

- CTA pill button.

## Color Tokens

Primary palette:

| Token | Value | Usage |
| --- | --- | --- |
| Document Black | `#090909` | Body background, deepest surfaces |
| Panel Black | `#111111` | Panel base, drawer base |
| Dusk Black | `#050505`, `#0c0c0d`, `#15100d` | Body background gradient |
| Text Warm White | `#f5f1ea` | Primary text, headings, icons |
| Body Text | `#d8d2c8` | Main body copy |
| Muted Grey | `#8b8b8b` | Secondary text and default muted UI |
| Projector Gold | `#d4a574` | Primary accent, active states, highlights |
| Signal Red | `#ff5e5e` | Secondary accent and warm gradient stops |
| Border Black | `#1f1f1f` | Base borders |
| Overlay Black | `rgba(0, 0, 0, 0.72)` | Legacy overlay |
| Drawer Overlay | `rgba(0, 0, 0, 0.76)` | Visible station drawer overlay |

Transparent surface tokens:

| Token | Value | Usage |
| --- | --- | --- |
| Hairline Border | `rgba(245, 241, 234, 0.08)` | Panels, drawer, headers, cards |
| Strong Border | `rgba(245, 241, 234, 0.12)` | Main player control button |
| Glass Fill | `rgba(17, 17, 17, 0.58)` | Panels and cards |
| Header/Footer Fill | `rgba(6, 6, 6, 0.38)` | Top and bottom bars |
| Input Fill | `rgba(255, 255, 255, 0.04)` | Search input |
| Card Gradient Start | `rgba(255, 255, 255, 0.07)` | Glass cards |
| Card Gradient End | `rgba(255, 255, 255, 0.02)` | Glass cards |

## Typography

Font families:

| Role | Font |
| --- | --- |
| Headings | `Cormorant Garamond`, Georgia, serif |
| Body/UI | `Inter`, `Assistant`, sans-serif |
| Accent labels/buttons | `Space Grotesk`, `Inter`, sans-serif |
| Icons | `fontello` |

Heading styles:

- `h1`: `320%`, hero overrides to `clamp(3rem, 9vw, 6.7rem)`
- `h2`: `230%`
- `h3`: `180%`, station title overrides to `clamp(2.2rem, 5vw, 4.7rem)`
- `h5`: `130%`
- Heading line-height: `1.05em`
- Heading color: `#f5f1ea`

Body:

- Base font size starts near `15px` and scales up to `18px` at large screens.
- Body line-height: `1.4em`
- Body weight: `500`
- Body color: `#d8d2c8`

Kicker text:

- Font: Space Grotesk
- Size: `72%`
- Letter spacing: `0.18em`
- Uppercase

## Layout

### App Shell

- `.app-wrap` fills the viewport.
- Player is centered horizontally and vertically.
- Uses CSS custom property `--height` to handle mobile viewport height.

### Player Container

Mobile:

- Full viewport width.
- Full viewport height.
- No outer margin.

Desktop from `820px`:

- Horizontal margin: `2em`
- Max width: `1080px`
- Height: `calc(viewport height - 4em)`
- Max height: `700px`
- Border radius: `18px`
- Border: `1px solid rgba(245, 241, 234, 0.08)`
- Shadow: `0 30px 100px rgba(0, 0, 0, 0.82)`

### Header And Footer

- Height: `3.6em`
- Padding: `1em` horizontally, `0.75em` on very small screens.
- Glass background with `backdrop-filter: blur(22px)`.
- Header has bottom border.
- Footer has top border.

### Content Area

- Flexible middle region.
- Scrollable if content overflows.
- Padding: `1em` mobile.
- Padding: `2em` from `820px`.
- Custom thin scrollbars.

### Panels

Now Playing and Recent Tracks:

- Padding: `1em`
- Radius: `14px`
- Border: `1px solid rgba(245, 241, 234, 0.08)`
- Background glass gradient over panel black.
- Shadow: `0 24px 80px rgba(0, 0, 0, 0.34)`
- Backdrop blur: `24px`

### Cards

Track and station cards:

- Padding: `1em`
- Radius: source card uses `6px`; Nightwave station cards use `14px`.
- Border: warm-white hairline.
- Background: glass gradient with gold or red radial highlight.
- Hover lift: `translateY(-2px)` or `translateY(-3px) scale(1.01)`.

## Imagery And Visual Layers

Background image:

- `public/img/bg.jpg`
- Positioned bottom-right.
- Cover size.
- Opacity: `0.28`
- Filter: `saturate(0.65) contrast(1.15)`
- Scale: `1.05`

Noise/grid layers:

- Body grid: `42px x 42px`, opacity `0.18`.
- Player micro-grid: `3px x 3px`, opacity `0.18`.

Canvas visualizer:

- Absolute layer over background.
- Behind UI content.
- Opacity: `0.55`.
- Blend mode: `screen`.

Station artwork:

- Main station art: `104px x 104px`, radius `28px`, rotated `-2deg`.
- Drawer station art: `78px x 78px`, radius `18px`.
- Generic image styling includes border and shadow.

## Motion And Interaction

Transition timing:

- Default animation duration: `520ms`
- Ease: `cubic-bezier(0.19, 1, 0.22, 1)`

Animation classes:

- Fade in
- Fade out
- Drop in
- Zoom in
- Zoom out
- Slide left
- Slide right
- Slide up
- Slide down
- Spin right
- Spin left
- Pulse fade

Specific motion:

- Loading spinner rotates continuously.
- Loader icon spins during stream loading.
- Welcome and content elements enter with staggered delays.
- Now-playing glow breathes over `7s`.
- Track cards show a gold sweep on hover.
- Drawer slides in from the right.
- Popover scales/fades in.

## Responsive Behavior

Breakpoints:

| Name | Width |
| --- | --- |
| Small | `480px` |
| Medium | `820px` |
| Large | `1200px` |

Small screens:

- Player uses the full viewport.
- Drawer width is full viewport.
- Header/footer horizontal padding reduces to `0.75em`.
- Footer controls max width becomes `calc(100vw - 5em)`.
- Volume slider max width becomes `4.8em`.
- Station cards remove fixed minimum height.

Medium screens and above:

- Player becomes a centered framed surface.
- Player gets border radius, max dimensions, and shadow.
- Header tag `SomaFM after dark` becomes visible.
- Content padding increases.

## Accessibility Notes

Current strengths:

- Buttons use real `button` elements.
- Links use real `a` elements.
- Drawer can receive focus with `tabindex="-1"`.
- Station cards are keyboard-focusable with `tabindex="0"`.
- Buttons include useful `title` attributes in key places.
- Keyboard shortcuts exist for playback and drawer control.

Areas to improve:

- Add explicit `aria-label` values for icon-only buttons.
- Add `aria-expanded` to drawer open/close controls.
- Add `aria-pressed` to favorite and playback buttons.
- Add visible focus rings beyond glow-only text effects.
- Add `aria-current` or equivalent for active station.
- Consider `prefers-reduced-motion` support for visualizer and entry animations.

## Next UI Direction

Current patch direction:

CelluloidFM should become a black cinematic Apple Music / Linear / Arc / MUBI-style radio player with a huge centered album-cover now-playing window, modern ambient animations, station grid browsing, track info, YouTube discovery, and mood-driven themes.

Working label:

- `CelluloidFM - Cinematic SomaFM player`

One-line direction:

- Patch the current app into a cinematic Apple Music-style radio player with a huge centered album-cover module, Linear-style black panels, Arc-like ambient gradients, Letterboxd/MUBI-inspired restraint, mood-based visual themes, and a YouTube discovery link for the current track.

Important implementation rule:

- Do not rebuild the app for this patch.
- Patch the existing Vue/Webpack app in place.
- Use `CelluloidFM` as the product and app name.
- Treat `Nightwave` only as a legacy/current theme label unless intentionally reused as a visual mode.

### Current Patch Boundaries

Do not touch:

```txt
src/js/audio.js
src/js/soma.js
src/js/store.js
webpack.config.js
package.json
```

Only patch:

```txt
index.html
public/css/nightwave.css
src/scss/app.scss
src/app.js only if needed
```

### Current Target Layout

```text
 -----------------------------------------------------------------
| Sidebar |          Huge Center Now-Playing          | Right Rail |
|         |       Station artwork as hero cover       | Tracks     |
| Home    |       Station name / current track        | Station    |
| Search  |       Main actions / YouTube link         | Info       |
| Favs    |                                           | Recent     |
| Genres  |       Station Cards Grid below/around     |            |
 -----------------------------------------------------------------
|              Cinematic Persistent Bottom Player Bar              |
 -----------------------------------------------------------------
```

### Current Visual Style

Use a restrained black cinematic palette:

```scss
$bg: #050507;
$panel: #0d0d11;
$panel-soft: #111116;
$card: #17171d;
$card-hover: #202028;
$text: #f4f1ea;
$muted: #a7a29a;
$accent: #c084fc;
$accent-2: #f97316;
$accent-3: #ef4444;
$border: rgba(255,255,255,0.08);
```

Primary visual changes:

- Use black as the dominant app shell, with Cinematic Violet as the primary accent and Projector Amber as the secondary accent.
- Avoid Spotify green, full gold, and red-heavy palettes.
- Add ambient blurred gradient fields using violet, amber, and soft red.
- Keep the UI premium, cinematic, and music-focused rather than exact Spotify.
- Use Linear-like black panels and MUBI/Letterboxd restraint.
- Keep station browsing visible, but make now-playing the emotional center of the app.

### Current App Shell Target

Make the player feel like a real desktop music app:

```scss
.player-wrap,
.player {
  width: min(96vw, 1440px);
  height: min(92vh, 860px);
  background: #050507;
  border-radius: 28px;
  overflow: hidden;
  border: 1px solid rgba(255,255,255,0.08);
  box-shadow: 0 40px 140px rgba(0,0,0,0.9);
}
```

### Current Animated Background Target

Add a modern ambient background layer:

```scss
.player::before {
  content: "";
  position: absolute;
  inset: -30%;
  background:
    radial-gradient(circle at 20% 20%, rgba(192,132,252,0.26), transparent 30%),
    radial-gradient(circle at 80% 30%, rgba(239,68,68,0.16), transparent 28%),
    radial-gradient(circle at 50% 90%, rgba(249,115,22,0.16), transparent 35%);
  filter: blur(60px);
  animation: ambientDrift 18s ease-in-out infinite alternate;
  pointer-events: none;
}

@keyframes ambientDrift {
  from { transform: translate3d(-4%, -2%, 0) rotate(0deg); }
  to { transform: translate3d(4%, 3%, 0) rotate(8deg); }
}
```

### Current Now-Playing Hero Target

Make now-playing the main center hero instead of a secondary panel:

```scss
.now-playing-panel {
  min-height: 520px;
  display: grid;
  place-items: center;
  text-align: center;
  background:
    radial-gradient(circle at 52% 42%, rgba(192,132,252,0.22), transparent 40%),
    radial-gradient(circle at center, rgba(249,115,22,0.16), transparent 48%),
    linear-gradient(180deg, #111116, #050505);
  border-radius: 30px;
  padding: 3rem;
}
```

Hero contents:

- Huge station artwork.
- Station name.
- Current track title.
- Artist and album.
- DJ, genre, and listener count.
- Favorite button.
- Station webpage and PLS buttons.
- YouTube search link for current track.

### Current Huge Station Artwork Target

Use the station image as a dramatic album-cover style object:

```scss
.station-artwork-main {
  width: clamp(220px, 28vw, 380px);
  height: clamp(220px, 28vw, 380px);
  border-radius: 34px;
  object-fit: cover;
  box-shadow:
    0 40px 100px rgba(0,0,0,0.8),
    0 0 86px rgba(192,132,252,0.22),
    0 0 34px rgba(249,115,22,0.12);
  animation: coverFloat 7s ease-in-out infinite;
}

@keyframes coverFloat {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-12px) scale(1.025); }
}
```

### Current Modular Layout Target

Use a modular layout, but make it more cinematic than Spotify:

- Left sidebar: brand and primary navigation.
- Main center: huge now-playing cover module.
- Right rail: recent tracks and station info.
- Station grid: visible browsing surface, below or alongside the hero depending on screen size.
- Bottom player: persistent playback controls.

### Current Mood Theme Target

The `Moods` section in the sidebar should change visual themes, not only search/filter stations. Mood buttons should update a root-level theme class or CSS custom properties, while optionally applying a matching station search.

Sidebar structure:

```text
Home
Discover
Favorites
Genres

Moods
- Night Drive
- Deep Focus
- Lounge
- Ambient
- Weird Internet
```

Primary nav behavior:

- `Home`: reset search and sort by listener count.
- `Discover`: focus or clear station search.
- `Favorites`: sort or filter by saved favorites.
- `Genres`: sort or group by genre.
- Mood buttons: change theme and optionally set a matching station query.

Recommended implementation:

```html
<main id="player-wrap" class="player-wrap" :class="'theme-' + activeMood">
```

```js
data: {
  activeMood: 'night-drive'
}

methods: {
  setMood( mood, search = '' ) {
    this.activeMood = mood;
    this.searchText = search;
  }
}
```

Mood buttons:

```text
Night Drive
Deep Focus
Lounge
Ambient
Weird Internet
```

Mood behavior:

- `Night Drive`: violet/amber default theme, cinematic and fast.
- `Deep Focus`: blue/violet theme, calmer contrast, lower red.
- `Lounge`: amber/rose theme, warmer panels and softer highlights.
- `Ambient`: teal/violet theme, more spacious and low-intensity.
- `Weird Internet`: magenta/red/violet theme, stranger and more electric.

Theme token examples:

```css
.theme-night-drive {
  --accent: #c084fc;
  --accent-2: #f59e0b;
  --accent-3: #ef4444;
}

.theme-deep-focus {
  --accent: #818cf8;
  --accent-2: #38bdf8;
  --accent-3: #a78bfa;
}

.theme-lounge {
  --accent: #f59e0b;
  --accent-2: #fb7185;
  --accent-3: #c084fc;
}

.theme-ambient {
  --accent: #5eead4;
  --accent-2: #a78bfa;
  --accent-3: #38bdf8;
}

.theme-weird-internet {
  --accent: #e879f9;
  --accent-2: #ef4444;
  --accent-3: #8b5cf6;
}
```

Themeable surfaces:

- Ambient gradient background.
- Brand mark glow.
- Active nav item.
- Now-playing panel glow.
- Station artwork glow.
- Featured card gradient.
- Active station glow.
- Play button gradient.
- Slider thumb.
- Focus outline.

### Current Station Grid Target

Turn the station grid into a curated listening wall, not a uniform list.

Station cards should keep:

- Station art.
- Station title.
- Genre.
- Listener count.
- Favorite toggle.
- Active/playing state.

Add:

- One large `Featured Station` card.
- Smaller regular cards.
- Active station glow.
- Hover lift.
- Play button appearing on hover.

### Current Right Rail Target

Right rail should include:

- Recent Tracks list.
- Station description.
- DJ and genre.
- Listener count.
- External station actions if they do not fit in the center panel.

### Current YouTube Discovery Link Target

Add a simple YouTube search link using existing current track metadata. This avoids adding a YouTube API.

```html
<a
  class="youtube-link"
  :href="'https://www.youtube.com/results?search_query=' + encodeURIComponent(track.title + ' ' + track.artist)"
  target="_blank"
  rel="noopener"
>
  Search on YouTube
</a>
```

Implementation notes:

- The existing app uses `track`, not `currentTrack`.
- Guard the link so it appears only when a track title or artist exists.

### Current Button Aesthetic

Use glassy, restrained action buttons:

```scss
.action-button {
  border: 1px solid rgba(255,255,255,0.1);
  background: rgba(255,255,255,0.06);
  color: #f4f1ea;
  border-radius: 999px;
  padding: 0.8rem 1.2rem;
  backdrop-filter: blur(20px);
  transition: transform 220ms ease, background 220ms ease, box-shadow 220ms ease;
}

.action-button:hover {
  transform: translateY(-2px);
  background: rgba(192,132,252,0.14);
  box-shadow: 0 0 40px rgba(192,132,252,0.16);
}
```

### Current Patch Order

1. CSS only first: black shell, bigger player, ambient background.
2. Template second: move now-playing to the center.
3. YouTube link third: simple search URL.
4. Animations fourth: floating cover, glow, card transitions.
5. Refactor later only if required.

### Current Best First Version

First implementation should focus on:

- Bigger black desktop-app shell.
- Left sidebar.
- Huge centered station artwork.
- Center now-playing hero module.
- Right rail for recent tracks and station info.
- Mood sidebar where mood buttons change themes.
- Curated station grid browsing with one featured station.
- Persistent bottom playback bar.
- Cinematic violet and projector amber action system.
- Ambient animated gradients.
- YouTube discovery link.
- No rebuild and no backend changes.

### Superseded Spotify Direction

The section below is the earlier Spotify-style direction. Keep it as historical context only; the current patch direction above takes priority.

Target concept:

CelluloidFM should move toward a Spotify-like black modular music dashboard: bigger player, left navigation, main station grid, modular now-playing area, and a persistent bottom player bar.

Working label for the redesign:

- `CelluloidFM — Spotify-style SomaFM player`

Important naming rule:

- Use `CelluloidFM` as the product and app name.
- Treat `Nightwave` only as a legacy/current theme label unless intentionally reused as a visual mode.

### Target Layout

```text
 -------------------------------------------------
| Sidebar |              Main Area                |
|         |  Search / Sort                        |
| Home    |  Station Cards Grid                   |
| Search  |                                       |
| Favs    |  Big Now Playing Module               |
| Genres  |                                       |
 -------------------------------------------------
|        Large Spotify-like Bottom Player         |
 -------------------------------------------------
```

### Target Visual Style

Use a black Spotify-inspired modular palette:

```scss
$bg: #000000;
$panel: #121212;
$panel-soft: #181818;
$card: #1f1f1f;
$text: #ffffff;
$muted: #b3b3b3;
$accent: #1db954;
$border: rgba(255,255,255,0.08);
```

Primary visual changes:

- Replace warm cinematic gold/red emphasis with black, soft charcoal, white, muted grey, and Spotify green.
- Keep the app premium and music-focused, but make it more functional, modular, and dashboard-like.
- Reduce reliance on hidden drawer navigation.
- Prioritize station browsing as the main screen.

### Target Player Size

Current desktop player is approximately `1080px` wide and `700px` high. Increase the app frame:

```scss
.player {
  width: min(96vw, 1440px);
  height: min(92vh, 860px);
  max-width: 1440px;
  max-height: 860px;
  background: #000;
  border-radius: 18px;
}
```

### Target Left Sidebar

Replace the current top-heavy station drawer flow with persistent navigation:

```html
<aside class="sidebar">
  <h1 class="brand">CelluloidFM</h1>

  <button>Home</button>
  <button>Search</button>
  <button>Favorites</button>
  <button>Genres</button>
</aside>
```

Expected sidebar actions:

- Home: show default station dashboard.
- Search: focus station search.
- Favorites: filter or highlight favorite stations.
- Genres: expose genre grouping or filter controls.

### Target Station Grid

Convert the station drawer into the main browsing surface:

```html
<section class="station-grid">
  <article class="station-card" v-for="station in channels">
    <img :src="station.largeimage" />
    <h3>{{ station.title }}</h3>
    <p>{{ station.genre }}</p>
  </article>
</section>
```

Grid style:

```scss
.station-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(170px, 1fr));
  gap: 1.2rem;
}

.station-card {
  background: #181818;
  border-radius: 12px;
  padding: 1rem;
  transition: 0.2s ease;
}

.station-card:hover {
  background: #282828;
  transform: translateY(-3px);
}
```

Station cards should keep:

- Station art.
- Station title.
- Genre.
- Listener count.
- Favorite toggle.
- Active/playing state.

### Target Now Playing Module

Make now playing a large modular panel:

```scss
.now-playing-module {
  background: linear-gradient(180deg, #1f1f1f, #121212);
  border-radius: 22px;
  padding: 2rem;
  min-height: 320px;
  display: grid;
  grid-template-columns: 220px 1fr;
  gap: 2rem;
}
```

The module should include:

- Large station art.
- Station name.
- Genre.
- DJ.
- Station description.
- Favorite button.
- Current track.
- Recent tracks.
- Webpage and PLS actions.

### Target Bottom Player

Make the footer feel like a Spotify playback bar:

```scss
.bottom-player {
  height: 96px;
  background: #000;
  border-top: 1px solid rgba(255,255,255,0.08);
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  padding: 0 1.5rem;
}
```

Bottom player content:

- Left: station art, station title, current track or genre.
- Center: play/stop button and playback timer.
- Right: volume slider and favorite button.

### Target File Update Order

1. `index.html`: change layout structure.
2. `public/css/nightwave.css` or `src/scss/app.scss`: add the new black Spotify-style UI.
3. `src/app.js`: only minor state changes if needed.
4. Keep `audio.js`, `soma.js`, and `store.js` untouched.

### Best First Version

First implementation should focus on:

- Bigger black player window.
- Left sidebar.
- Station grid.
- Modular now-playing panel.
- Persistent bottom playback bar.
- Green/black accent system.
- No backend changes.

## UX Copy

Brand and header:

- Product name in UI: `CelluloidFM`
- Browser title target: `CelluloidFM SomaFM Player`
- Current theme label: `Nightwave`
- Current header tag: `SomaFM after dark`

Main copy:

- Welcome kicker: `Late-night radio room`
- Welcome heading: `Tune into the midnight signal.`
- Welcome body: `Browse SomaFM stations as moods, save the ones that fit, and let the visualizer turn the room into a soft cinematic glow.`
- Primary CTA: `Enter Stations`

Station drawer:

- Search placeholder: `Search the signal...`
- Sort label prefix: `Sort:`

Error:

- Heading: `Oops, there's a problem!`
- CTA: `Try again`

## Data And Persistence

External data:

- Channels: `https://somafm.com/channels.json`
- Songs: `https://somafm.com/songs/{id}.json`
- Streams: `https://ice1.somafm.com/{id}-128-mp3`
- Station page: `https://somafm.com/{id}/`
- PLS file: `https://api.somafm.com/{id}.pls`

Local storage:

- `player_volume`: saved volume value.
- `sorting_data`: station sort parameter and order.
- `favorites_data`: saved favorite station IDs.

Refresh behavior:

- Channels and songs refresh every 30 seconds.

## Source Files For UI Updates

| UI Area | File |
| --- | --- |
| App template and markup | `index.html` |
| Current visible Nightwave override | `public/css/nightwave.css` |
| Source Sass theme | `src/scss/app.scss` |
| Color, spacing, typography tokens | `src/scss/variables.scss` |
| Buttons, inputs, sliders | `src/scss/forms.scss` |
| Typography helpers | `src/scss/type.scss` |
| Cards and utility styles | `src/scss/common.scss` |
| Drawer popover | `src/scss/popover.scss` |
| Icon classes | `src/scss/icons.scss` |
| App behavior and state | `src/app.js` |
| Favorite button component | `src/js/favorite.js` |
| SomaFM API handling | `src/js/soma.js` |
| Audio engine | `src/js/audio.js` |
| Visualizer | `src/js/scene.js`, `src/js/sphere.js` |

## Figma Handoff Checklist

When updating the UI/UX in Figma and code, verify:

- Loading screen matches current brand and color palette.
- Empty state still has one obvious first action.
- Header menu button is visible on all screen sizes.
- Drawer search, close, sort, favorite, and export controls are easy to tap.
- Station cards include artwork, title, listeners, favorite, genre, and description.
- Active station is visually distinct.
- Now Playing panel clearly separates station identity from current track.
- Recent Tracks remains readable with long song, album, and artist names.
- Footer controls do not overlap on mobile.
- Volume slider remains usable on mobile.
- Error state gives a clear recovery action.
- Keyboard behavior still works.
- Focus states are visible.
- Color contrast is checked after every palette change.
- Motion is not excessive and has a reduced-motion plan if needed.

## Current Known Naming Note

The app should be named `CelluloidFM`. Existing `Nightwave` references should be updated or reframed as a theme name during the Spotify-style UI pass.
