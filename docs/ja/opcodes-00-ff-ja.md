# SC62015 OPコード完全表 00h〜FFh

**バージョン：** v5_C  
**一次資料：** 「PC-E650・PC-U6000 活用研究」I/O 編集部 著、工学社、1995年11月25日、ISBN 4-87593-206-5

---

## 表記方針

| 項目 | 説明 |
|------|------|
| `bytes` | プレバイトを含まない命令バイト数 |
| `cycles` | 代表値。条件分岐命令は「成立/不成立」順。ループ命令は I レジスタ依存 |
| `status` | `CONFIRMED` / `PARTIAL` / `UNRESOLVED` |

**オペランド記法：**

| 記法 | 意味 |
|------|------|
| `(n)` | 内部RAMアドレス（プレバイトで指定モード変更可） |
| `[lmn]` | 外部メモリ絶対アドレス 20ビット |
| `[r3]` | レジスタ間接（r3 = X/Y/U/S） |
| `[(n)]` | 内部RAM間接 |
| `n,m,l` | 即値（低バイト→高バイト順） |

**レジスタ選択（r フィールド 3ビット）：**

| 値 | レジスタ |
|----|---------|
| 000 | A |
| 001 | IL |
| 010 | BA |
| 011 | I |
| 100 | X |
| 101 | Y |
| 110 | U |
| 111 | S |

---

## 00h〜3Fh

| opcode | status | mnemonic | operands | bytes | cycles | notes |
|--------|--------|----------|----------|-------|--------|-------|
| 00h | CONFIRMED | NOP | - | 1 | 1 | p.229「0000 0000」。フラグ変化なし |
| 01h | CONFIRMED | RETI | - | 1 | 7 | p.230「0000 0001 / cycles:7」。IMR←(S), C,Z←(S+1), PCL←(S+2), PCH←(S+3), PS←(S+4), S←S+5 |
| 02h | CONFIRMED | JP | mn | 3 | 4 | p.229「0000 0010 / cycles:4」。p.96「02 n m」。ショートジャンプ16ビット。PSに影響なし |
| 03h | CONFIRMED | JPF | lmn | 4 | 5 | p.229「0000 0011 / cycles:5」。p.97「03 n m l」。ファージャンプ20ビット |
| 04h | CONFIRMED | CALL | mn | 3 | 6 | p.229「0000 0100 / cycles:6」。p.98「04 n m」。ショートコール16ビット |
| 05h | CONFIRMED | CALLF | lmn | 4 | 8 | p.229「0000 0101 / cycles:8」。p.99「05 n m l」。ファーコール20ビット |
| 06h | CONFIRMED | RET | - | 1 | 4 | p.229「0000 0110 / cycles:4」。p.98「06」。CALLからのリターン |
| 07h | CONFIRMED | RETF | - | 1 | 5 | p.229「0000 0111 / cycles:5」。p.99「07」。CALLFからのリターン |
| 08h | CONFIRMED | MV | A, n | 2 | 2 | p.224「0000 1 r / r=0=A / cycles:A:2/IL:3」。即値ロード |
| 09h | CONFIRMED | MV | IL, n | 2 | 3 | p.224「r=1=IL / cycles:3」。IL書き込み時 IH←0 |
| 0Ah | CONFIRMED | MV | BA, mn | 3 | 3 | p.224「r=2=BA / cycles:3」 |
| 0Bh | CONFIRMED | MV | I, mn | 3 | 3 | p.224「r=3=I / cycles:3」 |
| 0Ch | CONFIRMED | MV | X, lmn | 4 | 4 | p.224「r=4=X / cycles:4」 |
| 0Dh | CONFIRMED | MV | Y, lmn | 4 | 4 | p.224「r=5=Y / cycles:4」 |
| 0Eh | CONFIRMED | MV | U, lmn | 4 | 4 | p.224「r=6=U / cycles:4」 |
| 0Fh | CONFIRMED | MV | S, lmn | 4 | 4 | p.224「r=7=S / cycles:4」 |
| 10h | CONFIRMED | JP | (n) | 2 | 6 | p.229「0001 0000 / cycles:6」。p.97「10 n」。内部RAM間接ファージャンプ20ビット |
| 11h | CONFIRMED | JP | r3 | 2 | 4 | p.229「0001 0001 / 0000 0 r3 / cycles:4」。p.97「11 0r3」。レジスタ間接ファージャンプ |
| 12h | CONFIRMED | JR | +n | 2 | 3 | p.229「0010 001⁰ / cycles:3」。p.95「12(13) n」。相対ジャンプ正方向 |
| 13h | CONFIRMED | JR | -n | 2 | 3 | p.95「-nのとき括弧内の値13を使う」。相対ジャンプ負方向 |
| 14h | CONFIRMED | JPZ | mn | 3 | 4/3 | p.229「0001 0100 / cycles:4/3」。p.96「14 n m」。Z=1でジャンプ |
| 15h | CONFIRMED | JPNZ | mn | 3 | 4/3 | p.229「0001 0101」。p.96「15 n m」。Z=0でジャンプ |
| 16h | CONFIRMED | JPC | mn | 3 | 4/3 | p.229「0001 0110」。p.96「16 n m」。C=1でジャンプ |
| 17h | CONFIRMED | JPNC | mn | 3 | 4/3 | p.229「0001 0111」。p.96「17 n m」。C=0でジャンプ |
| 18h | CONFIRMED | JRZ | +n | 2 | 3/2 | p.229「0001 100⁰₁ / cycles:3/2」。p.95「Z: 18(19) n」。Z=1で相対ジャンプ正方向 |
| 19h | CONFIRMED | JRZ | -n | 2 | 3/2 | p.95「Z: 18(19) n」括弧内=負方向 |
| 1Ah | CONFIRMED | JRNZ | +n | 2 | 3/2 | p.229「0001 101⁰₁」。p.95「NZ: 1A(1B) n」。Z=0で相対ジャンプ正方向 |
| 1Bh | CONFIRMED | JRNZ | -n | 2 | 3/2 | p.95「NZ: 1A(1B) n」括弧内=負方向 |
| 1Ch | CONFIRMED | JRC | +n | 2 | 3/2 | p.229「0001 110⁰₁」。p.95「C: 1C(1D) n」。C=1で相対ジャンプ正方向 |
| 1Dh | CONFIRMED | JRC | -n | 2 | 3/2 | p.95「C: 1C(1D) n」括弧内=負方向 |
| 1Eh | CONFIRMED | JRNC | +n | 2 | 3/2 | p.229「0001 111⁰₁」。p.95「NC: 1E(1F) n」。C=0で相対ジャンプ正方向 |
| 1Fh | CONFIRMED | JRNC | -n | 2 | 3/2 | p.95「NC: 1E(1F) n」括弧内=負方向 |
| 20h | CONFIRMED | PRE | (n)(BP+n) | - | - | p.231 H=2,L=0。p.68プレバイト表「(n)×(BP+n) = 20h」。bytes/cycles は本命令に依存 |
| 21h | CONFIRMED | PRE | (n)(BP+PY) | - | - | p.231 H=2,L=1。p.68「(n)×(BP+PY) = 21h」 |
| 22h | CONFIRMED | PRE | (BP+n)(n) | - | - | p.231 H=2,L=2。p.68「(BP+n)×(n) = 22h」 |
| 23h | CONFIRMED | PRE | (BP+n)(PY+n) | - | - | p.231 H=2,L=3。p.68「(BP+n)×(PY+n) = 23h」 |
| 24h | CONFIRMED | PRE | (BP+PX)(BP+n) | - | - | p.231 H=2,L=4。p.68「(BP+PX)×(BP+n) = 24h」 |
| 25h | CONFIRMED | PRE | (BP+PX)(BP+PY) | - | - | p.231 H=2,L=5。p.68「(BP+PX)×(BP+PY) = 25h」 |
| 26h | CONFIRMED | PRE | (BP+PX)(n) | - | - | p.231 H=2,L=6。p.68「(BP+PX)×(n) = 26h」 |
| 27h | CONFIRMED | PRE | (BP+PX)(PY+n) | - | - | p.231 H=2,L=7。p.68「(BP+PX)×(PY+n) = 27h」 |
| 28h | CONFIRMED | PUSHU | A | 1 | 3 | p.230「0010 1 r / r=0=A / cycles:3」。p.83「(28+r1)」 |
| 29h | CONFIRMED | PUSHU | IL | 1 | 3 | p.230「r=1=IL / cycles:3」 |
| 2Ah | CONFIRMED | PUSHU | BA | 1 | 4 | p.230「r=2=BA / cycles:4」 |
| 2Bh | CONFIRMED | PUSHU | I | 1 | 4 | p.230「r=3=I / cycles:4」 |
| 2Ch | CONFIRMED | PUSHU | X | 1 | 5 | p.230「r=4=X / cycles:5」。p.83「2C」 |
| 2Dh | CONFIRMED | PUSHU | Y | 1 | 5 | p.230「r=5=Y / cycles:5」。p.83「2D」 |
| 2Eh | CONFIRMED | PUSHU | F | 1 | 3 | p.230「0010 1110 = 2Eh / cycles:3」。p.83「2E」。(U-1)←C,Z |
| 2Fh | CONFIRMED | PUSHU | IMR | 1 | 3 | p.230「0010 1111 = 2Fh / cycles:3」。p.83「2F」 |
| 30h | CONFIRMED | PRE | (m)(BP+n) | - | - | p.231 H=3,L=0。p.68「(m)×(BP+n) = 30h」 |
| 31h | CONFIRMED | PRE | (m)(BP+PY) | - | - | p.231 H=3,L=1。p.68「(m)×(BP+PY) = 31h」 |
| 32h | CONFIRMED | PRE | (m)(n) | - | - | p.231 H=3,L=2。p.68「(m)×(n) = 32h」 |
| 33h | CONFIRMED | PRE | (m)(PY+n) | - | - | p.231 H=3,L=3。p.68「(m)×(PY+n) = 33h」 |
| 34h | CONFIRMED | PRE | (PX+m)(BP+n) | - | - | p.231 H=3,L=4。p.68「(PX+m)×(BP+n) = 34h」 |
| 35h | CONFIRMED | PRE | (PX+m)(BP+PY) | - | - | p.231 H=3,L=5。p.68「(PX+m)×(BP+PY) = 35h」 |
| 36h | CONFIRMED | PRE | (PX+m)(n) | - | - | p.231 H=3,L=6。p.68「(PX+m)×(n) = 36h」 |
| 37h | CONFIRMED | PRE | (PX+m)(PY+n) | - | - | p.231 H=3,L=7。p.68「(PX+m)×(PY+n) = 37h」 |
| 38h | CONFIRMED | POPU | A | 1 | 2 | p.230「0011 1 r / r=0=A / cycles:2」。p.83「(38+r1)」 |
| 39h | CONFIRMED | POPU | IL | 1 | 3 | p.230「r=1=IL / cycles:A:2/IL:3」。IL書き込み時 IH←0 |
| 3Ah | CONFIRMED | POPU | BA | 1 | 3 | p.230「r=2=BA / cycles:3」 |
| 3Bh | CONFIRMED | POPU | I | 1 | 3 | p.230「r=3=I / cycles:3」 |
| 3Ch | CONFIRMED | POPU | X | 1 | 4 | p.230「r=4=X / cycles:4」。p.83「3C」 |
| 3Dh | CONFIRMED | POPU | Y | 1 | 4 | p.230「r=5=Y / cycles:4」。p.83「3D」 |
| 3Eh | CONFIRMED | POPU | F | 1 | 2 | p.230「0011 1110 = 3Eh / cycles:2」。p.83「3E」。C,Z←(U) フラグ変化あり |
| 3Fh | CONFIRMED | POPU | IMR | 1 | 2 | p.230「0011 1111 = 3Fh / cycles:2」。p.83「3F」 |

---

## 40h〜7Fh

| opcode | status | mnemonic | operands | bytes | cycles | notes |
|--------|--------|----------|----------|-------|--------|-------|
| 40h | CONFIRMED | ADD | A, n | 2 | 3 | p.84「40(48) n」, p.227「ADD A,n = 40h」 |
| 41h | CONFIRMED | ADD | (m), n | 3 | 4 | p.84「41(49) m n」, p.227「ADD (m),n = 41h」 |
| 42h | CONFIRMED | ADD | A, (n) | 2 | 4 | p.84「42(4A) n」, p.227「ADD A,(n) = 42h」 |
| 43h | CONFIRMED | ADD | (n), A | 2 | 4 | p.84「43(4B) n」, p.227「ADD (n),A = 43h」 |
| 44h | PARTIAL | ADD | r2, r1' / r2, r2' | 2 | 5 | p.227拡大直読「r2,r1'とr2,r2'が同44h行に並列。cycles=5。第2バイト=0r 0r'」。r2,r2'形の第2バイト「00」の解釈未確定 |
| 45h | CONFIRMED | ADD | r3, r' | 2 | 7 | p.84「45(4D) r3r」, p.227「ADD r3,r' = 45h」 |
| 46h | CONFIRMED | ADD | r1, r1' | 2 | 3 | p.227拡大直読「ADD r1,r1' / 0100 0110 = 46h / bytes:2 / cycles:3」。第2バイト=0r 0r' |
| 47h | CONFIRMED | PMDF | (m), n | 3 | 4 | p.88, p.230「47h / bytes:3 / cycles:4」 |
| 48h | CONFIRMED | SUB | A, n | 2 | 3 | p.84「40(48)」括弧内=SUB, p.227「SUB A,n = 48h」 |
| 49h | CONFIRMED | SUB | (m), n | 3 | 4 | p.227「SUB (m),n = 49h」 |
| 4Ah | CONFIRMED | SUB | A, (n) | 2 | 4 | p.227「SUB A,(n) = 4Ah」 |
| 4Bh | CONFIRMED | SUB | (n), A | 2 | 4 | p.227「SUB (n),A = 4Bh」 |
| 4Ch | PARTIAL | SUB | r2, r1' / r2, r2' | 2 | 5 | p.227拡大直読「r2,r1'とr2,r2'が同4Ch行に並列。cycles=5。第2バイト=0r 0r'」。r2,r2'形の第2バイト「00」の解釈未確定 |
| 4Dh | CONFIRMED | SUB | r3, r' | 2 | 7 | p.84「45(4D)」括弧内=SUB, p.227「SUB r3,r' = 4Dh」 |
| 4Eh | CONFIRMED | SUB | r1, r1' | 2 | 3 | p.227拡大直読「SUB r1,r1' / 0100 1110 = 4Eh / bytes:2 / cycles:3」。第2バイト=0r 0r' |
| 4Fh | CONFIRMED | PUSHS | F | 1 | 3 | p.229「0100 1111 = 4Fh / bytes:1 / cycles:3」。(S-1)←C,Z S←S-1 |
| 50h | CONFIRMED | ADC | A, n | 2 | 3 | p.85, p.227「50h / bytes:2 / cycles:3」 |
| 51h | CONFIRMED | ADC | (m), n | 3 | 4 | p.85, p.227「51h」 |
| 52h | CONFIRMED | ADC | A, (n) | 2 | 4 | p.85, p.227「52h」 |
| 53h | CONFIRMED | ADC | (n), A | 2 | 4 | p.85, p.227「53h」 |
| 54h | CONFIRMED | ADCL | (m), (n) | 3 | 5+2×I | p.86, p.228「0101 0100 = 54h / bytes:3 / cycles:5+2×I」 |
| 55h | CONFIRMED | ADCL | (n), A | 2 | 4+I | p.86, p.228「55h / bytes:2 / cycles:4+I」 |
| 56h | PARTIAL | MVL | (m), [r3±n] | 4 | 5+2×I | p.230「56h / bytes:4 / cycles:5+2×I」。p.231拡大で MVL (m),(r3+n) 形の存在を確認。p.230 と p.231 間の表記差は未解消 |
| 57h | CONFIRMED | PMDF | (n), A | 2 | 4 | p.88, p.230「57h / bytes:2 / cycles:4」 |
| 58h | CONFIRMED | SBC | A, n | 2 | 3 | p.85, p.227「58h / bytes:2 / cycles:3」 |
| 59h | CONFIRMED | SBC | (m), n | 3 | 4 | p.85, p.227「59h」 |
| 5Ah | CONFIRMED | SBC | A, (n) | 2 | 4 | p.227「5Ah」 |
| 5Bh | CONFIRMED | SBC | (n), A | 2 | 4 | p.227「5Bh」 |
| 5Ch | CONFIRMED | SBCL | (m), (n) | 3 | 5+2×I | p.86, p.228「5Ch / bytes:3 / cycles:5+2×I」 |
| 5Dh | CONFIRMED | SBCL | (n), A | 2 | 4+I | p.86, p.228「5Dh / bytes:2 / cycles:4+I」 |
| 5Eh | PARTIAL | MVL | [r3±m], (n) | 4 | 5+2×I | p.230「5Eh / bytes:4 / cycles:5+2×I」。p.231拡大で MVL [r3++],(n) 形の存在を確認。p.230 と p.231 間の表記差は未解消 |
| 5Fh | CONFIRMED | POPS | F | 1 | 2 | p.230「0101 1111 = 5Fh / bytes:1 / cycles:2」。C,Z←(S) S←S+1 |
| 60h | CONFIRMED | CMP | A, n | 2 | 3 | p.89, p.229「0110 0000 = 60h / bytes:2 / cycles:3」 |
| 61h | CONFIRMED | CMP | (m), n | 3 | 4 | p.89, p.229「61h / bytes:3 / cycles:4」 |
| 62h | CONFIRMED | CMP | [klm], n | 5 | 6 | p.89, p.229「0110 0010 = 62h / bytes:5 / cycles:6」。p.89「[k l m]」 |
| 63h | CONFIRMED | CMP | (n), A | 2 | 4 | p.89, p.229「63h / bytes:2 / cycles:4」 |
| 64h | CONFIRMED | TEST | A, n | 2 | 3 | p.91, p.229「0110 0100 = 64h / bytes:2 / cycles:3」 |
| 65h | CONFIRMED | TEST | (m), n | 3 | 4 | p.91, p.229「65h / bytes:3 / cycles:4」 |
| 66h | CONFIRMED | TEST | [klm], n | 5 | 6 | p.91, p.229「0110 0110 = 66h / bytes:5 / cycles:6」 |
| 67h | CONFIRMED | TEST | (n), A | 2 | 4 | p.91, p.229「67h / bytes:2 / cycles:4」 |
| 68h | CONFIRMED | XOR | A, n | 2 | 3 | p.90, p.228「0110 1000 = 68h / bytes:2 / cycles:3」 |
| 69h | CONFIRMED | XOR | (m), n | 3 | 4 | p.90, p.228「69h」 |
| 6Ah | CONFIRMED | XOR | [klm], n | 5 | 7 | p.90, p.228「6Ah / bytes:5 / cycles:7」 |
| 6Bh | CONFIRMED | XOR | (n), A | 2 | 4 | p.90, p.228「6Bh」 |
| 6Ch | PARTIAL | INC | r | 2 | 3 | p.228拡大直読「0110 1100 = 6Ch / bytes:2 / cycles:3」。第2バイト=0000 0r（下位3ビットでレジスタ選択）。r展開を明示表に完全展開していないため PARTIAL |
| 6Dh | CONFIRMED | INC | (m) | 2 | 3 | p.228「0110 1101 = 6Dh / bytes:2 / cycles:3」 |
| 6Eh | CONFIRMED | XOR | (m), (n) | 3 | 6 | p.90, p.228「6Eh / bytes:3 / cycles:6」 |
| 6Fh | CONFIRMED | XOR | A, (n) | 2 | 4 | p.90, p.228「6Fh」 |
| 70h | CONFIRMED | AND | A, n | 2 | 3 | p.90, p.228「0111 0000 = 70h / bytes:2 / cycles:3」 |
| 71h | CONFIRMED | AND | (m), n | 3 | 4 | p.90, p.228「71h」 |
| 72h | CONFIRMED | AND | [klm], n | 5 | 7 | p.90, p.228「72h / bytes:5 / cycles:7」 |
| 73h | CONFIRMED | AND | (n), A | 2 | 4 | p.90, p.228「73h」 |
| 74h | CONFIRMED | MV | A, B | 1 | 1 | p.227「0111 0100 = 74h / bytes:1 / cycles:1」。p.73❷。p.225拡大直読「MV A,B → 74」 |
| 75h | CONFIRMED | MV | B, A | 1 | 1 | p.227「0111 0101 = 75h / bytes:1 / cycles:1」。p.73❷。p.225拡大直読「MV B,A → 75」 |
| 76h | CONFIRMED | AND | (m), (n) | 3 | 6 | p.90, p.228「76h / bytes:3 / cycles:6」 |
| 77h | CONFIRMED | AND | A, (n) | 2 | 4 | p.90, p.228「77h」 |
| 78h | CONFIRMED | OR | A, n | 2 | 3 | p.90, p.228「0111 1000 = 78h / bytes:2 / cycles:3」 |
| 79h | CONFIRMED | OR | (m), n | 3 | 4 | p.90, p.228「79h」 |
| 7Ah | CONFIRMED | OR | [klm], n | 5 | 7 | p.90, p.228「7Ah / bytes:5 / cycles:7」 |
| 7Bh | CONFIRMED | OR | (n), A | 2 | 4 | p.90, p.228「7Bh」 |
| 7Ch | PARTIAL | DEC | r | 2 | 3 | p.228拡大直読「0111 1100 = 7Ch / bytes:2 / cycles:3」。第2バイト=0000 0r（下位3ビットでレジスタ選択）。r展開を明示表に完全展開していないため PARTIAL |
| 7Dh | CONFIRMED | DEC | (m) | 2 | 3 | p.228「0111 1101 = 7Dh / bytes:2 / cycles:3」 |
| 7Eh | CONFIRMED | OR | (m), (n) | 3 | 6 | p.90, p.228「7Eh / bytes:3 / cycles:6」 |
| 7Fh | CONFIRMED | OR | A, (n) | 2 | 4 | p.90, p.228「7Fh」 |

---

## 80h〜BFh

| opcode | status | mnemonic | operands | bytes | cycles | notes |
|--------|--------|----------|----------|-------|--------|-------|
| 80h | CONFIRMED | MV | A, (n) | 2 | 3 | p.224「1000 0r / r=0=A / cycles:A:3/IL:4」。p.74❹「8r n」。プレバイト可（p.68） |
| 81h | CONFIRMED | MV | IL, (n) | 2 | 4 | p.224「r=1=IL / cycles:4」。IL書き込み時 IH←0 |
| 82h | CONFIRMED | MV | BA, (n) | 2 | 4 | p.224「r=2=BA / cycles:4」 |
| 83h | CONFIRMED | MV | I, (n) | 2 | 5 | p.224「r=3=I / cycles:5」 |
| 84h | CONFIRMED | MV | X, (n) | 2 | 5 | p.224「r=4=X / cycles:5」 |
| 85h | CONFIRMED | MV | Y, (n) | 2 | 5 | p.224「r=5=Y / cycles:5」 |
| 86h | CONFIRMED | MV | U, (n) | 2 | 5 | p.224「r=6=U / cycles:5」 |
| 87h | CONFIRMED | MV | S, (n) | 2 | 5 | p.224「r=7=S / cycles:5」 |
| 88h | CONFIRMED | MV | A, [lmn] | 4 | 6 | p.74❺「(88+r) n m l / r=0=A」。p.224拡大版「1000 1r / cycles:r1=6, r2=7, r3=8」 |
| 89h | CONFIRMED | MV | IL, [lmn] | 4 | 7 | p.224拡大版「r=1=IL / cycles:7」。IL書き込み時 IH←0 |
| 8Ah | CONFIRMED | MV | BA, [lmn] | 4 | 7 | p.224拡大版「r=2=BA / cycles:7」 |
| 8Bh | CONFIRMED | MV | I, [lmn] | 4 | 8 | p.224拡大版「r=3=I / cycles:8」 |
| 8Ch | CONFIRMED | MV | X, [lmn] | 4 | 8 | p.224拡大版「r=4=X / cycles:8」 |
| 8Dh | CONFIRMED | MV | Y, [lmn] | 4 | 8 | p.224拡大版「r=5=Y / cycles:8」 |
| 8Eh | CONFIRMED | MV | U, [lmn] | 4 | 8 | p.224拡大版「r=6=U / cycles:8」 |
| 8Fh | CONFIRMED | MV | S, [lmn] | 4 | 8 | p.224拡大版「r=7=S / cycles:8」 |
| 90h | CONFIRMED | MV | A, [r3] | 2 | 4 | p.74❺「9r 0r3 / r=0=A」。p.224「1001 0r / bytes:2 / cycles:A:4」。第2バイト=0r3。バリアント:[r3++]=2r3(cycles+0/+2),[--r3]=3r3,[r3±n]=8r3/Cr3(bytes=3) |
| 91h | CONFIRMED | MV | IL, [r3] | 2 | 5 | p.224「r=1=IL / cycles:5」。IL書き込み時 IH←0 |
| 92h | CONFIRMED | MV | BA, [r3] | 2 | 5 | p.224「r=2=BA / cycles:5」 |
| 93h | CONFIRMED | MV | I, [r3] | 2 | 6 | p.224「r=3=I / cycles:6」 |
| 94h | CONFIRMED | MV | X, [r3] | 2 | 5 | p.224「r=4=X / cycles:5」 |
| 95h | CONFIRMED | MV | Y, [r3] | 2 | 5 | p.224「r=5=Y / cycles:5」 |
| 96h | CONFIRMED | MV | U, [r3] | 2 | 5 | p.224「r=6=U / cycles:5」 |
| 97h | CONFIRMED | SC | - | 1 | 1 | p.100「SC → 97」直読。p.229「SC / 1001 0111 = 97h / bytes:1 / cycles:1」直読。C←1 |
| 98h | CONFIRMED | MV | A, [(n)] | 3 | 9 | p.74❺「(98+r) 00 n / r=0=A」。p.224「1001 1r / bytes:3 / cycles:A:9」。第2バイト=00h。バリアント:80h→[(m)+n](bytes=4,cycles=11),C0h→[(l)-n](bytes=5,cycles=12) |
| 99h | CONFIRMED | MV | IL, [(n)] | 3 | 10 | p.224「r=1=IL / cycles:10」。IL書き込み時 IH←0 |
| 9Ah | CONFIRMED | MV | BA, [(n)] | 3 | 10 | p.224「r=2=BA / cycles:10」 |
| 9Bh | CONFIRMED | MV | I, [(n)] | 3 | 11 | p.224「r=3=I / cycles:11」 |
| 9Ch | CONFIRMED | MV | X, [(n)] | 3 | 11 | p.224「r=4=X / cycles:11」 |
| 9Dh | CONFIRMED | MV | Y, [(n)] | 3 | 11 | p.224「r=5=Y / cycles:11」 |
| 9Eh | CONFIRMED | MV | U, [(n)] | 3 | 11 | p.224「r=6=U / cycles:11」 |
| 9Fh | CONFIRMED | RC | - | 1 | 1 | p.101「RC → 9F」直読。p.229「RC / 1001 1111 = 9Fh / bytes:1 / cycles:1」直読。C←0 |
| A0h | CONFIRMED | MV | (n), A | 2 | 3 | p.74❹「Ar n / r=0=A」。p.224「1010 0r / bytes:2 / cycles:r1=3, r2=4, r3=5」 |
| A1h | CONFIRMED | MV | (n), IL | 2 | 3 | p.224「r=1=IL / cycles:3」 |
| A2h | CONFIRMED | MV | (n), BA | 2 | 4 | p.224「r=2=BA / cycles:4」 |
| A3h | CONFIRMED | MV | (n), I | 2 | 4 | p.224「r=3=I / cycles:4」 |
| A4h | CONFIRMED | MV | (n), X | 2 | 5 | p.224「r=4=X / cycles:5」 |
| A5h | CONFIRMED | MV | (n), Y | 2 | 5 | p.224「r=5=Y / cycles:5」 |
| A6h | CONFIRMED | MV | (n), U | 2 | 5 | p.224「r=6=U / cycles:5」 |
| A7h | CONFIRMED | MV | (n), S | 2 | 5 | p.224「r=7=S / cycles:5」 |
| A8h | CONFIRMED | MV | [lmn], A | 4 | 5 | p.74❺「(A8+r) n m l / r=0=A」。p.224拡大版「1010 1r / cycles:r1=5, r2=6, r3=7」 |
| A9h | CONFIRMED | MV | [lmn], IL | 4 | 5 | p.224拡大版「r=1=IL / cycles:5」 |
| AAh | CONFIRMED | MV | [lmn], BA | 4 | 6 | p.224拡大版「r=2=BA / cycles:6」 |
| ABh | CONFIRMED | MV | [lmn], I | 4 | 6 | p.224拡大版「r=3=I / cycles:6」 |
| ACh | CONFIRMED | MV | [lmn], X | 4 | 7 | p.224拡大版「r=4=X / cycles:7」 |
| ADh | CONFIRMED | MV | [lmn], Y | 4 | 7 | p.224拡大版「r=5=Y / cycles:7」 |
| AEh | CONFIRMED | MV | [lmn], U | 4 | 7 | p.224拡大版「r=6=U / cycles:7」 |
| AFh | CONFIRMED | MV | [lmn], S | 4 | 7 | p.224拡大版「r=7=S / cycles:7」 |
| B0h | CONFIRMED | MV | [r3], A | 2 | 4 | p.74❺「Br 0r3 / r=0=A」。p.224「1011 0r / bytes:2 / cycles:r1=4, r2=5, r3=6」。第2バイト=0r3。バリアント:[r3++]=2r3,[--r3]=3r3,[r3±n]=8r3/Cr3(bytes=3) |
| B1h | CONFIRMED | MV | [r3], IL | 2 | 4 | p.224「r=1=IL / cycles:4」 |
| B2h | CONFIRMED | MV | [r3], BA | 2 | 5 | p.224「r=2=BA / cycles:5」 |
| B3h | CONFIRMED | MV | [r3], I | 2 | 5 | p.224「r=3=I / cycles:5」 |
| B4h | CONFIRMED | MV | [r3], X | 2 | 6 | p.224「r=4=X / cycles:6」 |
| B5h | CONFIRMED | MV | [r3], Y | 2 | 6 | p.224「r=5=Y / cycles:6」 |
| B6h | CONFIRMED | MV | [r3], U | 2 | 6 | p.224「r=6=U / cycles:6」 |
| B7h | CONFIRMED | MV | [r3], S | 2 | 6 | p.224「r=7=S / cycles:6」 |
| B8h | CONFIRMED | MV | [(n)], A | 3 | 9 | p.74❺「(B8+r) 00 n / r=0=A」。p.224「1011 1r / bytes:3 / cycles:r1=9, r2=10, r3=11」。第2バイト=00h。バリアント:80h→[(m)+n](bytes=4,cycles=11),C0h→[(l)-n](bytes=5,cycles=13) |
| B9h | CONFIRMED | MV | [(n)], IL | 3 | 9 | p.224「r=1=IL / cycles:9」 |
| BAh | CONFIRMED | MV | [(n)], BA | 3 | 10 | p.224「r=2=BA / cycles:10」 |
| BBh | CONFIRMED | MV | [(n)], I | 3 | 10 | p.224「r=3=I / cycles:10」 |
| BCh | CONFIRMED | MV | [(n)], X | 3 | 11 | p.224「r=4=X / cycles:11」 |
| BDh | CONFIRMED | MV | [(n)], Y | 3 | 11 | p.224「r=5=Y / cycles:11」 |
| BEh | CONFIRMED | MV | [(n)], U | 3 | 11 | p.224「r=6=U / cycles:11」 |
| BFh | CONFIRMED | MV | [(n)], S | 3 | 11 | p.224「r=7=S / cycles:11」 |

---

## C0h〜FFh

| opcode | status | mnemonic | operands | bytes | cycles | notes |
|--------|--------|----------|----------|-------|--------|-------|
| C0h | CONFIRMED | EX | (m), (n) | 3 | 7 | p.80「EX (m),(n) → C0 m n」。p.227「1100 0000 = C0h / bytes:3 / cycles:7」 |
| C1h | CONFIRMED | EXW | (m), (n) | 3 | 10 | p.80「EXW (m),(n) → C1 m n」。p.227「1100 0001 = C1h / bytes:3 / cycles:10」 |
| C2h | CONFIRMED | EXP | (m), (n) | 3 | 13 | p.80「EXP (m),(n) → C2 m n」。p.227「1100 0010 = C2h / bytes:3 / cycles:13」 |
| C3h | CONFIRMED | EXL | (m), (n) | 3 | 5+3×I | p.81「EXL (m),(n) → C3 m n」。p.227「1100 0011 = C3h / bytes:3 / cycles:5+3×I」 |
| C4h | CONFIRMED | DADL | (m), (n) | 3 | 5+2×I | p.87「C4(D4) m n」。p.228「1100 0100 = C4h / bytes:3 / cycles:5+2×I」 |
| C5h | CONFIRMED | DADL | (n), A | 2 | 4+I | p.87「C5(D5) n」。p.228「1100 0101 = C5h / bytes:2 / cycles:4+I」 |
| C6h | CONFIRMED | CMPW | (m), (n) | 3 | 8 | p.89「C6 m n」。p.230「1100 0110 = C6h / bytes:3 / cycles:8」 |
| C7h | CONFIRMED | CMPP | (m), (n) | 3 | 10 | p.89「C7 m n」。p.230「1100 0111 = C7h / bytes:3 / cycles:10」 |
| C8h | CONFIRMED | MV | (m), (n) | 3 | 6 | p.73❸「C8 m n」。p.225「1100 1000 = C8h / bytes:3 / cycles:6」 |
| C9h | CONFIRMED | MVW | (m), (n) | 3 | 8 | p.73❸「C9 m n」。p.225「1100 1001 = C9h / bytes:3 / cycles:8」 |
| CAh | CONFIRMED | MVP | (m), (n) | 3 | 10 | p.73❸「CA m n」。p.225「1100 1010 = CAh / bytes:3 / cycles:10」 |
| CBh | CONFIRMED | MVL | (m), (n) | 3 | 5+2×I | p.77「CB n m」。p.225「1100 1011 = CBh / bytes:3 / cycles:5+2×I」 |
| CCh | CONFIRMED | MV | (m), n | 3 | 3 | p.73❶「CC m n」。p.230「1100 1100 = CCh / bytes:3 / cycles:3」 |
| CDh | CONFIRMED | MVW | (l), mn | 4 | 4 | p.73❶「CD l m n」。p.230「1100 1101 = CDh / bytes:4 / cycles:4」 |
| CEh | CONFIRMED | TCL | - | 1 | 1 | p.231拡大 H=C,L=E「TCL」直読。p.230拡大「TCL / Divider←D / 1100 1110 = CEh / bytes:1 / cycles:1」直読 |
| CFh | CONFIRMED | MVLD | (m), (n) | 3 | 5+2×I | p.79「CF m n」。p.225「1100 1111 = CFh / bytes:3 / cycles:5+2×I」 |
| D0h | CONFIRMED | MV | (k), [lmn] | 5 | 7 | p.75❻「D0(D1,D2) k n m l」。p.225「1101 0000 = D0h / bytes:5 / cycles:7」 |
| D1h | CONFIRMED | MVW | (k), [lmn] | 5 | 8 | p.75❻「D1h=MVW」。p.225「1101 0001 = D1h / bytes:5 / cycles:8」 |
| D2h | CONFIRMED | MVP | (k), [lmn] | 5 | 9 | p.75❻「D2h=MVP」。p.225「1101 0010 = D2h / bytes:5 / cycles:9」 |
| D3h | CONFIRMED | MVL | (k), [lmn] | 5 | 6+2×I | p.77❻「D3 k n m l」。p.225「1101 0011 = D3h / bytes:5 / cycles:6+2×I」 |
| D4h | CONFIRMED | DSBL | (m), (n) | 3 | 5+2×I | p.87「C4(D4) m n」括弧内=DSBL。p.228「1101 0100 = D4h / bytes:3 / cycles:5+2×I」 |
| D5h | CONFIRMED | DSBL | (n), A | 2 | 4+I | p.87「C5(D5) n」括弧内=DSBL。p.228「1101 0101 = D5h / bytes:2 / cycles:4+I」 |
| D6h | CONFIRMED | CMPW | (m), r2 | 3 | 7 | p.89「D6 0r2 n」。p.230「1101 0110 = D6h / bytes:3 / cycles:7」。第2バイト=0r2 |
| D7h | CONFIRMED | CMPP | (m), r3 | 3 | 9 | p.89「D7 0r3 n」。p.230「1101 0111 = D7h / bytes:3 / cycles:9」。第2バイト=0r3 |
| D8h | CONFIRMED | MV | [klm], (n) | 5 | 6 | p.75❻「DS(D9,DA) m l k n」。p.226「1101 1000 = D8h / bytes:5 / cycles:6」 |
| D9h | CONFIRMED | MVW | [klm], (n) | 5 | 7 | p.75❻「D9h=MVW」。p.226「1101 1001 = D9h / bytes:5 / cycles:7」 |
| DAh | CONFIRMED | MVP | [klm], (n) | 5 | 8 | p.75❻「DAh=MVP」。p.226「1101 1010 = DAh / bytes:5 / cycles:8」 |
| DBh | CONFIRMED | MVL | [klm], (n) | 5 | 6+2×I | p.77❻「DB m l k n」。p.226「1101 1011 = DBh / bytes:5 / cycles:6+2×I」 |
| DCh | CONFIRMED | MVP | (k), lmn | 5 | 5 | p.73❶「DC k n m l」。p.230「1101 1100 = DCh / bytes:5 / cycles:5」 |
| DDh | CONFIRMED | EX | A, B | 1 | 3 | p.80「EX A,B → DD」。p.227「1101 1101 = DDh / bytes:1 / cycles:3」 |
| DEh | CONFIRMED | HALT | - | 1 | 1 | p.102「DE」直読。p.231 H=D,L=E「HALT」で確定。p.230拡大でも DEh=HALT / bytes:1 / cycles:1 直読 |
| DFh | PARTIAL | OFF | - | 1 | 判読不能 | p.102「DF」直読。p.230拡大「1101 1111 = DFh / bytes:1」直読。cycles欄は記号のみ（「ʻ」）で数値として読み取れない（p.230拡大 d2_pages複数枚確認） |
| E0h | CONFIRMED | MV | (n), [r3] | 3 | 6 | p.75❻「E0(E1,E2) 0r3 n」。p.226「1110 0000 = E0h / bytes:3 / cycles:6」。第2バイト=0r3 |
| E1h | CONFIRMED | MVW | (n), [r3] | 3 | 7 | p.75❻「E1h=MVW」。p.226「1110 0001 = E1h / bytes:3 / cycles:7」 |
| E2h | CONFIRMED | MVP | (n), [r3] | 3 | 8 | p.75❻「E2h=MVP」。p.226「1110 0010 = E2h / bytes:3 / cycles:8」 |
| E3h | CONFIRMED | MVL | (n), [r3++] | 3 | 5+2×I | p.77❻「E3 2r3 n」。p.226「1110 0011 = E3h / bytes:3 / cycles:5+2×I」。第2バイト=0010 0 r3 |
| E4h | CONFIRMED | ROR | A | 1 | 2 | p.92「E4」。p.228「1110 0100 = E4h / bytes:1 / cycles:2」 |
| E5h | CONFIRMED | ROR | (n) | 2 | 3 | p.92「E5 n」。p.228「1110 0101 = E5h / bytes:2 / cycles:3」 |
| E6h | CONFIRMED | ROL | A | 1 | 2 | p.92「(E6)」括弧内=ROL。p.228「1110 0110 = E6h / bytes:1 / cycles:2」 |
| E7h | CONFIRMED | ROL | (n) | 2 | 3 | p.92「(E7) n」。p.228「1110 0111 = E7h / bytes:2 / cycles:3」 |
| E8h | CONFIRMED | MV | (n), [r3++] | 3 | 6 | p.226「1110 1000 = E8h / bytes:3 / cycles:6」。第2バイト=0010 0 r3 |
| E9h | CONFIRMED | MVW | (n), [r3++] | 3 | 7 | p.226「1110 1001 = E9h / bytes:3 / cycles:7」 |
| EAh | CONFIRMED | MVP | (n), [r3++] | 3 | 9 | p.226「1110 1010 = EAh / bytes:3 / cycles:9」 |
| EBh | CONFIRMED | MVL | (n), [r3++] | 3 | 5+2×I | p.77❻「EB 2r3 n」。p.230「1110 1011 = EBh / bytes:3 / cycles:5+2×I」。第2バイト=0010 0 r3 |
| ECh | CONFIRMED | DSLL | (n) | 2 | 4+1 | p.94「FC(EC) n」括弧内=DSLL。p.229「1110 1100 = ECh / bytes:2 / cycles:4+1」 |
| EDh | CONFIRMED | EX | r2, r2' | 2 | 4 | p.80「ED r2r2'」。p.227「1110 1101 = EDh / bytes:2 / cycles:4」。第2バイト=r r'（各4ビット）。EX r3,r3'も同EDhで第2バイトがr3r3'(cycles:4) |
| EEh | CONFIRMED | SWAP | A | 1 | 3 | p.100「EE」。p.227「1110 1110 = EEh / bytes:1 / cycles:3」 |
| EFh | CONFIRMED | WAIT | - | 1 | 1+I | p.101「EF」直読。p.230拡大「1110 1111 = EFh / bytes:1 / cycles:1+I（I←I-1 if I=0 END）」直読（d2_pages 4,5,7枚目で一致）。p.101本文「(I+1) サイクル」と整合 |
| F0h | CONFIRMED | MV | (m), [(n)] | 4 | 11 | p.75❻「F0(F1,F2) 00 m n」。p.226「1111 0000 = F0h / bytes:4 / cycles:11」。第2バイト=00h |
| F1h | CONFIRMED | MVW | (m), [(n)] | 4 | 12 | p.75❻「F1h=MVW」。p.226「1111 0001 = F1h / bytes:4 / cycles:12」 |
| F2h | CONFIRMED | MVP | (m), [(n)] | 4 | 13 | p.75❻「F2h=MVP」。p.226「1111 0010 = F2h / bytes:4 / cycles:13」 |
| F3h | CONFIRMED | MVL | (m), [(n)] | 4 | 10+2×I | p.77❻「F3 00 l n等」。p.226「1111 0011 = F3h / bytes:4 / cycles:10+2×I」。第2バイト=00h |
| F4h | CONFIRMED | SHR | A | 1 | 2 | p.93「F4」。p.228「1111 0100 = F4h / bytes:1 / cycles:2」 |
| F5h | CONFIRMED | SHR | (n) | 2 | 3 | p.93「F5 n」。p.228「1111 0101 = F5h / bytes:2 / cycles:3」 |
| F6h | CONFIRMED | SHL | A | 1 | 2 | p.93「(F6)」括弧内=SHL。p.228「1111 0110 = F6h / bytes:1 / cycles:2」 |
| F7h | CONFIRMED | SHL | (n) | 2 | 3 | p.93「(F7) n」。p.228「1111 0111 = F7h / bytes:2 / cycles:3」 |
| F8h | CONFIRMED | MV | [(l)±m], (n) | 5 | 13 | p.75❻「F8(F9,FA) 80 l m n等」。p.226「1111 1000 = F8h / bytes:5 / cycles:13」。第2バイト=80h(+m)またはC0h(-m) |
| F9h | CONFIRMED | MVW | [(l)±m], (n) | 5 | 14 | p.75❻「F9h=MVW」。p.226「1111 1001 = F9h / bytes:5 / cycles:14」 |
| FAh | CONFIRMED | MVP | [(l)±m], (n) | 5 | 15 | p.75❻「FAh=MVP」。p.226「1111 1010 = FAh / bytes:5 / cycles:15」 |
| FBh | CONFIRMED | MVL | [(l)±m], (n) | 5 | 12+2×I | p.77❻「FB C0 l n m等」。p.226「1111 1011 = FBh / bytes:5 / cycles:12+2×I」 |
| FCh | CONFIRMED | DSRL | (n) | 2 | 4+1 | p.94「FC(EC) n」先頭=DSRL。p.229「1111 1100 = FCh / bytes:2 / cycles:4+1」 |
| FDh | CONFIRMED | MV | r2, r2' / r3, r3' | 2 | 2 | p.73❷「MV r2,r2' → FD r2r2' / MV r3,r3' → FD r3r3'」直読。p.227「1111 1101 = FDh / bytes:2 / cycles:2」直読。第2バイト = r r'（各4ビット） |
| FEh | PARTIAL | IR | - | 1 | 判読不能 | p.102「FE」直読。p.230「1111 1110 = FEh / bytes:1」直読。cycles は p.230 判読不能 |
| FFh | PARTIAL | RESET | - | 1 | 判読不能 | p.103「FF」直読。p.103優先でFFh確定。cycles は p.230 判読不能 |

---

## 集計

| 区分 | 件数 |
|------|------|
| 総エントリ数 | 256（00h–FFh 全件） |
| CONFIRMED | 247 |
| PARTIAL | 9（44h, 4Ch, 56h, 5Eh, 6Ch, 7Ch, DFh, FEh, FFh） |
| UNRESOLVED | 0 |

---

## PARTIAL 詳細

**44h / 4Ch：**
ADD/SUB r2 系。r2,r1' と r2,r2' の両形が同じ opcode 行に並列記載（p.227拡大直読）。opcode / bytes / cycles は確定。r2,r2' 形の第2バイト「00」の解釈未確定のため PARTIAL 維持。

**46h / 4Eh：**
CONFIRMED 済み。r1,r1' 形として単独行で確認（p.227拡大直読）。第2バイト=0r 0r'。

**56h / 5Eh：**
MVL の外部メモリ系 operand 記法について p.230 と p.231 間に表記差。p.231拡大で両形の存在を確認。bytes・cycles は確定。p.230/p.231 間の表記差が未解消のため最終 operand 記法を PARTIAL 維持。

**6Ch / 7Ch：**
INC/DEC r の第2バイト「0000 0r」形式を p.228拡大直読で確認。下位3ビットがレジスタ選択表に従いレジスタを選択。基本 opcode は確定。r 展開を明示表に完全展開していないため PARTIAL 維持。

**DFh / FEh / FFh：**
opcode / mnemonic / bytes は確認済み。cycles は原資料写真から安全に読めないため PARTIAL。

---

## 注意事項

### 1. PRE バイト（20h–27h, 30h–37h）

bytes / cycles は直後命令に依存。本表では「-」とする。プレバイト体系の詳細は p.68 参照。

### 2. 第2バイト依存命令（90h–96h, 98h–9Eh, B0h–B6h, B8h–BEh, 6Ch, 7Ch, EDh, FDh）

第1バイトが同一で第2バイトによりアドレッシングモード・レジスタが決まる。本表は基本形を代表エントリとして記載。ディスアセンブラ実装では第2バイト精査が必須。

### 3. IL 書き込み時 IH←0

IL をデスティネーションとする全命令で IH←0 が同時実行（p.224 注記）。

### 4. ループ命令の cycles

I レジスタの値に依存。I=0 の場合は 10000h 回実行とみなす（p.66 記載）。

### 5. CEh / DEh の根拠

p.231拡大 H=C,L=E「TCL」、H=D,L=E「HALT」を確認。p.230拡大でも CEh=TCL、DEh=HALT と整合。
