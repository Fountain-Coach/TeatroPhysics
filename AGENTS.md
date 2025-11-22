TeatroStageEngine is the canonical engine for the Teatro puppet stage. The Swift module `TeatroPhysics` holds the rigid‑body solver and puppet rig; the `spec/` tree describes camera, room, rig, style, authoring UX, and interchange in prose; and the `Demos/` tree shows how the same engine behaves in Three.js. Other repos (FountainKit, web frontends, tools) must treat this package as the source of truth and stay in sync with its specs.

This repository is strictly headless. The Swift code has no Metal, SDL, or UI dependencies; it advances bodies under gravity and constraints, and exposes snapshots that renderers map into their own coordinate systems. The TypeScript engine in `Public/teatro-stage-web` is a port of these types, not a separate design.

Working rules for agents:
- Spec‑first: when behaviour changes, update the relevant document under `spec/` (camera, physics, rig‑puppet, stage‑room, style, authoring, interchange) and then adjust Swift and JS implementations to match. The Three.js demos are historical references only.
- Deterministic physics: keep the integrator semi‑implicit Euler with explicit gravity and damping (`spec/physics/world-and-timestep.md`). Do not introduce randomness in the core engine.
- Narrow constraint set: constraint types are explicit (`TPDistanceConstraint` today). Add new constraints only when a real Teatro use‑case requires them, and document them under `spec/physics/constraints.md` before coding.
- Tests as guardrails: extend the test suite beyond unit math to cover rig behaviour (energy not exploding, strings staying near rest length, rig remaining within reasonable bounds over long runs). Treat failing tests as blockers for visual tweaks.

Implementation norms:
- Prefer small value types (`TPVec3`) and thin reference types (`TPBody`, `TPPuppetRig`) only where identity is required.
- Keep the Swift package renderer‑agnostic and reusable; visual choices belong in demos and host apps.
- Maintain the mapping between Swift, TS, and specs: field names and units must line up so snapshots can flow between the engine, web demo, and FountainKit without adapters that “fix” mismatches on the fly.

