# Gesture Slice Game

A Fruit Ninja style hand tracking game. Swipe your index fingertip through glowing neon fruit to score points, avoid the bombs, and climb the leaderboard.

## What it does

- **Name entry**: each session starts with a name, no account, so different people can take turns at a booth.
- **Neon fruit orbs**: apple, orange, watermelon, and kiwi, each a distinct glowing color with a faceted accent detail, launched upward from the bottom of the screen and arcing back down under gravity.
- **Bombs**: dark, pulsing hazard spheres mixed in randomly, slicing one costs a life.
- **Blade trail slicing**: your index fingertip leaves a glowing trail, and any fruit or bomb that trail crosses gets sliced, no precise timing needed, just swipe through it.
- **Combo scoring**: slicing multiple fruit in quick succession chains a combo, with bonus points and an on-screen flash.
- **Lives**: 3 lives, lost by missing a fruit or slicing a bomb. Losing all 3 ends the round.
- **Leaderboard**: tracks each player's best score locally in the browser, so people can compete against each other and their own record throughout the day.

## How to run it

1. Clone this repo.
2. Open the folder in VS Code.
3. Right click `index.html` and choose "Open with Live Server" (or open the file directly in a browser).
4. Enter a name, click "Let's Play."
5. Allow camera access when prompted.
6. Wait for the 3-2-1 countdown, then swipe your index fingertip through the falling fruit.

No installs, no build step, no paid services. Everything runs client side using MediaPipe for hand tracking and Tone.js for sound effects, both loaded from public CDNs.

## Tools used

- MediaPipe Tasks Vision (hand landmark detection)
- Canvas API (all visual rendering, including fruit, bombs, blade trail, slice effects, and HUD)
- Tone.js (slice chime, bomb explosion, combo chord, and game-over jingle)
- Browser `localStorage` (leaderboard persistence, local to whichever browser is running the page)

## How slicing works

Every frame, the fingertip's current position and previous position form a short line segment. Every active fruit or bomb on screen is checked against that segment using point-to-segment distance, if the object is close enough to the swipe path, it counts as sliced. This is far more forgiving than checking only the exact current fingertip position, since it accounts for the fast motion happening between frames, matching how a real swipe feels.

## Customizing fruit and difficulty

Near the top of the script:

```javascript
const GRAVITY = 0.35;
const STARTING_LIVES = 3;
const SPAWN_INTERVAL_MS = 1050;
const BOMB_CHANCE = 0.18;
```

Lower `SPAWN_INTERVAL_MS` for a faster, harder game. Raise `BOMB_CHANCE` for more hazards. `FRUIT_TYPES` further down lists each fruit's radius, colors, and point value, add a new object to that array to add a new fruit type entirely.

## Notes for tabling setup

- A real external speaker is recommended so the slice and bomb sound effects can actually be heard over festival noise.
- Works best with a clear, mostly static background so hand detection stays reliable.
- The bordered display box scales with the browser window rather than a fixed pixel size, adjust `width` and `max-width` on `#video-wrapper` in the CSS to fit whatever monitor is used at the event.
- Run the browser in fullscreen (F11) for the actual showcase.
- The leaderboard lives in that specific browser's storage. Avoid clearing browsing data mid-event.

## Status

Complete.
