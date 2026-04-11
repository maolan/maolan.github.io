+++
title = "Shortcuts"
description = "Reference for Maolan keyboard shortcuts, mouse gestures, transport controls, piano roll editing, markers, tempo editing, and plugin graph actions."
layout = "shortcuts"
+++

Reference Sheet

Updated 2026-03-21

# Maolan Shortcuts

Keyboard commands and pointer gestures for session management, transport, clip editing, markers, zoom controls, timing lanes, piano roll work, and plugin routing.

22

Keyboard shortcuts

63

Mouse and editing gestures

4

Context menu action groups

[Keyboard](#keyboard)

[Workspace](#workspace)

[Timeline](#timeline)

[Markers](#markers)

[Piano Roll](#pianoroll)

[Plugin Graph](#graph)

[Notes](#notes)

## Keyboard Shortcuts

Core commands for session control, playback, and piano-note processing.

### Global / Session

Ctrl+N

New session

Ctrl+O

Open session

Ctrl+S

Save session

Ctrl+Shift+S

Save session as

Ctrl+I

Import files

Ctrl+E

Open export dialog

Ctrl+T

Add track

Ctrl+R

Toggle session record arm

Ctrl+L

Send transport panic / MIDI panic

Ctrl+Z

Undo

Ctrl+Shift+Z

Redo

Ctrl+Y

Redo

Delete

Backspace

Remove selected item(s)

Escape

Cancel or clear the current context-dependent interaction

### Transport

Space

Toggle play/stop

Shift+Space

Pause

Home

Rewind to start

End

Rewind to end

### Piano Tools

Q

Quantize selected notes

H

Humanize selected notes

G

Groove selected notes

## Workspace and Track List

Track selection, track management, and context actions from the main editor.

Left click track

Select track

Ctrl+Left click track

Add track to the current selection

Double click track

Open track plugin/routing graph

Right click track

Open track context menu

Drag track (grab track body)

Reorder track

Drag bottom track edge

Resize track height

### Track Context Menu

- Automation lanes, rename, sends and returns, MIDI learn, freeze or flatten, template save, and grouping or VCA actions depending on track state

## Timeline, Selection, and Editing

Clip manipulation, selection gestures, ruler operations, zoom controls, and timing-lane edits.

### Timeline Clips

Left click clip

Select clip

Left click empty editor

Deselect clips

Left drag clip

Drag/move clip (or group if multi-selected)

Ctrl + drag clip

Copy clip while dragging

Drag clip left/right edge

Resize clip bounds

Shift + drag clip left/right edge

Stretch audio clip with Rubber Band on drop (when available)

Snapping is applied during drag-and-drop based on the current snap mode; the "Clips" snap mode enables snapping to other clip start/end positions

Drag fade handles

Resize fade-in/fade-out

Middle click clip

Split clip at current cursor or snap position

Double click MIDI clip

Open MIDI piano roll

Double click audio clip

Open per-clip plugin graph on supported Unix builds

Right click clip

Open clip context menu

### Clip Context Menu

- Group/ungroup
- Rename
- Take-lane controls
- Mute/unmute
- Fade toggle
- Audio warp actions for audio clips
- Pitch correction

### Pitch Correction

Right click audio clip → Pitch Correction

Open pitch-correction editor when transport is stopped

Left click pitch segment

Select one segment

Shift + Left click pitch segment

Add/remove a segment from the selection

Left drag selected pitch segment(s)

Retarget one or many selected segments vertically

Left drag empty area

Box-select pitch segments

Shift + Left drag empty area

Add box-selected segments to current selection

Double click pitch segment

Snap clicked segment(s) to nearest semitone

Ctrl+Z

/

Ctrl+Shift+Z

/

Ctrl+Y

in pitch view

Undo and redo local correction edits

### Selection Gestures

Left drag on empty editor

Marquee clip selection rectangle

Right drag on MIDI lane

Create empty MIDI clip

### Ruler (Top Timeline)

Left click ruler

Move transport playhead

Left drag ruler

Set loop range (snap-aware, supports "Clips" snap mode)

Right click ruler

Clear loop range

### Tempo / Time Signature Lane

Left click marker

Select marker

Shift+Left click marker

Add/remove marker from selection

Left drag selected marker(s)

Move marker(s) in time (supports "Clips" snap mode)

Left click empty timing lane

Clear timing selection and move the playhead

Left drag empty timing lane

Clear timing selection and set punch range (supports "Clips" snap mode)

Right click marker

Open marker context menu: duplicate, reset to previous, or delete

Right click empty timing lane

Clear timing selection and clear punch range

Right drag empty timing lane

Clear timing selection and set punch range (supports "Clips" snap mode)

Middle click on tempo lane

Add tempo point (supports "Clips" snap mode)

Middle click on time-signature lane

Add time-signature point (supports "Clips" snap mode)

Middle drag an existing punch-range edge

Adjust punch start or end (supports "Clips" snap mode)

Mouse wheel over left control zone on tempo row

Adjust tempo

Mouse wheel over left control zone on time-signature row, left half

Adjust numerator

Mouse wheel over left control zone on time-signature row, right half

Adjust denominator

### Zoom Controls

Main editor zoom

Bottom-right horizontal slider

Piano roll horizontal zoom

Bottom slider in the MIDI editor

Piano roll vertical zoom

Right-side vertical slider in the MIDI editor

## Track Header Markers

Marker creation, repositioning, and deletion from the header area.

Right click empty marker/header area

Open the create-marker dialog at the snapped timeline position

Left drag marker

Move marker horizontally; snapping is applied on drop (supports "Clips" snap mode)

Right click marker

Rename marker

Middle click marker

Delete marker

## Piano Roll

Note editing, note creation, selection, and controller lane manipulation.

Click/drag notes

Select and move notes

Drag note edge

Resize note start/end

Left drag empty area

Box-select notes

Right drag empty area

Create notes

Middle click note

Delete note

Mouse wheel over note

Adjust note velocity

### Controller and SysEx Lanes

Controller lanes

Left drag adjusts a point/value, middle click/drag erases, right drag draws

Mouse wheel over controller event

Adjust controller value

SysEx lane

Left drag moves a SysEx event, double click opens the SysEx editor

## Plugin Graph

Routing and graph interactions for plugins, tracks, audio, and MIDI connections.

Double click track

Open the track plugin/routing graph

Drag plugin node

Move plugin node in graph

Drag from port to port

Create audio or MIDI connection

Select connection + delete

Remove selected graph connection

Select plugin + delete

Remove selected plugin instance

## Notes

- Current keyboard handling is Ctrl-based in code paths, including on macOS builds.
- Some actions depend on the current view, active tool, and selection state.
- Q/H/G apply to the current selected notes in the piano roll.
- Audio stretch requires `rubberband` to be available on `PATH`; pitch correction uses per-session cached analysis and local undo/redo in its own editor.
- Group is only enabled when two or more non-group clips of the same type are selected on the same track, and grouped clips must be ungrouped before splitting.
- Per-clip plugin graphs are audio-only and currently available on supported Unix non-macOS builds.
- The main editor zoom is geometric rather than linear, so equal slider movement produces equal zoom-ratio changes.
- Undo/redo in the pitch-correction view only changes local correction history; it is separate from global history.
- Maolan also ships a `maolan-cli` binary that can play, pause, stop, send panic, and export the current session.
