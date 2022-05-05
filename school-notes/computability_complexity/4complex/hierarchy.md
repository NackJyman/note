# Hierarchy Theorems
##### Computability & Complexity
---

Missed class, alse create module for space.

Cover slides 0-21 out of 25_hierarchy here.

### Time and Space Constructibility

>#### Definition
>###### A function $t: N \rightarrow N$ is time constructible if the function
>###### $f_t : \{0,1\}^* \rightarrow \{0,1\}^*$, $$f_t(x) = \text{ binary representation of } t(|x|),$$
>###### is computable by a $O(t(n))$-time-bouded TM

...

### Efficient Universal Turing Machine

There is a 3-tape TM $U$ with the following properties:



---
>### Space Hierarchy Theorem
>###### Let $s_1, s_2 \ : \ N \rightarrow N$ have the following properties:
>- ###### $s_1(n) = o(s_2(n))$ and
>- ###### $s_2$ is space-constructible, and
>- ###### $s_1(n) = \Omega(log \ n)$
>###### Then DSPACE$(s_1) \subseteq$ DSPACE$(s_2)$

##### Proof:

Analogous to the Time Hierarchy Theorem.
$\boxdot$

---

Important consequence of the time and space hierarchy theorems are the following.

>#### Corollary
>###### $P \subsetneq E \subsetneq EXP$ and $L \subsetneq PSPACE \subsetneq ESPACE \subsetneq EXPSPACE$

##### Proof:

...

---

>![[Screen Shot 2022-04-12 at 11.15.57 AM.png]]

---

>### Nondeterministic Time Hierachy Theorem (Cook, 1983)
>###### Let $t_1, t_2 \ : \ N \rightarrow N$ have the following properties:
>- ###### $t_1(n+1) = o(t_2(n))$ and
>- ###### $t_1$ and $t_2$ are time-constructible, 
>- ###### and $t_1(n) = \Omega(log \ n)$
>###### Then NTIME$(t_1(n)) \subsetneq$ NTIME$(t_2(n))$

##### Proof:

This proof is due to Fortnow and Santhanam (2011). Let $N_1, N_2, ...$ be an enumeration of all multirape NTMs that run in time $t_1(n)$. Define an NTM $D$ that does the following

>![[Screen Shot 2022-04-12 at 11.19.54 AM.png]]

Let $A = L(D)$. There is a universal two-tape NTM that simulates any $t(n)$-time NTM in $O(t(n))$ time. Therefore 

$$A \in NTIME(t_2(n))$$

We now show that $A \not \in NTIME(t_1(n))$. Let $N_i$ be any $t_1(n)$-time NTM and suppose for sake of contradiction that $L(N_i) = A$.

- Choose $n_0$ so that $N_i$ runs in time $\le t_2(n)$ for all $n \ge n_0$.
- We have 

$$\begin{array}{rcl}
N_i(1^i01^{n_0}0) \text{ accepts } & \iff & D(1^i01^{n_0}0) \text{ accepts} \\
& \iff & N_i(1^i01^{n_0}00) \text{ and } N_i(1^i01^{n_0}01) \\ 
&&\text{ accept}
\end{array}$$

- Continuing by induction, we have ...

___Slide 50___...

---

Analogous to $E$ and $EXP$, define the nondeterministic exponential-time complexity classes

$$NE = \bigcup^\inf_{k = 0} NTIME(2^{kn})$$

and

$$NEXP = \bigcup^\inf_{k = 0} NTIME(2^{n^k})$$

>#### Corollary
>###### $NP \subsetneq NE \subsetneq NEXP$.

##### Proof: 

...

---

>![[Screen Shot 2022-04-12 at 11.30.39 AM.png]]

### End