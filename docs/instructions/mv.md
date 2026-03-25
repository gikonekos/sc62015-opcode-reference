# MV / MVW / MVP / MVL / MVLD

Data transfer instructions.

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.73–79

---

## Overview

The SC62015 has several related transfer instructions:

| Mnemonic | Size | Description |
|----------|------|-------------|
| `MV`     | 1-byte | Single-byte transfer |
| `MVW`    | 2-byte | Two-byte (word) transfer |
| `MVP`    | 3-byte | Three-byte (pointer) transfer |
| `MVL`    | I×bytes | Repeated forward transfer using the `I` register |
| `MVLD`   | I×bytes | Repeated reverse-direction transfer using the `I` register |

**Flags:** None affected by any MV family instruction (p.73).

The data size transferred by `MV` depends on the register or operand type:

| Size | When |
|------|------|
| 8-bit  | r1 (A, IL) or internal RAM single byte |
| 16-bit | r2 (BA, I) |
| 20-bit | r3 (X, Y, U, S) |

For internal RAM ↔ internal RAM or internal RAM ↔ external memory transfers where the size is not implied by a register, use **MVW** (2-byte) or **MVP** (3-byte) explicitly (p.73).

---

## Notation

### Register classes

| Class | Registers | Size |
|-------|-----------|------|
| `r1`  | A, IL     | 8-bit |
| `r2`  | BA, I     | 16-bit |
| `r3`  | X, Y, U, S | 20-bit |

### Internal RAM operands (parentheses)

`(n)`, `(BP+n)`, `(PX+n)`, `(PY+n)`, `(BP+PX)`, `(BP+PY)`

The exact meaning of `(n)` depends on the **prebyte**. See `docs/prebyte.md`.

### External memory operands (square brackets)

`[lmn]`, `[r3]`, `[r3++]`, `[--r3]`, `[r3+n]`, `[(n)]`, `[(m)+n]`, `[(l)-n]`

---

## 1. Immediate transfer forms

### `MV r, n` — Immediate to register

| Mnemonic | Operands | Opcode | Bytes | Cycles |
|----------|----------|--------|-------|--------|
| MV | A, n    | `08` n     | 2 | 2 |
| MV | IL, n   | `09` n     | 2 | 3 |
| MV | BA, mn  | `0A` n m   | 3 | 3 |
| MV | I, mn   | `0B` n m   | 3 | 3 |
| MV | X, lmn  | `0C` n m l | 4 | 4 |
| MV | Y, lmn  | `0D` n m l | 4 | 4 |
| MV | U, lmn  | `0E` n m l | 4 | 4 |
| MV | S, lmn  | `0F` n m l | 4 | 4 |

Immediate value byte order: low byte first (p.224).

### Immediate to internal RAM

| Mnemonic | Operands   | Opcode        | Bytes | Cycles |
|----------|------------|---------------|-------|--------|
| MV   | (m), n      | `CC` m n      | 3 | 3 |
| MVW  | (l), mn     | `CD` l m n    | 4 | 4 |
| MVP  | (k), lmn    | `DC` k n m l  | 5 | 5 |

---

## 2. Register ↔ register

| Mnemonic | Operands   | Opcode      | Bytes | Cycles |
|----------|------------|-------------|-------|--------|
| MV | A, B        | `74`        | 1 | 1 |
| MV | B, A        | `75`        | 1 | 1 |
| MV | r2, r2'     | `FD` r r'   | 2 | 2 |
| MV | r3, r3'     | `FD` r r'   | 2 | 2 |

Second byte of `FD` forms: upper nibble = source register, lower nibble = destination register.

---

## 3. Register ↔ internal RAM

Prebyte may be required depending on the addressing mode. See `docs/prebyte.md` (p.68).

### `MV r, (n)` — internal RAM to register

| Mnemonic | Operands   | Opcode  | Bytes | Cycles |
|----------|------------|---------|-------|--------|
| MV | A, (n)   | `80` n  | 2 | 3 |
| MV | IL, (n)  | `81` n  | 2 | 4 |
| MV | BA, (n)  | `82` n  | 2 | 4 |
| MV | I, (n)   | `83` n  | 2 | 5 |
| MV | X, (n)   | `84` n  | 2 | 5 |
| MV | Y, (n)   | `85` n  | 2 | 5 |
| MV | U, (n)   | `86` n  | 2 | 5 |
| MV | S, (n)   | `87` n  | 2 | 5 |

> Note: Writing IL also clears IH to 0 simultaneously (p.224).

### `MV (n), r` — register to internal RAM

| Mnemonic | Operands   | Opcode  | Bytes | Cycles |
|----------|------------|---------|-------|--------|
| MV | (n), A   | `A0` n  | 2 | 3 |
| MV | (n), IL  | `A1` n  | 2 | 3 |
| MV | (n), BA  | `A2` n  | 2 | 4 |
| MV | (n), I   | `A3` n  | 2 | 4 |
| MV | (n), X   | `A4` n  | 2 | 5 |
| MV | (n), Y   | `A5` n  | 2 | 5 |
| MV | (n), U   | `A6` n  | 2 | 5 |
| MV | (n), S   | `A7` n  | 2 | 5 |

---

## 4. Internal RAM ↔ internal RAM

Prebyte required for addressing modes other than direct (p.74). See `docs/prebyte.md`.

### Single/multi-byte transfer

| Mnemonic | Operands   | Opcode     | Bytes | Cycles |
|----------|------------|------------|-------|--------|
| MV   | (m), (n)  | `C8` m n   | 3 | 6  |
| MVW  | (m), (n)  | `C9` m n   | 3 | 8  |
| MVP  | (m), (n)  | `CA` m n   | 3 | 10 |

### Repeated transfer

| Mnemonic | Operands   | Opcode     | Bytes | Cycles   |
|----------|------------|------------|-------|----------|
| MVL  | (m), (n)  | `CB` m n   | 3 | 5+2×I |
| MVLD | (m), (n)  | `CF` m n   | 3 | 5+2×I |

For MVL and MVLD, I ← 0 after execution.

---

## 5. Register ↔ external memory (absolute)

> Note: r ≠ S except for `MV r, [lmn]` and `MV [lmn], r` (p.74).

### `MV r, [lmn]` — absolute external memory to register

| Mnemonic | Operands      | Opcode        | Bytes | Cycles |
|----------|---------------|---------------|-------|--------|
| MV | A, [lmn]    | `88` n m l    | 4 | 6 |
| MV | IL, [lmn]   | `89` n m l    | 4 | 7 |
| MV | BA, [lmn]   | `8A` n m l    | 4 | 7 |
| MV | I, [lmn]    | `8B` n m l    | 4 | 8 |
| MV | X, [lmn]    | `8C` n m l    | 4 | 8 |
| MV | Y, [lmn]    | `8D` n m l    | 4 | 8 |
| MV | U, [lmn]    | `8E` n m l    | 4 | 8 |
| MV | S, [lmn]    | `8F` n m l    | 4 | 8 |

### `MV [lmn], r` — register to absolute external memory

| Mnemonic | Operands      | Opcode        | Bytes | Cycles |
|----------|---------------|---------------|-------|--------|
| MV | [lmn], A    | `A8` n m l    | 4 | 5 |
| MV | [lmn], IL   | `A9` n m l    | 4 | 5 |
| MV | [lmn], BA   | `AA` n m l    | 4 | 6 |
| MV | [lmn], I    | `AB` n m l    | 4 | 6 |
| MV | [lmn], X    | `AC` n m l    | 4 | 7 |
| MV | [lmn], Y    | `AD` n m l    | 4 | 7 |
| MV | [lmn], U    | `AE` n m l    | 4 | 7 |
| MV | [lmn], S    | `AF` n m l    | 4 | 7 |

---

## 6. Register ↔ external memory (register indirect)

First byte = `(90h + r)` for loads, `(B0h + r)` for stores. Second byte encodes addressing mode and r3.

### `MV r, [r3]` and variants

| Operands      | First byte  | Second byte | Bytes | Cycles (r1) |
|---------------|-------------|-------------|-------|-------------|
| r, [r3]       | `(90h+r)`   | `0r3`       | 2 | 4 |
| r, [r3++]     | `(90h+r)`   | `2r3`       | 2 | 4 |
| r, [--r3]     | `(90h+r)`   | `3r3`       | 2 | 5 |
| r, [r3+n]     | `(90h+r)`   | `8r3` n     | 3 | 6 |
| r, [r3-n]     | `(90h+r)`   | `Cr3` n     | 3 | 6 |

### `MV [r3], r` and variants

| Operands      | First byte  | Second byte | Bytes | Cycles (r1) |
|---------------|-------------|-------------|-------|-------------|
| [r3], r       | `(B0h+r)`   | `0r3`       | 2 | 4 |
| [r3++], r     | `(B0h+r)`   | `2r3`       | 2 | 4 |
| [--r3], r     | `(B0h+r)`   | `3r3`       | 2 | 5 |
| [r3+n], r     | `(B0h+r)`   | `8r3` n     | 3 | 6 |
| [r3-n], r     | `(B0h+r)`   | `Cr3` n     | 3 | 6 |

---

## 7. Register ↔ external memory (internal RAM indirect)

First byte = `(98h + r)` for loads, `(B8h + r)` for stores.

### `MV r, [(n)]` and variants

| Operands      | First byte  | Second byte | Bytes | Cycles (r1) |
|---------------|-------------|-------------|-------|-------------|
| r, [(n)]      | `(98h+r)`   | `00` n      | 3 | 9  |
| r, [(m)+n]    | `(98h+r)`   | `80` m n    | 4 | 11 |
| r, [(l)-n]    | `(98h+r)`   | `C0` l n    | 5 | 12 |

### `MV [(n)], r` and variants

| Operands      | First byte  | Second byte | Bytes | Cycles (r1) |
|---------------|-------------|-------------|-------|-------------|
| [(n)], r      | `(B8h+r)`   | `00` n      | 3 | 9  |
| [(m)+n], r    | `(B8h+r)`   | `80` m n    | 4 | 11 |
| [(l)-n], r    | `(B8h+r)`   | `C0` l n    | 5 | 13 |

---

## 8. Internal RAM ↔ external memory

| Mnemonic | Operands          | Opcode                  | Bytes | Cycles   |
|----------|-------------------|-------------------------|-------|----------|
| MV   | (k), [lmn]        | `D0` k n m l            | 5 | 7      |
| MVW  | (k), [lmn]        | `D1` k n m l            | 5 | 8      |
| MVP  | (k), [lmn]        | `D2` k n m l            | 5 | 9      |
| MVL  | (k), [lmn]        | `D3` k n m l            | 5 | 6+2×I  |
| MV   | [klm], (n)        | `D8` m l k n            | 5 | 6      |
| MVW  | [klm], (n)        | `D9` m l k n            | 5 | 7      |
| MVP  | [klm], (n)        | `DA` m l k n            | 5 | 8      |
| MVL  | [klm], (n)        | `DB` m l k n            | 5 | 6+2×I  |
| MV   | (n), [r3]         | `E0` `0r3` n            | 3 | 6      |
| MVW  | (n), [r3]         | `E1` `0r3` n            | 3 | 7      |
| MVP  | (n), [r3]         | `E2` `0r3` n            | 3 | 8      |
| MVL  | (n), [r3++]       | `E3` `2r3` n            | 3 | 5+2×I  |
| MV   | (n), [r3++]       | `E8` `2r3` n            | 3 | 6      |
| MVW  | (n), [r3++]       | `E9` `2r3` n            | 3 | 7      |
| MVP  | (n), [r3++]       | `EA` `2r3` n            | 3 | 9      |
| MVL  | (n), [r3++]       | `EB` `2r3` n            | 3 | 5+2×I  |
| MV   | (m), [(n)]        | `F0` `00` m n           | 4 | 11     |
| MVW  | (m), [(n)]        | `F1` `00` m n           | 4 | 12     |
| MVP  | (m), [(n)]        | `F2` `00` m n           | 4 | 13     |
| MVL  | (m), [(n)]        | `F3` `00` m n           | 4 | 10+2×I |
| MV   | [(l)±m], (n)      | `F8` `80`/`C0` l m n    | 5 | 13     |
| MVW  | [(l)±m], (n)      | `F9` `80`/`C0` l m n    | 5 | 14     |
| MVP  | [(l)±m], (n)      | `FA` `80`/`C0` l m n    | 5 | 15     |
| MVL  | [(l)±m], (n)      | `FB` `80`/`C0` l m n    | 5 | 12+2×I |

For MVL forms, I ← 0 after execution.

> Note: `MVL (n), [r3]` and `MVL [r3], (n)` do not exist (p.77).

---

## MVW opr1, opr2

**Operation:** opr1L ← opr2L, opr1H ← opr2H  
**Flags:** None affected

2-byte (16-bit) variant of MV. Transfers two consecutive bytes, low byte at lower address.

Operand combinations are a subset of the forms in sections 4 and 8. Not available for register operands (register size is determined by the register itself).

---

## MVP opr1, opr2

**Operation:** opr1L ← opr2L, opr1M ← opr2M, opr1H ← opr2H  
**Flags:** None affected

3-byte (20-bit) variant of MV. Transfers three consecutive bytes, low byte first.

---

## MVL opr1, opr2

**Operation:** Transfers I register bytes from opr2 region to opr1 region, incrementing both addresses.  
**Flags:** None affected (p.77)  
**After execution:** I ← 0

Loop transfer instruction. Repeats the transfer I times, incrementing both source and destination addresses by 1 per iteration. When I = 0 at the start, executes 10000h (65536) times (p.77).

Operand combinations: internal RAM ↔ internal RAM, and internal RAM ↔ external memory only (p.77). See sections 4 and 8.

**Set the I register before executing MVL.** When source and destination regions overlap, use **MVLD** instead to avoid overwriting unread data (p.79).

### Example

```
; Copy 4 bytes from internal RAM 20h–23h to 10h–13h
; (absolute addressing, prebyte 32h)
MV    I, 04h      ; 0B 04 00
PRE   32h
MVL   (10h),(20h) ; CB 10 20
; After: I = 0, 10h–13h contain the copied data
```

---

## MVLD opr1, opr2

**Operation:** Transfers I register bytes from opr2 region to opr1 region, decrementing both addresses.  
**Flags:** None affected (p.79)  
**After execution:** I ← 0

Identical to MVL except addresses are decremented instead of incremented (p.79).

Operand: internal RAM ↔ internal RAM only — opcode `CF` m n.

Use MVLD instead of MVL when the destination region overlaps the source at a **higher** address. MVL copies low-to-high and will overwrite unread source data in this case; MVLD copies high-to-low and avoids the problem (p.79).

### Example

```
; Move internal RAM 10h–1Fh upward to 20h–2Fh
; (overlapping regions — must use MVLD, not MVL)
MV    I, 10h      ; I = 16
PRE   32h
MVLD  (3Fh),(2Fh) ; CF 3F 2F   (start from the top end)
```

---

## Notes

- **Writing IL also clears IH to 0** simultaneously (p.224). Applies to all MV instructions with IL as the destination.
- Prebyte is required for internal RAM indirect addressing modes. See `docs/prebyte.md` (p.68).
- Some MV forms share the same first byte and are distinguished by the second byte — especially the `90h–96h`, `98h–9Eh`, `B0h–B6h`, and `B8h–BEh` families. The second byte must be interpreted carefully when decoding binaries.
- Only forms confirmed from the source pages are listed here. Do not assume symmetric forms exist without source confirmation.

## Examples

```
; MV A, B  —  copy B into A
74

; MV X, 0BDC00h  —  load immediate 20-bit value into X
0C 00 DC 0B

; MV A, (10h)  —  load internal RAM address 10h into A
80 10

; MV (20h), A  —  store A into internal RAM address 20h
A0 20

; MV A, [X]  —  load from external memory at address in X  (r=0=A, r3=4=X)
90 04

; MV [X++], A  —  store A into [X], then X += 1
B0 24
```

---

## Related instructions

- `EX`, `EXW`, `EXP`, `EXL` — described in `exchange.md`

For exact opcode / bytes / cycles values, see also `opcodes-00-ff.md`.
