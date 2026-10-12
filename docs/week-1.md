# Week 1 milestone

## Goal

By the end of week 1, a nearest-job dispatcher should control three robots in the selected or mocked simulation boundary, produce understandable logs, and export a replay that a minimal viewer path can consume. This is an integration milestone, not a polished warehouse system. No trained RL policy is required.

The simulation backend will be selected only after evaluating TA-RWARE. Work should therefore target the small shared records in [`interfaces.md`](interfaces.md), not backend-specific objects.

## Group A: environment, routing, and viewer

Three students focus on the simulation-facing side:

1. Evaluate TA-RWARE against the project requirements and write a short recommendation covering continuous positions, waypoint routing, deterministic stepping, disruptions, and replay access.
2. Draft a tiny backend-neutral mock simulator contract with three robot records, pickup stations, a depot, and a controllable simulation clock. This task defines behavior for integration; it should not grow into a second full simulator.
3. Sketch the waypoint layout and define how route distance will be supplied to the dispatcher.
4. Agree on a minimal JSON replay example and document the mapping from floor `(x, y)` meters to viewer `(x, 0, z)` coordinates.
5. Confirm that a replay can be inspected independently of a live Python process. Frontend scaffolding and polished 3D assets are not week-one requirements.

Acceptance criteria:

- The TA-RWARE recommendation identifies fit, gaps, and the proposed next step.
- Three uniquely identified robots, at least two pickup stations, and one depot can be represented through the agreed boundary.
- Simulation time can advance without reference to wall-clock or rendering time.
- Route or distance information needed by nearest-job is deterministic and documented.
- A small hand-written sample replay follows the agreed event shapes and coordinate units.

## Group B: dispatch, RL interface, and evaluation

Three students focus on the decision and measurement side:

1. Write examples for dispatch requests containing idle robots and waiting orders.
2. Specify nearest-job tie-breaking so repeated runs make the same decision.
3. Define the first nearest-job dispatcher against the provisional request/decision contract.
4. Propose the Gymnasium observation and action mapping without implementing PPO training.
5. Define a small log schema and the metrics needed to confirm the first run completed correctly.

Acceptance criteria:

- Nearest-job behavior is precise for normal cases, ties, no idle robot, and no waiting order.
- Dispatch examples do not depend on viewer objects or a particular simulation library.
- Proposed RL observations expose no more information than the baseline dispatchers receive.
- The evaluation note distinguishes raw logged facts from later derived metrics.
- No fake performance numbers or claims about an untrained policy appear.

## Working independently with mocks

Both groups should begin before the real backend and UI exist. Group A can make a deterministic mock that accepts a `DispatchDecision` and emits a short sequence of state changes. Group B can call the dispatcher with hand-written `DispatchRequest` examples. A shared hand-written replay lets viewer work validate parsing and coordinate mapping without waiting for simulation export.

Mocks and sample data are temporary integration aids, not experimental evidence. Keep them small, label them clearly, and replace them with real behavior as integration proceeds.

## Coordination points

### Shared interface meeting

Hold this meeting early in the week with all six students. Review identifiers, units, robot and order states, eligible actions, tie-breaking, no-op and invalid-action behavior, and the first replay events. Record decisions in `docs/interfaces.md` before parallel implementations drift apart.

### Midweek integration checkpoint

Connect Group B's nearest-job decision to Group A's mock boundary. Use one tiny deterministic scenario, inspect the log and replay together, and write down mismatches. The checkpoint succeeds when data crosses the boundary cleanly; visual polish and learning performance do not matter yet.

## End-of-week demonstration

Demonstrate a deterministic scenario in which the nearest-job policy assigns work to three robots, deliveries complete, logs record the sequence, and an exported replay represents it. If the full path is not ready, report exactly which boundary failed and show the independently validated mock pieces. Do not substitute invented results.

Ownership is temporary. In later weeks, rotate reviews and implementation tasks so every student learns more than one of simulation, routing, dispatch, reinforcement learning, evaluation, and visualization.

