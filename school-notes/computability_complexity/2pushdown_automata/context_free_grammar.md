# Context-Free Grammars
#CFG
###### Lecture 11 (02/15/2022)
---
###### [[pa_home | unit homepage]]

Chapter 2 of textbook, Unit 2 of cource
```ad-hint
title: HW3 Notes/Mentions
Look at Myhill-Nerode Theorem
```

---

Two main modules of this unit
- Context-Free Grammars (CFG)
- Pushdown Automata (PDA)
	- Manipulates a Stack

$$ S \rightarrow LC | AR$$
$$ L \rightarrow aLb | \epsilon$$
$$ R \rightarrow bRc | \epsilon$$
$$ A \rightarrow Aa | \epsilon$$
$$ C \rightarrow Cc | \epsilon$$

Capitals are typically varables

```ad-example
title: Example CFG
$$A \rightarrow 0A1$$
$$A \rightarrow B$$
$$B \rightarrow \#$$

$A, B$ are variables or nonterminals
$A$ is the start variable
$0,1, \#$ are terminals
$X \rightarrow Y$ is a substitution rule
```

#### Formal Definition
A context-free grammar (CFG) is a 4-tuple G = (V, $\Sigma$, R, S) where

- V is a finite set called the variables (Usually V consists of capital letters)
- $\Sigma$ is a finite set, disjoint from V, called the terminals
- R is a finite set of Rules, with each rule begin in the form $$R \rightarrow \omega$$ where $R \in V$ and $\omega \in (V \cup \Sigma)^*$
- S is the start variable

```ad-abstract
title: Aside: Context-Sensitive Grammars (CSG)
More complicated and powerful than Context-Free grammars but not covered in the scope of this course,
$$00A1 \rightarrow 001B1$$
```

---
### Examples
The language of G is $$L(G) = \{w \in \Sigma^* | S \Rightarrow ^* w\}$$

...

---
Can be used to describe syntax of a programming language using something like
$$ S \rightarrow (S) | SS | \epsilon$$
Producing things like
- $S \Rightarrow (S) \Rightarrow ()$
- $S \Rightarrow (S) \Rightarrow (SS) \Rightarrow (()S) \Rightarrow (()())$
- $S \Rightarrow^* (()(()()))()$

---
$$E \rightarrow E + T \vert T$$
$$T \rightarrow T \times F \vert F$$
$$F \rightarrow (E) \vert N$$
$$N \rightarrow NN \vert 0 \vert 1 \vert 2 \vert 3 \vert 4 \vert 5 \vert 6 \vert 7 \vert 8 \vert 9$$

This example can generate most arithemtric equations:
- ...
- ...
----

$B = \{ a^n b^m c^l \vert n = m \text{ or } m = l \}$
$$S \rightarrow LC \vert AR$$
$$L \rightarrow aLc \vert \epsilon$$
$$R \rightarrow bRc \vert \epsilon$$
$$A \rightarrow Aa \vert \epsilon$$
$$C \rightarrow Cc \vert \epsilon$$

...

---
```ad-hint
title: Theorem
#### $REF \subseteq CFL$. That is, every regular language is context-free.
```

###### Proof:
Let $A \in REG$, and let $M = (Q, \Sigma, \delta, q_0, F)$ be a DFA for A. For each state $q \in Q$, we make a variable $T_q$. Formally, 
$$V = \{ T_q | q \in Q\}$$
The start variable is $T_{q_0}$. We add a rule $$T_q \rightarrow a T_r$$

if $\delta(q,a) = r$ is a transition in the DFA. Also, add $T_q \rightarrow \epsilon$ for each final state $q \in F$. Formally the set of rules is $$R = \{ T_q \rightarrow a T_{\delta(q,a)} \vert q \in Q \text{ and } a \in \Sigma \}$$

Our grammar is $G = (V, \Sigma, R, T_{q_0})$. Then $L(G) = L(M) = A$. $\boxdot$

---
