# MonoGlide Matrix Performance Engine

## Purpose

MonoGlide Matrix is a performance interpreter. It does not merely filter MIDI notes; it interprets how the performer is playing, assigns musical roles, and applies transition behavior appropriate to those roles.

The instrument should feel intuitive before the user understands its internal logic.

## Core Philosophy

- Trust the performer.
- Reward exploration.
- Keep the interface musical rather than overly technical.
- Preserve clean monophonic playing when only one role is active.
- Detect and protect a Foundation when a second role emerges.
- Let the expressive Voice use longer Overlap or Bloom releases.
- Never retrigger a note merely because its role changes.
- Never send Note Off merely because a note changes jobs.
- Only notes leaving the active role set should enter release processing.
- Reliability comes before additional features.

## Performance States

### Monophonic State

Only one musical role is active.

Examples:

- A bass line
- A lead melody
- A single-note sequence
- Legato or staccato monophonic playing

Behavior in Tonic mode:

- No Foundation/Voice split is active.
- Transitions remain clean and monophonic.
- Use a short fixed overlap for glide compatibility.
- Do not apply long Bloom releases to ordinary single-line playing.

Users who want always-Bloom mono behavior can select Bloom with a standard mono priority mode rather than Tonic.

### Anchored State

Two musical roles are active:

- Foundation
- Voice

This begins when one note remains held while another note is introduced above it.

Behavior:

- The Foundation is normally the lowest held note.
- The Voice is the most recently selected held note above the Foundation.
- Foundation movement stays tight and monophonic.
- Voice exits may use Classic, Overlap, or Bloom timing.

## Musical Roles

### Foundation

The Foundation is the protected structural role.

For the initial Tonic implementation:

- It is the lowest held note.
- It remains sounding while the Voice changes above it.
- A newly played lower note may replace it.
- Foundation-to-Foundation movement uses a short fixed overlap to preserve glide without muddying the bass line.
- A Foundation note that remains active must not retrigger merely because the Voice changes.
- A note moving between Foundation and Voice changes role without receiving new MIDI.

The user-facing mode name remains **Tonic**.

### Voice

The Voice is the expressive role above the Foundation.

For the initial Tonic implementation:

- It uses Last-note selection among held notes above the Foundation.
- Its transitions use the selected Transition Mode.
- Its exits may receive the full user-selected Overlap or Bloom duration.
- An unchanged Voice remains sounding when the Foundation moves.
- A Voice becoming Foundation must not retrigger.
- A Voice leaving the active role set receives the selected release treatment.

## Transition Modes

### Classic

- Exiting note: immediate Note Off
- Entering note: Note On
- Duration control: N/A

### Overlap

- Entering note: Note On first
- Exiting Voice note: delayed Note Off
- Range: approximately 0–500 ms
- Fine control concentrated in the shorter glide range

### Bloom

- Entering note: Note On first
- Exiting Voice note: delayed Note Off
- Range: approximately 0–2000 ms
- Creates gated releases, temporary polyphony, evolving harmony, and polyphonic blooms
- Useful even when the destination synth has no glide

### Planned for v1

- Legato
- Retrigger

These should be added after the Foundation/Voice engine is stable.

## Glide Clarification

MonoGlide Matrix does not synthesize portamento.

It creates note-priority and overlap conditions that allow a destination synth's own glide or portamento engine to respond.

Therefore:

- A synth with built-in glide can use Overlap or Tonic behavior to engage that glide musically.
- A synth without glide will not gain true portamento.
- A synth without glide can still use Overlap and Bloom for temporary polyphony, gated releases, pedal tones, and evolving articulation.

> MonoGlide Matrix does not create glide by itself. It creates the MIDI conditions that allow a synth's own glide engine to work.

## Active Voice Set

Tonic mode should reason about active pitches before roles.

```text
Active Voice Set = {Foundation, Voice}
```

Either role may be absent.

Examples:

```text
Foundation only: {C2}
Foundation + Voice: {C2, G3}
No active roles: {}
```

Roles are labels applied to active pitches. MIDI output is based on whether a pitch enters, stays in, or exits the active voice set.

## Change Classification

After every accepted Note On or Note Off:

1. Update held-note state.
2. Resolve the desired Foundation.
3. Resolve the desired Voice.
4. Build the previous active voice set.
5. Build the new active voice set.
6. Classify each pitch.

### STAY

Pitch is active before and after.

- Send no MIDI.
- Do not retrigger.
- Do not cancel the note because its role changes.

### ENTER

Pitch was inactive before and active afterward.

- Send Note On.
- In Overlap and Bloom, entries occur before exits.

### EXIT

Pitch was active before and inactive afterward.

- Classic: immediate Note Off.
- Overlap: queued Note Off using the Overlap duration.
- Bloom: queued Note Off using the Bloom duration.
- Foundation exits during anchored movement use the short Foundation transition instead of the full Voice release.

### ROLE CHANGE

Pitch remains active but changes role.

Examples:

- Foundation to Voice
- Voice to Foundation

Behavior:

- Send no MIDI.
- Change only the internal role assignment.

> The Transition Engine operates on role exits, not role changes.

## Queue Safety

Every queued release belongs to a pitch, not to its former role.

Before sending a queued Note Off, the timer asks:

```text
Is this pitch currently active as Foundation or Voice?
```

If yes, discard the queued release.

If no, send Note Off.

This prevents a stale release from silencing a pitch that became active again before its timer expired.

## Key Scenarios

### Release all upper notes while holding the Foundation

Before:

```text
Foundation C2
Voice G3
```

After:

```text
Foundation C2
```

Result:

- C2 is STAY and receives no MIDI.
- G3 is EXIT and follows the selected Voice release.

### Release Foundation and Voice together

Both roles EXIT.

In Bloom, both may receive delayed release when the entire performance ends, unless testing shows that a Foundation-specific ending is cleaner.

### Monophonic bass line in Tonic mode

```text
C2 -> D2 -> E2
```

Interpretation:

- One active role.
- No anchored state.
- Use the short monophonic transition.
- Do not apply long Bloom release.

### Establish Foundation and Voice

```text
Hold C2
Play G3
```

Interpretation:

- C2 becomes Foundation.
- G3 becomes Voice.
- Anchored state begins.

### Move Foundation while holding Voice

Before:

```text
Foundation C2
Voice G3
```

After pressing Bb1:

```text
Foundation Bb1
Voice G3
```

Classification:

```text
Bb1 = ENTER
G3 = STAY
C2 = EXIT
```

Result:

- G3 receives no MIDI and does not retrigger.
- Bb1 starts.
- C2 uses the short Foundation handoff.

### Role Swap

Before:

```text
Foundation C2
Voice G3
```

After:

```text
Foundation G3
Voice C2
```

Both pitches are STAY.

Result:

- No MIDI.
- Only internal role labels change.

## Initial v1 Interface

The initial version should remain a one-page instrument.

### Knobs

1. Priority / performance selection
2. Transition mode
3. Overlap or Bloom duration
4. Input channel: Omni or one exclusive channel 1–16

### Pads

- Panic
- Status displays
- Output channels 1–16 as independent multi-select pads

Output behavior:

- Any combination of output channels may be selected.
- No selected output channels means intentional silence.
- This allows performance muting and DAW automation without changing input state.

Input behavior:

- One input channel at a time, or Omni.
- A knob is preferred because input selection is exclusive.
- Multi-input selection may be considered in a later expanded version.

## v1 Scope

### Performance

- Last
- First
- High
- Low
- Tonic with intelligent Foundation/Voice interpretation

### Transitions

- Classic
- Overlap
- Bloom
- Legato
- Retrigger

### Routing

- Exclusive input channel knob
- Sixteen independently selectable output pads
- No-output mute state
- Panic Selected
- Long-press Panic All

### Documentation

- Clear explanation that glide depends on the destination synth
- Performance tips for Tonic and Bloom
- Unexpected musical uses
- Brief humorous Easter eggs that do not obstruct technical information

## Future Expansion

Potential later versions may include:

- User-selectable Foundation priority: Low, High, First, or Last
- Tri-phonic performance interpretation
- Middle-role chord detection
- Additional pages
- Multi-input selection
- Per-role routing
- Per-role transition settings
- Drone, pedal, trio, or chord-memory interpreters

The original one-page MonoGlide Matrix should remain available as the streamlined version.

## Final Guiding Statement

> MonoGlide Matrix is not merely a note processor. It is a performance interpreter that detects musical roles and gives each role the transition behavior that best serves the performance.

Rock and roll.

Toes Ma'goats.

Radical ravioli.
