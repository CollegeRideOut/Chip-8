# Chip-8

A Chip-8 interpreter/emulator written in Go with terminal-based display.

---

## Features

- 35+ Chip-8 instructions implemented — draw, jump, subroutine, arithmetic, logic, keypad, timers
- Termbox-based 64×32 pixel display
- Hex keypad mapped to keyboard
- Delay and sound timer support
- Old/new behavior mode toggle (handles Chip-8 vs SUPER-CHIP quirks)
- ROM loading from disk
- Test ROMs included (IBM logo, Pong, Space Invaders, Connect 4, test suites)

---

## Quick Start

```bash
go run main.go
```

Edit `main.go` to switch between included ROMs:
```go
n := "./pong.rom"       // Pong game
n := "./invaders.rom"   // Space Invaders
n := "./ibm_logo.ch8"   // IBM logo test
n := "./test_opcode.ch8" // Opcode test suite
n := "./connect4.rom"   // Connect 4
```

### Controls

```
Chip-8 Keypad → Keyboard
  1 2 3 4     → 1 2 3 4
  q w e r     → q w e r
  a s d f     → a s d f
  z x c v     → z x c v
```

---

## Architecture

| Package | Purpose |
|---------|---------|
| `chip/` | CPU core — fetch, decode, execute cycle, instruction set |
| `display/` | Termbox-go screen buffer with 64×32 pixel XOR drawing |
| `keyboard/` | Hex keypad state tracking and polling |
| `memory/` | 4096-byte addressable memory |
| `register/` | 16 general-purpose 8-bit Vx registers + 16-bit I register |
| `stack/` | 16-level call stack for subroutines |
| `timer/` | 60Hz delay and sound countdown timers |
| `util/` | Binary/decimal conversion utilities |

### Instruction Set

The emulator implements the standard Chip-8 instruction set including:

- **Clear/Return**: `00E0` (CLS), `00EE` (RET)
- **Jump/Call**: `1NNN` (JP), `2NNN` (CALL), `BNNN` (JP V0+addr)
- **Skip**: `3XNN`, `4XNN`, `5XY0`, `9XY0`, `EX9E`, `EXA1`
- **Arithmetic**: `6XNN` (LD Vx), `7XNN` (ADD Vx), `8XY0–8XYE` (LD/OR/AND/XOR/ADD/SUB/SHR/SUBN/SHL)
- **Logic/Compare**: `CXNN` (RND), `FX07/FX15/FX18/FX1E/FX29/FX33/FX55/FX65`
- **Draw**: `DXYN` (DRW) — XOR-based sprite rendering with collision detection
- **Keypad**: `FX0A` (wait for key), `EX9E` (skip if key), `EXA1` (skip if not key)
- **Timer/Sound**: `FX07` (get delay), `FX15` (set delay), `FX18` (set sound)

---

## Why This Matters

A Chip-8 emulator requires understanding:
- CPU fetch-decode-execute cycle
- Memory-mapped I/O and address spaces
- Bit-level sprite rendering with XOR collision
- Interrupt-driven timer systems
- Hexadecimal keypad polling

This is a foundational systems project that demonstrates low-level programming, architecture emulation, and Go concurrency.

---

## License

MIT
