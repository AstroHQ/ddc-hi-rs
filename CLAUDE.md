# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`ddc-hi` is a Rust crate providing high-level DDC/CI monitor controls. It abstracts communication with displays across multiple platforms (Linux i2c-dev, Windows WinAPI, macOS APIs, and NVIDIA NVAPI) for monitor control operations like brightness, contrast, and other VCP features.

## Build System

This project uses Cargo with Nix flakes for development environments:

- **Build**: `cargo build`
- **Test**: `cargo test --all-targets` (tests are run with no default features)
- **Format**: `cargo fmt` (uses custom .rustfmt.toml config)
- **Lint**: `cargo clippy`
- **Documentation**: `cargo doc` (use ONLY this command, no additional flags like --no-deps or --open)

The build system uses conditional compilation based on target platform and enabled features.

## Architecture

### Core Components

- **Display**: Main abstraction representing a connected monitor with enumeration capabilities
- **Handle**: Low-level communication wrapper supporting multiple backends (I2C, WinAPI, macOS, NVAPI)
- **DisplayInfo**: Container for display metadata (EDID, manufacturer, model, capabilities)
- **Backend**: Enum identifying the communication method used
- **Query**: System for filtering displays by various criteria

### Platform Support

The crate uses conditional compilation to support different platforms:
- Linux: `ddc-i2c` with i2c-dev driver
- Windows: `ddc-winapi` and optional `nvapi` for NVIDIA GPUs
- macOS: `ddc-macos`

Feature flags are dynamically enabled based on target platform via `build.rs`.

### Key Dependencies

- `ddc`: Core DDC/CI protocol implementation
- `edid`: EDID parsing for display identification
- `mccs*`: Monitor Control Command Set support
- Platform-specific DDC backends

## Development Workflow

1. Use `nix develop` for development environment (if using Nix)
2. Build with `cargo build` (no default features in tests)
3. Format code with `cargo fmt` (follows custom rustfmt configuration)
4. Test with `cargo test --all-targets`

The project includes CI checks for rustfmt, version consistency, and cross-platform compilation including Windows cross-compilation.

## Code Style

The project uses custom rustfmt configuration (.rustfmt.toml) with:
- 120 character line width
- Spaces instead of tabs (4 spaces)
- Grouped imports
- Unix line endings
- Various formatting preferences for consistency