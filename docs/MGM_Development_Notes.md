# Development Notes

## Bloom return-note investigation — July 2026

### Symptom

With long Overlap or Bloom times, a note returning before an earlier delayed
release expired could fail to articulate on Bananana Effects Quimera.

The failure became easier to reproduce as transition time increased:

- Around 40 ms: effectively absent
- Around 125 ms: rare
- Around 1000 ms: easy to reproduce on Quimera

### Diagnostic builds

- v0.4.12 — Resolver scope
- v0.4.13 — Queue scope
- v0.4.14 — MIDI output trace

### Result

For the test phrase:

    C4 → D4 → E4 → D4

MGM transmitted the same recent outbound sequence to both Quimera and Replay:

    OFF D4
    ON D4
    OFF C4
    OFF E4

Replay articulated the returned D4 correctly. Quimera sometimes did not.

The same MGM behavior was also successful on a Juno-style Organelle patch and
in the controlled Model 15 test. This strongly suggests that the specific
missed-return symptom is a Quimera firmware or MIDI voice-allocation
compatibility edge case, rather than MGM omitting or immediately canceling the
returning Note On.

### Development decision

Keep the standard MGM engine conservative and device-neutral. Do not add a
Quimera-specific workaround to the default path unless a future compatibility
option can be implemented without compromising behavior on other synths.

## Current priorities

1. Improve Tonic role handoff.
2. Build the input/output channel pad matrix.
3. Write the user manual.
4. Prepare broader community beta testing.
