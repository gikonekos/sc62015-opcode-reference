# Miscellaneous Instructions

**Source:** *PC-E650/PC-U6000 活用研究*, Kogakusha, 1995, ISBN 4-87593-206-5 — pp.100–103

---

## SC — Set Carry

**Operation:** C ← 1  
**Flags:** Z: no change / C: set to 1 (p.100)

| Opcode |
|--------|
| `97` |

```
; SC
97
; After: C = 1
```

---

## RC — Reset Carry

**Operation:** C ← 0  
**Flags:** Z: no change / C: reset to 0 (p.101)

| Opcode |
|--------|
| `9F` |

```
; RC
9F
; After: C = 0
```

---

## TCL — Timer Clock Load

**Operation:** Divider ← D  
**Flags:** None affected (p.230 enlarged)

| Opcode |
|--------|
| `CE` |

---

## NOP — No Operation

**Operation:** Does nothing (p.101)  
**Flags:** None affected (p.101)

| Opcode |
|--------|
| `00` |

---

## WAIT — Wait

**Operation:** CPU halts for (I + 1) cycles. I ← I − 1 each cycle; ends when I = 0 (p.101)  
**Flags:** None affected (p.101)  
**cycles:** 1+I

Used to introduce a delay. One cycle = 1.30 μs at 2.304 MHz (p.101). Set I to the desired wait count before executing.

| Opcode |
|--------|
| `EF` |

**Example:**
```
; Wait 1Fh (31) cycles  (I=001Eh → 001Eh+1 = 1Fh cycles)
MV  I, 001Eh   ; 0B 1E 00
WAIT           ; EF
; After: I = 0000h
```

---

## HALT — Halt System Clock

**Operation:** Stops the system clock. CPU halts (p.102)  
**Flags:** None affected (p.102)

Execution resumes when an interrupt occurs (p.102).

| Opcode |
|--------|
| `DE` |

---

## OFF — System Clock and Sub Clock Stop

**Operation:** Stops the system clock and sub clock (p.102)  
**Flags:** None affected (p.102)  
**cycles:** unreadable in source (p.230)

Equivalent to pressing CTRL+OFF. Stops the CPU and LCD controller. Power-on restores operation (p.102).

| Opcode |
|--------|
| `DF` |

---

## IR — Software Interrupt

**Operation:** Saves IMR and flags to system stack, then calls the interrupt vector at FFFFFAh (3 bytes) (p.102)  
**Flags:** None affected (p.102)

Triggers a software interrupt. IMR (internal RAM FBh) and flags are pushed to the system stack before jumping to the interrupt handler (p.102).

| Opcode |
|--------|
| `FE` |

---

## RESET — Software Reset

**Operation:** Jumps to the address stored at the reset vector (FFFFFDh, 3 bytes) (p.103)  
**Flags:** None affected (p.103)

Performs a software reset by reading the reset vector and jumping to it (p.103).

| Opcode |
|--------|
| `FF` |
