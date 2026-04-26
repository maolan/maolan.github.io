+++
title = "Maolan"
description = "A modern Rust-based DAW for recording, MIDI editing, automation, and music production. Open source, transparent, and community-driven."
layout = "index"
+++

# Maolan

> Open source digital audio workstation built in Rust for recording, MIDI editing, automation, routing, and modern production workflows.

![Maolan logo mark](/img/logo.svg)

[View on GitHub](https://github.com/maolan) [Browse Features](/features/) [Read Workflow Notes](/workflow/)

## See Maolan in Action

Explore the interface, workflow, and production tools already available in Maolan.

![Maolan workspace interface](/img/workspace.gif)

### Multi-Track Timeline

Organize and arrange tracks with precise timing control.

### Piano Roll Editor

Edit MIDI with note, velocity, and automation detail.

### Mixer & Routing

Build flexible routing chains for mixing, sends, and plugin workflows.

## The Challenge

- **Closed ecosystems:** most modern DAWs are proprietary, so you cannot inspect or adapt the internals.
- **High cost:** license pricing keeps professional tools out of reach for many students, hobbyists, and developers.
- **Limited flexibility:** custom workflows and deeper integrations are hard to build without source access.

## The Solution

Maolan is a free, open-source DAW built in Rust. It prioritizes transparency, performance, and community-driven development.

Inspect the code. Build custom features. Contribute improvements. Own your tools.

- ✓ 100% open source under a permissive license
- ✓ Built with Rust for safety and performance
- ✓ Community-driven development and improvements
- ✓ Forever free, no licensing fees

```bash
git clone https://github.com/maolan
cd maolan
cargo run --release
```

Ready to use. Ready to extend.

## Key Capabilities

### Multi-Track Audio & MIDI

Record and arrange unlimited audio and MIDI tracks with precise
timing and flexible mixing.

### Piano Roll MIDI Editing

Intuitive piano roll interface for composing, editing, and
refining MIDI performances.

### Automation & Envelopes

Create track and plugin automation for volume, balance, mute, and
loaded CLAP, VST3, or LV2 parameters. Save session and track
templates for repeatable setups.

### Plugin Hosting & Routing

Load CLAP, VST3, and LV2 plugins, create complex routing chains,
and design custom signal flows with explicit audio, MIDI, and
sidechain paths.

### Export & Format Support

Export mixdowns or stems to WAV, MP3, OGG, and FLAC with
normalization and master-limiter options saved in the session.

### Autosave & Recovery

Automatic project backups and recovery features protect your work
from unexpected interruptions.

## Complete Production Workflow

1. **Recording:** capture audio and MIDI from microphones, instruments, and controllers.
2. **Editing and composition:** shape clips, notes, harmonies, and arrangements with piano-roll tools.
3. **Plugin and routing:** load CLAP, VST3, and LV2 plugins and connect explicit audio, MIDI, and sidechain paths.
4. **Automation:** write volume, pan, send, and plugin automation across the timeline.
5. **Mixing and mastering:** balance levels and processing for a final production-ready session.
6. **Export:** render full mixes or stems to standard delivery formats.

## Open Source & Community Driven

### Why Open Source?

Maolan is open source because audio production tools should be transparent, accessible, and shaped by their users.

You can inspect the code, understand how features work, and contribute improvements directly.

- **Contribute code** to core features and plugins.
- **Report bugs** and propose improvements.
- **Write documentation** and tutorials.
- **Build extensions** for your own workflow.

### Project Stats

| Item | Value |
| --- | --- |
| GitHub | `github.com/maolan` |
| License | `BSD-2-Clause` |
| Language | `Rust` |
| Status | `Active Development` |

## Get Started with Maolan

- [Visit GitHub](https://github.com/maolan)
- [Review the feature set](/features/)
- [Read workflow reference](/workflow/)
- [Check keyboard shortcuts](/shortcuts/)
- [Explore OSC support](/osc/)

## Frequently Asked Questions

### Is Maolan free?

Yes. Maolan is completely free and open source with no licensing fees, subscriptions, or paywalls.

### Which platforms are supported?

Maolan supports Linux, FreeBSD, and macOS. Linux and FreeBSD currently force the X11 backend at startup, while macOS uses the native host path.

### Which plugin formats are supported?

Maolan currently supports CLAP, VST3, and LV2 on Linux and FreeBSD. macOS builds support CLAP and VST3, while LV2 remains Unix-only in the current codebase.

### How does autosave and recovery work?

Maolan writes autosave snapshots every 15 seconds into `.maolan_autosave/snapshots/` beside the live session, for example:

`<session>/.maolan_autosave/snapshots/<timestamp>/`

When opening a session, Maolan can detect a newer snapshot, preview the differences, and recover the latest valid autosave first.

### Does Maolan support templates?

Yes. Session templates preserve track structure, routing, plugin
graphs, plugin state, metadata, and export settings. Track
templates preserve one track's settings, plugin graph, plugin
state, and that track's connections, while intentionally leaving
out audio and MIDI clips.

### How can I contribute?

There are many ways to contribute to Maolan:

- **Code:** Submit pull requests for features, bug
fixes, or improvements
- **Documentation:** Help improve guides,
tutorials, and API docs
- **Testing:** Report bugs, test features, and
provide feedback
- **Design:** Contribute UI/UX improvements and
design ideas
- **Community:** Help other users, answer
questions, and promote the project

Visit the GitHub repository to get started. All contributors are
welcome!

### Can I reuse it in my Rust project?

Maolan is a two-part project: engine and GUI. The engine is independent of the GUI and can be reused in your own Rust project.

```bash
cargo add maolan_engine
```
