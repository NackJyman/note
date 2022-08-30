# Regular Expressions
###### Computability and Complexity
##### Lecture 5, Feburary 1st, 2022
#RegularExpresions

---
###### [[fsa_home | unit homepage]]
---

#### Motivation
A regular expression is a way of defining a language. Which is very importnat when building NFA's and DFA's.

##### Examples of a Regular Expressions:

| $\epsilon$      | 0                    | 011              | $(01 \cup 1)$ |
| --------------- | -------------------- | ---------------- | ------------- |
| $(0 \cup 01)^*$ | $(01^* \cup 0^*1)^*$ | $0(0 \cup 1)^*1$ |               |

_Note: $\cup$ is the same as $|$_

---

### Formal Definition
A regular expression is built from alphabet symbols $\epsilon, \emptyset, \cup, \cdot, *,$ and parentheses. Defined formally using induction.

>
Let $\Sigma$ be an alphabet. $R$ is a regular expression if $R$ is:
>* $a$ for some $a \in \Sigma$
>* $\epsilon$
>* $\emptyset$
>* $(R_1 \cup R_2)$ where $R_1$ and $R_2$ are regular Expressions
>* $(R_1 \cdot R_2)$ where $R_1$ and $R_2$ are regular Expressions
>* $(R_1^*)$ where $R_1$ is a regular Expressions
>
Often the parentheses and concatenation are omitted, if the meaning is clear.
>
>---
**Language Described by $R$ Definition ($L(R)$):**
Let $R$ be a regular expression. The language of $R$ (or the language described by $R$), $L(R)$, is defined as follows.
>* If $R = a$ for some $a \in \Sigma$, then $L(R) = \{a\}$.
>* If $R = \epsilon$, then $L(R) = \{ \epsilon \}$
>* If $R = \emptyset$, then $L(R) = \emptyset$
>* If $R = (R_1 \cup R_2)$ for two regular expressions $R_1$ and $R_2$, then 
>$L(R) = L(R_1) \cup L(R_2)$
>* If $R = (R_1 \cdot R_2)$ for two regular expressions $R_1$ and $R_2$, then 
>$L(R) = L(R_1) \cdot L(R_2)$
>* If $R = (R_1^*)$ for some regular expressions $R_1$, then 
>$L(R) = [L(R_1)]^*$.



---

Some identies to ponder:
$$R \cup \emptyset = R$$
Adding empty language to any other language will not change it. (Makes sense, 1+0=1)

$$R \circ \epsilon = R$$
Joining the empty string to any string will not change it.

However, upon exchanging $\emptyset$ and $\epsilon$ in these identies may cause them to fail.

$$R \cup \emptyset \not = R$$
**Ex:** $R = 0, \text{ then } L(R) = {0} \text{ but } L(R \cup \epsilon) = \{0, \epsilon \}$

$$R \circ \epsilon \not = R$$
**Ex:** $R = 0, \text{ then } L(R) = {0} \text{ but } L(R \circ \emptyset) = \emptyset$


---
### Conventions

In a regular expression, $\Sigma$ can be used as an abbreviation for the $\cup$ over all symbols in $\Sigma$. 

For example, if $\Sigma = \{0,1\}$, then $\Sigma$ means $(0 \cup 1)$ in a regular expression.

$$\text{e.g. can write } \Sigma^*010 \Sigma^* \text{ instead of } (0 \cup 1)^*010(0 \cup 1)^*$$

Often we just write $R$ when we formally mean $L(R)$.

$$0^* = \{0^n \vert n \ge 0 \}$$
$$0^*10^* = \{w \in \{0,1\}^* \vert w \text{ has exactly one } 1\}$$


---

>#### Theorem 
>###### "A language is regular if and only if it can be descrived by a Regular expression."

![[reg-exp.svg]]

_Time to explain how to turn a DFA into regular expressions :$_

---
>#### Lemma
>###### "If a language is described by a regular expression, then it is regular (there is an NFA that accepts it)"


##### Proof:
For any regular expression $R$, we will show that $L(R)$ is regular by explaining how to construct an NFA $N$ with $L(N) = L(R)$. The proof is by induction on the structure of regular expressions. 

We have three base cases.

1. $R = a$ for some $a \in \Sigma$. Then $L(R) = \{a\}$, and the following NFA accepts $L(R)$. 

2. $R = \epsilon$. Then $L(R) = \{ \epsilon \}$, and the following NFA accepts $L(R)$. 
3. $R = \emptyset$. Then $L(R) = \emptyset$, and the following NFA accepts $L(R)$.

Assume that $R_1$ and $R_2$ are regular expressions that have been shown regular, and $N_1$ and $N_2$ are NFAs with $L(N_1) = L(R_1)$ and $L(N_2) = L(R_2)$. (This is the inductive hypothesis.) We have three inductive steps.
1. $R = R_1 \cup R_2$. Then $L(R) = L(R_1) \cup L(R_2)$ by definition, so L(R) is also regular because the regular languages are closed under union. Specifically, we may use the NFA union construction to obtain an NFA N with $L(N) = L(N_1) \cup L(N_2) = L(R_1) \cup L(R_2) = L(R)$

3. $R = R_1 \cdot R_2$. Then $L(R) = L(R_1) \cdot L(R_2)$ by definition, so $L(R)$ is regular by closure under concatenation. We may use the concatenation construction to obtain an NFA N with $L(N) = L(N_1) \cdot L(N_2) = L(R_1) \cdot L(R_2) = L(R)$.
4. $R = R^*_1.$ Then $L(R) = L(R_1)^*$ by definition, so $L(R)$ is regular by closure under the star operation. We may use the star operation to obtain an NFA $N$ with $L(N) = L(N_1)^* = L(R_1)^* = L(R)$.

$\boxdot$

---

```ad-example
title: Convert $(01 \cup 10^*)^*$ to an NFA

![[Pasted image 20220306211916.png]]

![[Pasted image 20220306211939.png]]

---

However we can simiplify this NFA.

![[Pasted image 20220306212023.png]]

![[Pasted image 20220306212045.png]]

![[Pasted image 20220306212106.png]]

```

###### _End of 05_regexp_

---

### Converting a DFA to a Regular Expression

Let $M$ be a DFA
1. Creation of GNFA (Generalized NFA):
	- Add a new initial state $s$ and a new final state $f$.
	- Put an $\epsilon$-transition from $s$ to the old initial state.
	- Put $\epsilon$-transition from all old final states to $f$.
	- Replace transitions with multiple labels by $\cup$ regular expresions:
	- In a GNFA, all edges are labeled by regular expressions. Throughour the algorithm we will maintain a GNFA.
2. Select a state $q_{rip}$ other than $s$ or $f$ to remove. For every pair of states $q_i$, $q_j$ where there is a transition from $q_i$ to $q_{rip}$ and a transition from $q_{rip}$ to $q_j$, do the following.

![[Pasted image 20220307181207.png]]

3. Repeat step 2 (removing states) until only $s$ and $f$ remain.
4. The regular expression on the transition from $s$ to $f$ is equivalent to the original DFA.

#### Special Cases of Elimination Rule

![[Pasted image 20220307181331.png]]

![[Pasted image 20220307181344.png]]

```ad-example
title: Example DFA Minimization

![[Pasted image 20220307181647.png]]

```

---
### Equivalent Definitions of Regularity

> #### Theorem
> ###### Let $A$ be any language. The following are equivalent.
> ###### $\cdot$ $A$ is regular
> ###### $\cdot$ $A = L(M)$ for some DFA $M$.
> ###### $\cdot$ $A = L(N)$ for some NFA $N$.
> ###### $\cdot$ $A = L(N')$ for some NFA $N'$ without $\epsilon$-transitions.
> ###### $\cdot$ $A = L(R)$ for some regular expression $R$.
---
```ad-abstract
title: Regular Operations on Languages
- **Union:** $A \cup B = \{x | x \in A \text{ or } x \in B \}$.
- **Concatenation:** $A \cdot B = \{xy | x \in A \text{ and } y \in B \}$. Note: Textbook uses $\circ$ rather than $\cdot$
- **Star:** $A^* = \{ x_1 x_2 ... x_k | k \ge 0 \text{ and each } x_i \in A \}$
```

>#### Theorem 
>###### "The class of regular languages is closed under the concatenation operation. Meaning if $A_1$ and $A_2$ are regular languages then so is $A_1 \cdot A_2$ or $A_1 \cup A_2$"

**Tokens:** Elemental objects in programming language such as variable names and constants, may be described with regular expressions.

Once the syntax of a programming language has been descrived with a regular expresion in terms of its tokens, automatic systems can generate the **lexical analyzer**.

## Constructing a NFA From an Expression

![[Screen Shot 2022-02-09 at 12.45.44 AM.png]]
![[Screen Shot 2022-02-09 at 1.16.21 AM.png]]