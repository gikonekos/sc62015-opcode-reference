# Addressing Modes

This document summarizes the addressing forms used in the reconstructed **SC62015** instruction descriptions.

## Basic distinction

The source material distinguishes clearly between:

- **internal RAM operands**
- **external memory operands**

These are written differently and should not be mixed.

## Internal RAM notation

Internal RAM operands are written with parentheses.

Typical forms include:

- `(n)`
- `(BP+n)`
- `(PX+n)`
- `(PY+n)`
- `(BP+PX)`
- `(BP+PY)`

The exact interpretation depends on the prebyte placed before the instruction.

## Prebyte and internal RAM addressing

For internal RAM addressing, a **prebyte** may be required.

The prebyte determines how `(n)` should be interpreted, such as:

- absolute internal RAM address
- BP-relative form
- PX/PY-relative form
- combined base-plus-pointer form

This is an important part of SC62015 notation and should be preserved exactly as shown in the source tables.

## External memory notation

External memory operands are written with square brackets.

Typical forms include:

- `[lmn]`
- `[r3]`
- `[r3+n]`
- `[r3++]`
- `[--r3]`
- `[(n)]`
- `[(m)±n]`

These forms represent different external-memory addressing modes.

## Main external-memory forms

## 1. Absolute address

- `[lmn]`

This directly specifies a 20-bit address.

## 2. Register indirect

- `[r3]`

The address is taken from an `r3` register.

## 3. Register indirect with offset

- `[r3+n]`
- `[r3-n]`

The effective address is the value of `r3` plus or minus an offset.

## 4. Post-increment

- `[r3++]`

The access uses the current `r3` address, then increments the register by the data size used by the instruction.

## 5. Pre-decrement

- `[--r3]`

The register is decremented by the data size first, and the resulting address is used for the access.

## 6. Internal-RAM indirect

- `[(n)]`

A 20-bit address is formed from internal RAM contents and then used as an external-memory address.

## 7. Internal-RAM indirect with offset

- `[(m)+n]`
- `[(m)-n]`

A 20-bit address is formed from internal RAM contents, then adjusted by an offset.

## Data size and mnemonic form

The SC62015 distinguishes transfer size not only by operands, but also by mnemonic family.

Typical examples:

- `MV`   : 1-byte transfer
- `MVW`  : 2-byte transfer
- `MVP`  : 3-byte transfer
- `MVL`  : loop transfer
- `MVLD` : reverse-direction loop transfer

This is especially important when both operands are memory-like expressions and size cannot be inferred from a register alone.

## Editorial policy

This document preserves source-style notation.

It does not rewrite:

- `(n)` into a modern pseudo-syntax
- `[r3++]` into another assembler dialect
- internal/external operands into a unified invented notation

## Caution

Some addressing forms may look similar while belonging to different decoding families.

For example:

- `(n)` and `[(n)]`
- `[r3]` and `[r3+n]`
- `[r3++]` and `[--r3]`

must be treated as distinct forms.
