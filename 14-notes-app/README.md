# 14 - Personal Notes Manager

## Description
Personal note-taking system with an interactive CLI, tags, search, and persistence.

## Usage Example
```bash
$ notes new "Ideas for the Rust project"
Created note: 2024-01-15-001
ID: 001
Title: Ideas for the Rust project
Created: 2024-01-15 14:30
Tags: none

$ notes list
001  Ideas for Rust project       2024-01-15  [rust, ideas]
002  Weekly meeting notes         2024-01-14  [work]
003  Cooking recipes              2024-01-13  [personal]

$ notes search "project"
001: Ideas for Rust project
    Content preview...

$ notes edit 001 --tag "rust,ideas,project"
Updated tags: rust, ideas, project

$ notes delete 002 --force
Permanently delete note 002? (y/N): y
Deleted: 002

$ notes list --tag "rust"
001  Ideas for Rust project       2024-01-15  [rust]
```

## Concepts to Learn
- File-based CRUD
- Full-text search
- Tags and categories
- Interactive CLI
- Data modeling

## Suggested Data Structure
```rust
#[derive(Serialize, Deserialize)]
struct Note {
    id: String,
    title: String,
    content: String,
    tags: Vec<String>,
    created: DateTime<Utc>,
    modified: DateTime<Utc>,
}

struct NoteStore {
    notes: HashMap<String, Note>,
    index: NoteIndex,  // For fast search
}

impl NoteStore {
    pub fn search(&self, query: &str) -> Vec<&Note> {
        self.notes.values()
            .filter(|n| {
                n.title.contains(query) ||
                n.content.contains(query) ||
                n.tags.iter().any(|t| t.contains(query))
            })
            .collect()
    }
    
    pub fn by_tag(&self, tag: &str) -> Vec<&Note> {
        self.notes.values()
            .filter(|n| n.tags.contains(&tag.to_string()))
            .collect()
    }
}
```

## Disk File Structure
```
~/.notes/
|-- notes.db           # Database (JSON or binary)
|-- index.json         # Search index
`-- config.toml        # Configuration
```

## Suggested Structure
```
notes-app/
|-- src/
|   |-- main.rs
|   |-- cli.rs          # Interactive CLI
|   |-- store.rs        # Persistence
|   |-- note.rs         # Note model
|   |-- search.rs       # Search
|   `-- commands/
|       |-- mod.rs
|       |-- new.rs
|       |-- list.rs
|       |-- edit.rs
|       `-- search.rs
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `serde` - Serialization
- `serde_json` - JSON parsing
- `clap` - Argument parsing
- `chrono` - Dates
- `uuid` - Unique IDs

## Resources
- [Chrono crate](https://docs.rs/chrono/)
- [CLI design patterns](https://rust-cli.github.io/book/)

## Difficulty Level
3/5 - Medium, complete application
