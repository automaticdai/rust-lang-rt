# 1. A Ravenscar-style tasking profile for Rust

## Idea

Define a restricted subset of concurrent Rust that is statically analysable for timing, in the way the Ravenscar and Jorvik profiles restrict Ada tasking.

## Possible restrictions

- A static task set: all tasks are declared at the top level, with no dynamic creation or termination.
- Tasks are either periodic (time-triggered) or sporadic (event-triggered with a minimum inter-arrival time).
- Each task has a single release point per job.
- Shared state is accessed only through protected objects that use a priority ceiling protocol.
- No blocking inside a protected action.
- No heap allocation after initialisation.

## Delivery

- A crate that provides the task and protected-object abstractions.
- A lint set that rejects code outside the profile (see [predictability lints](05-predictability-lints.md)).
- A written profile definition, so that conformance can be claimed and checked.

## Research angle

"What is Ravenscar for Rust, and what does ownership give for free?" Ownership and `Send`/`Sync` already rule out data races, so the profile may need fewer rules than Ada's, or may be able to permit patterns that Ravenscar forbids (for example, moving owned data between tasks without a protected object).

## Related work

- Ada Ravenscar and Jorvik profiles
- RTIC (Stack Resource Policy for Cortex-M)
- Hubris (static task set, message passing)
