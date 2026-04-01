# 16 - Custom Memory Allocator

## Description
Implementation of a custom memory allocator by implementing Rust's `GlobalAlloc` trait.

## Usage Example
```rust
use std::alloc::{GlobalAlloc, Layout, System};

struct MyAllocator;

unsafe impl GlobalAlloc for MyAllocator {
    unsafe fn alloc(&self, layout: Layout) -> *mut u8 {
        // Your implementation
    }
    
    unsafe fn dealloc(&self, ptr: *mut u8, layout: Layout) {
        // Free logic
    }
}

// Use the allocator
#[global_allocator]
static GLOBAL: MyAllocator = MyAllocator;

fn main() {
    let v = vec![1, 2, 3]; // Uses your allocator!
}
```

## Concepts to Learn
- **OS-level memory management**
- Rust `GlobalAlloc` trait
- Bump allocator vs Pool allocator
- Memory fragmentation
- Alignment requirements
- **Unsafe blocks**

## Allocator Strategies

### Bump Allocator (Simple)
```rust
struct BumpAllocator {
    heap_start: usize,
    heap_end: usize,
    current: Cell<usize>,
}

unsafe impl GlobalAlloc for BumpAllocator {
    unsafe fn alloc(&self, layout: Layout) -> *mut u8 {
        let alloc_start = align_up(self.current.get(), layout.align());
        let alloc_end = alloc_start + layout.size();
        
        if alloc_end > self.heap_end {
            return std::ptr::null_mut();
        }
        
        self.current.set(alloc_end);
        alloc_start as *mut u8
    }
    
    unsafe fn dealloc(&self, _ptr: *mut u8, _layout: Layout) {
        // Bump allocators do not free memory!
    }
}
```

### Pool Allocator (Efficient)
```rust
struct PoolAllocator {
    pools: HashMap<usize, Pool>,  // Pool per size class
}

struct Pool {
    free_list: Vec<usize>,
    start: usize,
    end: usize,
}

unsafe impl GlobalAlloc for PoolAllocator {
    unsafe fn alloc(&self, layout: Layout) -> *mut u8 {
        let size_class = round_up(layout.size(), POOL_ALIGN);
        
        if let Some(pool) = self.pools.get_mut(&size_class) {
            if let Some(addr) = pool.free_list.pop() {
                return addr as *mut u8;
            }
            
            // Allocate new block from system
            let new_block = system_alloc(size_class * BLOCKS_PER_POOL);
            // Add blocks to pool's free list
            // Return one block
        }
        
        std::ptr::null_mut()
    }
    
    unsafe fn dealloc(&self, ptr: *mut u8, layout: Layout) {
        // Return block to pool's free list
    }
}
```

## Suggested Structure
```
custom-allocator/
|-- src/
|   |-- main.rs           # Demo and benchmarks
|   |-- bump.rs           # Bump allocator
|   |-- pool.rs           # Pool allocator
|   |-- linked.rs         # Linked-list allocator
|   |-- utils.rs          # Helpers (align, etc)
|   `-- tests/
|       |-- test_bump.rs
|       |-- test_pool.rs
|       `-- bench.rs      # Benchmarks
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `test` - Built-in benchmarking

## Resources
- [Rust Allocators RFC](https://rust-lang.github.io/rfcs/1974-global-allocators.html)
- [dlmalloc paper](http://gee.cs.oswego.edu/dl/html/malloc.html)
- [System allocator design](https://people.kernel.org/vequip/2022/12/25/memory-allocators-an-overview)

## Difficulty Level
5/5 - Expert, for OS-level understanding
