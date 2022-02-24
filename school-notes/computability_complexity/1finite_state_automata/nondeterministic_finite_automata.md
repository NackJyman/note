# Nondeterministic Finite Automata 
#NFA
###### Lecture 5, Feburary 1st, 2022
---
###### [[fsa_home | unit homepage]]

Nondeterminism is critical in simplifying expressions and DFA's. Nondeterminism is a generalization of determinism.
>
A *nondeterministic finite automataton* (NFA) is a 5-tuple
>>
$M=(Q,\Sigma,\delta,q_0,F)$ where:
>>
$Q$ is a finite set of states
$\Sigma$ is an alphabet
$\delta : Q \text{ x } \Sigma_{\epsilon} \rightarrow P(Q)$ is the transition function
$q_0 \in Q$ is the initial state
$F \subseteq Q$ is a set of accepting states

DFA: $\delta: Q \text{ x } \Sigma \rightarrow Q$
NFA: $\delta: Q \text{ x } \Sigma \rightarrow P(Q)$

Differences between DFA's and NFA's:
- *Domain:* $\Sigma_{\epsilon} = \Sigma \cup \{\epsilon\}$ to allow for $\epsilon$.
- *Codomain:* $P(Q)$ instead of Q so that $\delta (q,a)$ is a set of states rather than a single state - this represents nondeterminism.
>*Visual Example of an NFA with $\epsilon$-transitions:*
![[nfa2.svg]]

>**Definition:**
>Let $N = (Q ,\Sigma,\delta,q_0,F)$ be a NFA and $w \in \Sigma^*$. We say that $N$ accepts $w$ if we can write: 
$$w = y_1y_2 ···y_m$$
where each $y_i \in \Sigma^*$and a sequence of states  
$$r_0,r_1,...,r_m \in Q$$  
exists such that the following three conditions hold:  
$$r_0 = q_0 $$
$$r_{i + 1} \in \delta(r_i ,y_{i+1}) \text{ for i }= 0 ,...,m −1$$
$$r_m \in F$$

### Removing $\epsilon$-transitions
> [!CAUTION]
NEED REDONE PROLLY, JACK YOU DON REALLY UNDERSTAND THIS
```ad-abstract
title:Lemma
###### "Every NFA has an equivalent NFA that does not use $\epsilon$-transitions."
```

Proof: 
Let $N = (Q,\Sigma, \delta, q_0, F)$ be an NFA. for any $q \in Q$, let $E(q)$ be the set of all states that can be reached from q by taking only $\epsilon$-transitions. 

$$E(q) = \{ s \in Q \text{ | } s \text{ can be reached from } q \text{ by taking } 0 \text{ or more } \epsilon \text{-transitions.} \}$$  

Define a new transition function $\delta':Q×\Sigma \rightarrow P(Q)$ by  
$$\delta'(q,a) = ⋃_{s \in E(q)} \delta(s,a)$$

for all $q \in Q$ and $a \in \Sigma$. Then $\delta'(q,a)$ is the set of all states that can be reached from $q$ by taking $\epsilon$-transitions and then an $a$-transition

```ad-hint
title: Theorem

##### "Every NFA has an equivalent DFA"
```

Proof:
Let $N = (Q,\Sigma,\delta_A,q_0,F)$ be an NFA. By the previous lemma, we can assume than N has no $\epsilon$-transitions. (Otherwise, remove the ε-transitions first.)

### Subset Construction:
Let $N$

				/------- Visual Example: ---------------------/
				|        /----\         /-----\  0,1  /----\  |
				|   ---->| 0  |<-|       | {p} |----->|{r} |  |
				|        \____/  |      \_____/       \____/  |
				|         |      \_____                |      |
				|         |1           \0              |0,1   |
				|         |             \              |      |
				|        /----\  0,1    /----\        /----\  |
				|        | q0 | <-----> | q1 |        | q2 |  |
				|        \____/         \____/        \____/  |
				|         |           /                 |     |
				|         |1         /                  |     |
				|         |         /                   |     |
				| 1 /--->/---------\ 0                 /----\ |
				|   \____| {q,p,r} |          0,1 /--->|null| |
				|        \_________/              \____\____/ |
				/---------------------------------------------/
```ad-abstract
title: Corollary
###### "A language is regular if and only if it is accepted by an NFA."
```

```ad-hint
title: Theorems of Regular Languages

##### "The regular languages are closed under union."
$$\text{If } A, B \in REG \text{, then } A ∪ B \in REG$$

##### "The regular languages are closed under concatenation.""
$$\text{If } A, B \in REG \text{, then } A * B \in REG \text{ where } A * B = \{xy \text{ | } x \in A \text{ and } y \in B\}$$
(Copy everything and add epsilon-transitions)

##### "The regular languages are closed under the star operation."
$$\text{If } A \in REG,\text{ then } A^* \in REG\text{ where }A^* = \{x_1 ... x_k \text{ | } x_i \in A \text{ for }i, 1 \le i \le k, k \ge 0.\}$$
```

### End of Lecture
---
---