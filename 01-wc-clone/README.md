# 01 - Word Count Clone (wc)

## Description
A clone of the Unix `wc` command that counts lines, words, and characters in text files.

## Usage Example
```bash
$ mi-wc archivo.txt
   125  342  2048 archivo.txt
```

## Concepts to Learn
- File reading with `std::fs`
- Argument parsing with the `clap` crate
- Counters with iterators
- Handling `Result<T, E>`

## Suggested Structure
```
wc-clone/
|-- src/
|   |-- main.rs      # Entry point + arg parsing
|   |-- counter.rs   # Counting logic
|   `-- output.rs    # Output formatting
|-- Cargo.toml
`-- tests/
    `-- test_counter.rs
```

## Suggested Dependencies
- `clap` - Argument parsing

## Resources
- [Rust by Example - File I/O](https://doc.rust-lang.org/rust-by-example/std_misc/file.html)
- [clap documentation](https://docs.rs/clap/)

## Difficulty Level
1/5 - Very easy, perfect for getting started
