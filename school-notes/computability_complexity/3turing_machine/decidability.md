# Decidability
##### Computability and Complexity
###### March 3rd, 2022
---

## Decidable Problems 
### DFAs
The _acceptance problem_ for DFAs:
$$A_{DFA} = \{ \langle B, w \rangle \ | \ ... \}$$
>#### Theorem
>###### $A_{DFA}$ is decidable

Proof:
...


---
### NFAs

The _acceptance problem_ for DFAs:
$$A_{DFA} = \{ \langle B, w \rangle \ | \ B \text{ is a NFA that accepts input string }w \}.$$
>#### Theorem
>###### $A_{NFA}$ is decidable

Proof. Algorithm N: 
On input $\langle B,w \rangle$, where $B$ is a NFA and $w$ is a string: 
1. Convert $B$ into an equivalent DFA $C$ using the subset construction. 
2. Run $M$ from the previous proof to see if $\langle C,w \rangle \in A_{DFA}$. 
3. If $M$ accepts, accept; otherwise, reject.


---

### Regular Expressions

### CFGs

The _acceptance problem_ for CFGs
$$A_{CFG} = \{ \langle G,w \rangle \ | \ G \text{ is a CFG that generates string } w\}$$

>#### Theorem
>###### $A_{CFG}$ is decidable

Proof...

Cocke-Kasami-Younger: $O(n^3)$-time algorithm

---

## Equivalence Problems
### DFAs
The _equivalence problem_ for DFAs:

$$EQ_{DFA} = \{ \langle A,B \rangle \ | \ A \text{ and } B \text{ are DFAs and } L(A) = L(B) \}$$

>#### Theorem
>###### $EQ_{DFA}$ is decidable

---
### CFG
The _equivlence problem_ for CFGs:
$$...$$

No algorithm exists!
See undecidability...

---

>#### Theorem
>###### Every context-free language is decidable

Proof...

Let A be context-free and let G be a CFG for A. Algorithm: On input w: 1 Run Algorithm S (or the CKY algorithm) to see if hG,wi ∈ ACFG. 2 If S accepts, accept; otherwise, reject. Note that G is hardcoded into this algorithm

---

The _emptiness problem_ for CFGs:

$$...$$
>#### Theorem
>###### $E_{CFG}$ _is decidable_

Proof. Algorithm R: On input hGi, where G is a CFG: 1 Mark all terminal symbols in G. 2 Repeat until no new variables get marked. Mark any variable A where G has a rule A → U1U2 · · ·Uk and each symbol U1, . . .Uk has already been marked.
