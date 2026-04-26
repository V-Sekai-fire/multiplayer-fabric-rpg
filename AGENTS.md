# multiplayer-fabric-rpg

Focused monorepo for the VR-MMOG-RPG game layer.

## Submodules

| Path | Branch | Purpose |
|---|---|---|
| `multiplayer-fabric-artifacts-mmog` | `main` | Elixir HTN-planning bot for ArtifactsMMO game |
| `multiplayer-fabric-taskweft` | `main` | Taskweft library — HTN domain/task graph shared dep |

## Setup

```sh
git clone --recurse-submodules https://github.com/V-Sekai-fire/multiplayer-fabric-rpg.git
cd multiplayer-fabric-rpg
git submodule update --init --recursive
```

## Monorepo split

| Concern | Repo |
|---|---|
| MMOG infrastructure (zones, interest management, networking) | `multiplayer-fabric` |
| XR client (Godot engine, interaction system, XR simulator) | `multiplayer-fabric-xr-dev` |
| RPG game layer (HTN planning bot, taskweft, game logic) | `multiplayer-fabric-rpg` (this repo) |
