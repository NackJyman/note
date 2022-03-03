# Deterministic Finite Automata
#DFA
###### Lecture 1 (01/25/2022)
---
###### [[fsa_home | unit homepage]]
>
A deterministic finite automataton (DFA) is a 5-tuple
>>
$M=(Q,\Sigma,\delta,q_0,F)$ where:
>>
$Q$ = is a finite set of states
$\Sigma$ = is an alphabet
$\delta : Q * \Sigma \rightarrow Q$ is the transition function
$q_0 \in Q$ is the initial state
$F \subseteq Q$ is a set of accepting states
  
*Seen something similar in Digital System Design as State Machines*
  
--- 
**Formal Definition of a DFA:**
>
let $w \in \Sigma^*$ (all finite strings of symbols within the alphabet, Σ). We say that M accepts w if there is a sequence of states.
   $$r_0, r_1 , ... , r_{|w|} \in Q$$
Such that the following three conditions hold:
$$r_0 = q_0$$
$$\delta (r_{i}, w_{i+1}) = r_{i+1} \text{ for all } i \text{, } 0 \leq i < |w|$$
$$r_{|w|} \in F$$
The language accepted by M (or the language that M recognizes) is:
$$L(M) = \{w \in \Sigma^* | M \text{ accepts } w\}$$

---
**Definition of Regular Languages:**
>
A Language $B\subseteq \Sigma^*$ is regular if $B = L(M)$ for some DFA M.
$B = L(M)$ means that
>>
$x \in B \geq$ M accepts x
$x \not \in B \geq$ M rejects x

---
**Designing DFA's**

```ad-error

I don know how to put in pretty DFA diagrams :(
```


	|	  /----\   b   /----\   a   /----\                      |
	|---->| qε | ----> | qb | ----> |qba |                      |
	|	  \----/       \----/       \----/                      |
	|	  /  ↑         /  ↑                                     |
	|	a \__|       b \__|                                     |
	|	                                 __think_machine_state__|

---
When designing these DFA's each node/state needs to have a route for each number of symbols in the alpheabet. Meaning if I have 'a' and 'b,' each node/state needs to have a route for 'a' and 'b.'

```ad-note
title: Induction on Natural Numbers

Prove $(\forall n \ge 0) P(n)$

***Inductive Proof:***
>	
$\text{1. Base Case:}$
$$\text{Prove } P(0)$$
$\text{2. Inductive Hypothesis:}$
$$\text{Assume that  } P(k) \text{ hold for some } k \ge 0.$$
$\text{3. Inductive Step}$
$$\text{Use the inductive hypothesis to prove that P(k + 1) also holds.}$$
>
In the proof, we show that $P(0)$ holds and $$(\forall k \ge 0) P(k) \implies P(k+1).$$
```

---
##### Multiples of Three in binary 

| 0   | 3   | 6   | 9    | 12   | 15   |
| --- | --- | --- | ---- | ---- | ---- |
| 0   | 11  | 110 | 1001 | 1100 | 1111 |



And a DFA to accept multiples of three:

          \---------------------------------------\
          |   /----\    1    /----\    0   /----\ |
          | ->| 0  | <-----> | 1  | <----> | 2  | |
          |   \____/         \____/        \____/ |
          |   /  ↑                         /  ↑   |
          | 0 \__|                       1 \__|   |
          \---------------------------------------\

```ad-note
title:**Example Proof:** For all $x \in \{0, 1\}^*, \Sigma^*(0,x) = bin(x) (mod 3)$

###### Proof:
Base Case: 
$$x = \epsilon$$
$$\text{binary}(\epsilon) = 0$$
$$\delta^*(0,\epsilon) = 0 = bin(\epsilon)$$
Inductive Hypothesis:
$$\delta*(0,x) = bin(x) (\text{mod } 3)$$
Inductive Step:
$$\delta*(0,x_0) = bin(x_0) (\text{mod } 3)$$
$$\delta*(0,x_1) = bin(x_1) (\text{mod } 3)$$
$$δ*(0,x_b) = bin(x_b) (\text{mod } 3) \text{ for } b ∈ {0, 1}$$

$\delta*(0,x) = \delta(\delta^*(0,x), b)$ definition of $\delta^*$
$2 \delta*(0,x) + b (\text{mod } 3)$                      definition of $\delta$
$2*(binary(x) (\text{mod } 3)) + b (\text{mod } 3)$           inductive hypothesis
$2binary(x) + b (\text{mod } 3)$
$bin(x_b) (\text{mod } 3)$                            definition of bin

Therefore $\delta^*(0,x_b) = bin(x_b) (mod 3)$ for both $b \in \{0, 1\}$
```

---
### Product Construction 
Regular languages are closed inder the regular operations of union, concatenation, and star.

##### Regular Operations 
Regular languages are closed inder the regular operations of union, concatenation, and star.


>#### Theorem
>###### "If $A, B \in$ REG, then $A ∪ B \in$ REG"

Let A, B ∈ REG. Then there exist two DFAs $M_A=(Q_A,\Sigma,\delta_A,q_A,F_A)$ and $M_B (Q_B,\Sigma,\delta_B,q_B,F_B)$ with $L(M_A) = A\text{ and }L(M_B) = B.$ We will use $M_A$ and $M_B$ to constructa new DFA $M$ with $L(M) = A∪B$
> Let $M = (Q,\Sigma,\delta,q_0,F)$ where
$$Q = Q_A \text{ x } Q_B = \{(p,q)\text{ | }p \in Q_A, q \in Q_B\}$$
$$\delta : Q × \Sigma → Q\text{ is defined by}$$
$$\delta((p, q), a) = (\delta A(p, a), \delta B (q, a))$$
$$\text{forall } (p, q) \in Q \text{ and a } \in Z$$
$$q_0 = (q_A, q_B)$$

```ad-note
title: **Example Proof:** For all $x \in \Sigma^∗, \Sigma^∗(q_0, x) = (\Sigma^∗A(q_A, x), \Sigma^∗B(q_B , x)).$
###### Proof.
The proof is by induction on strings. For the base case, we have:
$$\delta^∗(q_0, \epsilon) = q_0 = (q_A, q_B) = (\delta^∗(q_A, \epsilon), \delta^∗(q_B , \epsilon)).$$
Now suppose the claim holds for some string x. For the inductive step, we must show the claim also holds for $x_0$ and $x_1.$
For all $b \in \{0, 1\}$,
$$\delta^∗(q_0, x_b) = \delta(\delta^∗(q_0, x), b)$$
$$=\delta ((\delta^∗A(q_A, x), \delta^*_B (q_B , x)), b)$$
$$= (\delta_A(\delta∗A(q_A, x), b), δB (δ∗B (q_B , x), b))$$
$$= (δ∗A(qA, x_b), δ∗B (q_B , x_b)).$$
```

##### Closure Under Complementation
Compliement of language A is defined as:
$$A^c = \{x \in \Sigma^* \text{ | } x \not \in A\}$$

>#### Theorem
>###### "if $A$ is regular, then $A^c$ is regular"

Proof:
Let $M = (Q,\Sigma,\delta,q_0,F)$ be a DFA for A. Define a new DFA:
$$M' = (Q,\Sigma,\delta,q_0,Q - F)$$
that flips the accepting and nonaccepting states of M. Then:
$$L(M') = L(M)^c = A^c$$

##### Closure Under Intersection 


>#### Theorem
>###### "If A and B are regular languages, then A∩B is regular"


Proof: $A∩B = (A^c ∪ B^c)^c$
1. Since A and B are regular, $A^c$ and $B^c$ is also regular.
2. Therefore, $A^c ∪ B^c$ is regular.
3. Finally, ($A^c ∪ B^c$) is regular.

### End of Lecture
---
---
