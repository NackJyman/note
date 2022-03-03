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

A configuration of a TM consists of the current state, the current tape contents, and the current tape head location. A configuration is often represented in the form $$uqv$$ where $u, v \in \gamma^*$ and $q \in Q$, meaning that 
- the current state is $q$ 
- $uv$ is the tape contents 
- the tape head is pointing to the first symbol of $v$

---

Let $M = (Q, Σ, Γ, δ, q0, q_{accept}, q_{reject})$ be a TM. 
- The start configuration of M on input $w \in \Sigma^*$ is the configuration q0w. 
- In an accepting configuration the state is $q_{accept}$.
- In a rejecting configuration the state is $q_{reject}$.
- Accepting and rejecting configurations are called halting configurations

We say that _M accepts w_ if there exists a sequence of configurations $C_1, . \ . \ , C_k$ where
1. $C_1$ is the initial configuration of $M$ on $w$. 
2. each $C_i$ yields $C_{i+1}$ via the transition function $\delta$ in one step.
3. $C_k$ is an accepting configuration.

The language of $M$ is $$L(M) = {w ∈ Σ ∗ | M accepts w}.$$ 

We say that $L(M)$ is the language recognized by $M$
>
---
#### Definition
###### A language is _Turing-Recognizable_ if some Turing machine recognizes it. That is, $B \subseteq \Sigma^*$ is Turing-recognizable if $B = L(M)$ for some Turing machine $M$.

On an input w, a TM M has three possible outcomes: 
- accept 
- reject 
- never halt 

A Turing machine that always halts (i.e. accepts or rejects every input) is called a _decider._ A decider that recognizes some language is said to decide that language.

#### Definition
###### A language is _Turing-decidable_ (or simply decidable) if some Turing machine decides it. That is, $B \subseteq \Sigma^*$ is decidable if $B = L(M)$ for some always-halting Turing machine $M$ (a decider $M$).
_Recursive is also used for this concept_

#### Proposition
###### Every decidable language is also _Turing-Recognizable_

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

