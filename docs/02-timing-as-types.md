# 2. Timing as types

## Idea

Encode timing properties in the type system so that violations are compile errors instead of run-time faults or analysis findings.

## Sketches

```rust
// Timing attributes as const generics
type Control = Task<Period<10>, Deadline<10>, Prio<3>>;

// Ceiling computed at compile time from the tasks that use the resource
static SENSOR: Protected<SensorState, Ceiling<3>> = Protected::new(..);
```

- **Priority ceilings**: compute each resource's ceiling at compile time from its users, as RTIC does for the Stack Resource Policy.
- **Typestate for resource access**: a task holds a priority token; locking a resource whose ceiling is below the token's level fails to compile.
- **Units of time**: distinct types for durations and instants per clock, so that values from different clocks cannot be mixed.
- **Bounded-blocking marker trait** (stretch): an auto trait analogous to `Send`/`Sync` that marks code free of unbounded blocking, required in hard real-time contexts.

## Open issues

- Const generics on stable Rust are limited; arithmetic over them (for example, `D <= T`) needs workarounds or nightly features.
- Type errors must stay readable. Deeply nested generic types produce poor diagnostics.
- Decide how much lives in types and how much in a separate analysis pass (see [schedulability analysis](03-schedulability-analysis.md)).
