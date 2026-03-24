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

- `r1`
- `r2`
- `r3`
- `r4`
- `r`

These should be read according to the original register table.

---

## 1. Immediate transfer forms

### `MV r, n`

Loads an immediate value into a register.

Examples of the family include:

- `MV A, n`
- `MV IL, n`
- `MV BA, mn`
- `MV I, mn`
- `MV X, lmn`
- `MV Y, lmn`
- `MV U, lmn`
- `MV S, lmn`

The number of immediate bytes depends on the destination register size.

---

## 2. Register and internal RAM forms

### `MV r, (n)`

Transfers data from internal RAM to a register.

Examples:

- `MV A, (n)`
- `MV IL, (n)`
- `MV BA, (n)`
- `MV I, (n)`
- `MV X, (n)`
- `MV Y, (n)`
- `MV U, (n)`
- `MV S, (n)`

### `MV (n), r`

Transfers data from a register to internal RAM.

Examples:

- `MV (n), A`
- `MV (n), IL`
- `MV (n), BA`
- `MV (n), I`
- `MV (n), X`
- `MV (n), Y`
- `MV (n), U`
- `MV (n), S`

### Internal RAM to internal RAM

The source also includes direct memory-to-memory forms:

- `MV (m), (n)`
- `MVW (m), (n)`
- `MVP (m), (n)`

Repeated transfer form:

- `MVL (m), (n)`

Reverse-direction repeated transfer form:

- `MVLD (m), (n)`

---

## 3. Register and absolute external memory forms

### `MV r, [lmn]`

Transfers data from external memory at absolute 20-bit address `[lmn]` to a register.

Examples:

- `MV A, [lmn]`
- `MV IL, [lmn]`
- `MV BA, [lmn]`
- `MV I, [lmn]`
- `MV X, [lmn]`
- `MV Y, [lmn]`
- `MV U, [lmn]`
- `MV S, [lmn]`

### `MV [lmn], r`

Transfers data from a register to external memory at `[lmn]`.

Examples:

- `MV [lmn], A`
- `MV [lmn], IL`
- `MV [lmn], BA`
- `MV [lmn], I`
- `MV [lmn], X`
- `MV [lmn], Y`
- `MV [lmn], U`
- `MV [lmn], S`

---

## 4. Register indirect external memory forms

### `MV r, [r3]`

Transfers data from external memory addressed by an `r3` register.

### `MV r, [r3++]`

Transfers data using the current `r3` address, then increments the register.

### `MV r, [--r3]`

Decrements the register first, then transfers data using the resulting address.

### `MV r, [r3+n]`

Transfers data using an indexed address based on `r3`.

The source pages should be consulted for the exact opcode subforms and byte layout of these variants.

### Reverse direction: store through register indirect

The corresponding store-side family also exists:

- `MV [r3], r`
- `MV [r3++], r`
- `MV [--r3], r`

These are distinct opcode families and should be decoded using the second byte as specified in the source tables.

---

## 5. Internal-RAM indirect external memory forms

### `MV r, [(n)]`

Transfers data from the external memory address obtained indirectly from internal RAM.

### `MV r, [(m)+n]`

Transfers data using an internal-RAM indirect base plus offset.

### `MV r, [(l)-n]`

Transfers data using an internal-RAM indirect base minus offset.

The corresponding store-side family includes:

- `MV [(n)], r`

Additional offset-based forms are also described in the source pages and should be interpreted using the original instruction tables.

---

## 6. Internal RAM and external memory transfer forms

The source also includes mixed forms between internal RAM and external memory.

Examples include:

- `MV (k), [lmn]`
- `MVW (k), [lmn]`
- `MVP (k), [lmn]`
- `MVL (k), [lmn]`

and the reverse direction:

- `MV [klm], (n)`
- `MVW [klm], (n)`
- `MVP [klm], (n)`
- `MVL [klm], (n)`

These forms are important because they show that transfer size is encoded not only by operands, but also by mnemonic family.

---

## 7. Repeated transfer forms

### `MVL`

`MVL` performs repeated forward transfer using the `I` register as the repeat counter.

Examples confirmed in the source family include:

- `MVL (m), (n)`
- `MVL (k), [lmn]`
- `MVL [klm], (n)`
- `MVL (n), [r3++]`
- `MVL (m), [(n)]`
- `MVL [(l)±m], (n)`

### `MVLD`

`MVLD` is the reverse-direction repeated transfer form.

Confirmed source family:

- `MVLD (m), (n)`

No additional `MVLD` forms should be assumed unless directly confirmed from the source pages.

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

- `90h–97h`
- `98h–9Fh`
- `B0h–B7h`
- `B8h–BFh`

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

These are described separately in the exchange instruction notes.

---

## 10. Scope note

This file is a structured explanation document, not a replacement for the opcode table itself.

For exact opcode / bytes / cycles values, see the opcode reference tables.
For final confirmation, always check the relevant source pages.
