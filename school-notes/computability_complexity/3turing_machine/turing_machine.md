# Turing Machines
##### Computability and Complexity
###### Feburary 24th, 2022
---
Begining of Unit 3.

---

### Turing Machines
- Simple Model of a general purpose computer.
- Finite automaton with infinite read/write tape.


#### Church-Turing Thesis
$$\text{intuitive notion of algorithms}$$
$$\textbf{equals}$$
$$\text{Turing machine algorithms}$$

You can take any turing machine and convert it to a $\lambda$-calculus expression.

#### Turing Machine Operation

Initially, the tape contains the input string on the left and is blank everywhere else. The tape head is on the first cell and the TM is in its initial state. 

The TM has a transition function telling what to do based on what the tape head is reading and the current state: the TM can move its tape head left or right, possibly writing information on the the tape.

---

##### Pseudocode/Informal description of a TM that accepts
###### $$\{w \in \{0, 1\}^* | w \text{ is a palindrome}\}.$$
1. If the current cell is blank, ACCEPT. // even length palindrome. 
Remember the symbol in the current cell and erase it with a blank symbol.
2. Scan to the right until a blank symbol is found. 
Move back to the left one cell.
4. If the current cell is blank, Accept. // odd length palindrome, 
If the symbol in the current cell matches the remembered symbol, erase it with a blank symbol and go to step 4. If it does not match, REJECT.
5. Scan to the left until a blank symbol is found. 
Move back to the right one cell.
Go back to step 1.

---

#### Formal Definition of Turing Machine.

A _Turing Machine_ (_TM_) is a 7-tuple
$M = (Q, \Sigma, \Gamma, \delta, q_0, q_{accept},q_{reject})$ where
1. $Q$ is a finite set of _states_.
2. $\Sigma$ is the _input alphabet_ not containing the special _blank_ symbol _
3. $\Gamma$ is the _tape alphabet_ where _ $\in \Gamma$ and $\Sigma \subseteq \Gamma$
4. $\delta : Q \times \Gamma \rightarrow Q \times \Gamma \times \{ L, R \}$ is the _transition function_
5. $q_0 \in Q$ is the _initial state_
6. $q_{accept} \in Q$ is the _accept state._
7. $q_{reject} \in Q$ is the _reject state._

---

##### Example TM
$\delta : Q \times \Gamma \rightarrow Q \times Γ \times {L, R}$ is the _transition function_

If δ(q, a) = (p, b, R), then if the TM is in state q and the tape head is reading a, the TM will write b on the current tape cell (replacing a), move the tape head to the right, and transition to state $p$

DIAGRAM GO HERE BRRR FIND SOME NON SHIT WAY TO MAKE THESE THAT DOESNT TAKE 30 MINUES TO MAKE ONE FUCKING TRANSITION.

If $δ(q, a) = (p, b, L)$, then the same things happen, except the head moves to the left.

DIAGRAM GO HERE

##### TM for Palindromes

BIG ASS FUCKING DIAGRAM THAT YOU HAVE NO LIVING CHANCE TO REPLICATE IN THIS SOFTWARE UNLESS YOU WANNA SPEND ~30 HOURS DOING THAT.
![[Pasted image 20220224112438.png]]

---
### Simplifing Turing Machine Diagrams

![[Pasted image 20220224112409.png]]
![[Pasted image 20220224112554.png]]

---
### Examples of Turing Machines
```ad-example
title: TM for $POWERS = \{ 0^{2^n} \ | \ n \ge 0 \}$

![[Pasted image 20220224113403.png]]
![[Pasted image 20220224113339.png]]
```

```ad-example
title: TM accepting $\{ w \# w \ | \ w \in \{0,1\}^*\}$

![[Pasted image 20220224113113.png]]
![[Pasted image 20220224113138.png]]
![[Pasted image 20220224113147.png]]
```

---
### Computation of a TM

Initially the input $w \in \Sigma^*$ is written on the tape, one symbol per tape cell, followed by blanks. The tape head points to the first symbol of w and the TM is in its initial state. 

The computation proceeds according to the transition function. If the TM ever tries to move its head off the left end of the tape, the tape head stays in place for that step.

The computation continues until the TM enters the accept state or the reject state, at which point it halts. Otherwise, the TM goes on forever.

---

A _configuration_ of a TM consists of the current state, the current tape contents, and the current tape head location. A configuration is often represented in the form $$uqv$$ where $u, v \in \Gamma^*$ and $q \in Q$, meaning that 
- the current state is $q$ 
- $uv$ is the tape contents 
- the tape head is pointing to the first symbol of $v$

---

Let $M = (Q, \Sigma, \Gamma, \delta, q_0, q_{accept}, q_{reject})$ be a TM. 
- The start configuration of $M$ on input $w \in \Sigma^*$ is the configuration $q_0w$. 
- In an accepting configuration the state is $q_{accept}$.
- In a rejecting configuration the state is $q_{reject}$.
- Accepting and rejecting configurations are called halting configurations

We say that _$M$ accepts $w$_ if there exists a sequence of configurations $C_1, . \ . \ , C_k$ where
1. $C_1$ is the initial configuration of $M$ on $w$. 
2. each $C_i$ yields $C_{i+1}$ via the transition function $\delta$ in one step.
3. $C_k$ is an accepting configuration.

The language of $M$ is $$L(M) = \{w \in \Sigma^* \ | \ M \text{ accepts } w\}$$ 

We say that $L(M)$ is the language recognized by $M$

>
---
>#### Definition
>###### A language is _Turing-Recognizable_ if some Turing machine recognizes it. That is, $B \subseteq \Sigma^*$ is Turing-recognizable if $B = L(M)$ for some Turing machine $M$.

On an input $w$, a TM $M$ has three possible outcomes: 
- accept 
- reject 
- never halt 

A Turing machine that always halts (i.e. accepts or rejects every input) is called a _decider._ A decider that recognizes some language is said to decide that language.

>#### Definition
>###### A language is _Turing-decidable_ (or simply decidable) if some Turing machine decides it. That is, $B \subseteq \Sigma^*$ is decidable if $B = L(M)$ for some always-halting Turing machine $M$ (a decider $M$).
_Recursive is also used for this concept_

>#### Proposition
>###### Every decidable language is also _Turing-Recognizable_

---
![[Pasted image 20220224114401.png]]

---

### Describing Turing Machines
- _Formal Description:_ low-level programming, complete detail, state-transition diagram.
- _implementation description:_ in English, how the TM moves its head and uses its tapes to store data.
- _High-Level description:_ Description of algorithm, ignoring model.

#### Other Turing Machine Models
1. Multitape Turing Machines
2. Nondeterministic Turing Machines
3. Enumerators

##### Multitape Turing Machines:

- $k$ tapes, for some $k \ge 2$
- transition function:

$$\delta: Q \times\Gamma^k \rightarrow Q \times \Gamma^k \times \{ L,R \}^k$$

If $\delta(q,a_1,...,a_k) = (p,b_1,...,b_k,D_1,...,D_k)$, then when the TM is in state $a$ and the $k$ tape heads are reading $a_1, ... , a_k$, the symbols $b_1, ... , b_k$ are written on each of the tapes and the heads are moved in directions $D_1,... D_k$.

###### Every multitape Turing machine has an equivalent single-tape Turing machine.

##### Nondeterministic Turing Machines (NTMs)

-	Transition function

$$\delta : Q \times \Gamma \rightarrow P(Q \times \Gamma \times \{ L,R\})$$

>#### Theorem
>###### Every NTM has an equivalent deterministic TM.

###### Proof:

Let $N$ be an NTM. The idea is to do a breadth-first search on $N$'s computation tree. We will design a 3-tape deterministic TM $D$ for this.

- Input tape; contains the input; is never altered.
- Simulation Tape: copy of N's tape on some branch of it's computation tree.
- Address Tape: used to keep track of D's location N's computation tree
- On Tape 3 (The address Tape), we use the alphabet $\Sigma_b = \{1,...,b \}$, where $b$ is the size of the largest set of possible choices in $N$'s transition function.
- Every node in $N$'s computation tree is given an address that is a string in $\Sigma^*_b$.
- For ecample, address 231 identifies the node we get by taking the $2^{nd}$ nondeterministic choice, then the $3^{rd}$ choice, and lastly thr $1^{st}$ choice.
- Some addresses may not correspond to nodes.

**Description of deterministic TM $D$ to simulate $N$**
1. Initially, tape 1 contains the input $w$.
2. Copy tape 1 to tape 2.
3. Use tape 2 to simulate $N$ with input $w$ on one branch of its nondeterministic omputation. Before each step of $N$, consult the next symbol on tape 3 to determine which nondeterministic choice to make.
 	- If no more symbols remain on tape 3 or this is not a valid choice, abort this branch and go to step 4. 
 	- Also go to step 4 if this choice leads to a rejecting configuration. 
 	- If this choice leads to an accepting configuration, ACCEPT.
4. Replace the string on tape 3 with lexicographically next string. Go to step 2.

Suppose $N$ accepts $w$.Then there is some sequence of nondeterministic choices that leads $N$ to an accepting state. Consider the lexicographically least sequence of such choices and let $a \in \Sigma^* b$ be its address. Eventually a will be on **tape 3** and $D$ will accept. 

Now suppose that $N$ does not accept $w$. Then no sequence of nondeterministic choices leads to an accepting configuration. This means $D$ will never accept (in fact, $D$ will run forever). 

Therefore $L(D) = L(N)$.

---

>#### Theorem
>###### Every NTM has an equivalent deterministic TM.

**Corollary**

A language is Turing-recognizable if only if some NTM recognizes it.

##### Enumerators

An _enumerator_ is a TM with a second output tape, which we may conceptualize as a printer

An enumerator $E$ starts out with an empty tape. As it computes, it may print a string from time to time. If it runs forever, it may print an infinite list of strings.

The _Language enumerated_ by $E$ is the collection of strings that it prints. The strings may be printed in any order, possibly with repetitions.

>#### Theorem
>###### A language is Turing-recognizable if and only if some enumerator enumerates it.

##### Proof:

...

### Quantifiers, Predicatesm, and Recogizability

Another way to define Turing-recognizability is in terms of existential queantifiers and decidable predicates.

>#### Theorem 
>###### A _language_ $A$ is Turing-recognizable if and only if there is a decidable language $D$ such that for all $w \in \Sigma^*$, $$x \in A \iff (\exists w \in \Sigma^*) \langle x,w \rangle \in D$$

- A string $w$ with $\langle x,w \rangle \in D$ is a _witness_ that $x \in A$
- We will use quantifiers, predicates, and witnessess again later when we study the complexity class NP.

##### Proof:

...

## Summary

The following names for Turing-recognizability are equivalent: 
- A is Turing-recognizable 
- A is computably enumerable (c.e.) 
- A is recursively enumerable (r.e.) 

The following definitions for Turing-recognizability are equivalent: 
- A is recognized by a TM 
- A is recognized by a multitape TM 
- A is recognized by an NTM 
- A is enumerated by an enumerator 
- A is definable with an existential quantifer and a decidable predicate

---

### End of Lecture