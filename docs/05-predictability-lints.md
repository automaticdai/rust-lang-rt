# 5. Predictability lints

## Idea

A lint set (Clippy-style or Dylint) that flags constructs whose timing or memory use cannot be bounded, applied to code marked as hard real-time.

## Candidate lints

- Heap allocation after initialisation.
- Loops without a provable or declared bound.
- Recursion, direct or mutual.
- Paths that can panic (indexing, `unwrap`, arithmetic overflow checks).
- Dynamic dispatch (`dyn Trait`, function pointers), which hinders WCET and stack analysis.
- Blocking calls inside protected actions or interrupt handlers.

## Related tooling

- **Stack-bound analysis**: compute the worst-case stack depth per task from the call graph.
- **`#[wcet(...)]` annotation**: attach a WCET budget or loop bound to a function, and pass it to an external WCET tool or to the [schedulability analysis](03-schedulability-analysis.md).
- **`#[hard_rt]` scope marker**: an attribute that turns the lint set on for a function, module or crate.

## Open issues

- Lints that depend on compiler internals tie the project to nightly toolchains.
- Dependencies are the hard part: a lint on the user's crate does not show what a third-party crate does internally.
