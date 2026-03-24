# Exchange Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.80–81

---

## EX opr1, opr2

**Operation:** opr1 ↔ opr2  
**Flags:** None affected (p.80)

Exchanges the contents of opr1 and opr2. Both operands must be the same size (p.80).

> Note: EX B, A is the same as EX A, B — there is no separate opcode for the reversed form (p.80).

### ❶ Register ↔ register

| opr1 | opr2 | Opcode |
|------|------|--------|
| A | B | `DD` |
| r2 | r2' | `ED` r2 r2' |
| r3 | r3' | `ED` r3 r3' |

Second byte for r2/r3 forms: each register is encoded in 4 bits (upper nibble = opr1, lower nibble = opr2).

### ❷ Internal RAM ↔ internal RAM

Exchange 1, 2, or 3 bytes. Prebyte required for absolute addressing (p.80).

| Mnemonic | opr1 | opr2 | Opcode |
|----------|------|------|--------|
| EX | (m) | (n) | `C0` m n |
| EXW | (m) | (n) | `C1` m n |
| EXP | (m) | (n) | `C2` m n |

**Example:**
```
; EX X, Y  —  X=12345h, Y=6789Ah → X=6789Ah, Y=12345h
; r3=4=X, r3'=5=Y → second byte = 45h
ED 45

; EXW (10h), (20h)  —  swap 2 bytes at 10h and 20h
; (internal RAM absolute addressing, prebyte 32h)
C1 10 20
```

---

## EXL opr1, opr2 — Exchange Loop

**Operation:** Exchanges I-register bytes between opr1 and opr2 regions, incrementing both addresses (p.81)  
**Flags:** None affected (p.81)  
**After execution:** I ← 0

Loop exchange of internal RAM regions. Operand combinations: internal RAM ↔ internal RAM only (p.81).

> Note: `MVL (n), [r3]` and `MVL [r3], (n)` do not exist — similarly, EXL is internal RAM only (p.81).

| opr1 | opr2 | Opcode |
|------|------|--------|
| (m) | (n) | `C3` m n |

**Example:**
```
; EXL (30h), (40h)  —  swap 3 bytes (I=0003h)
; (internal RAM absolute addressing, prebyte 32h)
; Before: (30h–32h)=12h,34h,56h  (40h–42h)=ABh,CDh,EFh
; After:  (30h–32h)=ABh,CDh,EFh  (40h–42h)=12h,34h,56h
; I → 0000h
C3 30 40
```
