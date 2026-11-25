# 7. Backends

## Idea

One programming model and API over several execution platforms, so that the same task set can run on a desktop for testing and on a target for deployment.

## Candidate targets

| Target | Strengths | Weaknesses |
|--------|-----------|------------|
| Linux `SCHED_FIFO` | Fastest route to a demo; easy debugging | Weak guarantees without PREEMPT_RT; ceiling locking is only approximated |
| Linux `SCHED_DEADLINE` | Native EDF with budget enforcement (CBS) | Linux-specific; needs privileges |
| Bare-metal Cortex-M / RISC-V | Strongest guarantees; smallest trusted base | Needs a small kernel or reuse of RTIC-style interrupt scheduling |
| RTOS (Zephyr, RTEMS, FreeRTOS) | Industrial relevance; existing drivers | Semantics differ per RTOS; FFI layer needed |
| seL4 | Verified kernel; mixed-criticality scheduling support | Steeper integration effort |

## Design points

- A small backend trait covering task release, clocks, delays, locking and budget timers.
- Features that a backend cannot support (for example, execution-time budgets) are absent at compile time, not silently ignored.
- A simulated backend with virtual time would allow deterministic tests of scheduling behaviour.

## Suggested order

Linux `SCHED_FIFO` first for quick iteration, then one bare-metal target for credibility.
