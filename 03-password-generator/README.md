# 03 - Password Generator

## Description
Random password generator with configurable length and character options.

## Usage Example
```bash
$ password-gen --length 32 --include-symbols
aB3$kL9@mN2#pQ5&rT7*sU1!wX4

$ password-gen -l 16 --no-numbers
aBcDeFgHiJkLmNoP
```

## Concepts to Learn
- Randomness with the `rand` crate
- Structured flag parsing
- String manipulation
- Property tests

## Suggested Structure
```
password-generator/
|-- src/
|   |-- main.rs
|   |-- generator.rs    # Generation logic
|   |-- charset.rs      # Character sets
|   `-- cli.rs          # Argument parsing
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `clap` - Argument parsing
- `rand` - Random generation

## Resources
- [rand crate](https://docs.rs/rand/)
- [clap documentation](https://docs.rs/clap/)

## Difficulty Level
1/5 - Easy, great exercise for randomness
