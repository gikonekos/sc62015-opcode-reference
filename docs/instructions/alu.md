# Arithmetic Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.84–88

---

## ADD opr1, opr2

**Operation:** opr1 ← opr1 + opr2  
**Flags:** Z: set if result = 0 / C: set if carry or borrow (p.84)

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `40` n |
| (m) | n | `41` m n |
| A | (n) | `42` n |
| (n) | A | `43` n |
| r2, r1' | — | `44` (PARTIAL — see opcodes table) |
| r3 | r' | `45` r3r' |
| r1' | r1' | `46` (PARTIAL — see opcodes table) |

---

## SUB opr1, opr2

**Operation:** opr1 ← opr1 − opr2  
**Flags:** Z: set if result = 0 / C: set if carry or borrow (p.84)

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `48` n |
| (m) | n | `49` m n |
| A | (n) | `4A` n |
| (n) | A | `4B` n |
| r2 | r1' | `4C` (PARTIAL) |
| r3 | r' | `4D` r3r' |
| r1' | r1' | `4E` (PARTIAL) |

---

## ADC opr1, opr2

**Operation:** opr1 ← opr1 + opr2 + C  
**Flags:** Z: set if result = 0 / C: set if carry (p.85)

Add with carry. Otherwise same as ADD (p.85).

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `50` n |
| (m) | n | `51` m n |
| A | (n) | `52` n |
| (n) | A | `53` n |

---

## SBC opr1, opr2

**Operation:** opr1 ← opr1 − opr2 − C  
**Flags:** Z: set if result = 0 / C: set if borrow (p.85)

Subtract with carry. Otherwise same as SUB (p.85).

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | n | `58` n |
| (m) | n | `59` m n |
| A | (n) | `5A` n |
| (n) | A | `5B` n |

---

## ADCL opr1, opr2 — Add with Carry Loop

**Operation:** Adds I-register bytes from opr2 region to opr1 region with carry propagation, incrementing both addresses (p.86)  
**Flags:** Z: set if result = 0 / C: set if carry (p.86)  
**After execution:** I ← 0

Loop addition. Repeats ADC I times, carrying across bytes. Set I to the byte count before executing (p.86).

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `54` m n |
| (n) | A | `55` n |

**Example:**
```
; ADCL (30h), (50h)  —  add 4-byte values at 30h and 50h
; (internal RAM absolute addressing, prebyte 32h)
; I = 0004h, C = ?
; After: I = 0, C = 1 if overflow
0B 04 00    ; MV I, 4
20          ; PRE 32h (prebyte for absolute addressing)  
54 30 50    ; ADCL (30h),(50h)
```

---

## SBCL opr1, opr2 — Subtract with Carry Loop

**Operation:** Subtracts I-register bytes of opr2 from opr1 with borrow propagation, incrementing both addresses (p.86)  
**Flags:** Z: set if result = 0 / C: set if borrow (p.86)  
**After execution:** I ← 0

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `5C` m n |
| (n) | A | `5D` n |

---

## DADL opr1, opr2 — BCD Add Loop

**Operation:** Adds I-register bytes from opr2 to opr1 in BCD (binary-coded decimal), decrementing both addresses (p.87)  
**Flags:** Z: set if result = 0 / C: set if carry (p.87)  
**After execution:** I ← 0

BCD loop addition. Each byte represents two BCD digits (4 bits each, 0–9). Addresses decrement, unlike ADCL (p.87).

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `C4` m n |
| (n) | A | `C5` n |

**Example:**
```
; DADL (10h), (20h)  —  add BCD values (prebyte 32h)
; I = 0003h (3 bytes = 6 BCD digits)
; 109524 + 032711 = 142235 (BCD)
C4 10 20
```

---

## DSBL opr1, opr2 — BCD Subtract Loop

**Operation:** Subtracts I-register bytes of opr2 from opr1 in BCD, decrementing both addresses (p.87)  
**Flags:** Z: set if result = 0 / C: set if borrow (p.87)  
**After execution:** I ← 0

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `D4` m n |
| (n) | A | `D5` n |

---

## INC opr — Increment

**Operation:** opr ← opr + 1  
**Flags:** Z: set if result = 0 / C: no change (p.88)

| opr | Opcode |
|-----|--------|
| r | `6C` `0r` (second byte selects register) |
| (m) | `6D` m |

**Example:**
```
; INC I  —  r=3=I, second byte = 03h
6C 03
```

---

## DEC opr — Decrement

**Operation:** opr ← opr − 1  
**Flags:** Z: set if result = 0 / C: no change (p.88)

| opr | Opcode |
|-----|--------|
| r | `7C` `0r` (second byte selects register) |
| (m) | `7D` m |

---

## PMDF opr1, opr2 — Pointer Modify

**Operation:** opr1 ← opr1 + opr2. No flag change (p.88)  
**Flags:** None affected (p.88)

Identical to ADD except no flags are affected. Used to modify address pointers without disturbing flags (p.88).

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | n | `47` m n |
| (n) | A | `57` n |

**Example:**
```
; PMDF (30h), A  —  A=4Ch, (30h)=21h → (30h)=6Dh, flags unchanged
; (internal RAM absolute addressing, prebyte 30h)
57 30
```
