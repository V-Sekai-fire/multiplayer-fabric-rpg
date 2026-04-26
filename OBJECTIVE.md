# Objective: multiplayer-fabric-rpg — Done Condition

## Scope

RPG game-logic layer. MMOG infrastructure lives in `multiplayer-fabric`; XR client lives in `multiplayer-fabric-xr-dev`.

**Win condition: the trained player bot, deployed via `artifacts-mmog`, achieves a meaningful goal in the live ArtifactsMMO game** (levels up a character, completes a resource loop, or clears a fight tier). Everything below is the path to that.

The local DM adversarial loop (GAN-style) is the training mechanism — a Dungeon Master bot injects obstacles into a synthetic world so the player bot learns robust replanning before it ever touches the real API.

## Pass conditions

| Capability | What it verifies |
|---|---|
| **Taskweft compiles** | `mix compile` in `multiplayer-fabric-taskweft` succeeds; C++20 NIF loads |
| **Property tests pass** | `mix test --include property` exits 0 |
| **Bot connects** | `artifacts_mmog.run` reaches the ArtifactsMMO REST API and receives character state |
| **HTN plan produced** | Domain JSON-LD → Taskweft `plan/1` returns a non-empty action sequence |
| **Episode executes** | Bot completes one goal loop (e.g. `fight_chickens`) without `:error` crash |
| **DM bot challenges** | DM bot injects an adversarial world-state mutation (blocked path, resource denial) mid-episode |
| **Player replans** | Player bot calls `replan/3`; produces a new valid sequence within the same episode |
| **Adversarial loop converges** | After N rounds neither bot trivially wins; DM difficulty and player success rate stabilise |
| **Domain transfers** | Trained player domain exports to `artifacts-mmog` without manual edits |
| **Live win** | Deployed bot achieves a real ArtifactsMMO goal (level-up, resource loop, or fight-tier clear) |

## Local training loop (no external API)

The DM and a local player bot run entirely in-process against a synthetic world
state — no ArtifactsMMO connection required. `multiplayer-fabric-dungeon-master`
owns the world simulation and both roles:

```
loop do
  dm_mutations  = DungeonMaster.plan(world_state)   # discriminator turn
  world_state   = World.apply(world_state, dm_mutations)
  player_plan   = Player.plan(world_state)           # generator turn
  world_state   = World.apply(world_state, player_plan)
  score         = World.score(world_state)           # +1 player win / -1 DM win
  DungeonMaster.update(score)
  Player.update(score)
end
```

`artifacts-mmog` is a deployment target for a trained player domain, not part
of the training loop.
