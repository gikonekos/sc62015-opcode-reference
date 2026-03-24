# Addressing Modes

> Source: primary printed material, p.65–p.72

## Data Size

The SC62015 handles 1, 2, or 3 bytes of data at a time.
For registers: 1 byte (A/IL), 2 bytes (BA/I), or 2.5 bytes / 20-bit (X/Y/U/S).

When only internal RAM or external memory is accessed (without a register operand),
the data size must be specified explicitly in the mnemonic.

| Mnemonic suffix | Size |
|-----------------|------|
| MV (no suffix)  | 1 byte |
| MVW             | 2 bytes |
| MVP             | 3 bytes |
| MVL             | I register bytes (loop) |

## Internal RAM Addressing

The SC62015 has 256 bytes of internal RAM.
When an operand is written as `(n)`, one of four addressing modes can be selected.

| Mode | Effective address | Description |
|------|-------------------|-------------|
| ❶ (n)      | address n (absolute) | Directly specifies address n in internal RAM |
| ❷ (BP+n)   | BP + n | Relative to the base pointer |
| ❸ (PX+n) / (PY+n) | PX (or PY) + n | Relative to PX or PY pointer |
| ❹ (BP+PX) / (BP+PY) | BP + PX (or PY) | Base pointer + pointer |

- **BP** (Base Pointer): address ECh in internal RAM
- **PX** (PX Pointer): address EDh in internal RAM
- **PY** (PY Pointer): address EEh in internal RAM

The mode is selected by a **prebyte** placed immediately before the instruction (see `prebyte.md`).

## External Memory Addressing

Addresses in external memory space are written enclosed in `[` and `]`.
There are 7 addressing modes.

| Notation | Description |
|----------|-------------|
| `[lmn]`   | 20-bit absolute address (l is the upper 4 bits) |
| `[r3]`    | Value of r3 register used as effective address |
| `[r3+n]`  | r3 + n as effective address |
| `[r3++]`  | Execute using r3, then r3 += data size |
| `[--r3]`  | r3 -= data size, then execute using r3 |
| `[(n)]`   | 3-byte value at internal RAM address n used as effective address |
| `[(m)±n]` | 3-byte value at internal RAM address m ± n used as effective address |

Valid r3 registers are **X, Y, U, S** only.

## Operand Notation Summary

| Notation | Meaning |
|----------|---------|
| `n`       | 8-bit immediate value |
| `mn`      | 16-bit immediate value (n = low byte, m = high byte) |
| `lmn`     | 20-bit immediate value (n = lowest byte, l = highest byte) |
| `(n)`     | Internal RAM address n |
| `(m),(n)` | Both operands are internal RAM addresses |
| `[lmn]`   | External memory absolute address (20-bit) |
| `[r3]`    | Register indirect |
| `[(n)]`   | Internal RAM indirect |

## Loop Instructions

Instructions whose mnemonic ends in `L` (MVL, ADCL, SBCL, EXL, etc.) are loop instructions.
They repeat execution for the number of times specified by the I register.
When I = 0, execution is treated as 10000h (65536) times.
After execution, the I register is set to 0.
