# 6. A real-time async executor

## Idea

Give `async`/`await` real-time semantics, so that the ergonomic concurrency model Rust developers already use becomes analysable.

## Candidates

- A priority-aware executor with fixed-priority pre-emptive scheduling of async tasks.
- An EDF executor that orders ready tasks by absolute deadline.
- Bounded wake latency: a guaranteed upper bound between a waker firing and the task being polled.
- Analysable await points: treat each segment between awaits as a non-pre-emptive section with its own WCET, which maps onto limited-pre-emption scheduling theory.

## Why it matters

Embassy has made async popular in embedded Rust, but its timing analysis is immature. Async tasks are stackless and cooperatively scheduled within an executor, which differs from the pre-emptive thread model that classic analysis assumes.

## Open issues

- Cooperative scheduling means a long poll blocks every task in the same executor; the profile must bound poll length or use one executor per priority level.
- Mapping async tasks onto the task model of the [tasking profile](01-tasking-profile.md): one model with two front ends, or two separate models?
