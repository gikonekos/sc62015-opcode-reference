# Data Transfer Instructions

This document summarizes the **SC62015** data transfer instructions centered on the **MV family**.

Source basis:
- Chapter 6 instruction description pages, especially **pp.73–79**
- Chapter 11 opcode tables and mnemonic mapping tables

This file is intended as a **reference-oriented explanation**.
It follows the original printed notation as closely as practical, while arranging the content in a more readable order.

---

## Overview

The SC62015 has several related transfer instructions:

- `MV`   — 1-byte transfer
- `MVW`  — 2-byte transfer
- `MVP`  — 3-byte transfer
- `MVL`  — repeated forward transfer using the `I` register
- `MVLD` — repeated reverse-direction transfer using the `I` register

These instructions appear in multiple addressing forms:

- register ↔ immediate
- register ↔ internal RAM
- register ↔ external memory
- internal RAM ↔ internal RAM
- internal RAM ↔ external memory
- repeated transfer forms

**Flags:** None affected by any MV family instruction (p.73).

---

## Notation used here

The notation below follows the source material.

### Internal RAM operands

Internal RAM operands use parentheses:

- `(n)`
- `(BP+n)`
- `(PX+n)`
- `(PY+n)`
- `(BP+PX)`
- `(BP+PY)`

The exact meaning of `(n)` depends on the **prebyte**.

### External memory operands

External memory operands use square brackets:

- `[lmn]`
- `[r3]`
- `[r3++]`
- `[--r3]`
- `[r3+n]`
- `[(n)]`
- `[(m)+n]`
- `[(l)-n]`

### Register classes

The source distinguishes register classes such as:

- `r1` — A, IL (8-bit)
- `r2` — BA, I (16-bit)
- `r3` — X, Y, U, S (20-bit)

These should be read according to the original register table.

---

## 1. Immediate transfer forms

### `MV r, n`

Loads an immediate value into a register.

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | A, n | `08` n | 2 | 2 |
| MV | IL, n | `09` n | 2 | 3 |
| MV | BA, mn | `0A` n m | 3 | 3 |
| MV | I, mn | `0B` n m | 3 | 3 |
| MV | X, lmn | `0C` n m l | 4 | 4 |
| MV | Y, lmn | `0D` n m l | 4 | 4 |
| MV | U, lmn | `0E` n m l | 4 | 4 |
| MV | S, lmn | `0F` n m l | 4 | 4 |

Immediate value byte order: low byte first (p.224).

### Immediate to internal RAM

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | (m), n | `CC` m n | 3 | 3 |
| MVW | (l), mn | `CD` l m n | 4 | 4 |
| MVP | (k), lmn | `DC` k n m l | 5 | 5 |

---

## 2. Register and internal RAM forms

### `MV r, (n)`

Transfers data from internal RAM to a register.

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | A, (n) | `80` n | 2 | 3 |
| MV | IL, (n) | `81` n | 2 | 4 |
| MV | BA, (n) | `82` n | 2 | 4 |
| MV | I, (n) | `83` n | 2 | 5 |
| MV | X, (n) | `84` n | 2 | 5 |
| MV | Y, (n) | `85` n | 2 | 5 |
| MV | U, (n) | `86` n | 2 | 5 |
| MV | S, (n) | `87` n | 2 | 5 |

Writing IL also clears IH to 0 (p.224).

### `MV (n), r`

Transfers data from a register to internal RAM.

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | (n), A | `A0` n | 2 | 3 |
| MV | (n), IL | `A1` n | 2 | 3 |
| MV | (n), BA | `A2` n | 2 | 4 |
| MV | (n), I | `A3` n | 2 | 4 |
| MV | (n), X | `A4` n | 2 | 5 |
| MV | (n), Y | `A5` n | 2 | 5 |
| MV | (n), U | `A6` n | 2 | 5 |
| MV | (n), S | `A7` n | 2 | 5 |

### Register ↔ register

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | A, B | `74` | 1 | 1 |
| MV | B, A | `75` | 1 | 1 |
| MV | r2, r2' | `FD` r r' | 2 | 2 |
| MV | r3, r3' | `FD` r r' | 2 | 2 |

Second byte of FDh forms: upper nibble = source register, lower nibble = destination register.

### Internal RAM to internal RAM

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | (m), (n) | `C8` m n | 3 | 6 |
| MVW | (m), (n) | `C9` m n | 3 | 8 |
| MVP | (m), (n) | `CA` m n | 3 | 10 |

Prebyte required for addressing modes other than direct (p.74).

Repeated transfer forms:

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MVL | (m), (n) | `CB` m n | 3 | 5+2×I |
| MVLD | (m), (n) | `CF` m n | 3 | 5+2×I |

For MVL and MVLD, I ← 0 after execution.

---

## 3. Register and absolute external memory forms

### `MV r, [lmn]`

Transfers data from external memory at absolute 20-bit address `[lmn]` to a register.

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | A, [lmn] | `88` n m l | 4 | 6 |
| MV | IL, [lmn] | `89` n m l | 4 | 7 |
| MV | BA, [lmn] | `8A` n m l | 4 | 7 |
| MV | I, [lmn] | `8B` n m l | 4 | 8 |
| MV | X, [lmn] | `8C` n m l | 4 | 8 |
| MV | Y, [lmn] | `8D` n m l | 4 | 8 |
| MV | U, [lmn] | `8E` n m l | 4 | 8 |
| MV | S, [lmn] | `8F` n m l | 4 | 8 |

### `MV [lmn], r`

Transfers data from a register to external memory at `[lmn]`.

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | [lmn], A | `A8` n m l | 4 | 5 |
| MV | [lmn], IL | `A9` n m l | 4 | 5 |
| MV | [lmn], BA | `AA` n m l | 4 | 6 |
| MV | [lmn], I | `AB` n m l | 4 | 6 |
| MV | [lmn], X | `AC` n m l | 4 | 7 |
| MV | [lmn], Y | `AD` n m l | 4 | 7 |
| MV | [lmn], U | `AE` n m l | 4 | 7 |
| MV | [lmn], S | `AF` n m l | 4 | 7 |

> Note: r ≠ S for these forms (p.74).

---

## 4. Register indirect external memory forms

### `MV r, [r3]` and variants

First byte = `(90h + r)`. Second byte encodes addressing mode and r3.

| operands | Second byte | bytes | cycles (r1) |
|----------|-------------|-------|-------------|
| r, [r3] | `0r3` | 2 | 4 |
| r, [r3++] | `2r3` | 2 | 4 |
| r, [--r3] | `3r3` | 2 | 5 |
| r, [r3+n] | `8r3` n | 3 | 6 |
| r, [r3-n] | `Cr3` n | 3 | 6 |

### `MV [r3], r` and variants

First byte = `(B0h + r)`. Second byte encodes addressing mode and r3.

| operands | Second byte | bytes | cycles (r1) |
|----------|-------------|-------|-------------|
| [r3], r | `0r3` | 2 | 4 |
| [r3++], r | `2r3` | 2 | 4 |
| [--r3], r | `3r3` | 2 | 5 |
| [r3+n], r | `8r3` n | 3 | 6 |
| [r3-n], r | `Cr3` n | 3 | 6 |

These are distinct opcode families and should be decoded using the second byte as specified in the source tables.

---

## 5. Internal-RAM indirect external memory forms

### `MV r, [(n)]` and variants

First byte = `(98h + r)`. Second byte encodes addressing variant.

| operands | Second byte | bytes | cycles (r1) |
|----------|-------------|-------|-------------|
| r, [(n)] | `00` n | 3 | 9 |
| r, [(m)+n] | `80` m n | 4 | 11 |
| r, [(l)-n] | `C0` l n | 5 | 12 |

### `MV [(n)], r` and variants

First byte = `(B8h + r)`. Second byte encodes addressing variant.

| operands | Second byte | bytes | cycles (r1) |
|----------|-------------|-------|-------------|
| [(n)], r | `00` n | 3 | 9 |
| [(m)+n], r | `80` m n | 4 | 11 |
| [(l)-n], r | `C0` l n | 5 | 13 |

---

## 6. Internal RAM and external memory transfer forms

| Mnemonic | operands | Opcode | bytes | cycles |
|----------|----------|--------|-------|--------|
| MV | (k), [lmn] | `D0` k n m l | 5 | 7 |
| MVW | (k), [lmn] | `D1` k n m l | 5 | 8 |
| MVP | (k), [lmn] | `D2` k n m l | 5 | 9 |
| MVL | (k), [lmn] | `D3` k n m l | 5 | 6+2×I |
| MV | [klm], (n) | `D8` m l k n | 5 | 6 |
| MVW | [klm], (n) | `D9` m l k n | 5 | 7 |
| MVP | [klm], (n) | `DA` m l k n | 5 | 8 |
| MVL | [klm], (n) | `DB` m l k n | 5 | 6+2×I |
| MV | (n), [r3] | `E0` `0r3` n | 3 | 6 |
| MVW | (n), [r3] | `E1` `0r3` n | 3 | 7 |
| MVP | (n), [r3] | `E2` `0r3` n | 3 | 8 |
| MVL | (n), [r3++] | `E3` `2r3` n | 3 | 5+2×I |
| MV | (n), [r3++] | `E8` `2r3` n | 3 | 6 |
| MVW | (n), [r3++] | `E9` `2r3` n | 3 | 7 |
| MVP | (n), [r3++] | `EA` `2r3` n | 3 | 9 |
| MVL | (n), [r3++] | `EB` `2r3` n | 3 | 5+2×I |
| MV | (m), [(n)] | `F0` `00` m n | 4 | 11 |
| MVW | (m), [(n)] | `F1` `00` m n | 4 | 12 |
| MVP | (m), [(n)] | `F2` `00` m n | 4 | 13 |
| MVL | (m), [(n)] | `F3` `00` m n | 4 | 10+2×I |
| MV | [(l)±m], (n) | `F8` `80`/`C0` l m n | 5 | 13 |
| MVW | [(l)±m], (n) | `F9` `80`/`C0` l m n | 5 | 14 |
| MVP | [(l)±m], (n) | `FA` `80`/`C0` l m n | 5 | 15 |
| MVL | [(l)±m], (n) | `FB` `80`/`C0` l m n | 5 | 12+2×I |

For MVL forms, I ← 0 after execution.

---

## 7. Repeated transfer forms

### `MVL`

`MVL` performs repeated forward transfer using the `I` register as the repeat counter. Both addresses increment by 1 per iteration. Set I to the byte count before executing (p.77).

After execution: **I ← 0**.

When I = 0 at the start, executes 10000h (65536) times (p.77).

### `MVLD`

`MVLD` is the reverse-direction repeated transfer form. Both addresses decrement instead of increment. Otherwise identical to MVL (p.79).

Confirmed source operand: `MVLD (m), (n)` only — opcode `CF` m n.

No additional `MVLD` forms should be assumed unless directly confirmed from the source pages.

Use MVLD instead of MVL when the destination region overlaps the source at a higher address, to avoid overwriting unread data (p.79).

---

## 8. Notes on interpretation

### Do not over-normalize notation

This project preserves source-oriented notation.
For example, it keeps distinctions such as:

- `(n)` vs `[(n)]`
- `[r3]` vs `[r3+n]`
- `[r3++]` vs `[--r3]`

These are not stylistic variants.
They belong to different decoding families.

### Opcode families with second-byte dependence

Some `MV` forms share the same first byte and are distinguished by the second byte.

This is especially important for families such as:

- `90h–96h` — MV r, [r3] variants
- `98h–9Eh` — MV r, [(n)] variants
- `B0h–B6h` — MV [r3], r variants
- `B8h–BEh` — MV [(n)], r variants

When decoding real binaries, the second byte must be interpreted carefully.

### On absent forms

This document only lists forms confirmed from the source pages used in this project.

If a seemingly symmetric form is not listed here, it should **not** be assumed to exist without source confirmation.

---

## 9. Related instructions

Closely related transfer/exchange instructions include:

- `EX`
- `EXW`
- `EXP`
- `EXL`

These are described separately in `exchange.md`.

---

## 10. Scope note

This file is a structured explanation document, not a replacement for the opcode table itself.

For exact opcode / bytes / cycles values, see `opcodes-00-ff.md`.
For final confirmation, always check the relevant source pages.
