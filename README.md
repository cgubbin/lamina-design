# Rust Project

## Overview

This template provides a modern Rust development environment using:

- **Nix flakes** for reproducibility
- **Fenix** for a consistent Rust toolchain
- **Crane** for Nix-native Cargo builds and checks
- **Cargo** for standard Rust workflows
- **just** for ergonomic command execution

It includes:

- a **library target** (`src/lib.rs`)
- a **binary target** (`src/main.rs`)
- a **CLI interface** using `clap`
- common tooling for testing, linting, coverage, and benchmarking

---

## Tooling Architecture

This template intentionally separates concerns:

### Toolchain (Fenix)

Provides:

- `rustc`
- `cargo`
- `clippy`
- `rustfmt`
- `rust-analyzer`

This is the **single source of truth** for Rust tooling.

---

### Build System (Cargo)

Used for:

- building (`cargo build`)
- running (`cargo run`)
- testing (`cargo test`)
- benchmarking (`cargo bench`)

---

### Nix Integration (Crane)

Used for:

- reproducible builds (`nix build`)
- CI-style checks (`nix flake check`)

Crane ensures:

- consistent builds across machines
- dependency caching
- integration with Nix CI pipelines

---

### Task Runner (`just`)

Provides:

- short, memorable commands
- a unified developer interface

---

## Getting Started

Enter the development shell:

    nix develop

Or enable automatic loading:

    direnv allow

Verify setup:

    just init

Run the application:

    just run

---

## Project Layout

    .
    ├── flake.nix
    ├── justfile
    ├── Cargo.toml
    ├── rust-toolchain.toml
    ├── .cargo/
    ├── src/
    │   ├── lib.rs
    │   ├── main.rs
    │   └── cli.rs
    ├── tests/
    ├── benches/
    └── .config/

- `lib.rs` — reusable library logic
- `main.rs` — application entrypoint
- `cli.rs` — command-line interface
- `tests/` — integration tests
- `benches/` — Criterion benchmarks

---

## Common Workflows

### Run the application

    just run

Pass CLI arguments:

    just run --name Alice

JSON output:

    just run-json Alice

---

### Development loop

    just check
    just test
    just run

---

### Formatting and linting

Format code:

    just fmt

Check formatting:

    just fmt-check

Run clippy:

    just lint

---

### Testing

Run standard tests:

    just test

Run nextest (faster):

    just nextest

---

### Benchmarking

    just bench

---

### Coverage

Run coverage:

    just coverage

If this fails on your platform:

    just coverage-llvm

Note:

- Default Tarpaulin backend uses `ptrace` (Linux x86_64 only)
- LLVM backend works more broadly

---

## Nix-Based Workflows

### Build with Nix (Crane)

    nix build

This builds the crate in a fully reproducible environment.

---

### Run all checks

    nix flake check

This runs:

- build
- clippy
- tests
- docs
- formatting checks

This is equivalent to a CI pipeline.

---

### When to use Cargo vs Nix

| Task                | Tool     |
|--------------------|----------|
| Fast iteration     | Cargo    |
| CI / reproducible  | Nix/Crane |
| Developer commands | just     |

---

## CLI Example

    just run --name Alice
    Hello, Alice!

    just run-json Alice
    {"message":"Hello, Alice!"}

---

## Reproducibility

- `flake.lock` pins:
  - Rust toolchain (via Fenix)
  - system dependencies
- `Cargo.lock` pins crate versions
- Crane ensures consistent builds across machines

---

## Philosophy

- **Fenix defines the toolchain**
- **Cargo is the source of truth for builds**
- **Crane integrates Cargo into Nix**
- **just provides ergonomics**

Each tool has a single, clear responsibility.

---

## Next Steps

Typical extensions:

- add subcommands via `clap`
- introduce structured logging (`tracing`)
- split into multiple crates (workspace)
- add external dependencies via `pkg-config` + Nix
- integrate CI using `nix flake check`

---

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or
  <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or <https://opensource.org/licenses/MIT>)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
