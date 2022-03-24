# Undecidability
##### Computability and Complexity
###### March 3rd, 2022
---

### Countable Sets
The natural numbers is in the set.

#### Definition
###### A set $X$ is _countable_ if ther is a bijection $f : N \rightarrow X$.

---
>#### Theorem (Cantor, 1874)
>###### The set of real numbers R is uncountable

Proof:
Let $f$ by any function mapping $N → R$. We will show that $f$ is not onto. Consider the following table, writing the fractional part of each number in decimal:
$$
\begin{array}{c}
f(0) & = & r_0.d_0^0 d_1^0 d_2^0 d_3^0 &. . .\\
f (1) & = & r_1.d_0^1 d_1^1 d_2^1 d_3^1 &. . .\\
f (2) & = & r_2.d_0^2 d_1^2 d_2^2 d_3^2 &. . .\\
. && . \\ . && . \\ . && .
\end{array}
$$

We define a new number $x$ by $x = 0.e_0e_1e_2e_3 . . .$, where

$$ e_i= \left\{ \begin{array}{rcl} 0 & \text{ if } d_i^i \not = 0 \\
1 & \text{ if } d_i^i = 0 \end{array}\right\}$$

Then for all $i, e_i \not = d_i^i$. Therefore $x \not = f(i)$ for all $i$, so $f$ is not onto.

*Use this technique with Turing Machines*

---
### The Accepting Problem for TMs

The _accepting problem_ for TMs:

$$...$$

>#### Theorem
>###### $A_{TM}$ is _Turing-Recognizable_

##### Proof

The following algorithm recognizes $A_{TM}$. 
Algorithm U: On input $\langle M,w \rangle$, where $M$ is a TM and $w$ is a string: 
1. Simulate M on w. 
2. If M ever enters an accept state, accept. If M ever enters a reject state, reject.
 
If M does not halt on w, then U will not halt. This is why U does not decide $A_{TM}$

A turing machine that takes an turing machine as an input, think emulator or virtual machine.

---

>#### Theorem
>###### $A_{TM}$ is not _decidable_

Proof: (by contradiction)

Assume that ATM is decidable. Suppose that H is a decider for ATM. Then

$H(\langle M,w \rangle)$ = { accept if $M$ accepts $w$ 
{ reject if $M$ does not accept $w$. 

Algorithm $D$: On input $\langle M \rangle$, where $M$ is a TM: 
1. Run $H$ on input $\langle M \langle M \rangle \rangle$. 
2. If $H$ accepts, reject.
If $H$ rejects, accept.

For any $M$, 

$D(\langle M \rangle)$ = ( accept if M does not accept hMi reject if M accepts hMi. 

What if we run $D$ on its own code $\langle D \rangle$?

$D(\langle D \rangle$) = ( accept if D does not accept $\langle D \rangle$ 
reject if D accepts $\langle D \rangle$. 

Whatever D does, this says that D does the opposite, a contradiction. Therefore neither D or H can exist, so ATM is undecidable.

---

To make the diagonalization argument more explicit, we define the _diagonal halting problem._

$$K = \{ ... \}$$

>#### Theorem 
>###### $K$ is undecidable

##### Proof:
...

---

>#### Theorem 
>###### $A_{TM}$ is undecidable

##### Second Proof:
Suppose that $A_{TM}$ is decidable by some TM $N$. We design a TM $D$ as follows: 
D: On input hMi, where M is a TM: 
1. Run N on input hM,hMii. 
2. If N accepts, reject. 
If N rejects, accept. 

Then D decides K: 
- If hMi ∈ K, then hM,hMii ∈/ $A_{TM}$, so N rejects hM,hMii and D accepts hMi. 
- If hMi $\not \in K$, then hM,hMii ∈ $A_{TM}$, so N accepts hM,hMii and D rejects hMi. 


But K is undecidable, a contradiction. Therefore ATM is undecidable.

---
This is about the limits of computation, Finding the possibility of a result.

---
### Co-Turing-Recognizability

>#### Definition
>###### A language is _co-Turing-recognizable,_ if it is the complement of a Turing-recognizable language.

If $A$ is co-turing-recognizable, then $A^c$ is Turing-recognizable.This means there is a Turing machine M such that for all inputs 
- w, w ∈ A c ⇒ M accepts w. 
- w 6∈ A c ⇒ M does not accept w.

Equivalently, 
- w ∈/ A ⇒ M accepts w. 
- w ∈ A ⇒ M does not accept w.

A is decidable if there is a TM M such that for all w, 
- w ∈ A ⇒ M accepts w. 
- w ∈/ A ⇒ M rejects w. 

A is Turing-recognizable if there is a TM M such that for all w, 
- w ∈ A ⇒ M accepts w. 
- w ∈/ A ⇒ M does not accept w. 
 
A is co-Turing-recognizable if there is a TM M such that for all w, 
- w ∈ A ⇒ M does not accept w. 
- w ∈/ A ⇒ M accepts w

>#### Theorem
>###### A language is decidable if and only if both it is both Turing-recognizable and co-Turing-Recognizable.
![[Pasted image 20220310111203.png]]
##### Proof.

Let A be a language. 

(⇒) If A is decidable, then A c is also decidable. 

Since every decidable language is Turing-recognizable, both A and A c are Turing-recognizable

(⇐) Suppose that A and A c are Turing-recognizable. Let M1 be a recognizer for A and let M2 be a recognizer for A c .
Then 
- w ∈ A ⇒ M1 accepts w. 
- w ∈/ A ⇒ M1 does not accept w. 

and 
- w ∈ A ⇒ M2 does not accept w. 
- w ∈/ A ⇒ M2 accepts w. 

When M1 or M2 does not accept, they may run forever.

Let M be the following TM: 
>M: On input w: 
>1. Run both M1 and M2 on w in parallel. 
>2. If M1 accepts, accept. If M2 accepts, reject. 

Running in “parallel” means using two tapes (which can be simulated by a one-tape TM). 
- Copy the input from the first tape to the second tape. Move both tape heads to the beginning. 
- Run M1 on the first tape and M2 on the second tape simultaneously. 
- Accept if M1 accepts. Reject if M2 accepts.

We have 
$w ∈ A ⇒ M1$ accepts $w ⇒ M$ accepts w 
and 
$w \not ∈ A ⇒ M_2$ accepts $w \\ ⇒ M$ rejects w. 

Then $M$ decides $A$, so A is decidable.

---

>#### Corollary
>###### 1. If A is Turing-recognizable but not decidable, then A is not co-Turing-recognizable. 
>###### 2. If A is co-Turing-recognizable but not decidable, then A is not Turing-recognizable.

_Get the diagram for dis_

---
#### Corollary
###### $A_{TM}$ is not co-turing-recognizable

##### Proof:
We know that $A_{TM}$ is TUring-recognizeable. If $A_{TM}$ is also co-Turing-recognizable, then $A_{TM}$ is decidable, but it is not. $\boxdot$

---

### Halting Problem for TMs

The _Halting Problem_ for TMs:

$$HALT_{TM} = \{ ... \}$$

>#### Theorem
>###### $HALT_{TM}$ is a Turing-recognizable.

Proof. The following algorithm recognizes $HALT_{TM}$. 

Algorithm U: On input $\langle M,w \rangle$, where M is a TM and w is a string: 
1. Simulate M on w. 
2. If M halts, accept. 

If M does not halt on w, then U will not halt. This is why U does not decide $HALT_{TM}$.

>#### Theorem
>###### $HALT_{TM}$ is undecidable.

##### Proof: 
Suppose that $HALT_{TM}$ is decidable. We will show that ATM is also decidable, obtaining a contradiction. Let R be a decider for $HALT_{TM}$. We construct the following TM S to decide ATM. 
S: On input $\langle M,w \rangle$, where M is a TM and w is a string: 
1. Run TM R on input $\langle M,w \rangle$. 
2. If R rejects, reject. 
3. If R accepts, simulate M on w until it halts. 
	- If M accepts, accept. 
	- If M rejects, reject.

We now verify that S decides ATM. First suppose $\langle M,w \rangle$ ∈ ATM: 
$\langle M,w \rangle$ ∈ ATM ⇒ M accepts w 
⇒ M halts on w 
⇒ R accepts $\langle M,w \rangle$
⇒ S simulates M on w 
⇒ S finds that M accepts w 
⇒ S accepts $\langle M,w \rangle$

Now suppose $\langle M,w \rangle \not \in A_{TM}$. Then $M$ does not accept $w$, so either $M$ rejects $w$ or $M$ does not halt on $w$. We consider these two cases separately: 

M rejects w ⇒ M halts on w 
⇒ R accepts $\langle M,w \rangle$ 
⇒ S simulates M on w 
⇒ S finds that M rejects w 
⇒ S rejects $\langle M,w \rangle$ 

M does not halt on w ⇒ R rejects $\langle M,w \rangle$ 
⇒ S rejects $\langle M,w \rangle$ 

In either case, $S$ rejects $\langle M,w \rangle$.

Since $A_{TM}$ is undecidable, decider $S$ does not exist. 

Therefore decider R for $HALT_{TM}$ does not exist and $HALT_{TM}$ must be undecidable.

_see slide 78 on 17_undecidable_

---

### Emptiness Problem for TMs

The emptiness problem for TMs:
$$E_{TM} = \{\langle M \rangle \ | \ M \text{ is a TM and } L(M) = \emptyset \}$$
>#### Theorem
>###### $E_{TM}$ is undecidable.

##### Proof:

Suppose that ETM is decidable. We will show that ATM is also decidable. The idea of the proof is that given any instance hM,wi of ATM, we can construct an instance hM(w) i of ETM so that: If M accepts w, then L(M(w) ) 6= ∅. If M does not accept w, then L(M(w) ) = ∅.

Let M be a TM and w be an input for M. Here is the description of M(w) . Note that w is hardcoded into M(w) . M(w) : on any input x: 1 If x 6= w, reject. 2 If x = w, then run M on input w and accept if M does. Then if M accepts w, L(M(w) ) = {w}. If M does not accept w, L(M(w) ) = ∅

Now, assuming that ETM is decidable, we can use an algorithm for ETM to solve ATM. Suppose R is a TM that decides ETM. S: on input hM,wi, where M is a TM and w is a string: 1 Use the description of M and w to construct the TM M(w) as described above. 2 Run R on input hM(w)i. If R accepts, reject. If R rejects, accept.

We verify that S decides A_{TM}. 
$\langle M,w \rangle$ ∈ ATM ⇒ M accepts w 
⇒ L(M(w) ) = {w} 
⇒ L(M(w) ) 6= ∅ 
⇒ hM(w) i ∈/ ETM 
⇒ R rejects hM(w) i 
⇒ S accepts hM,wi 
$\langle M,w \rangle$ ∈/ ATM ⇒ M does not accept w 
⇒ L(M(w) ) = ∅ 
⇒ hM(w) i ∈ ETM 
⇒ R accepts hM(w) i 
⇒ S rejects hM,wi 

Therefore S decides ATM, a contradiction, so ETM is undecidable. $\boxdot$

---

We just proved $E_{TM}$ is undecidable. Is it Turing-Recognizable.

>#### Theorem
>###### $E_{TM}$ is _co-turing-recognizable_

##### Proof:
Let s1,s2, . . . be an enumeration of all strings in Σ∗ . 

Algorithm A: On input hMi: 
	
for i = 1, 2, . . . 
for j = 1 to i 
Run M on input sj for i steps. 
If M accepts, accept. 

- If L(M) 6= ∅, then some string is accepted by M, so A will accept hMi. 
- If L(M) = ∅, then no string is accepted by M, and A will run forever on hMi. 

Therefore $E^c_{TM}$ is Turing-recognizable.

---

### Summary
![[Pasted image 20220310112849.png]]