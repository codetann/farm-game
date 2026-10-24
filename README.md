# Farm Game

A top-down farm game for kids, made for tablets. Each kid gets their own farm, taps to walk their character around, and taps animals to record a sound that the animal plays back from then on. Kids can visit each other's farms on the same device and listen to the sounds the others made.

Nothing is tied to one family. The first person to open the game on a device sets a parent PIN and adds their own kids. Profiles, farms and sounds are saved in that device's browser storage (IndexedDB), so there are no accounts and no server.

Built with [Phaser 3](https://phaser.io), TypeScript and Vite. Runs in Safari, Chrome and Edge on tablets, laptops and phones, and can be added to the home screen like an app.

## Getting started

You need [Node.js](https://nodejs.org) 20 or newer. This project uses [pnpm](https://pnpm.io); the exact version is pinned in `package.json`, and Corepack (included with Node) installs it for you:

```sh
corepack enable
pnpm install
```

Once the game is scaffolded (task T02), start the dev server with:

```sh
pnpm dev
```

## Project docs

- [TASKS.md](TASKS.md): the work broken into small tasks, one PR each. Claim a task there before starting it.
- Plan: the game design, tech stack and phases (shared separately).

## Working on a task

1. Pick an unclaimed task in `TASKS.md`, mark it `[~]` and add your name.
2. Create a branch named after it, for example `t05-load-sprites`.
3. Do only that task. If you use an AI coding assistant, give it the task block, `TASKS.md` and the plan as context.
4. Check the task's "Done when" line, mark it `[x]`, and open a PR.
