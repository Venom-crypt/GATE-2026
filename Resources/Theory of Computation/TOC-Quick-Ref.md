# Theory of Computation - GATE 2026 Quick Reference

## Quick Definitions

| Concept | Definition | Example |
|---------|-----------|---------|
| **Regex** | Pattern for regular language | (a\|b)*abb |
| **DFA** | Deterministic finite automaton | One transition per symbol |
| **NFA** | Non-deterministic finite automaton | Multiple transitions possible |
| **CFG** | Context-free grammar | S → aSb \| ε |
| **PDA** | Pushdown automaton | Stack-based machine |
| **TM** | Turing machine | Infinite tape, read-write head |
| **RE Language** | Recognizable by TM | May not halt |
| **Recursive** | Decidable by TM | Halts always |

---

## Regular Expressions Operations

```
a          - Single symbol
(ab)       - Concatenation
(a|b)      - Union/Alternation
a*         - Kleene star (0 or more)
a+         - Plus (1 or more)
a?         - Optional (0 or 1)
[a-z]      - Character class
.          - Any character
^          - Start of string
$          - End of string
```

---

## Finite Automata Conversions

### NFA to DFA (Subset Construction)
```
DFA state set = Power set of NFA states
DFA initial = ε-closure(NFA initial)
DFA final = States containing NFA final states
For each DFA state S and symbol a:
  DFA next = ε-closure(∪ NFA_next for all s in S)
```

### DFA to Regex (Arden's Method)
```
Q_i = Q_i A + Q' B  (state equations)
Use: Q = Q R + P → Q = P R*
Substitute and simplify until final state equation
Result: Regex for DFA
```

---

## Context-Free Grammars

### CFG Rules
```
S → aSb       (recursive rule)
S → aS        (right-recursive)
S → Sa        (left-recursive)
S → ε         (epsilon)
S → a | b     (alternation)
S → AB        (concatenation)
```

### Parsing
- **Leftmost derivation:** Expand leftmost non-terminal
- **Rightmost derivation:** Expand rightmost non-terminal
- **Ambiguous:** Multiple parse trees exist

### CNF (Chomsky Normal Form)
- All rules: A → BC or A → a
- Useful for pumping lemma proofs

---

## Pumping Lemma Quick Reference

### For Regular Languages
**If L regular, then ∃p such that:**
- For all w ∈ L with |w| ≥ p
- w = xyz where |y| > 0, |xy| ≤ p
- For all i ≥ 0: xy^i z ∈ L

**To prove NON-REGULAR:**
1. Assume L regular with pumping length p
2. Choose strategic w ∈ L
3. Show NO valid split xyz works
4. Contradiction!

### For Context-Free Languages
**If L CFL, then ∃p such that:**
- For all w ∈ L with |w| ≥ p
- w = uvwxy where |vwx| ≤ p, |vx| ≥ 1
- For all i ≥ 0: uv^i wx^i y ∈ L

**Difference:** Two strings pumped together

---

## Turing Machine Components

**Tape:** Infinite, contains cells (data)
**Head:** Reads/writes current cell, moves L/R
**State Register:** Holds current state
**Transition Function:** δ(q, a) = (q', b, L/R)

### Variants
- **Multitape:** Multiple independent tapes
- **Nondeterministic:** Multiple transitions possible
- **All equivalent to single-tape deterministic**

---

## Decidability Hierarchy

```
Decidable (Recursive)
    ↓
Recognizable (Recursively Enumerable)
    ↓
Unrecognizable (Co-RE)
    ↓
Undecidable
```

### Common Undecidable Problems
- **Halting Problem:** Does TM M halt on w?
- **Post Correspondence:** String matching problem
- **Word Problem:** Group element equality
- **Rice's Theorem:** Any non-trivial property of RE languages

---

## Language Hierarchy

```
Regular ⊂ CFL ⊂ Recursive ⊂ Recursively Enumerable
  ↓                                    ↓
Closed under union, intersection    May not halt
Decision problems decidable          Some undecidable
```

---

## Closure Properties

### Regular Languages (Closed under)
✓ Union, Intersection, Complement
✓ Concatenation, Kleene star, Reversal
✓ Homomorphism, Inverse homomorphism

### Context-Free Languages (Closed under)
✓ Union, Concatenation, Kleene star, Reversal
✗ Intersection, Complement

### Recursive (Closed under)
✓ Union, Intersection, Complement
✓ Concatenation, Kleene star

---

## Decision Problems

| Problem | Regular | CFL | Recursive | RE |
|---------|---------|-----|-----------|-----|
| Membership | YES | YES | YES | YES |
| Emptiness | YES | YES | YES | NO |
| Finiteness | YES | YES | NO | NO |
| Equivalence | YES | NO | NO | NO |
| Disjoint | YES | NO | NO | NO |

---

## Problem-Solving Strategies

### For Regex/DFA Problems
1. Understand language requirement
2. Design states for tracking
3. Add transitions for each symbol
4. Mark accepting states
5. Verify with test strings

### For CFG Problems
1. Identify recursive structure
2. Handle base case (ε or terminal)
3. Add recursive rule
4. Verify generates language

### For Pumping Lemma Problems
1. Assume language has property
2. Choose string w carefully
3. Consider all decompositions
4. Show one fails
5. Derive contradiction

### For Decidability Problems
1. Understand decision problem
2. Check if algorithm exists
3. For undecidable: Reduce from known problem
4. For decidable: Construct TM or mention class

---

## Key Theorems

**Kleene's Theorem:** Regex ↔ DFA ↔ NFA
**Myhill-Nerode:** Minimal DFA equivalence classes
**Chomsky Hierarchy:** RL ⊂ CFL ⊂ CSL ⊂ RE
**Rice's Theorem:** Non-trivial properties undecidable
**Closure under Kleene Star:** RL, CFL, Recursive

---

## GATE Question Patterns

| Pattern | Approach | Time |
|---------|----------|------|
| DFA design | Build states, add transitions | 3-4 min |
| NFA to DFA | Subset construction | 4-5 min |
| Regex match | Parse and verify | 2-3 min |
| Pumping lemma | Proof by contradiction | 5-8 min |
| CFG ambiguity | Find multiple derivations | 3-4 min |
| Turing machine | Define transitions | 5-6 min |
| Decidability | Recognize problem type | 2-3 min |

---

## Common Mistakes to Avoid

✗ Confusing DFA and NFA (NFA has multiple paths)
✗ Wrong pumping lemma application (both conditions must work)
✗ Incomplete state transitions in DFA
✗ Forgetting ε-transitions in NFA conversion
✗ Not considering all decompositions in pumping lemma
✗ Assuming CFL properties of RL
✗ Confusing decidable and recognizable

---

## Critical Formula

**NFA to DFA:** 2^n states maximum (n = NFA states)
**Pumping length p:** ≤ number of states in minimal DFA
**PDA power:** Equivalent to CFL
**TM variants:** All have same power (Church-Turing)

---

## Exam Time Strategy

**Total TOC questions:** 8-12 (12-18 marks)
**Time budget:** 18-24 minutes

- **Easy (1-2 marks):** 2 minutes each
- **Medium (2-3 marks):** 3 minutes each
- **Hard (3-4 marks):** 4 minutes each

**Start with:** Easy regex/DFA problems
**Then do:** CFG, PDA problems
**Save for last:** Undecidability, complex proofs

---

This sheet covers all major TOC topics for GATE!
