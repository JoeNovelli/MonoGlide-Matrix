# MonoGlide Matrix Testing

## Current test build

v0.2.6

## Core regression tests

- [x] Single note starts and stops cleanly.
- [x] Last priority returns to the previous held note.
- [x] First priority preserves the first-held note.
- [x] High priority selects the highest held note.
- [x] Low priority selects the lowest held note.
- [x] Input-channel filtering works.
- [x] Output-channel remapping works.
- [x] Panic silences the selected output channel.
- [x] Overlap enables glide on compatible synthesizers.
- [x] Bloom creates extended temporary polyphony.
- [x] TONIC preserves a held bass anchor.
- [x] Lead notes no longer retrigger unnecessarily during most anchor changes.
- [x] No reproducible stuck notes in v0.2.6.

## Current investigation

In TONIC + BLOOM mode, certain complex bass-anchor changes may shorten an upper-note queued release.

## Test environment

- Mozaic AUv3
- Loopy Pro
- MIDI keyboard controller
- AUv3 and external hardware synthesizers
