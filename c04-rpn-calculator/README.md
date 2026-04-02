# 04 - RPN Calculator

## Description
Calculator using Reverse Polish Notation (RPN).

## Usage Example
```bash
$ rpn-calc "3 4 + 2 *"
14

$ rpn-calc "10 5 - 2 /"
2.5

$ rpn-calc "5 3 2 + *"
25
```

## Concepts to Learn
- **Stack as a data structure**
- Expression parsing
- Granular error handling
- Trait `FromStr`

## Implementation Example
```rust
enum Token {
    Number(f64),
    Op(Operator),
}

fn evaluate(tokens: Vec<Token>) -> Result<f64, CalcError> {
    let mut stack = Vec::new();
    
    for token in tokens {
        match token {
            Token::Number(n) => stack.push(n),
            Token::Op(op) => {
                let b = stack.pop().ok_or(CalcError::NotEnoughOperands)?;
                let a = stack.pop().ok_or(CalcError::NotEnoughOperands)?;
                stack.push(apply_op(op, a, b)?);
            }
        }
    }
    
    stack.pop().ok_or(CalcError::EmptyStack)
}
```

## Suggested Structure
```
rpn-calculator/
|-- src/
|   |-- main.rs
|   |-- parser.rs      # Tokenization
|   |-- evaluator.rs   # Stack evaluation
|   `-- error.rs       # Error types
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- None required (std only)

## Resources
- [Rust Book - Enums](https://doc.rust-lang.org/book/ch06-00-enums.html)
- [Rust by Example - Option](https://doc.rust-lang.org/rust-by-example/std/option.html)

## Difficulty Level
2/5 - Easy-Medium, great exercise for structs/enums
