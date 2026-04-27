# Objective: multiplayer-fabric-rpg — Done Condition

## Scope

RPG game-logic layer. MMOG infrastructure lives in `multiplayer-fabric`; XR client lives in `multiplayer-fabric-xr-dev`.

**Win condition: the trained player bot, deployed via `artifacts-mmog`, achieves a meaningful goal in the live ArtifactsMMO game** (levels up a character, completes a resource loop, or clears a fight tier). Everything below is the path to that.

The local GEPA reflective cycle is the training mechanism — the bot self-critiques failed episodes, serializes failure context as Actionable Side Information (ASI), and a background Genetic-Pareto optimizer evolves its instruction set before it ever touches the real API.

## Pass conditions

| Capability               | What it verifies                                                                                                    |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **Property tests pass**  | `mix test --include property` exits 0                                                                               |
| **Bot connects**         | `artifacts_mmog.run` reaches the ArtifactsMMO REST API and receives character state                                 |
| **HTN plan produced**    | Domain JSON-LD → Taskweft `plan/1` returns a non-empty action sequence                                              |
| **Episode executes**     | Bot completes one goal loop (e.g. `fight_chickens`) without `:error` crash                                          |
| **GEPA cycle runs**      | Reflective cycle completes: episode executes → ASI serialized → self-critique generated → GEPA evolves instructions |
| **Instructions improve** | GEPA Pareto frontier advances across ≥2 metrics (success rate, plan length) after N episodes                        |
| **Domain transfers**     | Trained player domain exports to `artifacts-mmog` without manual edits                                              |
| **Live win**             | Deployed bot achieves a real ArtifactsMMO goal (level-up, resource loop, or fight-tier clear)                       |
