+++
title = "OSC"
description = "Reference for enabling Maolan OSC support, transport command addresses, listener settings, and the maolan-osc helper binary."
layout = "osc"
+++

# Maolan OSC

A compact Open Sound Control transport surface for session
navigation and playback, with a built-in listener and a small
command-line helper.

Default listener: `0.0.0.0:9000`  
Supported transport addresses: `5`  
Helper command aliases: `5`

[Enabling](#enabling) [Listener](#listener) [Commands](#commands) [Helper](#helper) [Notes](#notes)

## Enabling OSC

OSC is disabled by default and must be turned on from the GUI
preferences before the engine starts listening.

1. Open the preferences dialog from the GUI.
2. Turn on `Enable OSC`.
3. Save preferences so the engine starts its OSC listener thread.

### Config File

The preference is stored in:

`~/.config/maolan/config.toml`

## OSC Listener

Current support is intentionally small and transport-focused.

### Bind Address

The engine listens on:

`0.0.0.0:9000`

### Scope

- Designed for basic session navigation and playback.
- Only transport-oriented OSC addresses are currently accepted.

## Supported OSC Commands

These OSC addresses are accepted by the engine.

- `/transport/play`: start playback.
- `/transport/stop`: stop playback.
- `/transport/pause`: pause transport.
- `/transport/start`: jump to session start.
- `/transport/end`: jump to session end.

### Compatibility Aliases

- `/transport/jump_to_start` and `/transport/start_of_session` also jump to the session start.
- `/transport/jump_to_end` and `/transport/end_of_session` also jump to the session end.

## `maolan-osc` Helper

The repository includes a small helper binary that accepts exactly
one argument.

### Accepted Arguments

- `play`
- `stop`
- `pause`
- `start`
- `end`

### Command Mapping

- `play` sends `/transport/play`
- `stop` sends `/transport/stop`
- `pause` sends `/transport/pause`
- `start` sends `/transport/start`
- `end` sends `/transport/end`

### Examples

```
cargo run --bin maolan-osc -- play
cargo run --bin maolan-osc -- stop
cargo run --bin maolan-osc -- pause
cargo run --bin maolan-osc -- start
cargo run --bin maolan-osc -- end
```

## Notes

- OSC only starts after it is enabled in preferences and the setting has been saved.
- If OSC is disabled, `maolan-osc` can still send
packets, but the engine will not be listening for them.
