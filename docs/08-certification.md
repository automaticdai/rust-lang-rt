# 8. Certification angle

## Idea

Make the profile useful as certification evidence, not only as a programming convenience.

## Candidates

- Map profile rules to objectives in DO-178C (avionics), ISO 26262 (automotive) and IEC 61508 (industrial).
- Align with the Ferrocene Language Specification, since Ferrocene is the qualified Rust toolchain.
- Align with the coding guidelines from the Safety-Critical Rust Consortium.
- Produce machine-readable evidence from the build: the task model, analysis results, lint reports and stack bounds.

## Open issues

- A library or macro layer adds code that itself needs qualification or verification; keeping the run-time small helps.
- Tool qualification: if the [schedulability analysis](03-schedulability-analysis.md) is used as evidence, the tool's own correctness becomes a certification concern.
