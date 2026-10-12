# Planned architecture

This document describes the intended component boundaries, not an implemented system. The simulation backend will be selected after evaluating TA-RWARE. Adapters should keep the dispatcher, evaluation code, and replay viewer independent of that decision.

## Data flow

The planned flow is:

```text
scenario configuration
        |
        v
simulation <---- dispatch decision
        |                ^
        | status/request |
        +------------> dispatcher
        |
        +----> experiment logs ----> analysis and comparisons
        |
        +----> replay events ------> browser-based 3D viewer
```

The simulation advances a deterministic simulation clock, maintains robots and orders, requests assignments at fixed decision intervals, applies routes and reservations, and records what happened. A dispatcher receives a limited description of the current decision and returns one assignment. Baseline dispatchers and the future PPO policy will use the same contract.

Logs contain measured quantities used for evaluation. Replay events describe visible state changes for presentation. Both are outputs of the same run, but they serve different purposes: compact structured logs support analysis, while replay data supports animation.

## Motion and routing

Robot poses use continuous `(x, y)` coordinates measured in meters on a flat floor. The warehouse also has a waypoint graph. Nodes mark useful locations or route junctions, and edges mark allowed travel corridors. Conventional routing chooses a path through this graph; motion code then advances a robot continuously along the selected edges.

This hybrid keeps positions suitable for animation and travel-time measurement without turning navigation into a learned problem. A future reservation mechanism will coordinate access to shared aisles or graph segments. Full rigid-body physics and learned navigation are outside scope.

## Simulation and dispatching

The centralized dispatcher decides which eligible robot should serve which waiting order. It does not steer robots. Initially, nearest-job and oldest-job policies will establish understandable baselines. A Gymnasium-compatible environment will later expose the same decision problem to Stable Baselines3 PPO.

Fixed dispatch intervals are the initial design. Keeping decision time explicit makes baseline and learned policies easier to compare. Detailed action semantics remain provisional and are listed in [`interfaces.md`](interfaces.md).

## Logging and evaluation

Python is the source of truth for simulation state, seeded randomness, and measured results. Future evaluation will run repeated scenarios under identical seeds and movement rules for each dispatcher. pandas and Matplotlib will summarize results after a run; notebooks may explore results but must not contain required pipeline logic.

Bursty demand, temporary aisle blockages, and robot unavailability are later scenario features. Their schedules should be generated from explicit seeds and recorded so a run can be reproduced.

## Replay viewer

The future Three.js viewer will load exported replay data and map floor coordinates into a 3D scene. It may interpolate between recorded events to create smooth animation. It must never advance simulation time, choose routes, resolve collisions, assign jobs, or calculate experimental metrics.

This one-way boundary matters for reproducibility: training and evaluation must produce the same outcome whether the viewer is open, closed, fast, or slow. It also permits the team to replace the simulation backend without rewriting the viewer, as long as the replay format remains stable.

## Future components

The project is expected to add these components incrementally:

- a simulation-backend adapter and deterministic simulation clock;
- warehouse layout, waypoint routing, and aisle reservations;
- robot, station, order, and disruption state;
- nearest-job and oldest-job dispatchers;
- a Gymnasium environment and PPO training entry point;
- structured logs, evaluation summaries, and replay export;
- a Vite/Three.js replay viewer.

None of these components is implemented by the current scaffold.

