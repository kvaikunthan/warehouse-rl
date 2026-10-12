# Replay viewer

The planned viewer will use Vite, Three.js, and plain JavaScript to display exported simulation replays. It will be a consumer of timestamped replay data, not part of the simulation or evaluation loop.

The viewer must not determine routes, assignments, timing, collisions, or metrics. Training and evaluation must work without starting it. Its replay contract should remain independent of the simulation backend selected after evaluating TA-RWARE.

No frontend application, Node dependency, build output, or lockfile exists yet.

