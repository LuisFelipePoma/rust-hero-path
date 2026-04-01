# 09 - CSV Tool with Operations

## Description
CLI tool for CSV file manipulation with filtering, projection, and transformation operations.

## Usage Example
```bash
$ csv-tool data.csv --filter "age > 21"
$ csv-tool data.csv --select "name,email,phone"
$ csv-tool data.csv --filter "status == 'active'" --select "name,email" --output filtered.csv
$ csv-tool data.csv --aggregate "age:avg,salary:sum" --group-by "department"
$ csv-tool data.csv --sort "age:desc,name:asc"
```

## Concepts to Learn
- **CSV parsing** with the `csv` crate
- Filters and transformations
- Streams for large files
- Custom **Iterator trait** usage
- Generic trait bounds

## Pipeline Example
```rust
use csv::ReaderBuilder;

fn filter_and_project<R: std::io::Read>(
    reader: R,
    filter_fn: impl Fn(&Record) -> bool,
    columns: &[&str],
) -> csv::Result<Vec<Record>> {
    let mut csv_reader = ReaderBuilder::new()
        .has_headers(true)
        .from_reader(reader);
    
    let headers = csv_reader.headers()?.clone();
    let indices: Vec<usize> = columns
        .iter()
        .filter_map(|h| headers.iter().position(|x| x == *h))
        .collect();
    
    csv_reader.records()
        .filter_map(Result::ok)
        .filter(|r| filter_fn(r))
        .map(|r| indices.iter().map(|&i| r[i].to_string()).collect())
        .collect()
}
```

## Suggested Structure
```
csv-tool/
|-- src/
|   |-- main.rs
|   |-- parser.rs       # CSV parsing
|   |-- filter.rs       # Filters
|   |-- projector.rs    # Column selection
|   |-- aggregator.rs   # Aggregations
|   `-- pipeline.rs     # Operation chaining
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `csv` - CSV parsing
- `clap` - Argument parsing
- `serde` - Serialization

## Resources
- [csv crate documentation](https://docs.rs/csv/)
- [Rust iterators](https://doc.rust-lang.org/book/ch13-02-iterators.html)

## Difficulty Level
3/5 - Medium-Advanced, generics and traits
