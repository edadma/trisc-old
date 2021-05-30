TRISC Instruction Set Architecture
==================================

*TRISC* (Tiny RISC) has a 32-bit "load/store" architecture with a minimalist fixed length 16-bit instruction set.

Programming Model
-----------------

The *trisc* programming model comprises
- a register file with 8 32-bit general purpose registers
- 32-bit stack pointers *SP*, *SSP* (not in the general purpose register file)
- 32-bit program counter *PC*
- 24-bit temporary upper immediate value register *U*
- base address register for code
- base address register for data
- condition code register bits *S*, *I*, *N*, *Z*, *V*, *C*

Instruction Format Types
------------------------

### D-Type

Double register operations

`0100 0 ccccc aaa bbb`

- 00000 add
- 00001 sub
- 00010 mul
- 00011 muls
- 00100 div
- 00101 and
- 00110 or
- 00111 xor
- 01000 adc
- 01001 sbb
- 01010 mov
- 01011 
- 01100 
- 01101 
- 01110 
- 01111 ldb load byte indirect
- 10000 stb store byte indirect
- 10001 ldh load half-word indirect
- 10010 sth store half-word indirect
- 10011 ldw load word indirect
- 10100 stw store word indirect
- 10101 asr
- 10110 lsr
- 10111 shl
- 11000 cmp
- 11001 tst atomic test and set
.
.
.

### I-Type

Immediate/register operations

`1 cccc rrr iiiiiiii`

- 0000 ldi   load immediate
- 0001 lddb  load direct byte
- 0010 lddh  load direct half-word
- 0011 lddw  load direct word
- 0100 stdb  store direct byte
- 0101 stdh  store direct half-word
- 0110 stdw  store direct word
- 0111 stib  store immediate indirect byte
- 1000 stih  store immediate indirect half-word
- 1001 stiw  store immediate indirect word
- . . .

### V-Type

Immediate value only

0000 cccc iiiiiiii

- 0000 brk (immediate = 0)
- 0000 sys (immediate ≥ 1)
- 0001 pib push immediate byte
- 0010 pih push immediate half-word
- 0011 piw push immediate word
- 0100 ssb set status bits
- 0101 csb clear status bits
- 0110 lds load status
- 0111 sbc set base code
- 1000 sbd set base data
- 1001 ssp set stack pointer

### L-Type

Long immediate

`0001 iiiiiiiiii`   lui

`0010 iiiiiiiiii`   jmp

`0011 iiiiiiiiii`   jsr

### A-Type

Array indexed operations

`0100 1 cc iii aaa bbb`

- 00 ldwi   load word indexed
- 01 stwi   store word indexed
- . .

### B-Type

Conditional relative branch

0101 cccc dddddddd

- 0000 HS/CC  C = 0           carry clear
- 0001 LO/CS  C = 1           carry set
- 0010 EQ/ZE  Z = 1           equal/zero
- 0011 GE     N ⊕ V = 0       greater than or equal
- 0100 GT     Z + (N ⊕ V) = 0 greater than
- 0101 HI     C + Z = 0       higher
- 0110 LE     Z + (N ⊕ V) = 1 less than or equal
- 0111 LS     C + Z = 1       lower or same
- 1000 LT     N ⊕ V = 1       less than
- 1001 MI     N = 1           minus
- 1010 NE/NZ  Z = 0           not equal/not zero
- 1011 PL     N = 0           plus
- 1100 R      TRUE            always
- 1101 N      FALSE           never
- 1110 VS     V = 1           overflow set
- 1111 VC     V = 0           overflow clear

### S-Type

Small immediate/register operations

0110 rrr cccc iiiii

- addi
- muli
- asri
- lsli
- shri
- bset
- bclr
- . . .

### R-Type

Register only

01110 ---- cccc rrr

- 0000 seb   sign-extend byte
- 0001 seh
- 0010 zeb   zero-extend byte
- 0011 zeh
- 0100 jmp   indirect
- 0101 jsr   indirect
- 0110 pshb
- 0111 popb
- 1000 pshh
- 1001 poph
- 1010 pshw
- 1011 popw


### N-Type

No operand

01111 -------- ccc

- ret
- reti
- rets
- cui