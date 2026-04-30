# Adaptive Stealth AI in UE5: Inspired by Alien: Isolation

A Unreal Engine 5 project implementing an adaptive stealth AI system inspired by the creature AI in *Alien: Isolation*, built for the MED6 course at Aalborg University Copenhagen.

## Overview

The project recreates key elements of Alien: Isolation's Xenomorph behavior using UE5's built-in AI tools: an AI character that dynamically switches between patrol, investigation, and chase states based on sensory input (sight and hearing), navigating the environment via NavMesh pathfinding.

## Features

- **Behavior Tree** — modular, hierarchical decision-making with Roaming, Investigation, and Chase sequences driven by a top-level Selector node
- **Blackboard** — shared data store for AI state (`TargetActor`, `LastKnownLocation`, `TargetLocation`) used by tasks and decorators
- **AI Perception System** — `AISense_Sight` and `AISense_Hearing` configured with sight radius, peripheral angle, and hearing range; updates blackboard keys via `OnTargetPerceptionUpdated()`
- **NavMesh pathfinding** — A\* algorithm via UE5's Recast Navigation Mesh; single `NavMeshBoundsVolume` defines walkable corridors

## Behavior Tree Structure

```
[Root]
└── Selector
    ├── Roaming Sequence        (if TargetActor is NOT SET)
    │   └── Selector
    │       ├── Investigation Sequence  (if TargetLocation IS SET)
    │       │   ├── Move To TargetLocation
    │       │   ├── Wait 3 seconds
    │       │   └── Clear TargetLocation
    │       └── Move Randomly           (fallback)
    └── Chase Selector          (if TargetActor IS SET and within scale)
        ├── Sequence            (if TargetActor IS SET)
        │   └── Move To TargetActor
        └── Move To LastKnownLocation   (fallback)
```

## How It Works

1. **Sight** — when the player enters the AI's line of sight, `HandleSightSense` sets `TargetActor` on the blackboard, triggering the Chase sequence.
2. **Hearing** — when the player makes noise without being visible, `HandleSoundSense` sets `LastKnownLocation`, triggering Investigation.
3. **Patrol** — if no stimulus is active, the AI roams randomly within the NavMesh.

## Built With

- Unreal Engine 5
- Blueprints (AI Controller, Behavior Tree, Blackboard, Tasks)
- AI Perception System (`AISense_Sight`, `AISense_Hearing`)
- NavMesh / Recast Navigation

## Author

Steffen Møgelmose — Aalborg University Copenhagen (smogel22@student.aau.dk)
