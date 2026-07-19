# Bloom Control & Tonic Response

## Overview

As MonoGlide Matrix evolved from a traditional MIDI utility into a musical performance instrument, the front-panel controls were redesigned to emphasize musical intent rather than engineering implementation.

The objective is to allow players to think about performance character rather than parameter values.

---

# Bloom Control

The original interface contained two separate controls:

- Transition Mode
- Transition Time

During development it became clear these controls answered the same musical question:

> How should one note become the next?

The interface therefore evolves into a single continuous control:

## Bloom

Internally the engine still operates using overlap timing.

The user simply experiences one expressive control.

As the control is turned clockwise it moves through increasingly expressive transition regions.

Current working terminology:

Classic

↓

Glide

↓

Linger

↓

Shadow (optional)

↓

Bloom

Only one transition point is fixed.

Classic = 0 ms.

All remaining boundaries will be determined through musical play testing rather than arbitrary engineering values.

The displayed region names represent musical territory rather than algorithm changes.

Shadow remains an optional region pending future play testing.

---

# Design Philosophy

Bloom is intentionally presented as an expressive musical control rather than an engineering parameter.

Players should think:

"I want this transition to bloom longer."

rather than

"I want another 80 milliseconds of overlap."

Exact overlap values remain documented in the manual for users who want additional technical detail.

---

# Tonic Response

Tonic Response is planned as a future output adaptation layer.

It is **not** part of Puppeteer's musical decision making.

Instead it adapts how those decisions are expressed to different synthesizers.

Conceptually:

Puppeteer

↓

Performance State

↓

Transition Engine

↓

Synth Adapter
(Tonic Response)

↓

MIDI Output

The musical decision remains identical.

Only MIDI presentation changes.

Possible internal adaptations may include:

- Note On timing
- Note Off timing
- overlap bias
- role handoff timing
- candidate confirmation timing

These implementation details remain hidden from users.

Instead, Tonic Response provides one intuitive musical control whose default position is Stable.

The objective is compatibility across synthesizers while preserving one stable core engine.

---

# Design Principle

MonoGlide Matrix should expose:

- Musical controls

while hiding:

- Engineering controls

The more sophisticated the engine becomes internally, the simpler the front panel should become.
