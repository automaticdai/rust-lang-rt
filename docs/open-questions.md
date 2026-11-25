# Open questions

These decisions shape which directions to pursue and in what order.

1. **Library or language extension?** A library/DSL (crates and macros on stable Rust) is far more adoptable. A true language extension (compiler fork or RFC-style proposals) allows deeper checks but is costly to maintain. The repository name suggests the latter.
2. **Research vehicle or practitioner tool?** A research vehicle can use nightly features and target a narrow platform. A practitioner tool needs stable Rust, documentation and long-term maintenance.
3. **Which target comes first?** Linux, bare-metal or an RTOS (see [backends](07-backends.md)).
4. **How much scheduling theory at the start?** Uniprocessor fixed-priority first, or multicore and mixed-criticality from the beginning?
5. **Threads, async or both?** Whether the [tasking profile](01-tasking-profile.md) and the [async executor](06-async-executor.md) share one task model.
6. **Where do WCET values come from?** Declared budgets, measurement or static analysis (see [schedulability analysis](03-schedulability-analysis.md)).
