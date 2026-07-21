# RestGBA

A headless Game Boy Advance emulator with a REST API, built on [gbajs](https://github.com/endrift/gbajs). It runs a ROM in the background and exposes HTTP endpoints to send button inputs, grab screenshots, and manage save data — making it easy to drive a GBA game programmatically.

This was built to power the [**Twitter Plays Pokemon**](https://github.com/lucy-dot-exe/TwitterPlaysPokemon) project, where tweeted commands were translated into button presses sent to this server, letting Twitter collectively play Pokemon LeafGreen.

## How it works

On startup, the server loads the GBA BIOS and a ROM (`rom/PokemonLeafGreen.gba`) and starts emulation. A single Express endpoint, `POST /execute`, accepts a JSON command and applies it to the running emulator.

### `POST /execute`

Request body:

```json
{ "command": "A" }
```

Supported commands:

| Command | Effect |
| --- | --- |
| `START`, `SELECT`, `UP`, `DOWN`, `LEFT`, `RIGHT`, `A`, `B`, `L`, `R` | Presses the corresponding GBA button |
| `SCREENSHOT` | Returns a PNG image of the current screen |
| `LOAD_SAVE` | Loads save data from `save/game.sav` |
| `DOWNLOAD_SAVE` | Writes current save data to `save/game.sav` |
| `RESET` | Resets the emulator |

## Requirements

- A GBA ROM placed at `rom/PokemonLeafGreen.gba`
- Node.js and Yarn, or Docker

## Running locally

```bash
yarn install
yarn build
yarn gba
```

The server listens on port `3535`.

## Running with Docker

```bash
docker-compose up --build
```

`docker-compose.yml` mounts local `rom/` and `save/` directories into the container, so the ROM and save file live outside the image.

## Project structure

- [src/gba.ts](src/gba.ts) — emulator setup and Express server
- `rom/` — GBA ROM file (not included)
- `save/` — persisted save data
