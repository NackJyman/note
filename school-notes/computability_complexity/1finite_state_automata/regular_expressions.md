# Regular Expressions
#RegularExpresions
###### Lecture 5, Feburary 1st, 2022
---
###### [[fsa_home | unit homepage]]
>
**Definition:**
Let $\Sigma$ be an alphabet. $R$ is a regular expression if $R$ is:
>* $a$ for some $a \in \Sigma$
>* $\epsilon$
>* ∅
>* $(R_1 \cup R_2)$ where $R_1$ and $R_2$ are regular Expressions
>* $(R_1 \circ R_2)$ where $R_1$ and $R_2$ are regular Expressions
>* $R_1^*$ where $R_1$ is a regular Expressions
>
**Definition:**
Let R be a regular expression. The language of R (or the language described by R), $L(R)$, is defined as follows.
>* If $R = a$ for some $a \in \Sigma$, then L(R) = {a}.
>* If $R = \epsilon$, then $L(R)$ = {ε}
>* If $R =$ ∅ , then $L(R)$ = ∅
>* If $R = (R_1 \cup R_2)$ for two regular expressions $R_1$ and $R_2$, then $L(R) = L(R_1) \cup L(R_2)$

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
Conventions:
In a regular expression, Σ can be used as an abbreviation for the ($R_1 ∪ R_2$)
```ad-hint
title: Theorem 
##### "A language is regular if and only if it can be descrived by a Regular expression."
```

```ad-abstract
title: Lemma
###### "If a language is described by a regular expression, then it is regular (there is an NFA that accepts it)"
```
>
**Proof to show regular: **
Assume that R1 and R2 are regular expressions that have been shown regular, and N1 and N2 are NFAs with $L(N1) = L(R1)$ and $L(N2) = L(R2)$. (This is the inductive hypothesis.) We have three inductive steps.
>1. $R = R1 ∪ R2$. Then $L(R) = L(R1) ∪ L(R2)$ by definition, so L(R) is also regular because the regular languages are closed under union. Specifically, we may use the NFA union construction to obtain an NFA N with $L(N) = L(N1) ∪ L(N2) = L(R1) ∪ L(R2) = L(R)$.
2. $R = R1 · R2$. Then $L(R) = L(R1) · L(R2)$ by definition, so $L(R)$ is regular by closure under concatenation. We may use the concatenation construction to obtain an NFA N with $L(N) = L(N1) · L(N2) = L(R1) · L(R2) = L(R)$.
3. $R = R∗1. Then L(R) = L(R1)∗$ by definition, so $L(R)$ is regular by closure under the star operation. We may use the star operation to obtain an NFA $N$ with $L(N) = L(N1)∗ = L(R1)∗ = L(R)$.

---

```ad-abstract
title: Regular Operations on Languages
- **Union:** $A \cup B = \{x | x \in A \text{ or } x \in B \}$.
- **Concatenation:** $A \cdot B = \{xy | x \in A \text{ and } y \in B \}$. Note: Textbook uses $\circ$ rather than $\cdot$
- **Star:** $A^* = \{ x_1 x_2 ... x_k | k \ge 0 \text{ and each } x_i \in A \}$
```

```ad-hint
title: Theorem 
##### "The class of regular languages is closed under the concatenation operation. Meaning if $A_1$ and $A_2$ are regular languages then so is $A_1 \cdot A_2$ or $A_1 \cup A_2$"
```

**Tokens:** Elemental objects in programming language such as variable names and constants, may be described with regular expressions.

Once the syntax of a programming language has been descrived with a regular expresion in terms of its tokens, automatic systems can generate the **lexical analyzer**.

## Constructing a NFA From an Expression

![[Screen Shot 2022-02-09 at 12.45.44 AM.png]]
![[Screen Shot 2022-02-09 at 1.16.21 AM.png]]