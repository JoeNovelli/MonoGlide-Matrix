# MonoGlide Matrix v0.5.0 Beta

MonoGlide Matrix (MGM) is a Mozaic AUv3 MIDI processor for forcing mono-style
note behavior on polyphonic synths, with optional overlap and extended Bloom
transitions.

## Included

- Classic, Overlap, and Bloom modes
- Last, First, High, Low, and experimental Tonic priorities
- Adjustable transition duration
- Selectable output MIDI channel
- Panic control
- Re-entry ownership and delayed-release queue handling

## Beta status

Last + Bloom has been validated on Vongon Replay, a Juno-style Organelle patch,
and Moog Model 15.

Tonic is still experimental and is the next main engine-development target.

## Known compatibility note

Bananana Effects Quimera may miss returned notes during long Bloom overlaps,
despite MGM transmitting the same MIDI sequence handled correctly by other
tested synths.
