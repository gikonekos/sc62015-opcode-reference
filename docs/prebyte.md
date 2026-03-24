# Prebyte

> Source: primary printed material, p.67–p.68

## What is a Prebyte?

A prebyte is a 1-byte auxiliary code placed immediately before an instruction.
It switches the addressing mode used for the `(n)` operand in internal RAM.

When no prebyte is present, **absolute addressing (❶)** is used.

## Prebyte Table

The prebyte value is determined by the combination of addressing modes
for each of the two operands.

| 1st operand | 2nd operand | Prebyte |
|---|---|---|
| (n) | (n) | 32h |
| (n) | (BP+n) | 30h |
| (n) | (PY+n) | 33h |
| (n) | (BP+PY) | 31h |
| (BP+n) | (n) | 22h |
| (BP+n) | (BP+n) | n/a |
| (BP+n) | (PY+n) | 23h |
| (BP+n) | (BP+PY) | 21h |
| (PX+n) | (n) | 36h |
| (PX+n) | (BP+n) | 34h |
| (PX+n) | (PY+n) | 37h |
| (PX+n) | (BP+PY) | 35h |
| (BP+PX) | (n) | 26h |
| (BP+PX) | (BP+n) | 24h |
| (BP+PX) | (PY+n) | 27h |
| (BP+PX) | (BP+PY) | 25h |

## How to Read

- 1st operand: addressing mode of the first operand (or the only operand)
- 2nd operand: addressing mode of the second operand
- Prebyte: hexadecimal prebyte value for that combination

**Example:**

    27 CB 00 0A    MVL (BP+PX),(PY+0Ah)
    ^              Prebyte 27h = (PX+m) × (PY+n) combination

## Opcode Assignment

Prebytes are assigned to opcode positions 20h–27h and 30h–37h.

| Prebyte | opr1 | opr2 |
|---|---|---|
| 20h | (n) | (BP+n) |
| 21h | (n) | (BP+PY) |
| 22h | (BP+n) | (n) |
| 23h | (BP+n) | (PY+n) |
| 24h | (BP+PX) | (BP+n) |
| 25h | (BP+PX) | (BP+PY) |
| 26h | (BP+PX) | (n) |
| 27h | (BP+PX) | (PY+n) |
| 30h | (m) | (BP+n) |
| 31h | (m) | (BP+PY) |
| 32h | (m) | (n) |
| 33h | (m) | (PY+n) |
| 34h | (PX+m) | (BP+n) |
| 35h | (PX+m) | (BP+PY) |
| 36h | (PX+m) | (n) |
| 37h | (PX+m) | (PY+n) |

## Notes

- The prebyte's bytes and cycles are added to those of the following instruction
- For mode **❹ (BP+PX or BP+PY)**, no `n` value is needed at runtime, but a placeholder value (for example `00h`) must still be present in the opcode
- The combination **(BP+n) × (BP+n)** has no prebyte
- The `bytes` values in the opcode table do **not** include the prebyte
