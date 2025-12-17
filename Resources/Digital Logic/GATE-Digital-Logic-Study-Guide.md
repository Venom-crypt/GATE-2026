# Digital Logic Complete Study Guide - GATE 2026

## Part 1: Boolean Algebra

### 1.1 Boolean Operators

**AND (·):** Y = A · B (True only if both A and B are true)
**OR (+):** Y = A + B (True if A or B or both are true)
**NOT (¯):** Y = Ā (Opposite of A)
**NAND:** Y = (A · B)' = A' + B'
**NOR:** Y = (A + B)' = A' · B'
**XOR:** Y = A ⊕ B = A'B + AB' (True if inputs differ)
**XNOR:** Y = (A ⊕ B)' = AB + A'B' (True if inputs same)

### 1.2 Boolean Postulates

**Commutative:** A + B = B + A, A · B = B · A
**Associative:** (A + B) + C = A + (B + C)
**Distributive:** A(B + C) = AB + AC
**Identity:** A + 0 = A, A · 1 = A
**Null:** A + 1 = 1, A · 0 = 0
**Involution:** (A')' = A
**Idempotent:** A + A = A, A · A = A
**Complement:** A + A' = 1, A · A' = 0
**Absorption:** A + AB = A, A(A + B) = A
**De Morgan's:** (A + B)' = A'B', (AB)' = A' + B'

### 1.3 Canonical Forms

**SOP (Sum of Products):**
- OR of AND terms
- Example: Y = AB + A'B + AB'

**POS (Product of Sums):**
- AND of OR terms
- Example: Y = (A+B)(A'+B)(A+B')

---

## Part 2: Combinational Circuits

### 2.1 Definition

Circuits where output depends only on current inputs (no memory).

### 2.2 Common Combinational Circuits

**Multiplexer (MUX):**
- Selects one input from many using select lines
- 2^n inputs need n select lines
- Example: 4:1 MUX has 2 select lines

**Demultiplexer (DEMUX):**
- Routes one input to one of many outputs
- Opposite of multiplexer

**Decoder:**
- Converts n-bit binary to 2^n outputs
- Only one output high at a time
- Example: 2:4 decoder (2 inputs, 4 outputs)

**Encoder:**
- Converts 2^n inputs to n-bit binary
- Only one input high
- Reverse of decoder

**Half Adder:**
- Sum = A ⊕ B
- Carry = A · B

**Full Adder:**
- Sum = A ⊕ B ⊕ Cin
- Cout = AB + (A ⊕ B)Cin

**Comparator:**
- Compares two numbers
- Outputs: A>B, A=B, A<B

---

## Part 3: Sequential Circuits

### 3.1 Definition

Circuits with memory. Output depends on current input AND previous states.

**State:** Current condition of circuit (stored in flip-flops)

### 3.2 Flip-Flops

**SR (Set-Reset):**
- S=1, R=0: Output = 1 (Set)
- S=0, R=1: Output = 0 (Reset)
- S=1, R=1: Invalid (undefined)
- S=0, R=0: Hold state

**D (Data/Delay):**
- Output follows input (Q = D on clock)
- Single input, cleaner than SR
- Most common in practice

**JK (Jack-Kilby):**
- J=Set, K=Reset (like SR)
- J=1, K=1: Toggle
- More versatile than SR

**T (Toggle):**
- T=1: Toggle output
- T=0: Hold state
- Useful for counters

### 3.3 Counters

**Synchronous Counter:**
- All flip-flops triggered by same clock
- Faster, no ripple delay

**Asynchronous (Ripple) Counter:**
- Output of one FF triggers next
- Slower, but simpler

**Modulo Counter:**
- Counts up to specific value then resets
- Modulo-n counter: counts 0 to n-1

---

## Part 4: Minimization

### 4.1 Karnaugh Map (K-Map)

**Purpose:** Simplify Boolean expressions

**Rules:**
- Group adjacent 1's in powers of 2 (2, 4, 8, 16...)
- Larger groups = simpler expression
- Groups can wrap around edges
- Must cover all 1's

**Variables:** 
- 2-variable: 2×2 grid
- 3-variable: 2×4 grid
- 4-variable: 4×4 grid
- 5-variable: 2 4×4 grids
- >5 variable: Quine-McCluskey method

### 4.2 Quine-McCluskey Algorithm

For >5 variables or automated minimization.

Steps:
1. Convert to minterms
2. Group by number of 1's
3. Find prime implicants
4. Select essential prime implicants
5. Find minimal cover

### 4.3 Don't Cares

**Don't Care (X):** Input combination never occurs
- Can be treated as 0 or 1 to simplify
- Useful for minimization

---

## Part 5: Number Representations

### 5.1 Unsigned Binary

Range: 0 to 2^n - 1 (n-bit)

**Conversion:**
- Binary to Decimal: Sum of (bit × 2^position)
- Decimal to Binary: Repeated division by 2

### 5.2 Signed Binary Representations

**Sign-Magnitude:**
- MSB = sign (0=positive, 1=negative)
- Range: -(2^(n-1)-1) to +(2^(n-1)-1)
- Problem: Two zeros (00...0 and 10...0)

**One's Complement (1's):**
- Positive: Same as binary
- Negative: Flip all bits
- Range: -(2^(n-1)-1) to +(2^(n-1)-1)
- Problem: Two zeros

**Two's Complement (2's):**
- Positive: Same as binary
- Negative: 1's complement + 1
- Range: -2^(n-1) to +(2^(n-1)-1)
- **Single zero, most used**

### 5.3 Example: Represent -5 in 8-bit

**Sign-Magnitude:** 10000101
**1's Complement:** 11111010
**2's Complement:** 11111011

---

## Part 6: Fixed-Point Arithmetic

### 6.1 Fixed-Point Representation

**Structure:**
```
[Sign][Integer bits][Fractional bits]
```

**Binary Point:** Fixed position (e.g., after bit 4)

**Example:** 8-bit with 4 integer, 4 fractional
- 01100101 = 0110.0101 = 6.3125

### 6.2 Operations

**Addition/Subtraction:** Straightforward
**Multiplication:** Integer product, may need shift
**Division:** Requires normalization

**Range:** Limited by bit count
- Advantage: Simple operations
- Disadvantage: Limited range for very large/small numbers

---

## Part 7: Floating-Point Arithmetic

### 7.1 Representation

**Format:** Sign | Exponent | Mantissa

**Scientific Notation:** ±M × 2^E

**IEEE 754 Single Precision (32-bit):**
- Sign: 1 bit
- Exponent: 8 bits (biased by 127)
- Mantissa (Significand): 23 bits
- Normalized: 1.xxx × 2^E

**IEEE 754 Double Precision (64-bit):**
- Sign: 1 bit
- Exponent: 11 bits (biased by 1023)
- Mantissa: 52 bits

### 7.2 Special Values

**Zero:** Exponent = 0, Mantissa = 0
**Denormalized:** Exponent = 0, Mantissa ≠ 0 (very small)
**Infinity:** Exponent = all 1's, Mantissa = 0
**NaN (Not a Number):** Exponent = all 1's, Mantissa ≠ 0

### 7.3 Advantages

- Much larger range (very large and very small numbers)
- Scientific notation familiar
- Standardized (IEEE 754)

**Disadvantage:** Floating-point errors, rounding

---

## Part 8: Computer Arithmetic Operations

### 8.1 Addition

**Binary Addition Rules:**
- 0 + 0 = 0
- 0 + 1 = 1
- 1 + 0 = 1
- 1 + 1 = 10 (result 0, carry 1)

### 8.2 Subtraction

**Method:** A - B = A + (-B)
Convert B to 2's complement, then add

### 8.3 Multiplication

**Binary Multiplication:** Like decimal, but with 0 and 1 only

**Steps:**
1. Multiply by each bit
2. Shift partial products
3. Add all partial products

### 8.4 Division

**Binary Division:** Like long division

**Steps:**
1. Shift divisor
2. Subtract if possible
3. Write quotient bit
4. Repeat

---

## High-Weightage Topics for GATE

1. **Boolean Algebra** (8%) - Simplification, theorems
2. **K-Map Minimization** (12%) - 3/4 variable maps
3. **Combinational Circuits** (12%) - MUX, decoder, adder
4. **Sequential Circuits** (14%) - Flip-flops, counters
5. **Number Systems** (12%) - Conversions, representations
6. **Fixed-Point Arithmetic** (10%) - Representation, operations
7. **Floating-Point Arithmetic** (12%) - IEEE 754, operations
8. **Computer Arithmetic** (10%) - Add, subtract, multiply, divide
9. **Other Topics** (12%)

**Focus for GATE:** K-Map minimization, sequential circuits, number representations, floating-point
