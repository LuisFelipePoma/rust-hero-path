# 10 - Markdown to HTML Converter

## Description
Markdown-to-HTML converter with support for multiple Markdown language features.

## Usage Example
```bash
$ md2html README.md --output dist/
Converting README.md -> dist/README.html

$ md2html *.md --template minimal --output dist/
Processing 5 files...

$ cat input.md | md2html --standalone --css "style.css"
```

## Features to Implement
- Headings (H1-H6)
- Paragraphs and line breaks
- Bold, italic, strikethrough
- Links and images
- Code blocks (inline and fenced)
- Lists (ordered and unordered)
- Blockquotes
- Horizontal rules
- Tables (optional)

## Concepts to Learn
- **Recursive descent parsing**
- **AST for documents**
- HTML generation
- CLI with templates

## AST Example
```rust
enum Node {
    Document(Vec<Node>),
    Heading { level: u8, text: String },
    Paragraph(String),
    Bold(String),
    Italic(String),
    CodeBlock { lang: Option<String>, code: String },
    Link { url: String, text: String },
    List { ordered: bool, items: Vec<String> },
    Blockquote(Vec<Node>),
    Code(String),
    Text(String),
}

fn parse_markdown(input: &str) -> Node {
    let mut tokens = tokenize(input);
    parse_document(&mut tokens)
}
```

## Suggested Structure
```
md2html/
|-- src/
|   |-- main.rs
|   |-- parser/
|   |   |-- mod.rs
|   |   |-- tokenizer.rs   # Lexing
|   |   |-- ast.rs         # AST types
|   |   `-- parser.rs      # Parsing logic
|   |-- generator/
|   |   |-- mod.rs
|   |   `-- html.rs        # HTML generation
|   `-- templates/
|       `-- base.html
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `clap` - Argument parsing
- `regex` - Pattern matching

## Resources
- [CommonMark Spec](https://spec.commonmark.org/)
- [Rust Book - Enums and Pattern Matching](https://doc.rust-lang.org/book/ch06-00-enums.html)

## Difficulty Level
4/5 - Advanced, real language parser
