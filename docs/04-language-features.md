# 4. Real-time language features missing from `std`/`core`

## Idea

Provide the real-time facilities that Ada and RTSJ offer and that Rust's standard library lacks.

## Candidates

- **Monotonic clock abstraction**: a trait for real-time clocks with defined resolution and range, usable in `no_std`.
- **`delay_until`**: an absolute-time delay, so that periodic loops do not accumulate drift as they do with relative sleeps.
- **Deadline-miss handlers**: a defined hook that runs when a job overruns its deadline.
- **Execution-time budgets**: per-task CPU-time clocks and timers, like Ada's `Ada.Execution_Time.Timers`, with a WCET-overrun handler.
- **Mode changes**: a controlled way to switch between task sets or parameter sets.
- **Asynchronous transfer of control**: safe cancellation of a computation in progress. Cancellation by dropping a future in async Rust is an interesting basis for this.
- **Mixed-criticality support**: criticality levels as types, with budget-triggered mode switches that shed or degrade low-criticality tasks.

## Open issues

- Budget enforcement needs support from the underlying kernel or hardware timers, so availability varies by [backend](07-backends.md).
- Cancellation safety: dropping a future mid-way can leave shared state inconsistent unless the profile constrains where cancellation may occur.
