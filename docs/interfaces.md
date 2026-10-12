# Provisional shared interfaces

This is a small starting contract between the environment/routing/viewer group and the dispatch/RL/evaluation group. It should be revised at the shared interface meeting before code depends on it. Plain dataclasses, dictionaries, or typed records are sufficient; no interface framework is planned.

## Units and coordinates

- Distance and continuous positions use meters.
- Simulation time and durations use seconds.
- Floor positions use `(x, y)`, with a documented origin shared by the simulator and exported replay.
- The viewer should map simulation `(x, y)` to Three.js `(x, 0, z)`. A single documented scale converts meters to scene units; ideally one scene unit represents one meter.
- Headings, if needed, should use radians with one documented zero direction and rotation convention.

The viewer may interpolate positions for smoothness, but exported simulation timestamps and positions remain authoritative.

## Robot status

A dispatch decision needs only information relevant to assignment. A provisional record is:

```text
RobotStatus:
  id: string
  position_m: (x, y)
  state: IDLE | TO_PICKUP | TO_DEPOT | UNAVAILABLE
  current_order_id: string or null
  available: boolean
```

Route internals, animation state, and rendering objects should not appear in a dispatch observation.

## Orders

```text
Order:
  id: string
  pickup_station_id: string
  created_at_s: number
  state: WAITING | ASSIGNED | PICKED_UP | DELIVERED
  assigned_robot_id: string or null
```

Station locations belong to the scenario or observation data. The team should decide whether dispatchers receive coordinates, graph distance estimates, or a distance matrix. Every compared dispatcher must receive the same information.

## Dispatch request and decision

Start with fixed dispatch intervals. At one interval, the environment supplies current simulation time, robot statuses, waiting orders, and any shared station/distance information. A decision assigns at most one robot to one station/order:

```text
DispatchRequest:
  simulation_time_s
  robots
  waiting_orders
  station_information

DispatchDecision:
  robot_id
  order_id
```

If no valid assignment exists, the contract needs an explicit no-operation decision. Restricting the first version to one assignment per interval keeps the discrete action space and baseline behavior understandable. Multiple assignments can be reconsidered only after the first end-to-end milestone works.

## Simulation snapshots

A snapshot is a read-only description used for logging, debugging, or coarse replay checkpoints. It should include:

```text
SimulationSnapshot:
  simulation_time_s
  robot poses and states
  order states
  active blockages
  active robot unavailability
```

Snapshots should contain stable IDs and plain serializable values. Dispatchers should receive a purpose-built request rather than unrestricted mutable simulation state.

## Replay events

Replay output is a timestamped sequence that the viewer can consume without importing Python code. Example event shapes are:

```json
{"time_s": 4.0, "type": "order_created", "order_id": "o17", "station_id": "s2"}
{"time_s": 5.0, "type": "job_assigned", "order_id": "o17", "robot_id": "r1"}
{"time_s": 5.5, "type": "robot_pose", "robot_id": "r1", "x_m": 2.4, "y_m": 7.1}
```

Later events may represent pickup, delivery, route changes, blockages, and availability changes. The first sample replay should use only the minimum events needed to animate the week-one story. Version the replay schema once real consumers depend on it.

## Unresolved choices

The teams must agree on these points before implementing the shared boundary:

- When several robots are idle, does the dispatcher select the robot, or does the environment nominate one eligible robot and ask only for an order?
- Is a no-operation choice always available?
- Does an invalid action raise an error, receive a penalty, get masked out, or become a no-op? Training and baseline evaluation must use consistent semantics.
- How are temporarily blocked routes represented to the dispatcher, and what happens to a robot whose current route becomes blocked?
- Does “nearest” use Euclidean distance, graph distance, or estimated travel time?
- How frequently are poses recorded for replay without making files unnecessarily large?
- Which outcome and efficiency metrics are required for the research comparison?

Record decisions here before implementing them in multiple components.

