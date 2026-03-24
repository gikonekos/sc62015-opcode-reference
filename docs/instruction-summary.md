# Instruction Summary

This document provides a compact summary of the main instruction groups in the **SC62015** instruction set.

It is intended as a navigation-level reference.
It does **not** replace the full opcode tables.

## Purpose

The SC62015 instruction set includes several kinds of operations:

- data transfer and exchange
- stack operations
- arithmetic and logic
- rotate and shift
- conditional and unconditional branches
- call and return
- miscellaneous control instructions

This summary is meant to help readers understand the overall structure before consulting detailed opcode tables.

## Data transfer and exchange

Main mnemonics include:

- `MV`
- `MVW`
- `MVP`
- `MVL`
- `MVLD`
- `EX`
- `EXW`
- `EXP`
- `EXL`

These instructions move or exchange data between:

- registers
- internal RAM
- external memory space

The SC62015 distinguishes transfer size by mnemonic family.
For example, `MV`, `MVW`, and `MVP` are related but represent different data sizes.

Loop transfer instructions such as `MVL` and `MVLD` are a distinctive feature of this CPU family.

## Stack instructions

Main mnemonics include:

- `PUSHS`
- `POPS`
- `PUSHU`
- `POPU`

The source material distinguishes between:

- **system stack**
- **user stack**

These instructions save and restore register contents using the stack pointed to by `S` or `U`.

## Arithmetic instructions

Main mnemonics include:

- `ADD`
- `SUB`
- `ADC`
- `SBC`
- `ADCL`
- `SBCL`
- `DADL`
- `DSBL`
- `INC`
- `DEC`
- `PMDF`

These instructions cover:

- ordinary addition and subtraction
- carry-aware arithmetic
- multi-byte internal-RAM arithmetic
- decimal-style multi-byte operations in specific instruction families
- increment and decrement

Some instruction families operate only on specific operand forms.
That restriction should be checked in the detailed tables.

## Comparison and logical instructions

Main mnemonics include:

- `CMP`
- `CMPW`
- `CMPP`
- `AND`
- `OR`
- `XOR`
- `TEST`

These instructions are used for:

- comparison before branching
- bitwise logic
- flag-only tests

`TEST` is especially important because it affects flags without storing the logical result.

## Rotate and shift instructions

Main mnemonics include:

- `ROR`
- `ROL`
- `SHR`
- `SHL`
- `DSRL`
- `DSLL`

These instructions cover:

- 1-bit rotate
- 1-bit shift
- decimal-digit-style shift in internal RAM loop forms

The source material treats rotate and shift as a separate conceptual group.

## Relative branch instructions

Main mnemonics include:

- `JR`
- `JRZ`
- `JRNZ`
- `JRC`
- `JRNC`

These are relative branches.
They add a signed offset to the current program flow.

They are useful for short-range control flow and compact code.

## Short jump instructions (16-bit)

Main mnemonics include:

- `JP`
- `JPZ`
- `JPNZ`
- `JPC`
- `JPNC`

These are 16-bit jump forms.
They are different from relative branches and should be treated as a separate group.

## Far jump instructions (20-bit)

Main mnemonics include:

- `JPF`
- `JP`

In the source material, `JP` appears in both 16-bit and 20-bit forms depending on operand form.
This is an important detail and should be preserved exactly.

## Call and return instructions

Main mnemonics include:

- `CALL`
- `RET`
- `CALLF`
- `RETF`
- `RETI`

These instructions handle:

- short call / return
- far call / return
- interrupt return

`RETI` is separate from ordinary return and must not be merged conceptually with `RET` or `RETF`.

## Miscellaneous instructions

Main mnemonics include:

- `SWAP A`
- `SC`
- `RC`
- `NOP`
- `WAIT`
- `HALT`
- `OFF`
- `IR`
- `RESET`

These include:

- flag control
- no-operation and wait control
- clock stop instructions
- software interrupt generation
- software reset

Some of these are simple one-byte control instructions and appear prominently in the opcode tables.

## Notes

This summary follows the editorial policy of this repository:

- do not invent missing behavior
- do not silently normalize source notation
- keep distinctions that exist in the printed material

For exact operand forms, opcode values, byte lengths, cycles, and addressing restrictions, consult the detailed reconstructed tables and methodology notes.
