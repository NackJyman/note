# The Arithmetical Hierarchy
##### Computability and Complexity

---

Pre-Requesit Knowledege:

Turing-Recognizable and Co-Turing Recognizable.

>#### Theorem
>###### A _Language_ $A$ is Turing-Recognizable if and only if there is a decidable language $D$ such that there exists $x \in \Sigma^*$,
>###### $$x \in A \iff (\exists w \in \Sigma^*) \ \langle x,w \rangle \in D.$$

Using the $\forall$ quantifier defines the co-turing-rcognizable.

>#### Theorem
>###### A _Language_ $A$ is Co-Turing-Recognizable if and only if there is a decidable language $D$ such that for all $x \in \Sigma^*$,
>###### $$x \in A \iff (\forall w \in \Sigma^*) \ \langle x,w \rangle \in D.$$

##### Proof of Corollary:

Let $A$ be co-Turing-recognizable
Then A c is Turing-recognizable. By the Theorem, there is a decidable language C such that for all x ∈ Σ ∗ , 
x ∈ A c ⇐⇒ (∃w ∈ Σ ∗ ) hx,wi ∈ C. 

Let D = C^c . We have 
x ∈ A ⇐⇒ x ∈/ A c 
⇐⇒ ¬ [(∃w ∈ Σ ∗ ) hx,wi ∈ C] 
⇐⇒ (∀w ∈ Σ ∗ ) ¬ [hx,wi ∈ C]
⇐⇒ (∀w ∈ Σ ∗ ) hx,wi ∈/ C 
⇐⇒ (∀w ∈ Σ ∗ ) hx,wi ∈ D. 

This proves the left-to-right direction. The proof of the right-to-left direction is similar.

- Use of DeMorgan's Law

---

### Defining the Arithmetical Hierarchy

![[Pasted image 20220324111428.png]]

#### First Level
$A \in \Sigma_1$ if there is a decidable $B$ such that for all $x \in \Sigma^*$,
$$x \in A \iff (\exists w) \langle x,w \rangle \in B$$

$\Sigma_1 =$ Turing-recognizable 

$A \in |-|_1$ if there is a decidable $B$ such that for all $x \in \Sigma^*$,
$$x \in A \iff (\forall w) \langle x,w \rangle \in B$$

#### Second Level

...

#### Third Level

...

Same idea for lvl 4.

---

![[Pasted image 20220324111809.png]]

We will use Arithmetical Hierarchy to clasify ...

---

### Acceptance Problem for TMs 

Recall the acceptance problem for TMs: 

$$A_{TM} = \{ \langle M,w \rangle \ | \ M \text{ is a TM that accepts input string } w \}. $$

We showed $A_{TM}$ is undecidable and Turing-recognizable. Let’s see how ATM meets the quantifier definition for Σ1: 

We have 
$$\begin{array}{rcl} 
\langle M,w \rangle \in A_{TM} & \iff & M accepts w \\ 
& \iff & (\exists t) \ M \text{ accepts } w \text{ in } t \text{ steps.}\end{array}$$ 
Define 
$$D = \{\langle M, x,t \rangle \ | \ \text{ accepts  } x \text{ in } t \text{ steps}\}. $$
Then 
$$\langle M,w \rangle \in A_{TM} \iff (∃t) \langle \langle M,w \rangle,t\rangle \in D. $$
Since $D$ is a decidable predicate and we used $\exists$, this shows $A_{TM} \in \Sigma^1$.

![[Pasted image 20220324112149.png]]

### Halting Problem for TMs

slide 35
...

---

### Emptiness Problem for TMs 

slide 40
...

---

### Diaganol Halting Problem
slide 52
...

---

### All Problem for TMs 

slide 54

---

### Infinite Problem for TMs

The infinite problem for TMs is 

$$INF_{TM} = \{ \langle M \rangle \ | \ L(M) \text{ is infinite}\}.$$

Given $\langle M \rangle$, the problem is to determine whether $M$ accepts infinitely many strings.

$INF_{TM}$ is neither Turing-recognizable nor co-Turing-recognizable.

#### Classifying $INF_{TM}$

We have 

$$\begin{array}{rcl}
\langle M \rangle \in INF_{TM} & \iff  & L(M) \text{ is infinite} \\
& \iff  & L(M) \text{ is infinite} \\
& \iff  & L(M) \text{ is infinite} \\
& \iff  & L(M) \text{ is infinite} 
\end{array}$$

...
REVISE slide 64

---

### Finite Problem for TMs

...

#### Classifying $FIN_{TM}$

---

### Equivalence Problem

#### Classifying $EQ_{TM}$

---

### Regularity Problem for TMs

---

### Infinite Complement Problem for TMs

#### Classifying $INFCOMP_{TM}$

---

## Summary
![[Pasted image 20220324113410.png]]
_Arrows indicate subsets_