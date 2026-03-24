# MV / MVW / MVP / MVL / MVLD

Data transfer instructions.

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.73–79

---

## MV opr1, opr2

**Operation:** opr1 ← opr2  
**Flags:** None affected (p.73)

Copies the content of opr2 into opr1. The data size transferred depends on the register or operand type:

| Size | When |
|------|------|
| 8-bit | r1 (A, IL) or internal RAM single byte |
| 16-bit | r2 (BA, I) |
| 20-bit | r3 (X, Y, U, S) |

For internal RAM ↔ internal RAM or internal RAM ↔ external memory transfers where the size is not implied by a register, use **MVW** (2-byte) or **MVP** (3-byte) explicitly (p.73).

### Operand combinations

#### ❶ Immediate → register or internal RAM (p.73)

| opr1 | opr2 | Opcode |
|------|------|--------|
| r1 | n | `(08+r1)` n |
| r2 | mn | `(08+r2)` n m |
| r3 | lmn | `(08+r3)` n m l |
| (m) | n | `CC` m n |
| MVW (l), mn | — | `CD` l m n |
| MVP (k), lmn | — | `DC` k n m l |

#### ❷ Register ↔ register (p.74)

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | B | `74` |
| B | A | `75` |
| r2 | r2' | `FD` r2 r2' |
| r3 | r3' | `FD` r3 r3' |

#### ❸ Internal RAM ↔ internal RAM (p.74)

Prebyte required. See `docs/prebyte.md`.

| Mnemonic | opr1, opr2 | Opcode |
|----------|-----------|--------|
| MV | (m), (n) | `C8` m n |
| MVW | (m), (n) | `C9` m n |
| MVP | (m), (n) | `CA` m n |

#### ❹ Register ↔ internal RAM (p.74)

Prebyte may be used.

| opr1 | opr2 | Opcode |
|------|------|--------|
| r | (n) | `(8r)` n |
| (n) | r | `(Ar)` n |

#### ❺ Register ↔ external memory (p.74)

> Note: r ≠ S except for `MV r, [lmn]` and `MV [lmn], r` (p.74)

**MV r, [addr]:**

| opr2 addressing | Opcode (second byte) |
|-----------------|---------------------|
| [lmn] | `(88+r)` n m l |
| [r3] | `9r` `0r3` |
| [r3++] | `9r` `2r3` |
| [--r3] | `9r` `3r3` |
| [r3+n] | `9r` `8r3` n |
| [r3-n] | `9r` `Cr3` n |
| [(n)] | `(98+r)` `00` n |
| [(m)+n] | `(98+r)` `80` m n |
| [(l)-n] | `(98+r)` `C0` l n |

**MV [addr], r:**

| opr1 addressing | Opcode (second byte) |
|-----------------|---------------------|
| [lmn] | `(A8+r)` n m l |
| [r3] | `Br` `0r3` |
| [r3++] | `Br` `2r3` |
| [--r3] | `Br` `3r3` |
| [r3+n] | `Br` `8r3` n |
| [r3-n] | `Br` `Cr3` n |
| [(n)] | `(B8+r)` `00` n |
| [(m)+n] | `(B8+r)` `80` m n |
| [(l)-n] | `(B8+r)` `C0` l n |

#### ❻ Internal RAM ↔ external memory (p.75)

| Mnemonic | opr1 | opr2 | Opcode |
|----------|------|------|--------|
| MV / MVW / MVP | (k) | [lmn] | `D0/D1/D2` k n m l |
| MV / MVW / MVP | [klm] | (n) | `D8/D9/DA` m l k n |
| MV / MVW / MVP | (n) | [r3] | `E0/E1/E2` `0r3` n |
| MV / MVW / MVP | (n) | [r3++] | `E8/E9/EA` `2r3` n |
| MV / MVW / MVP | (m) | [(n)] | `F0/F1/F2` `00` m n |
| MV / MVW / MVP | [(l)±m] | (n) | `F8/F9/FA` `80`/`C0` l m n |

---

### Notes

- **Writing IL also clears IH to 0** simultaneously (p.224). This applies to all MV instructions with IL as the destination.
- Prebyte is required for internal RAM indirect addressing modes. See `docs/prebyte.md` (p.68).

### Examples

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

## MVW opr1, opr2

**Operation:** opr1L ← opr2L, opr1H ← opr2H  
**Flags:** None affected

2-byte (16-bit) variant of MV. Transfers two consecutive bytes. The low byte is at the lower address.

Operand combinations are a subset of MV ❸❻ (p.73, p.75). Not available for register operands (register size is determined by the register itself).

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

Loop transfer instruction. Executes MV I-register times, incrementing both source and destination addresses by 1 per iteration. When I = 0, executes 10000h (65536) times (p.77).

Operand combinations: ❸ internal RAM ↔ internal RAM, and ❻ internal RAM ↔ external memory only (p.77).

> Note: `MVL (n), [r3]` and `MVL [r3], (n)` do not exist (p.77).

### Notes

- **Set the I register before executing MVL.** The byte count to transfer must be loaded into I in advance.
- **After execution, I = 0.**
- When source and destination regions overlap, use **MVLD** instead to avoid overwriting unread data (p.79).

### Example

```
; Copy 4 bytes from internal RAM 20h–23h to 10h–13h
; (absolute addressing, prebyte 32h)

MV    I, 04h     ; I = 4  → 0B 04 00
PRE   32h        ; (m)×(n) prebyte
MVL   (10h),(20h) ; CB 10 20
; After: I = 0, addresses 10h–13h contain the copied data
```

---

## MVLD opr1, opr2

**Operation:** Transfers I register bytes from opr2 region to opr1 region, decrementing both addresses.  
**Flags:** None affected (p.79)  
**After execution:** I ← 0

Loop transfer instruction identical to MVL except addresses are decremented instead of incremented (p.79). Opcode: `CF` m n.

Operand: internal RAM ↔ internal RAM only (`(m), (n)`).

### When to use MVLD instead of MVL

Use MVLD when the source and destination regions overlap and the destination is at a higher address than the source. MVL always copies from low to high addresses; if the destination overlaps with the source at a higher offset, MVL will overwrite source data before it is read. MVLD copies from high to low, avoiding this problem (p.79).

### Example

```
; Move internal RAM 10h–1Fh upward to 20h–2Fh
; (overlapping regions — must use MVLD, not MVL)

MV    I, 10h      ; I = 16
PRE   32h
MVLD  (3Fh),(2Fh) ; CF 3F 2F   (start from the top end)
```
