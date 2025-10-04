# Repository Guidelines

## Project Structure & Module Organization
Core server sources live under `dix`, `os`, `mi`, and `hw`, mirroring the upstream Xorg layout. Android-specific glue, build scripts, and assets sit in `android/`, `Xsdl/`, and `data/`. Protocol extensions and drivers reside in `Xext/`, `Xi/`, `glx/`, and related subdirectories; use these as references when adding new modules. Tests for low-level subsystems are collected in `test/`, with further fixtures under `test/xi2/`. Docs and manpages live in `doc/` and `man/`.

## Build, Test, and Development Commands
Run `./autogen.sh && ./configure` from the repo root for a desktop-style build, then execute `make -j$(nproc)` and `make install` as needed. For the Android target, prefer `./configure-xsdl.sh`, which sets the SDL flags and launches a parallel `make`. Use `nice -n19 make -j$(nproc)` when iterating to match the shipped scripts. Launch the packaged server with `./start.sh` to confirm runtime behaviour.

## Coding Style & Naming Conventions
Follow the historical Xorg C style: tabs for indentation, braces on their own line, and 80-column conscious wrapping. Prefer descriptive `snake_case` for new identifiers and keep macros all caps. Reuse helper patterns from neighbouring modules before introducing new abstractions. Maintain `#include` order: local headers last, system headers first. Apply `clang-format` only when a directory already contains `.clang-format`; otherwise match the surrounding file manually.

## Testing Guidelines
Unit-style checks rely on GLib's gtest harness. Run `make check` inside the `test/` directory (or from the root) to build and execute all suites. Name new test binaries after their subsystem (e.g., `xi2-devices.c`) and document intent in `test/README`. Use `g_test_add()` patterns consistent with adjacent files, and run targeted binaries (such as `./test/xkb`) for focused debugging.

## Commit & Pull Request Guidelines
Keep commits focused and message subjects under 72 characters (e.g., "Fix NDK r12 include path"). Reference the subsystem first when practical (`Xi:`, `glx:`) so history stays searchable. Describe user-visible impact in the body and note any Android-specific considerations. Pull requests should summarise motivation, list manual test results (`start.sh` or unit suites), and link related issues or external patches from the SDL Android repo when relevant.

## Security & Configuration Tips
Avoid committing generated assets under `data/usr/`. Store API keys or certificates outside the tree; scripts expect environment variables for sensitive paths. When modifying input device handling, double-check default permissions and pointer confinement logic in `hw/` before shipping.

## Coordination & Record-Keeping
Every repository change is mirrored in the `Book/` directory: first-person chapters (`Book/%d - <Title>.md`) detail context, while matching ledgers (`Book/<Title>.md`) capture bullet summaries. Consult `Book/AGENTS.md` for the current logging roles and responsibilities when contributing to the narrative.
