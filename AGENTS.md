# Repository guidance

These instructions apply throughout this repository. They are for students and coding agents alike.

## Working style

- Keep changes small, readable, and suitable for contributors who are new to Python and reinforcement learning.
- Prefer direct code and plain data structures. Add a framework, service, abstraction, or dependency only when a concrete requirement justifies it.
- Explain non-obvious design choices in nearby documentation or comments, and keep documentation accurate when behavior changes.
- Preserve existing work and follow any more specific instructions added later in a subdirectory.
- Never invent experimental results or say an unfinished component works.
- When behavior is implemented, add checks that can catch a real mistake. Do not add trivial placeholder tests merely to increase a test count.

## System boundaries

- Keep simulation, dispatch policies, analysis, and rendering separate.
- Python owns simulation state and measured results. Rendering consumes exported replay data and must not change an outcome.
- Keep simulation time independent of wall-clock time and viewer frame rate.
- Use explicit seeds for reproducible order arrivals and disruption schedules, and record those seeds with results.
- Give every compared dispatcher the same observations, eligible actions, movement rules, and disruption conditions.
- Keep evaluation and the viewer independent of the simulation-backend choice. The backend will be selected after evaluating TA-RWARE.
- Do not silently expand the project into learned navigation, multi-agent control, or full robot physics. The learned component is the centralized job dispatcher.

## Scope discipline

Before adding code, check the current milestone and provisional interfaces. Prefer the smallest implementation that can answer the research question. If a proposed change crosses a component boundary or changes shared data, document it and discuss it with both groups before merging.

