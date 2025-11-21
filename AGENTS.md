TeatroPhysics is a small, renderer‑agnostic rigid‑body engine dedicated to Teatro rigs (Fadenpuppe, constellation props, doors, backline). It lives as a first‑party SwiftPM package under `Packages/` so FountainKit can depend on it without going through `External/`; later we can split it into a standalone GitHub repo without changing the public API.

Keep the scope narrow and explicit: bodies, forces, and constraints that match the Teatro demos, not a general physics playground. The engine should stay deterministic, headless, and testable — no Metal, no SDL, no UI dependencies. Visual demos (MetalViewKit, Three.js) sit on top of it and treat it as a pure model/solver.

Implementation norms:
- Prefer small, immutable value types (`Vec3`, `Quaternion`) and thin `class` wrappers only where identity is required.
- Keep the integrator simple (semi‑implicit Euler or Verlet) and stable at 60 Hz; document assumptions about timestep.
- Constraint types are explicit (`DistanceConstraint`, `PointToPointConstraint`); avoid a generic “do everything” constraint at this stage.
- Tests should lock in puppet behaviour at a coarse level (energy not exploding, distances staying near targets, ground constraints respected).

