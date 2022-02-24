# Nonregular Languages and God (Pumping Lemma)
#RegularProof #PumpingLemma 
###### Lecture 10 (Feburary 8, 2022)
---
###### [[fsa_home | unit homepage]]
---
### Summary of module:

###### Prove Nonregularity using the Pumping Lemma

---

#### Introduction

Is $A = \{ 0^n 1^n \ n \ge 0\}$ regular?

Suppose that $A$ is accepted by a DFA $M = (Q, \Sigma, \delta, q_0, F)$. Let $p = |Q|$ be the number of states in $M$. Let $n = p + 1$, and let $w = 0^n1^n$. Then $M$ accepts $w$.

Let's look at the sequence of states that $M$ visits on $w$. Let $q_i$ be the state $M$ is in after $0^i$.

![[ex1nreg.svg]]

Since $n > p, M$ must visit some state twice while reading the 0's.
$\rightarrow$ For some $i, j$ with $1 \le i < j \le n$, we must have $q_i = q_j$

$$\delta^* (q_0, 0^i) = q_i --------- \delta^*(q_0, 0^j) = q_i$$
$$\delta^* (q_0, 0^{j-i}) = q_i -------\delta^*(q_i, 0^{n-j}1^n) \in F$$

![[Pasted image 20220217180919.png]]

We must also have: 
$$ 0^i (0^{j-i}) (0^{j-i}) 0^{n-j} 1^n = 0^{n+(j-i)} 1^n$$
as an accepted state. Therefore $M$ does not accept $A$. 

We have a contradiction.

---

### Pumping Lemma 

![[Screen Shot 2022-02-08 at 11.21.35 AM.png]]

```ad-abstract
title: Pumping Lemma
if $A$ is a regular language, then there is a number $p$ (the pumping constant) such that got every $w \in A$ with $|w| \ge p$, $w$ can ve divided into three pieces $w + x y z$ satisfying the following conditions:
1. For all $i \ge 0, x y^i z \in A$
2. $|y| > 0$
3. $|x y| \le p$ 
```

###### Comments:
- Condition 1 says that $xz, xyz, xyyz, xyyyz, \text{ . . are all in } A$. 
- Condition 2 says that $y \not = \epsilon$. It is possible for $x$ and $z$ to be $\epsilon$. 
- Condition 3 says the combined length of $x$ and $y$ is at most $p$.

##### Proof:
- Let $M = (Q, \Sigma, \delta, q_0, F)$ be a DFA that recognizes $A$. Let $p = |Q|$ be the number of states in $M$. 
- Let $w \in A$ have length $\vert w \vert = n \ge p$. For any $1 \le i \le j \le n$, let $w[i \text{ . . } j]$ be the substring of the $i^{\text{th}}$ through $j^{\text{th}}$ symbols of $w$.
- For each $1 \le i \le n$, let $q_i = \delta^*(q_0, w[1 \text{ . . } i])$. 

Then $q_n \in F$ since $w \in A$. Since $n + 1 > p$ and there are only $p$ states in $Q$, some state must be repeated in the sequence:
$$q_0,q_1, \text{ . . } ,q_n$$
of $n + 1$ states. Let $0 \le i < j \le p$ such that $q_i = q_j$. We now break $w$ as $w = xyz$ where:
$$\begin{array}{rcl}
   x & = & w[1 \text{ . . } i ]
\\ y & = & w[i+1 \text{ . . } j ]
\\ z & = & w[j + 1 \text{ . . } n ]
\end{array}$$
Since $i < j$, $\vert y \vert > 0$, and since $j \ge p$, $\vert x y \vert \le p$. Therefore conditions (2 & 3) are satisfied. Then:
$$\begin{array}{rcl}
   \delta^*(q_0,x) & = & q_i,
\\ \delta^*(q_i,y) & = & q_i & = & q_j,
\\ \delta^*(q_i,z) & = & q_n
\end{array}$$
We also have $\delta^*(q_i,y^k) = q_i$ for all $k$ (by induction). Therefore, for any $k \ge 0$,
$$\delta^*(q_i,y^k) = \delta^*(\underline{\delta^*(q_0, x)}, y^k) = q_i$$
so,
$$\delta^*(q_i,y^k) = \delta^*(\underline{\delta^*(q_0, xy^k)}, z) = q_n \in F$$

and $xy^kz \in A$
$^{\scriptstyle{\text{Underlined Segments are equal to: } q_i}}$

---
### Using the Pumping Lemma to Prove Nonregularity
1. Assume that $A$ is regular, and let $p$ be the pumping constant for $A$.
2. Choose a string $w \in A$ with $\vert w \vert \ge p$. ($w$ needs to be in an accepting state)
3. Let $xyz = w$ be any way of breaking $w$ into three pieces such that $y \not = \epsilon$ and $\vert x y \vert \le p$.
4. _Find_ some $i > 0$ such that $xy^i z \not \in A$.
5. Conclude that $A$ is not regular.

Note: We choose $w$ and $i$ in steps 2 and 4. We do $\underline{not}$ getto choose $p$ and $x,y,z$ in steps 1 and 3. We must argue for any possibility of $p$ and $x,y,z.$
#### Contrapositive's Revenge
```ad-abstract
title: Contrapositive Definition


| Implication	    | Contrapositive                       |
|-----------------|--------------------------------------|
|$$p \implies q$$ |$$\urcorner  q \implies \urcorner p$$ |

```

---
### Examples

```ad-example
title:Prove: $A = \{ 0^n 1^n \vert n \ge 0\}$ is not regular

Proof: 
Suppose by contradiction that A is a regular language, and let $p$ be the pumping constant for $A$. Let $w = 0^p 1^p$. Then $w$ can be broken into $w = x y z$ where $|x y| \le p$ and $|y| > 0$ such that $x y^i z \in A$ for all $i$.

Then we know that $x = 0^k$, $y = 0^l$ and $z = 0^{p-k-l} 1^p$ for some $k \ge 0 \text{, and } l > 0$.

Let $i = 2$ Then.
$$x y^2 z = 0^k 0^{2l} 0^{p-k-l} 1^p$$
$$ = 0^{p+1} 1^p \in A.$$

But since $l > 0$, $p + 1 \not = p$, so $x y^2 z$ should not be in $A$. This is a contradiction, so $A$ must not be regular. $\boxdot$
```

```ad-example
title: Prove $A = \{ 0^{n^2} | n \ge 0 \}$ is not regular:

Suppose that $A$ is regular and let $p$ be the pumping constant for $A$. Let $w = 0^{p^2}$. Then $|w| \ge p$ and $w \in A$. Let $w = xyz$ where $|xy| \le p$ and $|y| > 0$.

Then we know that $x = 0^k$, $y = 0^l$, and $z = 0^{p^2-k-l}$ for some $k \ge 0$, $l > 0$. Let $i = 2$. 
We claim that 

$$\begin{array}{rcl} xy^2 z & = & 0^k 0^{2l} 0^{p^2-k-l} \\ & = & 0^{p^2+l} \not \in  A. \end{array}$$ 

We must show that $p^2 + 1$ is not a perfect square; by showing it lies strictly between two consecutive perfect squares. we have
$$\begin{array}{rcl} (p+1)^2  & = & p^2 + 2p + 1 \\ & > &p^2 +p \\ & \ge & p^2 + 1 \\ & > & p^2, \end{array}$$
so $$p^2 < p^2 + 1 < (p+1)^2.$$ and $p^2 + 1$ is not a perfect square. Therefore $A$ is not regular by the pumping lemma. $\boxdot$

```

```ad-example
title: Prove $E = \{ 0^i 1^j \vert i \ge j \}$ is not regular:

Suppose that E is regular, and let $p$ be the pumping constant for $E$. Let $w = 0^p 1^p$. Let $w = xyz$ where $\vert x y \vert \le p$ and $\vert y \vert > 0$. Then we know that $x = 0^k$, $y = 0^l$, and $z = 0^{p-k-l} 1^p$ for some $k \ge 0$, $l > 0$.

Let $i = 0$. Then $$\begin{array}{rcl} xy^0 z & = & 0^{p-k-l} 1^p \\ & = & 0^{p-l} 1^p \not \in E \end{array}$$
because $l>0$. Therefore $E$ is not regular by the pumping lemma. $\boxdot$
```

```ad-example
title: Prove $B = \{ vv | v \in \{0,1\}^* \}$ is not regular:

Suppose $B$ is regular, and let $p$ be the pumping constant for $B$. 
Let $w = 0^p10^p1$. Then $w \in B$ and $|w| \ge p$. Let $xyz = w$ such that $|xy| \le p$ and $|y| > 0$. 
We must have have $x = 0^k$, $y = 0^l$, and $z = 0^{p-k-l}10p1$ for some $k \ge 0$, $l > 0$. Then $$xy^2 z = 0^{p+l} 10^p 1 \not \in B,$$
so $B$ is not regular by the pumping lemma. $\boxdot$
```

#### We can also use closure properties to prove nonregularity.

```ad-example
title: Prove $A = \{ w \in \{0,1\}^* | \#_0(w) = \#_1(w) \}$ is not regular:

Suppose $A$ is regular. Then since the regular languages are closed under intersection, $$A \cap 0^* 1^*= \{0^n 1^n \vert n \ge 0 \}$$ is also regular. This is a contradiction since we already proved $\{0^n 1^n \vert n \ge 0 \}$ is not regular. $\boxdot$
```

```ad-example
title: Prove $\textbf{ADD} = \left\{ \begin{array}{rcl} a + b = c & \vert & a,b,c \text{ are the binary integers } \\ & \vert &c \text{ is the sum of } a \text{ and } b \end{array}\right\}$ is not regular:

Suppose $\textbf{ADD}$ is regular. and let $p$ be the pumping constant for $\textbf{ADD}$. Let $w$ be the string "$1^p + 1 = 10^p$". Then $w \in \textbf{ADD}$ and $\vert w \vert \ge p.$ Let $xyz = w$ such that $\vert y \vert > 0.$

We must have $x = 1^k$,$y = 1^l$, and $z$ is the string "$1^{p-k-l} + 1 = 10^p$". Then:
$$\begin{array}{rcl} xy^2 & = & 1^{p+1} + 1 = 10^p & \not \in \textbf{ADD} \end{array}$$
because $1^{p+1} + 1 = 10^{p+1} \not = 10^p$ since $l > 0$. $\boxdot$
```
#### Exceptions to the Pumping Lemma Exist

I could give you an example but i don wanna.

```ad-example
title: Prove $F = \{ a^i b^j c^k | i, k, k, \ge 0$ and if $i = 1$ then $j = k \}$ is a not regular:

Suppose $F$ is regular. Then 
$$F \cap ab^*c^* = \{ab^n c^n | n \ge 0 \}$$

is also regular by closure under intersection. This language is not regular by a proof similar ot the one we wrote for $\{0^n 1^n | n \ge 0 \}$. This is a contraction so $F$ is not regular.

```ad-caution
title: Caution: $F$ satisfies the conclusion of the Pumping Lemma

![[Screenshot_2022-02-18_03-17-53.png]]
```



```ad-caution
title: Caution Unrelated Sexy Example: piecewise in .md
$f(z) = \left\{ \begin{array}{rcl} \overline{\overline{z^2}+\cos z} & \mbox{for} & |z|<3 \\ 0 & \mbox{for} & 3\leq|z|\leq5 \\ \sin\overline{z} & \mbox{for} & |z|>5 \end{array}\right.$
---
$_*\scriptscriptstyle \textbf{Found in "symbols.pdf"}$
```