# Farm Game: Tasks

Plan: https://claude.ai/artifact/76JBiE6mQPJSX6tEADnPhg

Each task is meant to be one small change: one branch, one PR, roughly one AI session. Do them roughly in order within a phase; `Needs` lists the tasks that must be done first.

## How we use this file

- **Claim a task:** change `[ ]` to `[~]` and replace `Owner: —` with your name. Commit that change by itself so everyone sees it.
- **Finish a task:** change it to `[x]` in the same PR as the work.
- **Branch name:** `t05-load-sprites` (task id + short name).
- **Done when** is the test. If it isn't true yet, the task isn't done.
- **Type** says what kind of work it is: `code`, `art` (sprites, tiles, maps) or `testing` (on real devices). Anyone can take any task.
- **Using an AI coding assistant** (any tool works): give it the task block, this file and the plan as context, and ask it to do only that task.

## Ground rules (apply to every task)

- Nothing is hard-coded to one family. Profiles, farms and sounds are created in the app and stored in that device's IndexedDB.
- Must work in Safari, Chrome and Edge on tablets, laptops and phones, with touch or mouse.
- Touch targets are at least 64px. Use icons over words, because kids may not read yet.
- Pixel art is drawn at 32px and scaled by whole numbers (Phaser `pixelArt: true`).
- Game logic that doesn't draw anything (pathfinding, storage, models) lives in `src/lib/` and has unit tests.

---

## Phase 0: Setup

- [x] **T01 Fix README** · Type: code · Owner: Tyler · Needs: none
  Rename `READEME.md` to `README.md`. Write what the game is, how to install, and how to run it.
  Done when: README explains `pnpm install` and `pnpm dev`.

- [ ] **T02 Scaffold Vite + TypeScript + Phaser 3** · Type: code · Owner: — · Needs: T01
  Set up a Vite TypeScript project (keep the `packageManager` field) and add `phaser`. One empty scene with a grass-green background.
  Done when: `pnpm dev` shows the green screen in a browser and `pnpm build` passes.

- [ ] **T03 Lint, format, tests** · Type: code · Owner: — · Needs: T02
  Add ESLint, Prettier and Vitest, plus `pnpm lint`, `pnpm format` and `pnpm test`. One sample test.
  Done when: all three scripts pass.

- [ ] **T04 Game config and scene list** · Type: code · Owner: — · Needs: T02
  Phaser config: `pixelArt: true`, scale mode that fits any screen and handles rotation. Empty scenes: `Boot`, `Preload`, `ProfileSelect`, `Farm`, `Ui`.
  Done when: the app moves Boot → Preload → ProfileSelect and fills the screen on a phone, tablet and laptop.

- [ ] **T05 Load the animal sprites** · Type: code · Owner: — · Needs: T04
  Copy `sprites/hybrid/farm_animals_hybrid_32px.png` and its atlas into `public/assets/sprites/`. Convert the atlas to Phaser's format if needed. Preload loads it.
  Done when: all 16 animals show in a test grid at 3x with crisp edges.

- [ ] **T06 Run on a tablet over Wi-Fi** · Type: code · Owner: — · Needs: T02
  `vite --host` with local HTTPS (`@vitejs/plugin-basic-ssl`), because the microphone only works over HTTPS. Write the steps in the README.
  Done when: the dev server opens on a tablet on the same Wi-Fi and sees the game.

- [ ] **T07 CI** · Type: code · Owner: — · Needs: T03
  GitHub Actions runs lint, tests and build on every PR.
  Done when: a PR shows a green check.

- [ ] **T08 Deploy to GitHub Pages** · Type: code · Owner: — · Needs: T07
  Build and publish `main` to GitHub Pages so anyone can open one hosted copy.
  Done when: the public URL loads the game on the iPad.

## Phase 1: Walkable farm

- [ ] **T10 Farm tileset art** · Type: art · Owner: — · Needs: none
  32px tiles in the hybrid style: grass (2–3 variants), dirt path, fence pieces, water edges, tree, bush, flowers, rocks. One PNG sheet.
  Done when: the sheet is in `public/assets/tiles/` and loads in Tiled.

- [ ] **T11 First farm map in Tiled** · Type: art · Owner: — · Needs: T10
  About 30×20 tiles. Layers: `ground`, `obstacles` (fences, water, trees), `decor`. An object layer marks the player start, animal spawn spots and the visit signpost.
  Done when: the map JSON is in `public/assets/maps/farm.json`.

- [ ] **T12 Render the map** · Type: code · Owner: — · Needs: T05, T11
  Farm scene loads and draws the Tiled map, and builds a walkable/blocked grid from `obstacles`.
  Done when: the farm shows on screen and the grid matches the obstacles (debug overlay toggle).

- [ ] **T13 Player character art** · Type: art · Owner: — · Needs: none
  A kid character in 4 directions with 2–4 walk frames each and one idle frame. Two or three colour variants so each kid can pick a look.
  Done when: the sheet and atlas are in `public/assets/sprites/`.

- [ ] **T14 Pathfinding module** · Type: code · Owner: — · Needs: T03
  `src/lib/pathfinding.ts` wraps easystar.js: given a grid, a start and a goal, return a list of tiles or "no path". Tapping a blocked tile goes to the nearest open one.
  Done when: unit tests cover a straight path, a path around a wall, and no path.

- [ ] **T15 Tap to choose a destination** · Type: code · Owner: — · Needs: T12
  Convert a tap or click to a tile. Show a small marker where they tapped.
  Done when: tapping anywhere on the farm logs the right tile and shows the marker.

- [ ] **T16 Player walks the path** · Type: code · Owner: — · Needs: T13, T14, T15
  Player entity moves tile to tile along the path at a steady speed and plays the matching walk animation. A new tap mid-walk changes course.
  Done when: tapping around the farm walks the player around fences and trees.

- [ ] **T17 Camera** · Type: code · Owner: — · Needs: T16
  Camera follows the player and stops at the map edges. Works when the screen rotates.
  Done when: walking to each corner of the map keeps the player on screen with no black edges.

## Phase 2: Animals and sounds

- [ ] **T20 Data models** · Type: code · Owner: — · Needs: T03
  TypeScript types for `Profile`, `Farm`, `Animal`, `Sound` in `src/lib/models.ts`, with ids and a version number for future changes.
  Done when: the types compile and match the plan's data table.

- [ ] **T21 Storage module** · Type: code · Owner: — · Needs: T20
  `src/lib/storage.ts` on IndexedDB (the `idb` library): save, load and delete profiles, farms, animals and sounds. Ask for persistent storage on first run.
  Done when: unit tests pass using `fake-indexeddb`.

- [ ] **T22 Animal entity** · Type: code · Owner: — · Needs: T05, T12
  An animal sprite that bobs while idle and wanders a few tiles from home, never onto blocked tiles.
  Done when: one animal wanders on the farm without walking through fences.

- [ ] **T23 Starter animals** · Type: code · Owner: — · Needs: T22
  New farms start with a few animals placed at the map's spawn spots, from a simple config list.
  Done when: a fresh farm shows the starter animals.

- [ ] **T24 Tap an animal** · Type: code · Owner: — · Needs: T16, T22
  Tapping an animal walks the player next to it, then the animal stops and faces the player and opens its card.
  Done when: tapping an animal from across the farm ends with the card open.

- [ ] **T25 Animal card UI** · Type: code · Owner: — · Needs: T24
  A big overlay card: the animal's picture, Record, Play (greyed out if no sound) and Close. Icons only, large buttons.
  Done when: the card opens and closes and works with touch and mouse.

- [ ] **T26 Recorder module** · Type: code · Owner: — · Needs: T03
  `src/lib/recorder.ts`: ask for the mic, record with MediaRecorder (pick `audio/mp4` on Safari, `audio/webm` elsewhere), stop at 5 seconds, return the clip.
  Done when: it records and plays back in Safari and Chrome, and handles "mic denied" and "not supported".

- [ ] **T27 Record flow** · Type: code · Owner: — · Needs: T21, T25, T26
  Record button shows a countdown ring, saves the clip with that animal, and Play replays it. Re-recording replaces the old clip.
  Done when: a recorded sound is still there after reloading the page.

- [ ] **T28 Animal plays its sound** · Type: code · Owner: — · Needs: T27
  Tapping an animal that has a sound plays it with a little bounce. Unlock audio on the first tap (needed on iPad).
  Done when: on the iPad, tapping an animal plays its sound every time.

## Phase 3: Profiles for any family

- [ ] **T30 Profile functions** · Type: code · Owner: — · Needs: T21
  Create, list, rename and delete profiles. Deleting a profile deletes its farm and sounds.
  Done when: unit tests cover all four.

- [ ] **T31 PIN pad** · Type: code · Owner: — · Needs: T04
  A reusable number pad for a 4-digit PIN. Store only a hash of the PIN.
  Done when: entering the right PIN passes and a wrong one shakes and clears.

- [ ] **T32 First run** · Type: code · Owner: — · Needs: T31
  On a device with no data: a welcome screen, then "Grown-up: set a PIN", then add the first kid.
  Done when: clearing site data and reopening walks through first run.

- [ ] **T33 Avatar art** · Type: art · Owner: — · Needs: none
  8–12 big, friendly profile pictures (animal faces work well) at 64 or 96px.
  Done when: the sheet is in `public/assets/sprites/`.

- [ ] **T34 Add a kid** · Type: code · Owner: — · Needs: T30, T31, T33
  Behind the PIN: type a name, pick a picture, pick a character colour. Creates the profile and a fresh farm from the template map.
  Done when: two profiles can be added and each has its own farm.

- [ ] **T35 Profile select screen** · Type: code · Owner: — · Needs: T34
  One big picture per profile on this device, plus a small grown-up button. Tap a picture to load that kid's farm.
  Done when: switching between profiles shows each kid's own animals and sounds.

- [ ] **T36 Save the farm automatically** · Type: code · Owner: — · Needs: T35
  Save the player position and animals when things change and when the app is hidden.
  Done when: closing and reopening returns the kid to where they were.

- [ ] **T37 Home button** · Type: code · Owner: — · Needs: T35
  A house icon on the farm returns to the profile select screen.
  Done when: it saves and goes back.

- [ ] **T38 Grown-up settings** · Type: code · Owner: — · Needs: T35
  Behind the PIN: rename or delete a profile, clear an animal's sound, reset a farm, change the PIN.
  Done when: each action works and asks "are you sure?" first.

## Phase 4: Visiting

- [ ] **T40 Visit signpost** · Type: code · Owner: — · Needs: T35
  Tapping the signpost shows the other profiles on this device.
  Done when: the list shows everyone except the current kid.

- [ ] **T41 Visit a farm** · Type: code · Owner: — · Needs: T40
  Load the other kid's farm in visitor mode, with the visitor's own character walking around.
  Done when: Kid A can walk around Kid B's farm.

- [ ] **T42 Listen only** · Type: code · Owner: — · Needs: T41
  In visitor mode the animal card shows only Play, so visitors can't change sounds.
  Done when: a visitor can hear sounds but sees no Record button.

- [ ] **T43 Go home** · Type: code · Owner: — · Needs: T41
  A button (or the signpost) takes the visitor back to their own farm.
  Done when: the visitor returns home and their own farm is unchanged.

## Phase 5: Install and release

- [ ] **T50 App manifest and icons** · Type: code + art · Owner: — · Needs: T08
  Web manifest, app name, theme colour and home-screen icons.
  Done when: "Add to Home Screen" on the iPad shows the icon and opens full screen.

- [ ] **T51 Works offline** · Type: code · Owner: — · Needs: T50
  Service worker (`vite-plugin-pwa`) caches the game so it opens with no internet.
  Done when: the installed app opens in airplane mode.

- [ ] **T52 iPad test pass** · Type: testing · Owner: — · Needs: T51
  On the iPad home-screen app, check: make two profiles, record, reload, visit, rotate the screen, and use the mic in full-screen mode.
  Done when: bugs found are added below as new tasks.

- [ ] **T53 Other device test pass** · Type: testing · Owner: — · Needs: T51
  Same checklist on an Android tablet or a laptop.
  Done when: bugs found are added below as new tasks.

- [ ] **T54 Install guide for other families** · Type: code · Owner: — · Needs: T51
  A short README section: open the URL, add to home screen, set a PIN, add your kids.
  Done when: the friend can set up his kids using only the guide.

## Later (not sized yet)

- Kids place, move and name their own animals.
- Back up or move a farm to another device (export and import a file).
- More maps, decorations, a barn and house.
- Background music, day and night.
- Second animation frame for each animal.

## Found along the way

Add new tasks here with the next free id in their phase.
