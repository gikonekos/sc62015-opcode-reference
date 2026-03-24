# Registers

> Source: primary printed material, p.66–p.67

## Register List

The SC62015 has the following registers.

| Symbol | Opcode value | Binary | Size | Description |
|--------|-------------|--------|------|-------------|
| A  | 0 | 000 | 8-bit | Accumulator (also the high byte of the B register) |
| IL | 1 | 001 | 8-bit | Low byte of the I register |
| BA | 2 | 010 | 16-bit | 2-byte register: B (high) + A (low) |
| I  | 3 | 011 | 16-bit | Loop counter / general purpose (IL + IH) |
| X  | 4 | 100 | 20-bit | Index register (XL + XH + XX) |
| Y  | 5 | 101 | 20-bit | Index register (YL + YH + YX) |
| U  | 6 | 110 | 20-bit | User stack pointer (UL + UH + UX) |
| S  | 7 | 111 | 20-bit | System stack pointer (SL + SH + SX) |

## Register Classes

The following class symbols are used in the instruction tables.

| Symbol | Registers included |
|--------|--------------------|
| r  | A, IL, BA, I, X, Y, U, S (all registers) |
| r1 | A, IL (8-bit) |
| r2 | BA, I (16-bit) |
| r3 | X, Y, U, S (20-bit) |
| r4 | X, Y, U, S (same as r3) |

## Notes

- **Writing to IL also clears IH to 0** (IH is automatically zeroed)
- **The I register** is used as a loop counter. When I = 0, the loop executes 10000h (65536) times
- **F (Flags)** can be pushed or popped only with PUSHS / POPS / PUSHU / POPU instructions
- **IMR** (Interrupt Mask Register) corresponds to address FBh in internal RAM

## Flags

| Flag | Description |
|------|-------------|
| Z | Zero flag. Set to 1 when the result is 0, otherwise 0 |
| C | Carry flag. Set to 1 when a carry or borrow occurs, otherwise 0 |
