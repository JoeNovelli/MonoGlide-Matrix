# MonoGlide Matrix

A performance-focused monophonic MIDI processor and multi-channel router for the Mozaic AUv3 plugin.

MonoGlide Matrix converts polyphonic MIDI input into reliable monophonic output, with selectable note priority, configurable glide behavior, and routing to one or more MIDI channels.

## Project status

**Current stable build:** v0.1.1  
**Status:** Core engine validated in Mozaic  
**Regression tests:** 8/8 passed

### Working

- Polyphonic-to-monophonic note conversion
- Last-note priority
- First-held priority
- Highest-note priority
- Lowest-note priority
- Input-channel filtering
- Single output-channel remapping
- Selected-channel panic
- Duplicate Note Off protection
- Current-note and held-note display

### In development

- Adjustable note overlap for portamento and glide
- Legato and retrigger transition modes
- Sixteen independently selectable output channels
- Mono bypass/router mode
- Non-note MIDI forwarding
- Loopy Pro automation workflow

## Design goals

1. Never leave a stuck note.
2. Reliability before features.
3. Deterministic note behavior.
4. Stable controls for Loopy Pro automation.
5. No selected outputs means intentional silence.
6. Keep the code readable and educational.

## Planned controls

### Knobs

1. Priority
2. Glide mode
3. Overlap time
4. Input channel

### Pads

- Mono enable
- Panic
- MIDI output channels 1–16

## Development

MonoGlide Matrix is being designed and tested by Joe Novelli with development assistance from ChatGPT, affectionately known as Clem.

Keep making weird noises.

Totally tubular. Awesome salsa. Bodacious MIDI.
