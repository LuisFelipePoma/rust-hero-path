# Rust Learning Path: Personal Projects

A curated collection of 16 hands-on Rust projects, ordered by difficulty, to help you progress from beginner fundamentals to systems-level programming.

---

## Path Structure

```
Level 1: CLI Fundamentals              -> 4 projects
Level 2: Networking and Concurrency    -> 4 projects
Level 3: Data Processing               -> 3 projects
Level 4: Persistence and State         -> 3 projects
Level 5: Systems and Unsafe Rust       -> 2 projects
```

---

## Level 1: CLI Fundamentals

| # | Project | Core Concepts |
|---|---------|---------------|
| 01 | [wc-clone](./01-wc-clone) | File I/O, iterators, `Result` |
| 02 | [grep-clone](./02-grep-clone) | Regex, lifetimes |
| 03 | [password-generator](./03-password-generator) | Random generation, string manipulation |
| 04 | [rpn-calculator](./04-rpn-calculator) | Stack, parsing, enums |

**You will practice**: ownership, borrowing, lifetimes, error handling, iterators.

---

## Level 2: Networking and Concurrency

| # | Project | Core Concepts |
|---|---------|---------------|
| 05 | [chat-tcp](./05-chat-tcp) | Threads, channels, `Arc`, `Mutex` |
| 06 | [http-server](./06-http-server) | TCP parsing, HTTP protocol basics |
| 07 | [web-scraper](./07-web-scraper) | Async execution, rate limiting, HTML parsing |
| 08 | [file-watcher](./08-file-watcher) | File system events, debouncing |

**You will practice**: multithreading, socket programming, async fundamentals.

---

## Level 3: Data Processing

| # | Project | Core Concepts |
|---|---------|---------------|
| 09 | [csv-tool](./09-csv-tool) | Generics, traits, iterator adapters |
| 10 | [md2html](./10-md2html) | Recursive descent parsing, AST |
| 11 | [image-processor](./11-image-processor) | Image transformations, pixel processing |

**You will practice**: parsing, serialization, trait bounds, data transformations.

---

## Level 4: Persistence and State

| # | Project | Core Concepts |
|---|---------|---------------|
| 12 | [kv-store](./12-kv-store) | Binary storage, B-trees, WAL |
| 13 | [task-runner](./13-task-runner) | YAML parsing, DAGs, topological sort |
| 14 | [notes-app](./14-notes-app) | CRUD operations, full-text search, interactive CLI |

**You will practice**: serialization, state management, data modeling.

---

## Level 5: Systems and Unsafe Rust

| # | Project | Core Concepts |
|---|---------|---------------|
| 15 | [chip8-emulator](./15-chip8-emulator) | CPU emulation, timing loops, graphics |
| 16 | [custom-allocator](./16-custom-allocator) | `GlobalAlloc`, memory management |

**You will practice**: unsafe Rust, systems programming, hardware abstraction.

---

## Suggested Timeline

```
Weeks 1-2   -> 01, 02 (fundamentals)
Weeks 3-4   -> 03, 04 (data manipulation)
Weeks 5-6   -> 05, 06 (concurrency and networking)
Weeks 7-8   -> 07, 08 (larger end-to-end projects)
Weeks 9-10  -> 09, 10 (parsing and transformation)
Weeks 11-12 -> 11, 12, 13 (persistence)
Weeks 13-14 -> 14 (complete application)
Weeks 15-16 -> 15, 16 (advanced systems level)
```

---

## Learning Resources

### Getting Started
- [The Rust Programming Language](https://doc.rust-lang.org/book/) - Official Rust book
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/) - Practical examples
- [Rustlings](https://github.com/rust-lang/rustlings) - Interactive exercises

### Recommended Focus by Level
- **Level 1**: Standard library, `Option`, `Result`
- **Level 2**: `std::thread`, `std::sync`, Tokio documentation
- **Level 3**: Serde, parser design patterns
- **Level 4**: Binary formats, filesystem APIs
- **Level 5**: Unsafe Rust guidelines, OS fundamentals

---

## Before You Start

1. Install Rust:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

2. Set up your editor (VS Code + rust-analyzer recommended).

3. Learn the essential Cargo commands:

```bash
cargo new <project>    # Create a new project
cargo run              # Run the project
cargo test             # Execute tests
cargo build --release  # Build optimized binary
cargo doc --open       # Open local documentation
```

---

## Golden Rule

> Pick 2-3 projects you genuinely care about. Interest sustains motivation when ownership errors hit late at night.

Do not try to complete all 16 projects at once. It is better to finish a few projects thoroughly with tests and documentation than to leave many half done.

---

## Contributing

If you build one of these projects and want to share it:

1. Publish the repository on GitHub.
2. Add automated tests.
3. Include documentation tests using `#[test]` and `assert!` where relevant.
4. Publish as a crate if reusable: `cargo publish`.

---

*Last updated: April 2026*
