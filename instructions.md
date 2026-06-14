# Online Archery Gaming Website

**Category:** Web Development

---

## Context

This project is a browser-based archery shooting game built for a casual-gaming audience who want a quick, skill-based experience that runs instantly in any modern web browser with no install, no sign-up, and no plugins. It is being built as a self-contained MVP to validate the core "aim, draw, release, score" loop and to serve as a portfolio-grade showcase of HTML5 Canvas game development. The target player picks a difficulty, shoots a fixed number of arrows at a scoring target, and competes for a spot on a local high-score leaderboard.

This milestone IS a complete, playable, single-player web game that runs client-side only. It is NOT a multiplayer game, NOT an account-based platform, and NOT a backend service. The deployment target is a static web bundle (HTML/CSS/JS files) that runs by opening `index.html` in a desktop or mobile browser, or by serving the folder from any static host. The desktop baseline resolution is 1280×720 and the experience must remain fully playable down to a 320px-wide mobile viewport.

---

## Project Information

| Field | Details |
|---|---|
| Category | Web Development |
| Project Type | Browser-Based Archery Shooting Game (HTML5 Canvas) |
| Client / Brand | Online Archery Gaming Website |
| Platform / Medium | Web Browser (Desktop + Mobile Responsive) |
| Deliverable Type | Static web bundle (`.html` + `.css` + `.js` + assets) |
| Primary Goal | Deliver a fully playable, client-side archery game with scoring, difficulty levels, and a local leaderboard. |

---

## Main Goal

Build a complete, playable browser-based archery game using HTML5, CSS3, and Vanilla JavaScript (Canvas API), that loads and runs without any backend, database, or authentication. The player must be able to aim and fire arrows at a scoring target, earn points based on where each arrow lands, select from multiple difficulty levels, and see their best scores ranked on a leaderboard that persists in the browser. The deliverable is the following:

1. A playable archery shooting game rendered on an HTML5 `<canvas>` running at a target of 60 FPS.
2. A target scoring system where a bullseye hit awards the highest points and accuracy decreases value outward.
3. A difficulty selector with 3 difficulty levels that measurably change gameplay.
4. A leaderboard that records and displays the top 10 high scores using browser `localStorage`.
5. A medieval/nature-themed UI consistent across all screens.

---

## About Company

**Quivermark Studios** is a small, two-person independent web-game studio launching its first title, *Online Archery Gaming Website*, as a single-product casual gaming site. There is no pre-existing brand kit: no logo, no defined color palette, and no typography system are provided beyond the direction in this document. The freelancer is responsible for applying the medieval/nature theme defined in the **Visual & Technical Specs** and **Design Direction** sections consistently across the game.

**Brand tone:** medieval, natural, focused, skill-driven
**Brand line / tagline:** `Draw. Aim. Release.`

---

## Requirements

The freelancer will build a single-player, browser-based archery game that renders on an HTML5 canvas, scores each shot by landing zone, supports multiple difficulty levels, and stores high scores locally for a leaderboard.

### Core Gameplay

1. The game must render entirely inside a single HTML5 `<canvas>` element at a logical resolution of 1280×720 px, scaled responsively to the viewport.
2. The game loop must run using `requestAnimationFrame` at 60 FPS; on a 1280×720 desktop view in the latest Chrome, the measured frame rate must hold 60 FPS during active play.
3. The player must control an aim indicator (crosshair or trajectory line) using the mouse on desktop and touch on mobile.
4. A single round must consist of exactly 10 arrows and a per-difficulty countdown timer (Easy 60s, Medium 45s, Hard 30s) shown on screen; the round ends automatically after the 10th arrow lands or when the timer reaches 0.
5. Each arrow shot must be triggered by a single discrete input: mouse click / `mousedown`-then-`mouseup` (desktop) or `touchstart`-then-`touchend` (mobile).
6. After each arrow lands, the score for that arrow must be calculated and added to the running total within 100 ms, and the running total must be displayed on screen.
7. The arrow count remaining (e.g., `Arrows: 7 / 10`) must be visible on screen at all times during a round.
8. At the end of a round, a results screen must display the round score and the total score, prompt the player for 3-character initials, and offer a "Play Again" control and a "Home" control.

### Aiming & Shooting Mechanics

- The aim direction must be set by the pointer position relative to the bow/launch point; the rendered aim indicator must update every animation frame while aiming.
- Shot power must be controllable via a draw mechanic: holding the input fills a power meter from 0% to 100% linearly over 2 seconds; the meter holds at 100% (it does not cycle back to 0% and does not auto-fire), and releasing fires at the current power. The meter resets to 0% immediately after each shot.
- A visible power meter must render the current draw strength as a bar that fills 0% to 100% with a width of `200px` at desktop baseline.
- Arrow flight must be animated as a parabolic trajectory (constant downward gravity applied per frame); the arrow must visibly travel from the launch point to the target.
- Arrow physics baselines: initial arrow velocity is proportional to the power bar percentage at release, with a maximum of `15px per frame` at 100% power; gravity applies a constant downward acceleration of `0.3px per frame squared`; wind offset is applied horizontally each frame using the active difficulty's wind speed range. These values are baselines; minor tuning for game feel is permitted provided scoring and hit detection remain consistent.
- An arrow that lands outside all scoring zones must score 0 points and be recorded as a miss.

### Target & Scoring System

- The target must render as concentric circular scoring rings. There must be exactly 5 scoring zones from center outward.
- Scoring values, colors, and ring radii (at 1280×720 baseline), from the center (bullseye) outward, must be: **Bullseye = 10 pts** (Deep Red `#8B1A1A`, 20px radius), **Gold = 8 pts** (`#8B6914`, 40px radius), **Red = 6 pts** (Crimson Red `#C0392B`, 65px radius), **Blue = 4 pts** (Sky Blue `#87CEEB`, 95px radius), **Black = 2 pts** (`#000000`, 130px radius). A complete miss scores **0**. Radii scale proportionally when the canvas is resized below 1280×720.
- Hit detection must be based on the Euclidean distance from the arrow's landing point to the target center compared against each ring's radius.
- The bullseye (innermost zone) must be the smallest ring and award the highest value (10 points); it must never award fewer points than any outer ring.
- The maximum achievable single-round score must be `100` points (10 arrows × 10 points).
- Each landed arrow must remain visibly stuck in the target until the round ends, so the player can see their shot grouping.

### Difficulty Levels

- The game must provide exactly 3 selectable difficulty levels: **Easy**, **Medium**, **Hard**.
- Difficulty must be selectable from a start/menu screen before a round begins.
- Each level changes target scale, wind speed, and round timer:
  - **Easy:** 1.0× target scale (outer ring radius `130px`); wind `±1 to ±3 px per frame`; `60s` round timer.
  - **Medium:** 0.7× target scale (outer ring radius `91px`); wind `±4 to ±6 px per frame`; `45s` round timer.
  - **Hard:** 0.45× target scale (outer ring radius `58px`); wind `±7 to ±12 px per frame`, randomized each arrow; `30s` round timer.
- A wind indicator must be shown on screen and must measurably alter arrow trajectory using the active level's wind speed range.
- The active difficulty name must be displayed on screen during play and stored with each leaderboard entry.

### Leaderboard

- The leaderboard must store and display the top **10** scores, sorted in descending order by score.
- Leaderboard data must persist across browser sessions using `localStorage` (key must be namespaced, e.g., `archery_leaderboard`).
- Each leaderboard entry must store: player initials (3 characters), score (integer), and difficulty level (string).
- At the end of a round, if the score qualifies for the top 10, the player must be prompted on the Results Screen to enter their initials (3 characters).
- The leaderboard screen must include a "Back" control that returns to the Home Screen.
- If no scores exist, the leaderboard must display an empty-state message (e.g., `No scores yet. Be the first!`).

### Audio

- The game must include sound effects for the following six events: **bow draw**, **arrow release**, **bullseye hit**, **regular hit** (any non-bullseye scoring ring), **miss**, and **round complete**.
- Audio files must be `.mp3` or `.wav` (MP3 preferred) and must be free for commercial use.
- Sound files are bundled under `assets/sounds/`, follow the `ARCHERY_[asset-name].[ext]` naming convention, and every sound source and license is recorded in `assets/attribution.txt`.

> **Rule for this section:** Every requirement must be testable. Replace fuzzy words ("nice", "modern", "professional") with concrete values (pixel sizes, hex codes, frame rates, file formats, count of items).

---

## Visual & Technical Specs

### Dimensions / Sizing

- **Canvas / Logical size:** `1280×720 px` (desktop baseline)
- **Responsive minimum width:** `320px` viewport, fully playable
- **Safe zone / Live area:** All interactive UI (buttons, score, arrow count) must remain within the visible viewport at every supported width; no control may be clipped at 320px.
- **Resolution handling:** Canvas must scale to fit the viewport while preserving the 16:9 aspect ratio; account for `devicePixelRatio` so rendering is crisp on high-DPI screens.
- **Color mode:** sRGB

### Typography

| Usage | Font | Notes |
|---|---|---|
| Headings | Developer's choice | Title case; used for game title, screen headers; must fit the medieval/nature theme |
| Body / UI | Developer's choice | Score, arrow count, buttons, leaderboard rows; must fit the theme |
| Numerals (score) | Developer's choice | Tabular feel for score readouts |

**Fonts:** Typeface selection is at the developer's discretion to best fit the medieval and nature theme.

### Colors

| Purpose | Color Name | Hex |
|---|---|---|
| Background, primary | Forest Green | `#2D5A27` |
| UI panels, borders | Aged Wood | `#8B6914` |
| Text, labels | Cream Parchment | `#F5E6C8` |
| Bullseye, accents | Deep Red | `#8B1A1A` |
| Wind indicator, highlights | Sky Blue | `#87CEEB` |

Crimson Red `#C0392B` is approved exclusively for the Red ring on the target only. Pure Black `#000000` and Pure White `#FFFFFF` are permitted for contrast only.

**Color rules:**
1. Use only the five palette colors above plus pure white (`#FFFFFF`) and pure black (`#000000`) where needed for contrast.
2. No neon colors, no rainbow gradients; the bullseye uses Deep Red `#8B1A1A`, and Crimson Red `#C0392B` is used only for the Red ring on the target.
3. Body text must meet a minimum contrast ratio of 4.5:1 (WCAG AA).

---

## Technical Requirements

- **Engine / Framework:** None. Vanilla JavaScript only.
- **Language:** HTML5, CSS3, Vanilla JavaScript (ES6+) using the Canvas 2D API.
- **Platform / Target:** Web browser, desktop + mobile responsive ≥ `320px`.
- **Render Pipeline:** HTML5 `<canvas>` 2D context with `requestAnimationFrame` game loop.
- **Resolution / Target FPS:** `1280×720 @ 60 FPS` baseline (must hold 60 FPS in latest Chrome desktop).
- **Physics / Storage:** Custom lightweight 2D physics (gravity + velocity) in JavaScript; leaderboard persisted via `localStorage`. No external physics library.
- **Browser support:** Latest stable Chrome, Edge, Firefox, and Safari (desktop + mobile).
- **Code quality:** Commented, sensibly organized folders; no console errors or warnings during normal play.

### Allowed Tech Stack
1. HTML5 (semantic markup, single `<canvas>` for the game).
2. CSS3 (responsive layout via flexbox and/or media queries; transitions allowed).
3. Vanilla JavaScript (ES6+) with the Canvas 2D API and `localStorage`.

### Not Allowed
- Paid plugins, premium libraries, or licensed assets.
- JavaScript frameworks/libraries (React, Vue, jQuery, Phaser, p5.js, etc.); must be vanilla JS.
- Backend servers, databases, or server-side code.
- User authentication, login, or accounts.
- Multiplayer or AI opponents.
- AI-generated assets (all images and audio must be free for commercial use, sourced from human-made or licensed-free libraries).
- External network calls during gameplay (the game must run fully offline after load; Google Fonts may be self-hosted or `<link>`-embedded but the game must not break offline).

---

## Design Direction

**Do:**
- Apply a consistent medieval/nature theme: parchment backgrounds, forest greens, aged gold accents, wood/stone UI textures kept subtle and flat.
- Keep the UI uncluttered: clear score readout, clear arrow count, clearly labeled buttons.
- Use flat or lightly textured shapes drawn on canvas or styled in CSS; keep button corner radius consistent at `6px`.
- Ensure all controls have an obvious hover (desktop) and active (touch) state.

**Do not:**
- Use harsh neon colors, heavy drop shadows, or glossy 3D bevels.
- Add gore, blood, or violent imagery; keep it sport/target archery focused.
- Use anime, sci-fi, or modern-corporate styling that conflicts with the medieval/nature theme.
- Clutter the play area with more than the score, arrow count, power meter, difficulty label, and aim indicator.

---

## Reference Materials

| Asset | File |
|---|---|
| Game screen UI mockup | reference_image.png |

reference_image.png shows the Game Screen layout, element positions, color theme, and UI structure. Use it as the visual baseline for all 4 screens.

---

## Deliverables

| # | Item | Filename | Format | Notes |
|---|---|---|---|---|
| 1 | Main entry point | `index.html` | `.html` | Opens and runs the game |
| 2 | Styling | `style.css` | `.css` | All styling; theme and palette applied |
| 3 | Game logic | `game.js` | `.js` | All gameplay, scoring, leaderboard |
| 4 | Assets | `assets/` | folder | All images and sounds used |
| 5 | Documentation | `README.md` | `.md` | Setup and play instructions |

### File Naming Convention

All delivered files must follow this exact pattern:

```
ARCHERY_[item_name]_v1_[YYYYMMDD].[ext]
```

**Example:** `ARCHERY_game_logic_v1_20260614.js`

Rules:
- Use lowercase for the `[item_name]` segment.
- No spaces in file names.
- No alternate naming structures.
- Exception: `index.html`, `style.css`, `game.js`, and `README.md` keep their exact names.
- All asset files follow the `ARCHERY_` naming convention.

### Folder Structure

```
archery-game/
  index.html
  style.css
  game.js
  assets/
    images/
    sounds/
    attribution.txt
  README.md
```

---

## Mockup / Export Requirements

1. Minimum screenshot size: `1280 px` wide.
2. Color mode: sRGB.
3. JPG/PNG quality: PNG preferred; if JPG, 85% quality or higher.
4. Screenshots must show: the start/difficulty screen, an active round, and the leaderboard.
5. All UI text must remain fully readable; no cropping of the score, arrow count, or buttons.

---

## Print / Build / Export Specifications

- **Build:** Static web bundle: a single folder that runs by opening `index.html` in a browser, with no build step required.
- **Source files:** Human-readable, unminified `.html`, `.css`, and `.js` (commented). A minified copy is optional but the readable source is mandatory.
- **Offline requirement:** After first load, the game must run with no network connection; any fonts/assets must be local or gracefully fall back.

---

## Scope Boundaries

### DO
- Build a complete, playable single-player archery game that runs client-side.
- Implement the 5-zone scoring system, 3 difficulty levels, and a `localStorage` leaderboard.
- Use free placeholder or original assets where art is needed, all commercial-use-free.
- Make the game fully responsive and playable from 320px up to 1280×720 and wider.

### DO NOT
- Build multiplayer, online play, or real-time competition.
- Add AI opponents.
- Add ads, in-app purchases, or any monetization.
- Add a backend, database, server, cloud saves, or user accounts/authentication.
- Use paid or framework libraries; the game must be vanilla HTML/CSS/JS.
- Add violent or gore content beyond sport target archery.

---

## Tool Requirements

This task is largely tool-agnostic; any code editor is acceptable.

- **Required tools:** A plain text/code editor (e.g., VS Code) and a modern browser for testing.
- **Acceptable alternatives:** Any editor (Sublime, WebStorm, Notepad++); any static local server (e.g., `python -m http.server`, VS Code Live Server) for testing.
- **Forbidden:** Paid plugins, premium themes, no-code/site-builder exports, or framework CLIs that inject a build dependency.


---

## Acceptance Checklist

1. [ ] Game renders on an HTML5 `<canvas>` at a 1280×720 baseline and scales down to 320px width.
2. [ ] Game loop runs via `requestAnimationFrame` at 60 FPS on latest Chrome.
3. [ ] Bow charges by holding mouse or tap; power bar fills 0% to 100% and releases on input release.
4. [ ] Target has exactly 5 rings scoring 10, 8, 6, 4, 2; a miss scores 0; maximum round score is 100.
5. [ ] Bullseye is the smallest ring, colored Deep Red `#8B1A1A`, and awards 10 points.
6. [ ] Each round is exactly 10 arrows.
7. [ ] 3 difficulty levels exist: Easy (large target, slow wind, 60s), Medium (medium target, moderate wind, 45s), Hard (small target, strong random wind, 30s).
8. [ ] Wind indicator is shown on screen and measurably alters arrow trajectory.
9. [ ] Round ends after 10 arrows or when the timer reaches 0.
10. [ ] Leaderboard stores the top 10 scores in `localStorage`, sorted descending, and persists across page refresh.
11. [ ] Results Screen accepts a 3 character initials entry and offers Play Again and Home buttons.
12. [ ] All 4 screens (Home, Game, Results, Leaderboard) are reachable without a page reload.
13. [ ] Palette uses only the 5 defined hex values plus black and white; fonts are the developer's choice within the medieval and nature theme.
14. [ ] No gradients, no neon colors, no anime style.
15. [ ] No console errors on launch.
16. [ ] All files follow the `ARCHERY_[item_name]_v1_[YYYYMMDD].[ext]` naming convention.

---

## Evaluation Criteria

The project will be evaluated against the following points:

- **Gameplay correctness:** 10-arrow rounds, accurate distance-based hit detection, correct point values (10/8/6/4/2/0), max score of 100.
- **Performance:** Sustained 60 FPS at 1280×720 with smooth arrow animation.
- **Difficulty implementation:** All 3 levels present and each measurably distinct per the specified parameters.
- **Leaderboard:** Correct top-10 sorting, `localStorage` persistence, and complete entry data.
- **Responsiveness:** Fully playable and readable from 320px to the 1280×720 baseline and wider.
- **Theme & UI fidelity:** Correct palette (hex-accurate), developer's-choice fonts that fit the theme, uncluttered layout.
- **Code quality:** Readable, commented vanilla JS; sensible function naming and folder organization; no console errors.

---

## Developer's Choices

| Decision | Accepted Options |
|---|---|
| Aim mechanic style | Crosshair indicator, trajectory/arc preview line, or angle-based bow rotation |
| Draw/power input | Hold-to-charge meter, or click-drag-and-release slingshot style |
| Target art style | Flat vector rings drawn on canvas, or lightly textured ring image |
| Background scene | Forest clearing, archery range, or castle courtyard (within the palette) |
| Sound effect selection | Required (see **Audio** under Requirements): all six sound events must play. The specific sound files, sources, and any mute/volume control are the developer's choice, but every sound must be free for commercial use |
| Wind indicator visual | Arrow/flag/text readout (all levels) |


## Final Goal

Success is a player opening the website in any modern browser (on a desktop at 1280×720 or a 320px phone) and immediately understanding how to play: they pick a difficulty, draw the bow, aim, release, and watch their arrow arc into the target, earning 10 points for a dead-center bullseye and less toward the edges. After ten arrows they see their round and total score, enter their initials if they made the top ten, and find their place on a leaderboard that's still there the next time they return. The whole experience feels like a focused, handsome medieval archery range: fast, fair, and fun, running entirely in the browser with no install, no sign-up, and no server.
