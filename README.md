# Warehouse RL

Warehouse RL is a six-week undergraduate group project about warehouse dispatching. Three robots collect packages from pickup stations and deliver them to a depot. The project asks a focused research question:

> How does a learned centralized dispatcher compare with simple nearest-job and oldest-job schedulers when demand is bursty or robots and aisles become temporarily unavailable?

The robots will move continuously across a flat floor, but they will choose routes through a waypoint network. Dispatching and movement are separate problems: a dispatcher assigns work, while conventional graph routing and aisle reservations control how robots travel safely.

The simulation backend will be selected after evaluating TA-RWARE. Python will remain the owner of simulation state, experimental measurements, and exported replay data regardless of that choice. A browser-based 3D viewer will visualize replays, but training and evaluation will run without it.

## Current status

This repository currently contains project documentation, dependency metadata, and minimal package folders only. It does **not** yet contain a simulator, dispatchers, a Gymnasium environment, PPO training, experiments, or a 3D viewer. The documents describe planned work and provisional interfaces; they do not claim that those components work.

## Intended stack

- Python 3.12
- NumPy for numerical operations
- NetworkX for waypoint graphs and routing
- Gymnasium for the reinforcement-learning environment interface
- Stable Baselines3 for PPO
- pandas and Matplotlib for analysis
- pytest for meaningful automated checks
- Three.js, Vite, and plain JavaScript for the eventual replay viewer

Dependencies are intentionally unpinned during the initial scaffold. The team should record compatible constraints later, after the first working integration has been tested.

## Repository layout

| Path | Responsibility |
| --- | --- |
| `configs/` | Small, human-readable experiment and scenario settings added when needed |
| `docs/` | Architecture, provisional interfaces, and milestone plans |
| `src/warehouse_rl/simulation/` | Future simulation adapter, state transitions, routing, and logging |
| `src/warehouse_rl/policies/` | Future nearest-job, oldest-job, and learned dispatch policies |
| `scripts/` | Future short entry points for training, evaluation, and replay export |
| `notebooks/` | Exploratory analysis that does not contain required project logic |
| `viewer/` | Documentation and, later, the independent replay viewer |
| `tests/` | Meaningful checks added alongside implemented behavior |
| `outputs/` | Generated results and replays; only its README is tracked |

## Python setup

Python 3.12 is required. Run these commands from the repository root.

### macOS and Linux

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

### Windows PowerShell

```powershell
py -3.12 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

The editable install makes the `warehouse_rl` package importable while reflecting source edits immediately. The `dev` extra adds pytest. Installing the package does not create the planned simulator or viewer.

## Collaboration workflow

Use one short-lived branch per small task:

1. Update local `main`, then create a descriptive branch such as `docs/replay-format` or `feature/nearest-dispatcher`.
2. Make a small, readable change and add meaningful checks when behavior is introduced.
3. Run the relevant checks locally and update documentation if an interface or command changed.
4. Push the branch and open a pull request. Describe what changed, how it was checked, and any unresolved questions.
5. Ask at least one teammate to review. Address feedback, then merge only when the change is understandable and the checks pass.

Avoid combining unrelated work in one pull request. Coordinate before editing shared interface documents so both student groups can keep working independently.

## Planning documents

- [`docs/architecture.md`](docs/architecture.md) explains the planned component boundaries.
- [`docs/interfaces.md`](docs/interfaces.md) proposes the first shared data contract.
- [`docs/week-1.md`](docs/week-1.md) breaks the first milestone into beginner-sized tasks.
