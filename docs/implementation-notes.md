# Implementation Notes

> Source: primary printed material, p.65–p.103, p.224–p.231

## Notes for Disassembler Implementation

### Instructions Determined by the Second Byte

The following opcodes share the same first byte; the **second byte determines the addressing mode**.
A disassembler must read the second byte before resolving the instruction.

| First byte range | Instruction group | Second byte meaning |
|---|---|---|
| 90h–97h | MV r, [external memory] | 0r3=[r3], 2r3=[r3++], 3r3=[--r3], 8r3=[r3+n], Cr3=[r3-n] |
| 98h–9Fh | MV r, [(internal RAM indirect)] | 00h=[(n)], 80h=[(m)+n], C0h=[(l)-n] |
| B0h–B7h | MV [external memory], r | 0r3=[r3], 2r3=[r3++], 3r3=[--r3], 8r3=[r3+n], Cr3=[r3-n] |
| B8h–BFh | MV [(internal RAM indirect)], r | 00h=[(n)], 80h=[(m)+n], C0h=[(l)-n] |
| 6Ch | INC r | Second byte = 0000 0r (register select) |
| 7Ch | DEC r | Second byte = 0000 0r (register select) |
| EDh | EX r, r' | Second byte = r r' (4 bits each) |
| FDh | MV r, r' | Second byte = r r' (4 bits each) |

### Identifying Prebytes

Opcodes 20h–27h and 30h–37h are prebytes.
When a prebyte is encountered, apply its addressing mode to the operands of the following instruction.

### Byte Count

- The `bytes` values in the opcode table **do not include the prebyte**
- When a prebyte is present, actual instruction length = bytes + 1
- For `[r3+n]` form (second byte 8r3/Cr3), one additional `n` byte follows the second byte

## Side Effect When Writing to IL

Any instruction that writes to IL (the low byte of I) **also clears IH to 0** simultaneously.
This applies to all such instructions including MV IL,xxx / POPU IL / etc.

## I Register Value of 0 in Loop Instructions

When the I register is 0 in a loop instruction (MVL, ADCL, EXL, etc.),
execution is treated as **10000h (65536) iterations**.

## r3 Restriction

Some external memory addressing instructions restrict r3 operands:
**r ≠ S (S register cannot be used)**.

- Instructions other than `MV r,[lmn]`: S cannot be specified as r
- Instructions other than `MV [lmn],r`: S cannot be specified as r

The encoding for r=7 (S) exists in the opcode space, but its behavior is not guaranteed.

## PUSHS / POPS

PUSHS and POPS have **dedicated opcodes only for F (Flags)**:

- PUSHS F = 4Fh (bytes: 1, cycles: 3)
- POPS F  = 5Fh (bytes: 1, cycles: 2)

For all other operands (r1, r2, X, Y, IMR), the opcodes are identical to the corresponding MV instructions (`MV [--S],r` etc.).

## Unresolved: CEh

CEh cannot be confirmed at this time (UNRESOLVED).
The corresponding cell in the p.231 machine code map is illegible in the available photographs,
and no matching entry was found in p.224–p.230.

## DEh (HALT)

A reading of "CE" appears in the opcode column of p.230, which would conflict with DEh.
However, p.102 clearly states "HALT → DE", and the p.231 machine code map
at H=D, L=E cell shows "HALT".
This repository uses **DEh = HALT** based on p.102 and p.231.

## SC and RC Opcodes

- SC (Set Carry) = **97h** (p.229: "SC → 1001 0111")
- RC (Reset Carry) = **9Fh** (p.229: "RC → 1001 1111")
