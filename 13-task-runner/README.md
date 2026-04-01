# 13 - Task Runner (Make-like)

## Description
Minimal Make-like build tool that executes tasks based on dependencies.

## Usage Example
```yaml
# Taskfile.yaml
version: "1"

tasks:
  build:
    deps: [deps]
    cmds:
      - rustc src/*.rs -o bin/app
    generates: bin/app
  
  test:
    deps: [build]
    cmds:
      - cargo test
  
  deps:
    cmds:
      - cargo fetch
  
  clean:
    cmds:
      - rm -rf bin/ target/
```

```bash
$ my-task build
[deps] Running cargo fetch...
[build] Running rustc src/*.rs -o bin/app...
Build complete: bin/app

$ my-task test
[build] Running rustc src/*.rs -o bin/app... (up to date)
[test] Running cargo test...
Tests passed: 15/15

$ my-task clean && my-task build
[clean] Running rm -rf bin/ target/...
[deps] Running cargo fetch...
[build] Running rustc src/*.rs -o bin/app...
```

## Concepts to Learn
- **YAML parsing with `serde_yaml`**
- Dependency DAG (Directed Acyclic Graph)
- Parallel execution of independent tasks
- Task graphs and topological sort
- Build system fundamentals

## Topological Sort Example
```rust
fn topological_sort(tasks: &HashMap<String, Task>) -> Result<Vec<String>, CycleError> {
    let mut visited = HashSet::new();
    let mut result = Vec::new();
    
    fn visit(
        task: &str,
        tasks: &HashMap<String, Task>,
        visited: &mut HashSet<String>,
        result: &mut Vec<String>,
    ) -> Result<(), CycleError> {
        if visited.contains(task) {
            return Ok(()); // Already processed
        }
        if result.contains(&task.to_string()) {
            return Err(CycleError(task.to_string())); // Cycle detected
        }
        
        visited.insert(task.to_string());
        
        if let Some(t) = tasks.get(task) {
            for dep in &t.deps {
                visit(dep, tasks, visited, result)?;
            }
        }
        
        result.push(task.to_string());
        Ok(())
    }
    
    for task_name in tasks.keys() {
        visit(task_name, tasks, &mut visited, &mut result)?;
    }
    
    Ok(result)
}
```

## Suggested Structure
```
task-runner/
|-- src/
|   |-- main.rs
|   |-- parser.rs        # YAML parsing
|   |-- graph.rs         # Dependency graph
|   |-- executor.rs      # Task execution
|   `-- error.rs         # Error handling
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `serde` - Serialization
- `serde_yaml` - YAML parsing
- `clap` - Argument parsing

## Resources
- [GNU Make documentation](https://www.gnu.org/software/make/)
- [Topological sorting](https://en.wikipedia.org/wiki/Topological_sorting)

## Difficulty Level
4/5 - Advanced, graphs and parsing
