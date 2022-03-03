# Decidability
##### Computability and Complexity
###### March 3rd, 2022
---

## Decidable Problems 

```ad-abstract
title: Algorithm $M$: _Acceptance problem_ for DFAs

$$A_{DFA} = \{ \langle B, w \rangle \ | \ B \text{ is a DFA that accepts input string }w \}.$$
>#### Theorem
>###### $A_{DFA}$ is decidable
##### Proof:
Algorithm $M$:
On inupt $\langle B, w \rangle $, where $B$ is a DFA and $w$ is a string.
1. Simulate $B$ on $w$.
2. If the simulation ends in an accepting state, accept.
If it ends in a nonaccepting state, reject.
$\boxdot$
```

```ad-abstract
title: Algorithm $N$: _Acceptance problem_ for NFAs:
$$A_{NFA} = \{ \langle B, w \rangle \ | \ B \text{ is a NFA that accepts input string }w \}.$$
>#### Theorem
>###### $A_{NFA}$ is decidable

Proof. Algorithm $N$: 
On input $\langle B,w \rangle$, where $B$ is a NFA and $w$ is a string: 
1. Convert $B$ into an equivalent DFA $C$ using the subset construction. 
2. Run $M$ from the previous proof to see if $\langle C,w \rangle \in A_{DFA}$. 
3. If $M$ accepts, accept; otherwise, reject.
```

```ad-abstract
title: Algorithm $P$: _Acceptance problem_ for Regular Expressions
$$A_{REX} = \left\{ \begin{array}{l} 
\langle R, w \rangle & | & R \text{ is a regular expression that} \\
& | & \text{generates input string } w \end{array}\right\}$$

>#### Theorem
>###### $A_{REX}$ is decidable

Proof. Algorithm $P$: 
On input $\langle B,w \rangle$, where $B$ is a Regular Expression and $w$ is a string: 
1. Convert $R$ into an equivalent NFA $A$.
2. Run the TM $N$ for $A_{NFA}$ on input  $\langle A,w \rangle$. 
3. If $N$ accepts, accept; otherwise, reject.
```

```ad-abstract
title: Algorithm $T$: _Emptiness problem_ for DFAs

$$E_{DFA}=\{\langle A\rangle\ |\ A\text{ is a DFA and }L(A)=\emptyset\}.$$
>#### Theorem
>###### $E_{DFA}$ is decidable
##### Proof:
Algorithm $T$:
On inupt $\langle A \rangle $, where $A$ is a DFA.
1. Mark the initial state of $A$.
2. Repeat until no new states get marked:
	- Mark any state that has a transition coming into it from any state that is already marked.
3. If no accepting state is marked, accept. Otherwise, reject.
$\boxdot$
```

```ad-abstract
title: Algorithm $F$: _Equivalence problem_ for DFAs

$$EQ_{DFA} = \{ \langle A,B \rangle \ | \ A \text{ and } B \text{ are DFAs and } L(A) = L(B) \}$$

>#### Theorem
>###### $EQ_{DFA}$ is decidable

##### Proof:
We use the product construction to construct a DFA $C$ with 
$$L(C) = L(A) circleWitXinCenter L(B) = (L(A) - L(B)) \cup (L(B)-L(A)).$$
Then
$$L(C) = \emptyset \iff L(A) = L(B).$$
Algorithm $F$:

On inupt $\langle A,B \rangle$, where $A$ and $B$ are DFAs.
1. Construct the DFA $C$ as described above.
2. Run Algorithm $T$ from previous proof to see if $\langle C \rangle \in E_{DFA}$
3. If $T$ accepts, accept. If $T$ rejects, reject.
$\boxdot$
```

---
### CFGs

>#### Theorem
>###### Every context-free language is decidable

##### Proof:

Let $A$ be context-free and let $G$ be a CFG for $A$. 

Algorithm: 
On input $w$: 
1. Run Algorithm $S$ (or the CKY algorithm) to see if $\langle G,w \rangle \in A_{CFG}$. 
2. If S accepts, accept; otherwise, reject. 

$\boxdot$
_Note that $G$ is hardcoded into this algorithm_

```ad-abstract
title: Algorithm $S$: _Acceptance problem_ for CFGs
$$A_{CFG} = \{ \langle G,w \rangle \ | \ G \text{ is a CFG that generates string } w\}$$

>#### Theorem
>###### $A_{CFG}$ is decidable

##### Proof:
Algorithm $S$:
On input $\langle G,w \rangle$, where $G$ is a CFG and $w$ is a string:
1. Convert $G$ into Chomsky normal form.
(All rules are of the form $A \rightarrow BC$ or $A \rightarrow a$.)
2. List all derivations with $2n - 1$ steps, where $n = |w|$.
3. If any of these derviations generate $w$, accept; if not, reject
$\boxdot$

Cocke-Kasami-Younger: $O(n^3)$-time algorithm
```

```ad-abstract
title: Algorithm $R$: _Emptiness problem_ for CFGs

$$...$$
>#### Theorem
>###### $E_{CFG}$ _is decidable_

##### Proof:
Algorithm $R$: 
On input $\langle G \rangle$, where $G$ is a CFG: 
1. Mark all terminal symbols in $G$. 
2. Repeat until no new variables get marked. 
	- Mark any variable $A$ where $G$ has a rule $A \rightarrow U_1 U_2 \ . \ . \ U_k$ and each symbol $U_1, \ . \ . \ U_k$ has already been marked.
3. If the start symbol is not marked, accept; otherwise, reject.
$\boxdot$
```
---

The _equivlence problem_ for CFGs:
$$...$$

No algorithm exists!
See undecidability...
