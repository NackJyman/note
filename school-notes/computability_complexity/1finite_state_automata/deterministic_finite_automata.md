# Deterministic Finite Automata
#DFA
###### Lecture 1 (01/25/2022)
---
###### [[fsa_home | unit homepage]]

---

Deterministic finite automataton describe the behavior of a process to check for a given condition.

*Seen something similar in Digital System Design as State Machines*
#### Definition of DFA's

>
A deterministic finite automataton (DFA) is a 5-tuple
>
$M=(Q,\Sigma,\delta,q_0,F)$ where:
>
>- $Q$ = is a finite set of states
>- $\Sigma$ = is an alphabet
>- $\delta : Q * \Sigma \rightarrow Q$ is the transition function
>- $q_0 \in Q$ is the initial state
>- $F \subseteq Q$ is a set of accepting states  
>--- 
>**Formal Definition of a DFA:**
>
let $w \in \Sigma^*$ (all finite strings of symbols within the alphabet, Σ). We say that M accepts w if there is a sequence of states.
   $$r_0, r_1 , ... , r_{|w|} \in Q$$
Such that the following three conditions hold:
>- $r_0 = q_0$
>- $\delta (r_{i}, w_{i+1}) = r_{i+1} \text{ for all } i \text{, } 0 \leq i < |w|$
>- $r_{|w|} \in F$
>
The language accepted by M (or the language that M recognizes) is:
$$L(M) = \{w \in \Sigma^* | M \text{ accepts } w\}$$

Take this example DFA:

![[dfaex1.svg]]

Here we can describe the qualities of the DFA:

- $Q = \{ q_0 , q_1, q_2\}$
- $\Sigma = \{0,1\}$
- $q_0$ is the initial state
- $F = \{ q_2 \}$
- $\delta : Q \times \Sigma \rightarrow Q$ is given by

$$\begin{array}{rcl}
\delta(q_0, 0) = q_0 & \delta(q_0, 1) = q_1 \\
\delta(q_1, 0) = q_1 & \delta(q_1, 1) = q_2 \\
\delta(q_2, 0) = q_2 & \delta(q_2, 1) = q_0 
\end{array}$$

#### Strings and Languages

$\Sigma^* =$ all finite strings of symbols from the alphabet $\Sigma$
$|w| =$ length of the string $w$

if $\Sigma = \{0,1\}$, then

$$\Sigma^* = \{ \epsilon ,01,00,01,10,11,000,001,010,\ . \ . \ ,111,0000,....\}$$

$\epsilon$ is the _empty string_ of length 0.
$| \epsilon | = 0$

A _language_ is a subset $B \subseteq \Sigma^*$.

_Langugaes_ are also called _decision problems_, primarily in the areas covered in the second half of the course.

---
A useful, equivalent way to define acceptance uses induction.

Recall the transition function $\delta$ maps $Q \times \Sigma$ into $Q$.
We define an extension $\delta^*$ thtat naos $Q \times \Sigma^*$ into $Q$ as follows:

$\delta^* : Q \times \Sigma^* \rightarrow Q$
- For all $q \in Q, \delta^*(q, \epsilon) = q$
- For all $q \in Q, w \in \Sigma^*$, and $a \in \Sigma,$
$$\delta^*(q,wa) = \delta(\delta^*(q,w),a).$$
For any state $q$ and string $w, \delta^*(q,w)$ is the state $M$ will finish in if it starts processing $w$ in state $q$.

##### Alternate Definition

Let $M = (Q, \Sigma, \delta, q_0, F)$ be a DFA. Let $w \in \Sigma^*$. We sat that $M$ accepts $w$ if 

$$\delta^*(q_0, w) \in F$$

The _language accepted_ by $M$ is 
$$L(M) - \{w \in \Sigma^* \ | \ \delta^*(q_0, w) \in F \}$$



---
#### Definition of Regular Languages:
>
A Language $B\subseteq \Sigma^*$ is regular if $B = L(M)$ for some DFA M.
$B = L(M)$ means that
>>
$x \in B \geq$ M accepts x
$x \not \in B \geq$ M rejects x

---
### Designing DFA's
```ad-example
title: $\{w \in \{ a,b\}^* \ | \ w \text{ begins with } ab \}$

![[Pasted image 20220307165919.png]]
```

```ad-example
title: $\{w \in \{ a,b\}^* \ | \ w \text{ ends with } ba \}$

![[Pasted image 20220307165941.png]]

or

![[Pasted image 20220307170304.png]]
```

```ad-example
title: $\{ x010y \ | \ x,y \in \{0,1\}^* \}$

![[Pasted image 20220307170342.png]]
```

```ad-example
title: $\{w \in \{ a,b\}^* \ | \ w \text{ begins with } ab \}$

![[Pasted image 20220307170441.png]]
```

```ad-example
title: $\{w \in \{0,1\}^* \ | \ \text{the first bit of } w \text{ equals the last bit} \}$

![[Pasted image 20220307170505.png]]
```

```ad-example
title: $\{a^n \ | \ n \text{ is a multiple of } 6 \}$

![[Pasted image 20220307170638.png]]
```


#### Slides Summary
1. Each state in a DFA "remembers" or represents something about the input string.
2. Design each state with a clear idea of what is represetns.
3. Each state in a DFA should have exactly one transition for each alphabet symbol.
4. Transitions move the DFA from one state to another as an input symbol is read. Think about how to use these transitions to update what the DFA remembers as it reads each symbol.
5. Include comments explaining how your DFA works.
(Analogous to commenting code.)
6. Test your DFA on sample inputs both in and out of the language. Check whether it operates as expected. Try to find examples that make it fail. (Analogous to software testing.)

###### End of 01_dfa Slides

---
### Examining Binary Numbers

Each binary string $x \in \{0,1\}^*$ corresponds to a natural number _bin(x)_ $\in N$.

| x          | bin(x) |     | x    | bin(x) |
| ---------- | ------ | --- | ---- | ------ |
| $\epsilon$ | 0      |     |      |        |
| 0          | 0      |     | 1000 | 8      |
| 1          | 1      |     | 1001 | 9      |
| 10         | 2      |     | 1010 | 10     |
| 11         | 3      |     | 1011 | 11     |
| 100        | 4      |     | 1100 | 12     |
| 101        | 5      |     | 1101 | 13     |
| 110        | 6      |     | 1110 | 14     |
| 111        | 7      |     | 1110 | 15     |

#### Inductive Definition of _bin(x)_

For any $x \in \{ 0,1 \}^*$, let _bin(x)_ be the number $x$ encodes in binary.
This is defined inductively as follows.
- The base case is bin($\epsilon$) = 0
- For any $x$, assuming bin(x) has already been defined, 
$$\begin{array}{rcl}
bin(x0) & = & 2bin(x), \\
bin(x1) & = & 2bin(x) + 1.
\end{array}$$

##### Multiples of Three in binary 

| 0   | 3   | 6   | 9    | 12   | 15   |
| --- | --- | --- | ---- | ---- | ---- |
| 0   | 11  | 110 | 1001 | 1100 | 1111 |



And a DFA to accept multiples of three:

![[Pasted image 20220307173949.png]]


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
>
Use the inductive hypothesis to prove that $P(k + 1)$ also holds.
>
In the proof, we show that $P(0)$ holds and $$(\forall k \ge 0) P(k) \implies P(k+1).$$
```

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

When designing these DFA's each node/state needs to have a route for each number of symbols in the alpheabet. Meaning if I have 'a' and 'b,' each node/state needs to have a route for 'a' and 'b.'

---
### Product Construction 
Regular languages are closed inder the regular operations of union, concatenation, and star.

##### Regular Operations 
Regular languages are closed inder the regular operations of union, concatenation, and star.


>#### Theorem
>###### "If $A, B \in$ REG, then $A ∪ B \in$ REG"

Let A, B ∈ REG. Then there exist two DFAs
$M_A=(Q_A,\Sigma,\delta_A,q_A,F_A)$ and $M_B (Q_B,\Sigma,\delta_B,q_B,F_B)$ with $L(M_A) = A\text{ and }L(M_B) = B.$ We will use $M_A$ and $M_B$ to constructa new DFA $M$ with $L(M) = A \cup B$.
> Let $M = (Q,\Sigma,\delta,q_0,F)$ where
>- $Q = Q_A \text{ x } Q_B = \{(p,q)\text{ | }p \in Q_A, q \in Q_B\}$
>- $\delta : Q × \Sigma → Q\text{ is defined by}$
$$\delta((p, q), a) = (\delta_A(p, a), \delta_B (q, a))$$
$\text{forall } (p, q) \in Q \text{ and a } \in Z$
>- $q_0 = (q_A, q_B)$
>- $F = \{(p,q) \in Q \ | \ p \in F_A \text{ or } q \in F_B \}$

```ad-example
title: Product Construction

![[Pasted image 20220307175142.png]]

```

```ad-example
title: Proof: For all $x \in \Sigma^*, \delta^*(q_0, x) = (\delta_A^*A(q_A, x), \delta^*B(q_B , x)).$
###### Proof.
The proof is by induction on strings. For the base case, we have:
$$\delta^*(q_0, \epsilon) = q_0 = (q_A, q_B) = (\delta^*(q_A, \epsilon), \delta^*(q_B , \epsilon)).$$
Now suppose the claim holds for some string x. For the inductive step, we must show the claim also holds for $x_0$ and $x_1.$
For all $b \in \{0, 1\}$,

$$\begin{array}{rcl}
\delta^*(q_0, x_b) &=& \delta(\delta^*(q_0, x), b) \\
&=&\delta ((\delta^*_A(q_A, x), \delta^*_B (q_B , x)), b) \\
&=& (\delta_A(\delta^*_A(q_A, x), b), \delta_B (\delta^*_B (q_B , x), b)) \\
&=& (\delta^*_A(q_A, x_b), \delta^*_B (q_B , xb)). \end{array}$$
$\boxdot$
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


Proof: $A \cap B = (A^c \cup B^c)^c$
1. Since A and B are regular, $A^c$ and $B^c$ is also regular.
2. Therefore, $A^c \cup B^c$ is regular.
3. Finally, ($A^c \cup B^c$) is regular.

### End of Lecture
---
---
