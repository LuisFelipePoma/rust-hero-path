# 02 - Grep Clone

## Description
A simplified clone of the `grep` command that searches for patterns in text files.

## Usage Example
```bash
$ mi-grep "pattern" archivo.txt
archivo.txt:2:This line contains pattern
archivo.txt:5:Pattern appears here
```

## Concepts to Learn
- **Regex basics** with the `regex` crate
- Searching across multiple files
- Line numbers
- Handling binary files
- **Lifetimes** (connecting input and output)

## Lifetime Example
```rust
fn find_pattern<'a>(content: &'a str, pattern: &str) -> Vec<&'a str> {
    content.lines()
        .filter(|line| line.contains(pattern))
        .collect()
}
```

## Suggested Structure
```
grep-clone/
|-- src/
|   |-- main.rs
|   |-- search.rs      # Search logic
|   `-- matcher.rs     # Pattern matching
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `clap` - Argument parsing
- `regex` - Regular expressions

## Resources
- [Rust Book - Lifetimes](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html)
- [regex crate](https://docs.rs/regex/)

## Difficulty Level
1/5 - Easy, requires understanding lifetimes
