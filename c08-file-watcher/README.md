# 08 - File Watcher

## Description
File system watcher similar to `fswatch` or `inotifywait`. Detects changes in files and directories.

## Usage Example
```bash
$ file-watcher ./src --extensions rs,toml,md
Watching ./src...
[Modified]   src/main.rs       14:32:05
[Created]    src/new_file.rs   14:35:22
[Deleted]    src/old_file.rs   14:40:01
[Modified]   Cargo.toml        14:42:33

$ file-watcher ./src --recursive --exclude "target/*,*.log"
```

## Concepts to Learn
- File system events with the `notify` crate
- Event debouncing
- Recursive watching
- Configurable filters and patterns
- Path normalization

## Debouncing Example
```rust
use std::time::{Duration, Instant};
use std::collections::HashMap;

struct Debouncer {
    events: HashMap<PathBuf, Instant>,
    delay: Duration,
}

impl Debouncer {
    fn should_emit(&mut self, path: &Path) -> bool {
        let now = Instant::now();
        
        if let Some(last) = self.events.get(path) {
            if now.duration_since(*last) < self.delay {
                self.events.insert(path.to_path_buf(), now);
                return false; // Too soon, suppress
            }
        }
        
        self.events.insert(path.to_path_buf(), now);
        true
    }
}
```

## Suggested Structure
```
file-watcher/
|-- src/
|   |-- main.rs
|   |-- watcher.rs      # Main watcher
|   |-- debouncer.rs    # Event debouncing
|   |-- filter.rs       # File filters
|   `-- printer.rs      # Formatted output
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `notify` - File system events
- `clap` - Argument parsing
- `chrono` - Timestamps

## Resources
- [notify crate](https://docs.rs/notify/)
- [Rust fs module](https://doc.rust-lang.org/std/fs/)

## Difficulty Level
2/5 - Medium, requires understanding OS events
