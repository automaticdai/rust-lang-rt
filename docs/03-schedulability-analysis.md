# 3. Compile-time schedulability analysis

## Idea

Extract the task model from the program and run schedulability analysis during the build. An unschedulable system fails to compile.

## How it could work

1. A proc-macro or a `cargo rt-check` subcommand collects the task set: periods, deadlines, priorities, WCET budgets and the resources each task uses.
2. The tool computes blocking terms from the ceiling protocol and runs response-time analysis.
3. It reports per-task worst-case response times and fails the build if any exceeds its deadline.

## Scope options

- Start with uniprocessor fixed-priority pre-emptive scheduling and classic response-time analysis.
- Later add EDF, limited pre-emption, multicore (partitioned first) and mixed-criticality analyses.
- Export the task model in a neutral format so that external analysis tools can consume it.

## Where WCET comes from

The analysis needs a WCET per task. Options, in rising order of rigour:

- Declared budgets, enforced at run time (see [language features](04-language-features.md)).
- Measurement-based estimates from a test harness.
- Static WCET analysis by an external tool, fed back through annotations.

## Open issues

- A proc-macro sees one crate at a time; a whole-system view probably needs a cargo subcommand or a build script.
- The result is only as sound as the WCET inputs, so the tool must state its assumptions clearly.
