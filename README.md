# Phantom Slice Game

A Fruit Ninja style hand tracking game with a neon cybercore aesthetic. Swipe your index fingertip through fruit to score points, avoid the bombs, and climb the leaderboard.

**Live demo:** https://rafiaauthoi.github.io/phantom-slice/

## What it does

- **Name entry**: each session starts with a name, no account, so different people can take turns at a booth.
- **18 fruits**: melon, orange, peach, dragonfruit, grape, papaya, pineapple, banana, berry, coconut, cherry, lemon lime, watermelon, apple, blueberry, kiwi, pear, and strawberry, each launched upward from the bottom of the screen and arcing back down under real gravity.
- **Bombs**: dark, pulsing hazard spheres with a lit fuse, mixed in randomly. Slicing one costs a life and triggers an explosion sound and screen flash. Bombs require a more precise, deliberate swipe to slice than fruit does, so a fast pass nearby won't accidentally cost a life.
- **Blade trail slicing**: the index fingertip leaves a glowing trail, and any fruit or bomb that trail crosses gets sliced, no precise timing needed, just swipe through it.
- **Combo scoring**: slicing multiple fruit in quick succession chains a combo, with bonus points and an on-screen flash.
- **Bigger fruit, bigger reward**: melon, papaya, pineapple, coconut, and watermelon are worth 20 points; most others are worth 10 to 15.
- **Lives**: 3 lives, lost by letting a fruit fall past the bottom of the screen or by slicing a bomb. Losing all 3 ends the round and shows a final score screen.
- **Leaderboard**: tracks each player's best score locally in the browser, so people can compete against each other and their own record throughout the day, with a reset button (protected by a confirmation prompt) to clear it.
- **Real audio**: an actual slice sound effect plays on every successful fruit slice, a layered explosion sound (synthesized, no file needed) plays on bombs, and a calm background music track loops quietly throughout.

## How to run it

**Option 1: Use the live version**
Open https://rafiaauthoi.github.io/gesture-slice-game/ in a browser. No setup needed.

**Option 2: Run it locally**
1. Clone this repo.
2. Open the folder in VS Code.
3. Right click `index.html` and choose "Open with Live Server" (or open the file directly in a browser).
4. Enter a name, click "Let's Play."
5. Allow camera access when prompted.
6. Wait through the 3-2-1 countdown, then swipe your index fingertip through the falling fruit.

No installs, no build step, no paid services. Everything runs client side using MediaPipe for hand tracking and Tone.js for audio, both loaded from public CDNs.

## Tools used

- MediaPipe Tasks Vision (hand landmark detection)
- Canvas API (all visual rendering, including fruit, bombs, blade trail, slice effects, and HUD)
- Tone.js (explosion sound synthesis, combo chime, game-over jingle, and playback of the real slice and music audio files)
- Browser `localStorage` (leaderboard persistence, local to whichever browser is running the page)

## Assets

- Fruit sprites: [Fruit Icon Pack (16x16)](https://scrimsy.itch.io/fruit-icons-16x16) by scrimsy, licensed CC BY-SA 4.0.
- Slice and background music audio: sourced separately as free-to-use audio files (see `assets/sounds/`).
- The bomb icon and heart/life icons are drawn entirely in code (a hand-defined pixel grid), no image files needed for either.

## How slicing works

Every frame, the fingertip's current position and previous position form a short line segment. Every active fruit or bomb on screen is checked against that segment using point-to-segment distance, if the object is close enough to the swipe path, it counts as sliced. This is far more forgiving than checking only the exact current fingertip position, since it accounts for the fast motion happening between frames, matching how a real swipe feels. Bombs use a tighter detection radius than fruit specifically to require a more deliberate hit.

## Customizing fruit and difficulty

Near the top of the script:

```javascript
const GRAVITY = 0.35;
const STARTING_LIVES = 3;
const SPAWN_INTERVAL_MS = 1050;
const BOMB_CHANCE = 0.18;
```

Lower `SPAWN_INTERVAL_MS` for a faster, harder game. Raise `BOMB_CHANCE` for more hazards. The `FRUIT_TYPES` array further down lists each fruit's radius, colors, and point value, add a new object to that array to add a new fruit type entirely (drop a matching PNG into `assets/fruits/`, or it'll automatically fall back to a plain glowing circle in that fruit's color if the file is missing).

## Notes for tabling setup

- A real external speaker is recommended so the slice and explosion sound effects can actually be heard over festival noise.
- Works best with a clear, mostly static background so hand detection stays reliable.
- The bordered display box scales with the browser window rather than a fixed pixel size, adjust `width` and `max-width` on `#video-wrapper` in the CSS to fit whatever monitor is used at the event.
- Run the browser in fullscreen (F11) for the actual showcase.
- The leaderboard lives in that specific browser's storage. Avoid clearing browsing data mid-event.
- The console currently logs a line every time a life is lost, useful for debugging but safe to leave in for the event since it's invisible to anyone not looking at developer tools.

## Status

Complete and deployed.