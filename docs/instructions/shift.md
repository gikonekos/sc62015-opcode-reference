# Shift and Rotate Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.92–94

---

## ROR opr — Rotate Right

**Operation:** Rotates opr one bit to the right. Bit 0 moves to bit 7 and to C (p.92)  
**Flags:** Z: set if result = 0 / C: takes the value shifted out of bit 0 (p.92)

```
ROR:  [bit7 ← bit6 ← ... ← bit1 ← bit0] → C
       ↑_______________ (bit0 wraps to bit7) ___|
```

| opr | Opcode |
|-----|--------|
| A | `E4` |
| (n) | `E5` n |

**Example:**
```
; ROR A  —  A=B5h (1011 0101), C=0
; After:     A=DAh (1101 1010), C=1
E4
```

> Note: ROR differs from SHR. In ROR, bit 0 wraps to bit 7. In SHR, C moves into bit 7 (p.92).

---

## ROL opr — Rotate Left

**Operation:** Rotates opr one bit to the left. Bit 7 moves to bit 0 and to C (p.92)  
**Flags:** Z: set if result = 0 / C: takes the value shifted out of bit 7 (p.92)

```
ROL:  C ← [bit7 → bit6 → ... → bit1 → bit0]
            |___ (bit7 wraps to bit0) ______↑
```

| opr | Opcode |
|-----|--------|
| A | `E6` |
| (n) | `E7` n |

---

## SHR opr — Shift Right

**Operation:** Shifts opr one bit to the right. C moves into bit 7. Bit 0 moves to C (p.93)  
**Flags:** Z: set if result = 0 / C: takes the value shifted out of bit 0 (p.93)

```
SHR:  C → [bit7 ← bit6 ← ... ← bit1 ← bit0] → C
```

| opr | Opcode |
|-----|--------|
| A | `F4` |
| (n) | `F5` n |

**Example:**
```
; SHR A  —  A=B5h (1011 0101), C=0
; After:     A=5Ah (0101 1010), C=1
F4
```

> Note: SHR differs from ROR. In SHR, C moves into bit 7. In ROR, bit 0 wraps directly to bit 7 (p.93).

---

## SHL opr — Shift Left

**Operation:** Shifts opr one bit to the left. C moves into bit 0. Bit 7 moves to C (p.93)  
**Flags:** Z: set if result = 0 / C: takes the value shifted out of bit 7 (p.93)

```
SHL:  C ← [bit7 → bit6 → ... → bit1 → bit0] ← C
```

| opr | Opcode |
|-----|--------|
| A | `F6` |
| (n) | `F7` n |

---

## DSRL opr — Decimal Shift Right Loop

**Operation:** Shifts I-register bytes of internal RAM starting at opr 4 bits to the right, incrementing address (p.94)  
**Flags:** Z: set if all bytes become 0 / C: no change (p.94)  
**After execution:** I ← 0

Shifts a BCD number (stored in internal RAM) one decimal digit (4 bits) to the right. Equivalent to dividing a BCD value by 10 (p.94).

| opr | Opcode |
|-----|--------|
| (n) | `FC` n |

**Example:**
```
; DSRL (10h)  —  I=0004h, (10h–13h) = 12h,34h,56h,78h
; After: 01h,23h,45h,67h  (right shift by 4 bits = ÷10)
; (internal RAM absolute addressing, prebyte 30h)
FC 10
```

---

## DSLL opr — Decimal Shift Left Loop

**Operation:** Shifts I-register bytes of internal RAM starting at opr 4 bits to the left, decrementing address (p.94)  
**Flags:** Z: set if all bytes become 0 / C: no change (p.94)  
**After execution:** I ← 0

Shifts a BCD number one decimal digit (4 bits) to the left. Equivalent to multiplying a BCD value by 10 (p.94).

| opr | Opcode |
|-----|--------|
| (n) | `EC` n |

---

## SWAP A — Swap Nibbles

**Operation:** A[0–3] ↔ A[4–7] (upper and lower 4 bits of A are exchanged) (p.100)  
**Flags:** Z: set if result = 0 / C: no change (p.100)

| Opcode |
|--------|
| `EE` |

**Example:**
```
; SWAP A  —  A=3Ah → A=A3h
EE
```
