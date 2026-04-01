# 06 - Basic HTTP Server

## Description
Minimalist HTTP server built from scratch, without frameworks.

## Usage Example
```bash
$ cargo run
Server running on http://localhost:8080

# In another terminal:
$ curl http://localhost:8080/hello
Hello, World!

$ curl http://localhost:8080/time
{"timestamp": 1704067200}

$ curl http://localhost:8080/echo -d "Hello"
You sent: Hello
```

## Concepts to Learn
- **Manual HTTP parsing** (without frameworks)
- Basic routing
- Headers and response formatting
- TCP socket handling
- Basic async (optional with `tokio`)

## Suggested Structure
```
http-server/
|-- src/
|   |-- main.rs
|   |-- server.rs      # TCP listener
|   |-- router.rs      # Path -> Handler
|   |-- request.rs     # HTTP request parser
|   |-- response.rs    # HTTP response builder
|   `-- handlers/
|       |-- mod.rs
|       |-- hello.rs
|       |-- time.rs
|       `-- echo.rs
|-- Cargo.toml
`-- tests/
```

## HTTP Parser Example
```rust
struct HttpRequest {
    method: String,
    path: String,
    version: String,
    headers: HashMap<String, String>,
    body: Option<String>,
}

fn parse_request(raw: &str) -> Result<HttpRequest, HttpError> {
    let mut lines = raw.lines();
    let request_line = lines.next().ok_or(HttpError::InvalidRequest)?;
    
    let parts: Vec<&str> = request_line.split_whitespace().collect();
    if parts.len() != 3 {
        return Err(HttpError::InvalidRequest);
    }
    
    // Parse headers, body, etc...
    Ok(HttpRequest { ... })
}
```

## Suggested Dependencies
- None required (std only, or optional `tokio`)

## Resources
- [RFC 7230 - HTTP/1.1 Message Syntax](https://tools.ietf.org/html/rfc7230)
- [Rust Book - Error Handling](https://doc.rust-lang.org/book/ch09-00-error-handling.html)

## Difficulty Level
2/5 - Medium, requires understanding protocols
