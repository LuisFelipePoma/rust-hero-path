# 15 - CHIP-8 Emulator

## Description
CHIP-8 emulator for a simple virtual machine created in the 1970s to simplify game development.

## Concepts to Learn
- **CPU emulation** (instruction decoding, registers)
- Clock cycles and timing
- Graphics rendering (64x32 monochrome display)
- Input handling (16-key keypad)
- **Unsafe for FFI or shared memory**
- Disassembling

## CHIP-8 Specifications
- **RAM**: 4KB (0x000-0xFFF)
- **Registers**: V0-VF (16 registers, 8 bits)
- **Index Register**: I (16 bits)
- **Program Counter**: PC (16 bits)
- **Stack**: 16 levels
- **Display**: 64x32 pixels (monochrome)
- **Input**: 16-key keypad (0-F)

## Core Emulation Topics
- Fetch-Decode-Execute cycle
- Memory-mapped I/O
- Frame timing (60 FPS)
- Opcode mapping

## CPU Structure
```rust
struct Chip8 {
    memory: [u8; 4096],      // 4KB RAM
    v: [u8; 16],             // Registers V0-VF
    i: u16,                   // Index register
    pc: u16,                  // Program counter
    sp: u8,                   // Stack pointer
    stack: [u16; 16],         // Stack
    delay_timer: u8,
    sound_timer: u8,
    display: [[u8; 64]; 32],  // 64x32 display
    keys: [bool; 16],         // Keypad state
}

// Fetch-Decode-Execute
impl Chip8 {
    fn cycle(&mut self) {
        let opcode = self.fetch();
        self.execute(opcode);
        
        if self.delay_timer > 0 {
            self.delay_timer -= 1;
        }
        if self.sound_timer > 0 {
            self.sound_timer -= 1;
        }
    }
    
    fn execute(&mut self, opcode: u16) {
        let nnn = opcode & 0x0FFF;
        let kk = (opcode & 0x00FF) as u8;
        let n = (opcode & 0x000F) as u8;
        let x = ((opcode >> 8) & 0x000F) as usize;
        let y = ((opcode >> 4) & 0x000F) as usize;
        
        match opcode & 0xF000 {
            0x0000 => match kk {
                0x00E0 => { /* CLS - Clear display */ }
                0x00EE => { /* RET - Return */ }
                _ => { /* SYS addr (ignored) */ }
            },
            0x1000 => self.pc = nnn,       // JP addr
            0x2000 => self.call(nnn),      // CALL addr
            0x3000 => if self.v[x] == kk { self.pc += 2; }, // SE Vx, byte
            // ... more opcodes
            _ => {}
        }
    }
}
```

## Suggested Structure
```
chip8-emulator/
|-- src/
|   |-- main.rs
|   |-- cpu.rs           # CPU emulation
|   |-- memory.rs        # Memory and ROM loading
|   |-- display.rs       # Rendering
|   |-- input.rs         # Keypad handling
|   |-- opcode.rs        # Opcode definitions
|   `-- debugger.rs      # Optional debugger
|-- roms/
|   `-- PONG
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `sdl2` or `winit` - Graphics and input
- `log` + `env_logger` - Logging

## Resources
- [Cowgod's CHIP-8 Technical Reference](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM)
- [Wikipedia - CHIP-8](https://en.wikipedia.org/wiki/CHIP-8)
- [CHIP-8 ROMs](https://github.com/dmatlack/chip8/tree/master/roms)

## Difficulty Level
5/5 - Very hard, requires understanding hardware
