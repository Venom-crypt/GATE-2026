# Digital Logic - GATE 2026 Quick Reference

## Boolean Operators

| Operator | Symbol | Truth | Use |
|----------|--------|-------|-----|
| **AND** | · | Both 1 | Y = A · B |
| **OR** | + | Any 1 | Y = A + B |
| **NOT** | ' | Inverse | Y = Ā |
| **NAND** | (·)' | ~(Both 1) | Y = (AB)' |
| **NOR** | (+)' | ~(Any 1) | Y = (A+B)' |
| **XOR** | ⊕ | Different | Y = A⊕B |
| **XNOR** | ⊙ | Same | Y = A⊙B |

---

## Boolean Postulates

**Commutative:** A + B = B + A
**Associative:** (A + B) + C = A + (B + C)
**Distributive:** A(B + C) = AB + AC
**Identity:** A + 0 = A, A · 1 = A
**Null:** A + 1 = 1, A · 0 = 0
**Idempotent:** A + A = A, A · A = A
**Complement:** A + A' = 1, A · A' = 0
**Involution:** (A')' = A
**Absorption:** A + AB = A, A(A + B) = A
**De Morgan's:** (A + B)' = A'B', (AB)' = A' + B'

---

## K-Map Rules

**Grouping:**
- Groups must be powers of 2 (1, 2, 4, 8, 16...)
- Larger groups = fewer variables
- Adjacent cells (horizontally, vertically, wrap-around)
- Can overlap groups

**Objective:**
- Minimize number of groups
- Maximize size of each group
- Cover all 1's and X's (don't cares)

**Variables Eliminated:**
- 2 cells: 1 variable
- 4 cells: 2 variables
- 8 cells: 3 variables
- 16 cells: 4 variables

---

## Number System Conversions

### Unsigned Binary (n-bit)
**Range:** 0 to 2^n - 1

### Sign-Magnitude (n-bit)
**Range:** -(2^(n-1)-1) to +(2^(n-1)-1)
**Format:** [Sign bit][Magnitude]
**Problem:** Two zeros

### 1's Complement (n-bit)
**Range:** -(2^(n-1)-1) to +(2^(n-1)-1)
**Negative:** Flip all bits
**Problem:** Two zeros

### 2's Complement (n-bit)
**Range:** -2^(n-1) to +(2^(n-1)-1)
**Negative:** 1's complement + 1
**Advantage:** Single zero, standard

### Conversion Example (8-bit)
```
+5:  00000101
-5 (Sign-Mag): 10000101
-5 (1's Comp): 11111010
-5 (2's Comp): 11111011
```

---

## Fixed-Point Arithmetic

**Structure:** [Sign][Integer bits][Fractional bits]

**Example:** 8-bit format = 1 sign + 3 integer + 4 fractional
- 01100101 = 0|110|0101 = 6.3125

**Advantages:**
- Simple operations (integer-like)
- No special hardware

**Disadvantages:**
- Limited range
- Not for very large/small numbers

---

## IEEE 754 Floating-Point

### Single Precision (32-bit)
```
[Sign: 1][Exponent: 8][Mantissa: 23]
Bias: 127
Range: ±1.4×10^(-45) to ±3.4×10^38
```

### Double Precision (64-bit)
```
[Sign: 1][Exponent: 11][Mantissa: 52]
Bias: 1023
Range: ±5×10^(-324) to ±1.8×10^308
```

**Format:** ±1.mantissa × 2^(exponent - bias)

**Special Values:**
- **Zero:** Exponent = 0, Mantissa = 0
- **Denormalized:** Exponent = 0, Mantissa ≠ 0 (very small)
- **Infinity:** Exponent = all 1's, Mantissa = 0
- **NaN:** Exponent = all 1's, Mantissa ≠ 0

---

## Flip-Flops

### SR (Set-Reset)
| S | R | Action |
|---|---|--------|
| 0 | 0 | Hold |
| 0 | 1 | Reset (Q=0) |
| 1 | 0 | Set (Q=1) |
| 1 | 1 | Invalid |

### D (Data)
- Q = D on clock edge
- Simple, clean, most used

### JK (Jack-Kilby)
| J | K | Action |
|---|---|--------|
| 0 | 0 | Hold |
| 0 | 1 | Reset |
| 1 | 0 | Set |
| 1 | 1 | Toggle |

### T (Toggle)
- T = 1: Toggle
- T = 0: Hold

---

## Combinational Circuit Functions

### Multiplexer (2^n:1)
- **Select lines:** n
- **Inputs:** 2^n
- **Output:** 1

### Demultiplexer (1:2^n)
- **Select lines:** n
- **Input:** 1
- **Outputs:** 2^n

### Decoder (n:2^n)
- **Inputs:** n
- **Outputs:** 2^n (only one active)

### Encoder (2^n:n)
- **Inputs:** 2^n (only one active)
- **Outputs:** n

### Half Adder
- Sum = A ⊕ B
- Carry = A · B

### Full Adder
- Sum = A ⊕ B ⊕ Cin
- Cout = AB + (A ⊕ B)Cin

---

## Arithmetic Operations

### Binary Addition
```
  1001 (9)
+  0110 (6)
---------
  1111 (15)
```

### Binary Subtraction (A - B = A + (-B))
```
A = 1001 (9)
-B: Find 2's comp of 0110 = 1010
  1001
+  1010
--------
 10011 (ignore carry, result = 0011 = 3)
```

### Multiplication
```
     1011 (11)
   ×  1101 (13)
   -------
     1011
    0000
   1011
  1011
  ----------
 10001111 (143)
```

### Division
Use long division method (repeated subtraction and shift)

---

## Sequential Circuit Counters

**Asynchronous (Ripple):**
- Output of one FF triggers next
- Slower, simple

**Synchronous:**
- All FFs triggered by same clock
- Faster, more complex

**Modulo-n:** Counts 0 to n-1, then resets

---

## Common Gate Delays

**NOT:** ~1 unit
**AND/OR/NAND/NOR:** ~2 units
**XOR/XNOR:** ~3 units

**Critical Path:** Longest delay through circuit

---

## GATE Question Patterns

**Pattern 1:** Boolean simplification using algebra or K-map
**Pattern 2:** Circuit design given truth table
**Pattern 3:** Sequential logic state diagrams
**Pattern 4:** Number system conversions
**Pattern 5:** Floating-point representation
**Pattern 6:** Counter/timing analysis
**Pattern 7:** Combinational circuit analysis

---

## Common Mistakes

✗ De Morgan's law application
✗ K-map grouping overlaps missed
✗ Floating-point bias calculation wrong
✗ 2's complement conversion error
✗ Forgetting clock requirements in FF
✗ Confusing flip-flop behavior
✗ MUX/decoder confusion

---

**Print for last-minute revision!**
