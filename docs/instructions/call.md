# Call and Return Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.98–99, 103

---

## CALL mn — Short Call (16-bit)

**Operation:** Push next instruction address (16-bit PC) to system stack, then PC ← mn  
**Flags:** None affected (p.98)

Calls a subroutine at a 16-bit address. PS is not changed; the callable range is 64K bytes within the current PS segment. Return with RET (p.98).

| opr | Opcode |
|-----|--------|
| mn | `04` n m |

**Example:**
```
; CALL 80A2h — PS unchanged, calls (PS, 80A2h)
04 A2 80
```

---

## RET — Return from CALL

**Operation:** PC ← pop from system stack  
**Flags:** None affected (p.98)

Returns from a subroutine called with CALL. Pops the 16-bit return address from the system stack (p.98).

| Opcode |
|--------|
| `06` |

---

## CALLF lmn — Far Call (20-bit)

**Operation:** Push next instruction address (20-bit: PC and PS) to system stack, then PC ← lower 16 bits of lmn, PS ← upper 4 bits of lmn  
**Flags:** None affected (p.99)

Calls a subroutine at a full 20-bit address. Both PC and PS are saved and updated. Range: 1M bytes. Return with RETF (p.99).

| opr | Opcode |
|-----|--------|
| lmn | `05` n m l |

**Example:**
```
; CALLF 0BDD69h — calls BDD69h
05 69 DD 0B
```

---

## RETF — Return from CALLF

**Operation:** PC ← pop from system stack, PS ← pop from system stack  
**Flags:** None affected (p.99)

Returns from a subroutine called with CALLF. Pops both PC and PS from the system stack (p.99).

| Opcode |
|--------|
| `07` |

---

## RETI — Return from Interrupt

**Operation:** IMR ← (S), C,Z ← (S+1), PCL ← (S+2), PCH ← (S+3), PS ← (S+4), S ← S+5  
**Flags:** Restored from stack (p.103)

Returns from an interrupt handler. Pops IMR, flags, PC, and PS from the system stack in that order, then executes RETF (p.103).

| Opcode |
|--------|
| `01` |

### Notes

- RETI is the only return instruction that restores flags. RET and RETF do not affect flags.
- IMR (Interrupt Mask Register) is at internal RAM address FBh (p.103).
