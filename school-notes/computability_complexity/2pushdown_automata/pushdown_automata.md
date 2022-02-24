# Pushdown Automata
#PDA
###### Lecture 11 (02/15/2022)
---
###### [[pa_home | unit homepage]]

---

#### Formal Defninition
A (Nondeterministic) *pushdown automaton* (PDA) is a 6-tuple $M = (Q \Sigma, \Gamma, \delta, q_0, F)$ where
-	Q is a finite set of states
-	$\Sigma$ is the input alphabet
-	$\Gamma$ is the stack alphabet
-	$\delta : Q \times \Sigma_{\epsilon} \times \Gamma_{\epsilon} \rightarrow P(Q \times \Gamma_{\epsilon})$ is the transition function
-	$q_0 \in Q$ is the initial state
-	$F \subseteq Q$ is the set of accepting states

---
In notation the $ symbol ...

##### PDA for $\{0^n 1^n \vert n \ge 0 \}$
![[Pasted image 20220217112820.png]]

q2 -> q2, read a 0 pop a 0

Going from $q_1$ to $q_2$, it reads an empty string and pushes an empty string to the stack.

---
You can use a stack to describe context-free grammar.

---
### Computing
#### Configurations
A Configurateion of M is a 3-tuple $(q,w,z) \in Q \times \Sigma^* \times \Gamma^*$

Here q denotes the current state, w denotes the remaining input, and z denotes the current stack.

The Initial configuration of M is ...

#### Computation of PDA

$(q_1,w_1, z_1) = (q_1, aw, b_1z) \rightarrow (q_2, w, b_2z) = (q_2, w_2, b_2)$

Computation on 000111:
$$(q_1, 000111, \epsilon) \rightarrow (q_2, 000111, $)$$
$$\rightarrow (q_2, 00111, 0$)$$
$$\rightarrow (q_2, 0111, 00$)$$
$$\rightarrow (q_2, 111, 000$)$$
$$\rightarrow (q_3, 111, 000$)$$
$$\rightarrow (q_3, 11, 00$)$$
$$\rightarrow (q_3, 1, 0$)$$
$$\rightarrow (q_3, \epsilon, $)$$
$$\rightarrow (q_4, \epsilon, \epsilon)$$

---

### Examples
##### PDA for $\{ ww^R | w \in \{0, 1\}^*\}$
![[Pasted image 20220217111857.png]]

---
##### PDA for $\{ww^R | w \in \{0,1\}^*\}$
![[Pasted image 20220217112058.png]]
###### Example Computation with 01011010 as input.
$$(q_1, 01011010, \epsilon)$$
$$\rightarrow (q_2, 01011010, $)$$
$$\rightarrow (q_2, 1011010, 0$)$$
$$\rightarrow (q_2, 011010, 10$)$$
$$\rightarrow (q_2, 11010, 010$)$$
$$\rightarrow (q_2, 1010, 1010$)$$
$$\rightarrow (q_3, 1010, 1010$)$$
$$\rightarrow (q_3, 010, 010$)$$
$$\rightarrow (q_3, 10, 10$)$$
$$\rightarrow (q_3, 10, 10$)$$
$$\rightarrow (q_3, 0, 0$)$$
$$\rightarrow (q_4, \epsilon, \epsilon)$$

---

![[Pasted image 20220217112629.png]]