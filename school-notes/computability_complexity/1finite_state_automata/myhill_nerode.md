# Myhill Nerode Theorem
---
###### [[fsa_home | unit homepage]]

### The Relation $\equiv_M$
let $A \subseteq \Sigma^*$ be a regular language, and let $M = (Q, \Sigma, \delta, q_0, F)$ be a DFA for $A$ with no inaccessible states.

M induces an equivalence relation $\equiv_M$ on $\Sigma^*$ defined by:
$$x \equiv_M y \iff \delta^*(q_0,x) = \delta^*(q_0, y)$$
for all $x, y \in \Sigma^*$ (inputs/strings).
###### Properties:
- Reflexive, Symmetric, and Transitive.
- _Right Congruence:_ for all $x,y \in \Sigma^*$ and $a \in \Sigma$, $$x \equiv_M y \Rightarrow xa \equiv_M ya.$$
- _Refines $A$:_ for all $x,y \in \Sigma^*$, $$x \equiv_M y \Rightarrow (x \in A \iff y \in A).$$
- It is of finite index, has finite many equivalence classes.

### Myhill-Nerode Relations

##### Definition
Let $A \subseteq \Sigma^*$. An equivalence relation $\equiv$ on $\Sigma^*$ is called a _Myhill-Nerode relation_ for $A$ if it has three properties from above, right congruence of finite index that refines $A$.

- We saw that every DFA $M$ for $A$ yields a Myhill-Nerode relation $\equiv_M$ for $A$.
- We will now show that every Myhill-Nerode relation $\equiv$ for $A$ yields a DFA $M_\equiv$ for $A$.
#### $$\text{Myhill-Nerode Relation}\equiv  \text{to DFA } M_\equiv$$

Let $A \subseteq \Sigma^*$ and let $\equiv$ be an arbitrary Myhill-Nerode relation for $A$. 
The $\equiv$-class of a string $x \in \Sigma^*$ is $$[x] = \{y \vert y \equiv x \}.$$
There are finitely many $\equiv$-classes since $\equiv$ has finite index.

We define DFA $M_\equiv = (Q, \Sigma, \delta, q_0, F)$ where:
-	$Q = \{[x] \vert x \in \Sigma^*\}$
-	$q_0 = [\epsilon]$
-	$F = \{[x] \vert x \in A \}$
-	$\delta([x],a) = [xa]$ for all $x \in \Sigma^*$ and $a \in \Sigma$

Because $\equiv$ is a _right congruence_, $\delta$ is well defined. Since $\equiv$ refines $A$, we have $x \in A \iff [x] \in F$ for all $x \in \Sigma^*$

>#### Lemma 
>###### For all $x,y \in \Sigma^*, \delta^*([x],y) = [xy]$.

##### Proof

...

>#### Theorem
>###### $L(M_\equiv) = A$.

##### Proof
...

---
The following lemma states the mappings $M \rightarrow \equiv_M and \equiv \rightarrow M_\equiv$ are inverses of each other, up to isomorphism of automaton.

>#### Lemma
>###### If $\equiv$ is a myhill-Nerode relation for $A$, and if we apply the construction $\equiv \rightarrow M_{\equiv}$ and then apply the construction $M_{\equiv} \rightarrow \equiv_{M_\equiv}$, the relation $\equiv_{M_\equiv}$ is identical to $\equiv$. $$\equiv \rightarrow M_\equiv \rightarrow \equiv_{M_\equiv} = \equiv$$
>###### If M is a DFA for $A$ with no inaccessible states, and if we apply the construction $M \rightarrow \equiv_M$ and then apply the construction $\equiv_M \rightarrow M_{\equiv_M}$, the resulting DFA $M_{\equiv_M}$ is isomorphic to $M$. $$M \rightarrow \equiv_M \rightarrow M_{\equiv_M} \approx M$$

---
### Refining Relations
A relation $\equiv_1$ _refines_ a relation $\equiv_2$ if: $$x \equiv_1 y \Rightarrow x \equiv_2 y.$$

- If $\equiv_1$ and $\equiv_2$ are equivalence relations, this says that all of $\equiv_1$'s equivalence classes are contained in equivalence classes of $\equiv_2$.
- If $\equiv_1$ refines $\equiv_2$, then we say $\equiv_2$ is _coarser_ than $\equiv_1$.

Let $A \subseteq \Sigma^*$. We define an equivalance relation $\equiv_A$ on $\Sigma^*$ by $$x \equiv_A y \iff (\forall z \in \Sigma^*) (xz \in A \iff yz \in A)$$
for all $x,y \in \Sigma^*$

>#### Lemma 
>###### Let $\subseteq \Sigma^*$ The relation $\equiv_A$ is _right congruence refining_ $A$ and is the coarsest such relation on $\Sigma^*$

##### Proof:
...

---
### Myhill-Nerode Theorem

Let $A \subseteq \Sigma^*$. The following are equivalent:
1. $A$ is regular.
2. There exists a Myhill-Nerode relation fro $A$
3. The relation $\equiv_A$ is of finite index

##### Proof:
Given a DFA $M$ for $A$, the construction $M \rightarrow \equiv_M$ yields a Myhill-Nerode relation for $A$.

Let $\equiv$ be a Myhill-Nerode relation for $A$. Then $\equiv$ is a right congruence that refines $A$, so $\equiv_A$ is coarser than $\equiv$ by the Lemma. Since $\equiv$ is of finite index, this implies $\equiv_A$ is also of finite index.

If $\equiv_A$ is of finite index, then it is a Myhill-Nerode relation for $A$ by the Lemma. The construction $\equiv_a \rightarrow M_{\equiv_A}$ produces a DFA for $A$.

---
>#### Corollary
>###### For any regular language A, the DFA $M_{\equiv_A}$ is the minimal DFA for $A$.

##### Proof:
...

---
### Applications and Examples
```ad-example
title:  Prove $A = \{0^n 1^n \vert n \ge 0 \}$ is not regular using Myhill-Nerode Theorem.

 If $k \not = m$, then $0^k \not \equiv_A 0^m$ since $$0^k 1^k \in A \text{ and } 0^m 1^k \not \in A.$$ Therefore $\equiv_A$ has infinitely many equivalence classes, at least one for each $0^k, k \ge 0$. By the Myhill-Nerode Theorem, $A$ is not regular.
$\boxdot$
```

```ad-example
title: Prove $B = \{0^{n^2} \vert n \ge 0 \}$ is not regular using Myhill-Nerode Theorem.

For any $0 \le k < j, 0^{j^2} \not \equiv_B 0^{j^2}$ since $$0^{k^2} 0^{2k+1} \in B \text{ and } 0^{j^2} 0^{2k+1} \not \in B.$$
- $0^{k^2} 0^{2k+1} \in B$ because $k^2 + 2k + 1 = (k + 1)^2$.
- $0^{j^2} 0^{2k+1} \not \in B$ because $j^2 < j^2 + 2k + 1 < j^2 + 2j + 1 = (j + 1)^2$ implies $j^2 + 2*k + 1$ is not a square.

Therefore $\equiv_B$ has infinitely many equivalence classes, so $B$ is not regular by the Myhill-Nerode Theorem. 
$\boxdot$
```

---

Let $A$ be a regular language and let $M = (Q, \Sigma, \delta, q_0, F)$ be the minimal DFA for $A.$ Recall that for every $x,y \in \Sigma^*$, $$x \equiv_A y \text{ if }(\forall z \in \Sigma^*) (xz \in A \iff yz \in A)$$ and $$x \equiv_M y \text{ if } \delta^*(q_0, x) = \delta^*(q_0, y).$$

From the proof of the Myhill-Nerode Theorem, we know that $\equiv_A$ and $\equiv_B$ are the same relation. In other words, the states of $M$ "remember" the equivalence of x with respect to $\equiv_A$:
$$\delta^*(q_0, x) \text{ is essentially } [x]_A.$$

---

#### Finding $\equiv_A$ in a given expression

Let $A = \{ x00 \vert x \in \{0, 1\}^* \}.$
Recall $x \equiv_A y$ if ($\forall z \in \Sigma^*) (xz \in A \iff yz \in A)$.
- What is the $\equiv_A$-equivalent to $\epsilon$?
	- $\begin{array}{rcl} \epsilon \equiv_A 1 & \epsilon \equiv_A 11 & \epsilon \equiv_A 01 \end{array}$
	- $\epsilon \not \equiv_A 0 (\epsilon 0 \not \in A \text{ but } 00 \in A)$
	- $\epsilon \not \equiv_A 00 (\epsilon \not \in A \text{ but } 00 \in A)$
		 $\begin{array}{rcl} [\epsilon]_A & = & \{ \epsilon \} \cup \{x \in \{0,1\}^* \vert x \text{ ends in } 1\} \\  & = & \{x \in \{0,1\}^* \vert x \text{ does not end in } 0 \} \end{array}$
- What is the $\equiv_A$-equivalent to 0?
	- $\begin{array}{rcl} 0 \equiv_A 10 & 0 \equiv_A 1010 & 0 \equiv_A 0010 \end{array}$
	- $0 \not \equiv_A 100 (0 \in A \text{ but } 100 \in A)$
	- $0 \not \equiv_A 1 (00 \not \in A \text{ but } 10 \not \in A)$
		 $\begin{array}{rcl} [0]_A & = & \{x \in \{0,1\}^* \vert x \text{ ends in exactly one } 0\} \end{array}$
- What is the $\equiv_A$-equivalent to 00?
	- $\begin{array}{rcl} 00 \equiv_A 100 & 00 \equiv_A 10100 & 00 \equiv_A 10000 \end{array}$
	- $00 \not \equiv_A 10 (00 \in A \text{ but } 10 \not \in A)$
	- $00 \not \equiv_A 001 (00 \in A \text{ but } 001 \not \in A)$
		 $\begin{array}{rcl} [00]_A & = & \{x \in \{0,1\}^* \vert x \text{ ends in two or more } 0 \text{'s} \} \end{array}$

---

