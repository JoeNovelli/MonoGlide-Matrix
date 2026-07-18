# MonoGlide Matrix (MGM)

*A Mozaic AUv3 MIDI processor for expressive monophonic performance on polyphonic synthesizers.*

---

## Overview

MonoGlide Matrix (MGM) transforms incoming MIDI into expressive monophonic note behavior while preserving compatibility with modern polyphonic synthesizers.

Rather than simply limiting polyphony, MGM provides musical note transitions, configurable priority systems, and expressive overlap behavior that allows polyphonic synthesizers to be performed with the feel of dedicated monophonic instruments.

Designed for live performance, MGM works especially well with Mozaic AUv3 and Loopy Pro on iPad, while remaining compatible with any host capable of running Mozaic.

---

## Features

### Transition Modes

- **Classic** – Immediate note handoff with no intentional overlap.
- **Overlap** – Short configurable overlap designed to encourage natural synth glide.
- **Bloom** – Extended overlap creating evolving transitions and expressive sustained textures.

### Priority Modes

- Last
- First
- High
- Low
- Tonic *(Experimental)*

### Additional Features

- Adjustable Transition Time
- Panic (Selected or All)
- MIDI input/output channel routing
- Designed for live performance
- Hardware and software synthesizer compatible

---

## Current Status

### Current Release

**v0.5.0 Beta**

Validated on:

- ✅ Vongon Replay
- ✅ Critter & Guitari Organelle
- ✅ Moog Model 15

Known compatibility observations:

- Bananana Effects Quimera exhibits a returned-note edge case during long Bloom overlaps that appears related to the synth's internal voice allocation.

---

## Philosophy

MonoGlide Matrix is not intended simply to emulate vintage monophonic synthesizers.

Instead, it explores new ways of performing polyphonic synthesizers as expressive monophonic instruments while remaining predictable, musical, and suitable for live performance.

Features such as Bloom, Tonic, and future articulation modes are designed to expand musical performance possibilities rather than merely reproduce historical behavior.

---

## Understanding Polyphony

MGM intentionally creates note overlap in Overlap and Bloom modes.

The musical result depends on your synthesizer's:

- Available polyphony
- Voice allocation strategy
- Voice stealing behavior
- Envelope implementation
- Glide implementation

For this reason, identical MIDI data may produce different musical results on different synthesizers.

This is expected behavior and is one of the areas of ongoing development.

---

## Installation

1. Import the `.moz` file into Mozaic.
2. Insert Mozaic before your synthesizer.
3. Select the desired MIDI input and output channels.
4. Begin playing.

---

## Roadmap

### Toward v1.0

- Improve Tonic role handoff
- Build input/output channel matrix
- Complete user documentation
- Community beta testing

### Future Enhancements

- Legato / Retrigger articulation modes
- Polyphony-aware performance profiles
- Optional compatibility modes
- Expanded MIDI routing
- Additional performance modes

---

## About the Name

**MonoGlide** reflects the project's expressive monophonic performance engine.

**Matrix** represents the long-term vision of flexible MIDI routing and intelligent musical decision making between performer and synthesizer.

---

## License

(Choose a license before the v1.0 public release.)

---

## Acknowledgements

MonoGlide Matrix is developed through extensive real-world testing on hardware and software synthesizers with a focus on expressive live performance.
