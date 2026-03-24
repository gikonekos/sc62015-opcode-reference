# Stack Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.82–83

The SC62015 has two stacks:

| Stack | Pointer | Push direction | Used by |
|-------|---------|---------------|---------|
| System stack | S register | S decrements | CALL/CALLF/RETI |
| User stack | U register | U decrements | PUSHU/POPU |

---

## PUSHS opr — Push to System Stack

**Operation:** Equivalent to `MV [--S], opr` (p.82)  
**Flags:** None affected (except F operand — see POPS)

Saves opr onto the system stack (S-based). Equivalent to the corresponding MV instruction (p.82).

| opr | Opcode |
|-----|--------|
| r1 | equivalent to `MV r1, [--S]` |
| r2 | equivalent to `MV r2, [--S]` |
| X | equivalent to `MV X, [--S]` |
| Y | equivalent to `MV Y, [--S]` |
| F | `4F` |
| IMR | equivalent to `MV (IMR), [--S]` |

> For F and IMR operands, see note on POPS below (p.82).

---

## POPS opr — Pop from System Stack

**Operation:** Equivalent to `MV opr, [S++]` (p.82)  
**Flags:** Z and C change when opr = F (p.82)

Restores opr from the system stack.

| opr | Opcode |
|-----|--------|
| r1 | equivalent to `MV r1, [S++]` |
| r2 | equivalent to `MV r2, [S++]` |
| X | equivalent to `MV X, [S++]` |
| Y | equivalent to `MV Y, [S++]` |
| F | `5F` |
| IMR | equivalent to `MV (IMR), [S++]` |

**Flag note:** When opr = F, C and Z are restored from the stack value. This is the only case where POPS affects flags (p.82).

---

## PUSHU opr — Push to User Stack

**Operation:** U ← U - size(opr), then [U] ← opr. Equivalent to `MV [--U], opr` (p.83)  
**Flags:** None affected (p.83)

Saves opr onto the user stack (U-based) (p.83).

| opr | Opcode |
|-----|--------|
| r1 (A, IL) | `(28+r1)` |
| r2 (BA, I) | `(28+r2)` |
| X | `2C` |
| Y | `2D` |
| F | `2E` |
| IMR | `2F` |

**Example:**
```
; PUSHU X  —  equivalent to MV [--U], X
2C

; PUSHU F  —  saves flags onto user stack
2E
```

---

## POPU opr — Pop from User Stack

**Operation:** opr ← [U], then U ← U + size(opr). Equivalent to `MV opr, [U++]` (p.83)  
**Flags:** Z and C change when opr = F (p.83)

Restores opr from the user stack (p.83).

| opr | Opcode |
|-----|--------|
| r1 (A, IL) | `(38+r1)` |
| r2 (BA, I) | `(38+r2)` |
| X | `3C` |
| Y | `3D` |
| F | `3E` |
| IMR | `3F` |

**Flag note:** When opr = F, C and Z are updated from the popped value (p.83).

**Example:**
```
; POPU X  —  equivalent to MV X, [U++]
3C
```
