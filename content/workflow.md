+++
title = "Workflow"
description = "Operational reference for Maolan covering plugin routing, sidechains, session storage, templates, autosave recovery, diagnostics, and export behavior."
layout = "workflow"
+++

_Workflow reference. Operations updated 2026-04-30. Routing updated 2026-03-21._

# Maolan Workflow

A practical map of how Maolan sessions move from track setup and
plugin routing to autosave recovery, diagnostics, templates, and
final export.

[Explore Signal Flow](#signal-flow)

[See Recovery Path](#recovery)

At a glance

15s

Autosave cadence

3

Plugin host formats on Linux / FreeBSD

5

Core session subdirectories

3

Diagnostics bundle files

[Signal Flow](#signal-flow)

[Storage](#storage)

[Templates](#templates)

[Recovery](#recovery)

[Diagnostics](#diagnostics)

[Platform Notes](#platform)

## Routing and Signal Flow

Maolan uses a per-track plugin graph rather than a fixed insert
chain. Audio and MIDI paths are explicit, so the graph itself is
the workflow.

### Track Graph Model

Sidechains in orange

#### Default track state

New tracks start with passthrough audio routing from input to
output, plus MIDI passthrough on the first input/output path.

#### Plugin loading

Loading a plugin does not rewrite existing connections. The
graph decides what actually receives or emits signal.

#### Per-clip FX

Audio clips on supported Unix builds can open their own
plugin graph. If a clip has no saved graph yet, Maolan
seeds a default passthrough clip graph first.

#### Audio nodes

Main ports and extra auxiliary or sidechain ports are shown
separately so unusual bus layouts remain visible.

#### MIDI nodes

MIDI input and output are explicit too. If a plugin emits
MIDI, it must be routed onward to keep events moving.

### Typical Graph Paths

Serial insert

`Track Input -> Plugin A -> Plugin B -> Track Output`

Sidechain

Keep the main audio input connected to the plugin’s main
input, then route the source track or upstream output into
the orange sidechain input separately.

MIDI instrument / effect

Connect MIDI source to the plugin, then wire the plugin’s
audio output to the next stage or track output. Route MIDI
output explicitly if the plugin generates events.

Grouped audio render order

Child clips render first, then group-level fades are
applied, and finally the group clip plugin graph is
processed.

## Session Storage

Sessions are directory-based, with the main project state in
`session.json` and supporting assets stored alongside
it.

### Session Layout

`session.json`

Tracks, clips, connections, plugin graph topology, plugin
state, transport state, metadata, export settings, MIDI
learn bindings, clip-group membership in
`grouped_clips`, per-audio-clip plugin graph
state in `plugin_graph_json`,
pitch-correction segment settings, and UI sizing values.

`audio/`,

,

`midi/`,

,

`peaks/`,

,

`pitch/`,

,

`plugins/`

Imported media, waveform peak cache, cached pitch analysis
data, and plugin session assets live inside the session
directory.

`.maolan_autosave/snapshots/`

Timestamped recovery snapshots written beside the live
session.

### App-Level State

Config file

`~/.config/maolan/config.toml`

If the file does not exist, Maolan creates it on startup with
defaults. It stores UI sizing, export defaults, preferred I/O
devices, snap mode, bit depth, and recent session paths.

- font_size
- mixer_height
- track_width
- default_export_sample_rate_hz
- default_snap_mode
- default_audio_bit_depth
- default_output_device_id
- default_input_device_id
- recent_session_paths

#### Recent sessions

Stored in `recent_session_paths`, normalized
before display, deduplicated, pruned when invalid, and
capped to the configured limit.

#### Session save/load

Saved sessions restore plugin graph topology, connections,
plugin state, clip groups, and per-audio-clip plugin graphs
rather than just track ordering.

## Templates and Restore Paths

Maolan separates full-session reuse from single-track reuse, while
keeping graph state part of both save and restore workflows.

### Session templates

`~/.config/maolan/session_templates/<name>/`

Each template includes `session.json` and the same
supporting subdirectories as a normal session.

- Includes track structure, routing, plugin graphs, plugin state, metadata, and export settings.
- Excludes audio clips, MIDI clips, frozen render state, and frozen backups.

### Track templates

`~/.config/maolan/track_templates/<name>/`

A track template stores `track.json` and a
`plugins/` directory for one reusable channel strip.

- Includes track settings, plugin graph, plugin state, and connections involving that track.
- Excludes audio clips and MIDI clips.

### Group templates

`~/.config/maolan/group_templates/<name>/`

Each group template stores `group.json` plus a `plugins/` directory. Group templates keep:

- all tracks that share the same VCA master (group)
- each track's settings and plugin graph
- connections between tracks in the group

When a group template is loaded from the Add Track dialog:

- the base name you enter becomes the new group name (VCA master)
- each track in the group is created with the base name as a prefix
  - single-track groups use the base name directly
  - multi-track groups use `"<base> <original>"`
- plugin graphs are restored per track
- intra-group connections are remapped to the new track names
- all tracks in the group are automatically assigned to the new VCA master

Group templates intentionally do not keep:

- audio clips
- MIDI clips
- frozen render state or frozen backups
- connections to tracks outside the group

### Cross-format restore behavior

Mixed Unix graphs containing LV2 and CLAP plugins are restored in
saved order. VST3 state is preserved through the current host
restore path as well, so sessions and templates retain routing
intent instead of reconstructing a simplified chain.

## Autosave and Recovery

Recovery is built around timestamped snapshots saved inside the
session folder, with the newest valid snapshot preferred first.

1

### Snapshot generation

Every 15 seconds, Maolan writes a snapshot to
`<session>/.maolan_autosave/snapshots/<timestamp>/`.

2

### Validity check

A snapshot is considered valid when it contains
`session.json`.

3

### Startup / open comparison

On startup or open, Maolan can detect when the latest
autosave is newer than the live session file.

4

### Preview and restore

Recovery preview summarizes track, audio-clip, and MIDI-clip
count deltas. If newer snapshots fail to restore, older
snapshots are offered as fallback candidates. Restoring a
snapshot loads it as current state and marks the session as
unsaved.

### Recovery boundaries

#### Newest-first sorting

Snapshot selection prefers the most recent valid entry rather
than trying to merge newer pieces from multiple points in
time.

#### Unsaved state after restore

Restore is a recovery load, not a silent replacement of the
live project file, so the session remains dirty until saved.

#### Undo / redo note

Graph connection edits and plugin load or unload actions are
covered by undo and redo. Low-level state-restore mechanics
are internal replay behavior, not user-facing history items.

- Playback/record toggles are intentionally non-undoable.
- Loop and punch-range actions are also excluded from history
to avoid noisy playback-driven snapshots.
- Track automation level, balance, and mute updates are runtime
path edits and are also excluded.
- Transport/query/report actions are intentionally kept out of
history.
- High-frequency automation playback adjustments are also
excluded to avoid history noise.
- MIDI learn arm actions are excluded; actual mapping changes
remain history-recorded.
- Plugin discovery and host-state introspection actions are not
recorded in history.

## Diagnostics and Export

The workflow closes with session inspection and multi-format export
behavior that persists relevant settings back into the project.

### Diagnostics bundle

UI reports

Session Diagnostics, MIDI Mappings Report, and Export
Diagnostics Bundle are exposed in the interface.

Bundle location

Written to
`<session>/maolan_diagnostics_<unix-seconds>/`
when a session is open, otherwise to
`/tmp/maolan_diagnostics_<unix-seconds>/`.

Bundle contents

`session_diagnostics.txt`,
`midi_mappings.txt`, and
`ui_summary.json`.

### maolan-generate operations

GUI launch path

The desktop app launches the local
`maolan-generate` binary and exchanges progress
and result data over a socketpair IPC path.

Runtime split

Prompt generation runs in a dedicated HeartMuLa
token-generation subprocess, then decode continues
in-process with HeartCodec.

Model resolution

Without `--model-dir <path>`, model assets
are resolved through the Hugging Face cache using
`hf-hub`.

Expected repos and files

HeartMuLa uses
`maolandaw/HeartMuLa-happy-new-year-burn` or
`maolandaw/HeartMuLa-RL-oss-3B-20260123` with
`heartmula.bpk`, `tokenizer.json`,
and `gen_config.json`. HeartCodec uses
`maolandaw/HeartCodec-oss-20260123-burn` with
`heartcodec.bpk`.

CLI boundaries

The current CLI supports
`--model <happy-new-year|RL>`,
`--length` in milliseconds,
`--decode-only` with
`--frames-json`, and decode-only worker control
through `--decode-threads`.

### Pitch-correction cache and render flow

Session requirement

Opening pitch correction for an audio clip requires a
saved/opened session.

Cached source analysis

Analysis results are cached under `pitch/` and
reused for matching source file+offset+length combinations.

Apply and playback

Apply saves correction points/settings to the clip and keeps
source audio untouched while enabling real-time playback.

Freeze pipeline

Frozen audio previews render through the pitch-corrected
signal path so exported/flattened results match.

### Export behavior

Mixdown and stems

Mixdown renders selected hardware output ports. Stem export
writes one file per eligible selected track into
`<base>_stems/`.

Formats and limits

Multiple output formats can be written in a single export
run. MP3 export is limited to mono or stereo output.

Persisted settings

Normalization and master-limiter settings are stored in the
session file along with export dialog settings.

## Project Structure

The codebase is split across multiple repositories:

- `daw/` — Main application and GUI
- `engine/` — Audio engine
- `widgets/` — Reusable iced widgets
- `generate/` — AI audio generation via `maolan-generate`
- `mixosc/` — OSC mixing control integration

## Build and Test

- Code coverage is tracked and reported.
- Unit test coverage has been expanded across the codebase.
- Cleanup and dead-code removal passes are performed regularly.

## Recent Fixes

- Fixed note names display in the piano roll.
- Fixed GitHub Actions workflow configuration.

## Platform Notes

Runtime behavior differs by OS and plugin host path, which affects
how the workflow starts before any session is even opened.

### Linux / FreeBSD

Startup forces the X11 backend by unsetting
`WAYLAND_DISPLAY` and `WAYLAND_SOCKET`.
Plugin discovery runs for LV2, VST3, and CLAP.

### Debugging

Passing `--debug` enables tracing output to stdout for
low-level startup and runtime inspection.

### macOS host discovery

macOS builds support CLAP and VST3 discovery on startup, but
they do not use the Unix LV2 host path.
