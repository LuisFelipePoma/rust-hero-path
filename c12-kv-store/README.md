# 12 - Key-Value Store

## Description
Simple key-value database inspired by SQLite/Redis concepts, with disk persistence.

## Usage Example
```bash
$ kv-store create mydb.kv
Database created: mydb.kv

$ kv-store set mydb.kv "user:1" '{"name":"Alice","age":30}'
Set: user:1 = '{"name":"Alice","age":30}'

$ kv-store get mydb.kv "user:1"
{"name":"Alice","age":30}

$ kv-store delete mydb.kv "user:1"
Deleted: user:1

$ kv-store list mydb.kv
user:1 -> '{"name":"Alice","age":30}'
user:2 -> '{"name":"Bob","age":25}'

$ kv-store exists mydb.kv "user:1"
true

$ kv-store keys mydb.kv | grep "user"
user:1
user:2
```

## Concepts to Learn
- **Binary file handling**
- **Serialization with `serde` + `bincode`**
- B-tree or LSM tree implementation (optional)
- ACID basics (without being a full database)
- Write-Ahead Log (WAL)

## Structure Example
```rust
use serde::{Serialize, Deserialize};

struct KVStore {
    data: BTreeMap<Vec<u8>, Vec<u8>>,
    wal: Vec<WriteEntry>,  // Write-Ahead Log
}

impl KVStore {
    pub fn set(&mut self, key: &[u8], value: &[u8]) -> Result<(), StoreError> {
        // Append to WAL first (durability)
        self.wal.push(WriteEntry::Set {
            key: key.to_vec(),
            value: value.to_vec(),
        });
        
        // Update in-memory
        self.data.insert(key.to_vec(), value.to_vec());
        Ok(())
    }
    
    pub fn flush(&self, file: &mut File) -> Result<(), StoreError> {
        // Serialize entire state
        let bytes = bincode::serialize(&self)?;
        file.write_all(&bytes)?;
        file.sync_all()?;
        Ok(())
    }
}
```

## Suggested Structure
```
kv-store/
|-- src/
|   |-- main.rs
|   |-- store.rs         # Core logic
|   |-- persistence.rs   # Serialization
|   |-- wal.rs           # Write-Ahead Log
|   |-- tree.rs          # B-Tree (advanced)
|   `-- error.rs         # Error types
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `serde` - Serialization
- `bincode` - Binary serialization
- `clap` - Argument parsing

## Resources
- [LSM Tree vs B-Tree](https://en.wikipedia.org/wiki/Log-structured_merge-tree)
- [Write-Ahead Logging](https://en.wikipedia.org/wiki/Write-ahead_logging)
- [bincode documentation](https://docs.rs/bincode/)

## Difficulty Level
5/5 - Hard, complex data structures
