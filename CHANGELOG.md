# Changelog

## v0.5.0 Beta — 2026-07-17

### Highlights

- Stable Classic, Overlap, and Bloom transition modes.
- Priority modes: Last, First, High, Low, and experimental Tonic.
- Improved note re-entry and ownership handling.
- Delayed-release queue cancellation for returning notes.
- Stable Panic behavior.
- Cleaner beta layout with diagnostic counters removed.

### Validation

The Last + Bloom return-note test was verified successfully on:

- Vongon Replay
- Critter & Guitari Organelle using a Juno-style community patch
- Moog Model 15

Model 15 can exhibit envelope and voice-allocation behavior under dense, long
overlaps that is also reproducible when played directly without MGM.

### Known issues

- Tonic remains experimental. Bass-anchor and lead role handoffs can still
  produce retrigger, cutout, or timing anomalies in some playing patterns.
- Bananana Effects Quimera can miss returned notes during long Bloom overlaps.
  MGM's outbound MIDI trace matched the sequence accepted by Replay, Organelle,
  and Model 15, suggesting a Quimera firmware compatibility edge case.
- Output routing is currently a single selectable MIDI channel. The planned
  multi-output pad matrix is not yet implemented.

## Earlier development milestones

### v0.4.x

- Re-entry ownership work.
- Performance-state and queue diagnostics.
- MIDI output tracing used to isolate device-specific behavior.

### v0.3.x

- Tonic role-set rewrite and performance-state experiments.

### v0.2.x

- Added Bloom mode.
- Added Tonic priority.
- Improved overlap timing and stuck-note handling.
