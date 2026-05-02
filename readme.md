# Lyra Synthesizer Module

Lyra is a high-performance procedural synthesis engine and musical DSL integrated into the Aether Engine. Unlike traditional trackers or MIDI systems, Lyra leverages the power of C++20 to describe musical structures as a typed AST (Abstract Syntax Tree) directly at compile-time.

## Design Philosophy

*   Zero-Allocation Parsing (CT): No real-time string parsing. Note, Bar, and Sentence structures are resolved by the compiler at Compile-Time.
*   DOD-Aligned (Data Oriented Design): Patterns expand into flat, cache-friendly arrays of audio events (Event).
*   Composite + Decorator: Music is built as a hierarchy of components wrapped in filters.
*   Pipes & Filters: Flexible signal processing via the pipeline operator |.

## DSL Anatomy

### AST Hierarchy
Composition (Master) -> Track (Channel) -> Sentence (Cycle) -> Bar (Step) -> Note (Leaf)

### Syntax Example
```cpp
Track subKick = Note{"c0"}
    | Synth{"supersaw"}
    | Slow{8}
    | Lpf{140}
    | Gain{0.28f};
```

## Gameplay Integration (State Sync)

Lyra allows coupling sound parameters with game state without using callbacks through dynamic filters:

*   Trigger: Binary on/off state based on events.
*   Leveler: Continuous volume/intensity changes (e.g., based on player health).
*   Switcher: Switching music layers based on game state (e.g., transitioning to "combat mode").

```cpp
Track guitars = riffA 
    | Switcher{"combat_intensity", 2} 
    | Leveler{"player_speed"};
```

## Architecture

*   Core: Base data types (Event, TimeRange, Pattern).
*   DSL: A set of structures for describing musical logic.
*   SynthEngine: Host for miniaudio, wave generators, and DSP filters.
