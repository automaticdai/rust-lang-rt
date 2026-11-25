# rust-lang-rt design notes

Brainstorm of potential directions for a real-time extension to Rust for safety-critical systems. Nothing here is decided; each page describes one candidate direction.

## The gap

Rust gives memory safety and data-race freedom, but says nothing about time. It has no notion of periods, deadlines, priorities, worst-case execution time (WCET) or bounded blocking. Ada has the Ravenscar and Jorvik profiles and Java has RTSJ, but Rust has no equivalent. Existing pieces (RTIC, Embassy, Ferrocene, Hubris) each cover a slice, and none offers an analysable real-time programming model at the language level.

## Directions

| # | Direction | Summary |
|---|-----------|---------|
| 1 | [Tasking profile](01-tasking-profile.md) | A Ravenscar-style restricted, analysable tasking subset for Rust |
| 2 | [Timing as types](02-timing-as-types.md) | Periods, deadlines, priorities and ceilings encoded in the type system |
| 3 | [Compile-time schedulability analysis](03-schedulability-analysis.md) | Response-time analysis at build time; unschedulable means compile error |
| 4 | [Real-time language features](04-language-features.md) | Clocks, `delay_until`, budgets, overrun handlers, mode changes |
| 5 | [Predictability lints](05-predictability-lints.md) | Lints against allocation, unbounded loops, panics and dynamic dispatch |
| 6 | [Real-time async executor](06-async-executor.md) | Priority-aware or EDF `async`/`await` with analysable await points |
| 7 | [Backends](07-backends.md) | One API over Linux, bare-metal and RTOS targets |
| 8 | [Certification](08-certification.md) | Mapping to DO-178C, ISO 26262, IEC 61508 and the Ferrocene spec |

See also [open questions](open-questions.md).

## Suggested starting point

Directions 1, 2 and 3 together form a coherent core: a `#![no_std]` crate that defines the tasking profile with typed timing attributes, plus a build-time response-time analysis check. A Linux `SCHED_FIFO` backend gives easy demos and one bare-metal target gives credibility. Directions 4 to 6 can grow out of that core.
