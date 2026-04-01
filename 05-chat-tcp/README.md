# 05 - Chat Multiplayer TCP

## Description
Multi-user chat server over TCP. Multiple clients can connect and exchange messages.

## Usage Example
```bash
# Terminal 1 - Server
$ chat-server --port 8080
Server listening on 0.0.0.0:8080
Client connected: 127.0.0.1:54321
Client connected: 127.0.0.1:54322

# Terminal 2 - Client 1
$ chat-client --port 8080 --username Alice
Connected to server
> Hello everyone!
< Bob: Hi Alice!

# Terminal 3 - Client 2
$ chat-client --port 8080 --username Bob
Connected to server
< Alice: Hello everyone!
> Hi Alice!
```

## Concepts to Learn
- **Networking with `std::net::TcpListener` and `TcpStream`**
- **Multithreading with `std::thread`**
- **Channels for inter-thread communication**
- Shared state with `Arc<Mutex<T>>`

## Safe Concurrency Example
```rust
use std::sync::{Arc, Mutex};
use std::sync::mpsc;

struct ChatRoom {
    history: Vec<Message>,
    clients: Vec<TcpStream>,
}

let room = Arc::new(Mutex::new(ChatRoom::new()));

// Each client runs in its own thread
for stream in listener.incoming() {
    let room = Arc::clone(&room);
    std::thread::spawn(move || {
        handle_client(stream, &room);
    });
}
```

## Suggested Structure
```
chat-tcp/
|-- src/
|   |-- main.rs
|   |-- server.rs      # Server logic
|   |-- client.rs      # Client logic
|   |-- protocol.rs    # Message protocol
|   `-- room.rs        # Shared chat room
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `clap` - Argument parsing

## Resources
- [Rust Book - Concurrency](https://doc.rust-lang.org/book/ch16-00-concurrency.html)
- [std::net documentation](https://doc.rust-lang.org/std/net/)

## Difficulty Level
2/5 - Medium, introduces real concurrency
