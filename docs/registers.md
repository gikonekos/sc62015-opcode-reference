# Registers

This document summarizes the register classes used by the **SC62015** instruction set.

## Overview

The SC62015 uses several registers of different sizes.

In the reconstructed instruction notation, registers are commonly grouped into classes such as:

- `r1`
- `r2`
- `r3`
- `r4`
- `r`

These class symbols are used in opcode tables to describe which registers are valid for a given instruction form.

## Register set

Based on the source material, the register table is:

| Register | Opcode value | Binary |
|---|---:|---:|
| A  | 0 | 000 |
| IL | 1 | 001 |
| BA | 2 | 010 |
| I  | 3 | 011 |
| X  | 4 | 100 |
| Y  | 5 | 101 |
| U  | 6 | 110 |
| S  | 7 | 111 |

## Register classes

## `r1`

`r1` is the 1-byte register class.

- `A`

## `r2`

`r2` is the 2-byte register class.

- `IL`
- `BA`
- `I`

## `r3`

`r3` is the main 20-bit register class used in many external-memory addressing forms.

- `X`
- `Y`
- `U`
- `S`

## `r4`

In the printed register table, `r4` appears as a restricted subclass used under `r3`.

In practice, the printed diagram groups:

- `Y`
- `U`

under `r4`

This distinction should be preserved as source notation, even if some later working notes simplify it.

## `r`

`r` is the general register selector used in opcode descriptions.

It covers the full register table:

- `A`
- `IL`
- `BA`
- `I`
- `X`
- `Y`
- `U`
- `S`

## Notes on notation

The source material uses register class notation as part of opcode formulas.

Examples:

- `(08 + r)`
- `0 r r'`
- `0000 1 r`

These are not separate mnemonics. They are compact ways to express opcode generation rules.

## Caution

This document follows the notation of the printed source material as closely as possible.

It does **not** attempt to normalize the register classes into a more modern or simplified notation when the original distinction may matter for opcode interpretation.
