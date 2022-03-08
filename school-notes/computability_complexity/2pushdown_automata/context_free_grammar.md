# Context-Free Grammars
##### Computability and Complexity
###### Feburary 15, 2022
#CFG

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

$$S \rightarrow LC \ | \ AR$$
$$L \rightarrow aLb \ | \ \epsilon$$
$$R \rightarrow bRc \ | \ \epsilon$$
$$A \rightarrow Aa \ | \ \epsilon$$
$$C \rightarrow Cc \ | \ \epsilon$$

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

We begin with the start variable and apply substitutions to _derive_ strings.

$$\begin{array}{rcl} A & \Rightarrow & 0A1 \\
& \Rightarrow & 00A11 \\
& \Rightarrow & 000A111 \\
& \Rightarrow & 000B111 \\
& \Rightarrow & 000\#111 \\ \end{array}$$

The _Language_ L(G) of G is the set of all strings tht can be derived from it. In this case, $$L(G) = \{ 0^n \# 1^n \vert n \ge 0\}.$$

---

>#### Formal Definition
>A context-free grammar (CFG) is a 4-tuple $G$ = $(V, \Sigma, R, S)$ where
>- $V$ is a finite set called the variables (Usually $V$ consists of capital letters)
>- $\Sigma$ is a finite set, disjoint from $V$, called the terminals
>- $R$ is a finite set of Rules, with each rule begin in the form $$R \rightarrow \omega$$ where $R \in V$ and $\omega \in (V \cup \Sigma)^*$
>- $S$ is the start variable

```ad-abstract
title: Aside: Context-Sensitive Grammars (CSG)
More complicated and powerful than Context-Free grammars but not covered in the scope of this course,
$$00A1 \rightarrow 001B1$$
```

---
$G:$
$\begin{array}{rcl} A & \rightarrow & 0A1 \\A & \rightarrow & B \\B & \rightarrow & \# \end{array}$

In this example, $G = (V, \Sigma, R, A)$ where:
- $V = \{A, B\}$
- $\Sigma = \{0,1,\#\}$
- $R = \{A \rightarrow 0A1, A \rightarrow B, B \rightarrow \# \}$
- $A$ is the start variable

Let $G = (V, \Sigma, R, S)$ be a CFG. If $u,v,w \in (V \cup \Sigma)^*$ are strings of variables and terminals and $A \rightarrow w$ is a rule in the grammar, then we say $uAv$ yields $uwv$, written $$uAv \Rightarrow uwv.$$

We write $u \Rightarrow^*v v$ if
1. $u = v$ or
2. if a sequence $u_1, . \ . \ , u_k$ exists for $k \ge 0$ such that $$u \Rightarrow u_1 \Rightarrow u_2 \Rightarrow . \ . \ \Rightarrow u_k \Rightarrow v.$$

The language of G is $$L(G) = \{w \in \Sigma^* | S \Rightarrow ^* w\}$$

>#### Definition
>###### If $A = L(G)$ for some context-free grammar, then $A$ is a context-free language. We let CFL be the class of all context-free languages.

---

>#### Theorem
>###### $REF \subseteq CFL$. That is, every regular language is context-free.

##### Proof:
Let $A \in REG$, and let $M = (Q, \Sigma, \delta, q_0, F)$ be a DFA for A. For each state $q \in Q$, we make a variable $T_q$. Formally, 
$$V = \{ T_q | q \in Q\}$$
The start variable is $T_{q_0}$. We add a rule $$T_q \rightarrow a T_r$$

if $\delta(q,a) = r$ is a transition in the DFA. Also, add $T_q \rightarrow \epsilon$ for each final state $q \in F$. Formally the set of rules is 
$$\begin{array}{rcl} R & = & \{ T_q \rightarrow a T_{\delta(q,a)} \ | \ q \in Q \text{ and } a \in \Sigma \} \\ & & \cup \ \{T_q \rightarrow \epsilon \ | \ q \in F \}. \end{array}$$

Our grammar is $G = (V, \Sigma, R, T_{q_0})$. Then $L(G) = L(M) = A$. $\boxdot$

---

### Examples

Often we use a compact form for specifying a grammar:
$$S \rightarrow 0S1 \ \vert \ \epsilon$$
Here the explicit meaning is
- $V = \{ S \}$
- $\Sigma = \{ 0,1\}$
- $R = \{S \rightarrow 0S1, \ S \rightarrow \epsilon \}$
- $S$ is the start variable

The language of this grammar is $$\{0^n1^n \ \vert \ n \ge 0\}$$

---
```ad-example
title: $CFG$ to describe properly nested parentheses.
$$ S \rightarrow (S) | SS | \epsilon$$

---
This grammar generates all strings of properly nested parentheses.
- $S \Rightarrow (S) \Rightarrow ()$
- $S \Rightarrow (S) \Rightarrow (SS) \Rightarrow (()S) \Rightarrow (()())$
- $S \Rightarrow^* (()(()()))()$

$S \Rightarrow^* (()(()()))()$ because 
$$\begin{array}{rcl}S & \Rightarrow & SS \\
& \Rightarrow & (S)S \\
& \Rightarrow & (SS)S \\
& \Rightarrow & ((S)S)S \\
& \Rightarrow & (()S)S \\
& \Rightarrow & (()(S))S \\
& \Rightarrow & (()(SS))S \\
& \Rightarrow & (()((S)S))S \\
& \Rightarrow & (()(()S))S \\
& \Rightarrow & (()(()(S)))S \\
& \Rightarrow & (()(()()))S \\
& \Rightarrow & (()(()()))(S) \\
& \Rightarrow & (()(()()))()\end{array}$$

This grammar be used to describe syntax of a programming language.
```

```ad-example
title: $CFG$ to describe most arithmetric equations:
$$\begin{array}{rcl}
E & \rightarrow & E + T \ \vert \  T \\
T & \rightarrow & T \times F \ \vert \ F\\
F & \rightarrow & (E) \ \vert \ N\\
N & \rightarrow & NN \ \vert \ 0 \ \vert \ 1 \ \vert \ 2 \ \vert \ 3 \ \vert \ 4 \ \vert \ 5 \ \vert \ 6 \ \vert \ 7 \ \vert \ 8 \ \vert \ 9
\end{array}$$
---
This example can generate most arithemtric equations:
$$(12 + 8) \times 66$$
$$2020 + (4200 \times 30)$$
$$87 \times ((33 + ((12 + 7) \times 99)) + 47)$$
```

```ad-example
title: $CFG$ for $A = \{a^n b^m \ \vert \ n < m \}$
$A = \{a^n b^m \ \vert \ n < m \}$
$$ S \rightarrow aSb \ \vert \ Sb \ \vert \ b$$

---

$$\begin{array}{rcl}
S & \Rightarrow & aSb & & S & \Rightarrow & aSb \\
& \Rightarrow & aaSbb & & & \Rightarrow & aaSbb \\
& \Rightarrow & aaaSbbb & & & \Rightarrow & aaaSbbb \\
& \Rightarrow & aaaaSbbbb & & & \Rightarrow & aaaSbbbb \\
& \Rightarrow & aaaabbbbb = a^4 b^5 & & & \Rightarrow & aaaSbbbbb \\
& & & & & \Rightarrow & aaabbbbbb = a^3 b^6 \end{array}$$

Where these two columns are possible strings to come out of A.
```

```ad-example
title: $CFG$ for $B = \{ a^n b^m c^l \ | \ n = m \text{ or } m = l \}$
$B = \{ a^n b^m c^l \ | \ n = m \text{ or } m = l \}$
$$S \rightarrow LC \ | \ AR$$
$$L \rightarrow aLc \ | \ \epsilon$$
$$R \rightarrow bRc \ | \ \epsilon$$
$$A \rightarrow Aa \ | \ \epsilon$$
$$C \rightarrow Cc \ | \ \epsilon$$

---

$$\begin{array}{rcl}
S & \Rightarrow & LC & & S & \Rightarrow & AR \\
& \Rightarrow & aLbC & & & \Rightarrow & AR \\
& \Rightarrow & aaLbbC & & & \Rightarrow & AbRc \\
& \Rightarrow & aaaLbbbC & & & \Rightarrow & AbbRcc \\
& \Rightarrow & aaabbbC & & & \Rightarrow & Abbcc \\
& \Rightarrow & aaabbbCc & & & \Rightarrow & Aabbcc \\
& \Rightarrow & aaabbbCcc & & & \Rightarrow & Aaabbcc \\
& \Rightarrow & aaabbbcc = a^3 b^3 c^2 & & & \Rightarrow & Aaaabbcc \\
& & & & & \Rightarrow & Aaaaabbcc \\
& & & & & \Rightarrow & aaaabbcc = a^4 b^2 c^2 \end{array}$$
```

---

![[Pasted image 20220223185847.png]]
$_{Too \ lazy \ to \ make \ proper \ md \ example \ sorry.}$