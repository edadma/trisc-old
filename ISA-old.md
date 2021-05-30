TRISC Instruction Set Architecture
==================================

*trisc* (Tiny RISC) has a 32-bit "load/store" architecture with a minimal fixed length 16-bit instruction set.

Registers
---------

*trisc* has a register file comprising 16 32-bit registers, as well as a 32-bit program counter register and a condition code register.

The 16 registers in the register file are freely accessible except that register *r15* functions as the 32-bit call/return stack pointer, and is referred to as *SP* when it is being referenced as a stack pointer.  Registers *r1* through *r14* are general purpose.

There is a 32-bit program counter *PC*.

There is a 24-bit temporary upper immediate value register *I*.

There condition code bits *N*, *Z*, *V*, *C*.

Instruction Format Types
------------------------

### T-Type

triple register operations

1 ccc dddd bbbb aaaa

- 000 add
- 001 sub
- 010 mul
- 011 muls
- 100 div
- 101 and
- 110 or
- 111 xor

### D-Type

double register operations

0011 dddd cccc aaaa

- 0000 adc
- 0001 sbb
- 0010 mov
- 0011 psh
- 0100 pop
- 0101 ldb load byte indirect
- 0110 stb store byte indirect
- 0111 ldh load half-word indirect
- 1000 sth store half-word indirect
- 1001 ldw load word indirect
- 1010 stw store word indirect
- 1011 asr
- 1100 lsr
- 1101 shl
- 1110 cmp
- 1111 tst atomic test and set

### R-Type

single register operations

01 cc rrrr iiiiiiii

ldi
stwd store word direct
jmp
jsr

### I-Type

immediate, no register

0000 cccc iiiiiiii

NOTE: cccc can't start with 11

- 0000 brk (immediate = 0)
- 0000 sys (immediate ≥ 1)
- 0001 tuc
- 0010 pshi
- 0011 sets
- 0100 clrs

### N-Type

no operand

0011110 cc --- ----

ret
rets
reti

### C-Type

conversion

0011111 ccc tt rrrr

tt:
- 01 byte
- 10 half-word
- 11 word

ccc:
- se[bhw]
- ze[bhw]
- re[hw]

### S-Type

small immediate operations

0010 rrrr ccc iiiii

addi
muli
asri
lsli
shri
sst set status bits
bset
bclr

### B-Type

conditional relative branch

0001 cccc disp

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
- ...
- ...
