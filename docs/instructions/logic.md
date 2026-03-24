# Logic and Compare Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.89–91

---

## AND opr1, opr2

**Operation:** opr1 ← opr1 AND opr2  
**Flags:** Z: set if result = 0 / C: no change (p.90)

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `70` n |
| (m) | n | `71` m n |
| [klm] | n | `72` m l k n |
| (n) | A | `73` n |
| (m) | (n) | `76` m n |
| A | (n) | `77` n |

**Example:**
```
; AND A, 55h  —  A=C7h → A=45h
70 55
```

---

## OR opr1, opr2

**Operation:** opr1 ← opr1 OR opr2  
**Flags:** Z: set if result = 0 / C: no change (p.90)

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `78` n |
| (m) | n | `79` m n |
| [klm] | n | `7A` m l k n |
| (n) | A | `7B` n |
| (m) | (n) | `7E` m n |
| A | (n) | `7F` n |

---

## XOR opr1, opr2

**Operation:** opr1 ← opr1 XOR opr2  
**Flags:** Z: set if result = 0 / C: no change (p.90)

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `68` n |
| (m) | n | `69` m n |
| [klm] | n | `6A` m l k n |
| (n) | A | `6B` n |
| (m) | (n) | `6E` m n |
| A | (n) | `6F` n |

> Note: OR and XOR share the same operand forms as AND. For each row, the opcodes for AND (OR, XOR) are the leading values listed in the source table, with OR using the first parenthesized value and XOR the second (p.90).

---

## TEST opr1, opr2

**Operation:** Computes opr1 AND opr2, affects flags only; result is discarded (p.91)  
**Flags:** Z: set if result = 0 / C: no change (p.91)

Neither opr1 nor opr2 is modified. Used to test individual bits (p.91).

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `64` n |
| (m) | n | `65` m n |
| [klm] | n | `66` m l k n |
| (n) | A | `67` n |

**Example:**
```
; TEST (10h), 55h  —  (10h)=28h, 28h AND 55h = 00h → Z=1
; (internal RAM absolute addressing, prebyte 30h)
65 10 55
```

---

## CMP opr1, opr2

**Operation:** Computes opr1 − opr2, affects flags only; result is discarded (p.89)  
**Flags:** Z: set if result = 0 / C: set if carry or borrow (p.89)

Neither operand is modified. Used before conditional branch instructions (p.89).

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `60` n |
| (m) | n | `61` m n |
| [klm] | n | `62` m l k n |
| (n) | A | `63` n |

**Flag results for CMP A, 50h (p.89):**

| A value | Z | C |
|---------|---|---|
| 00h–4Fh | 0 | 1 |
| 50h | 1 | 0 |
| 51h–FFh | 0 | 0 |

---

## CMPW opr1, opr2 — Compare Word (2-byte)

**Operation:** Computes 2-byte opr1 − opr2, affects flags only (p.89)  
**Flags:** Z: set if result = 0 / C: set if borrow (p.89)

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `C6` m n |
| (m) | r2 | `D6` `0r2` n |

---

## CMPP opr1, opr2 — Compare Pointer (3-byte)

**Operation:** Computes 3-byte opr1 − opr2, affects flags only (p.89)  
**Flags:** Z: set if result = 0 / C: set if borrow (p.89)

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `C7` m n |
| (m) | r3 | `D7` `0r3` n |
