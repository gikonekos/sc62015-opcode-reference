# Branch Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.95–97

---

## JR opr — Relative Jump

**Operation:** PC ← PC + 2 ± n  
**Flags:** None affected (p.95)

Adds a signed 8-bit offset to PC. The offset is relative to the address of the instruction following JR (i.e., PC has already advanced by 2 when the offset is applied) (p.95).

| opr | Opcode |
|-----|--------|
| +n | `12` n |
| -n | `13` n |

**Example:**
```
; JR +20h — instruction at BDC31h
; PC at execution time = BDC33h (BDC31h + 2)
; Jump destination = BDC33h + 20h = BDC53h
12 20
```

---

## JRxx opr — Conditional Relative Jump

**Operation:** If condition true: PC ← PC + 2 ± n. If false: no operation.  
**Flags:** None affected (p.95)

| xx | Condition | Opcode |
|----|-----------|--------|
| Z | Z = 1 | `18` (+n) / `19` (-n) |
| NZ | Z = 0 | `1A` (+n) / `1B` (-n) |
| C | C = 1 | `1C` (+n) / `1D` (-n) |
| NC | C = 0 | `1E` (+n) / `1F` (-n) |

**Example:**
```
; JRZ +20h — instruction address D0000h, Z=1
; Jump destination = D0022h
18 20

; Typical use with CMP (p.95):
CMP  A, 20h   ; 60 20
JRC  L1       ; 1C ...   (jump if A < 20h, C=1)
JRZ  L2       ; 18 ...   (jump if A = 20h, Z=1)
; falls through if A > 20h
```

---

## JP mn — Short Jump (16-bit)

**Operation:** PC ← mn  
**Flags:** None affected (p.96)

Jumps to the 16-bit address specified. PS is not affected; the jump range is limited to 64K bytes within the current PS segment (p.96).

| opr | Opcode |
|-----|--------|
| mn | `02` n m |

**Example:**
```
; JP 3A7Dh — jumps to address (PS, 3A7Dh)
02 7D 3A
```

---

## JPxx mn — Conditional Short Jump (16-bit)

**Operation:** If condition true: PC ← mn. If false: no operation.  
**Flags:** None affected (p.96)

| xx | Condition | Opcode |
|----|-----------|--------|
| Z | Z = 1 | `14` n m |
| NZ | Z = 0 | `15` n m |
| C | C = 1 | `16` n m |
| NC | C = 0 | `17` n m |

**Example:**
```
; JPNC 1A54h — C=1: proceed to next instruction
;            — C=0: jump to (PS, 1A54h)
17 54 1A
```

---

## JPF lmn — Far Jump (20-bit)

**Operation:** PC ← lower 16 bits of lmn, PS ← upper 4 bits of lmn  
**Flags:** None affected (p.97)

Jumps to the full 20-bit address. Both PC and PS are updated. Range: 1M bytes (p.97).

| opr | Opcode |
|-----|--------|
| lmn | `03` n m l |

**Example:**
```
; JPF 0BD12Ah — jumps to BD12Ah
03 2A D1 0B
```

---

## JP (n) — Far Jump via Internal RAM (20-bit)

**Operation:** PC ← (n), upper 8 bits; PC ← (n+1), lower 8 bits; PS ← lower 4 bits of (n+2)  
**Flags:** None affected (p.97)

Indirect far jump. The 20-bit destination address is read from three consecutive internal RAM bytes starting at address n (p.97).

| opr | Opcode |
|-----|--------|
| (n) | `10` n |

---

## JP r3 — Far Jump via Register (20-bit)

**Operation:** PC ← lower 16 bits of r3, PS ← upper 4 bits of r3  
**Flags:** None affected (p.97)

Indirect far jump using a 20-bit register (X, Y, U, or S) (p.97).

| opr | Opcode (second byte) |
|-----|---------------------|
| r3 | `11` `0r3` |

**Example:**
```
; JP X — X = C35BAh → jump to C35BAh
11 04   ; r3=4=X
```
