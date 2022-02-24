# Equivalence of CFG and PDA
#PDA
###### Lecture 12 (02/17/2022)
---
###### [[pa_home | unit homepage]]

---
## CFG $\rightarrow$ PDA
#### Theorem
###### A language is context-free if and only if it is accepted by some pushdown automaton.

Must show that for every CFG G, there is an equivalent PDA M with L(M) = L(G).

Show that for every PDA M, there is an equivalent CFG G with L(G) = L(M).

### Converting a CFG into a PDA

Consider the grammar $S \rightarrow 0S1 | \epsilon$.

![[Pasted image 20220217113409.png]]

On $q_{\text{loop}}$, we have
- $a, a \rightarrow \epsilon$ for each $a \in \Sigma$
- $\epsilon, A \rightarrow \eta$ for each rule $A \rightarrow \eta$ 

_Simple Construction_

---

## PDA $\rightarrow$ CFG

#### Theorem
###### If a language is accepted by a PDA, then it is context-free.

##### Proof:
Let $A$ be accepted by PDA $M = (Q, \Sigma, \Gamma, \delta, q_0, F)$. We will construct a CFG $G = (V, \Sigma, R, S)$ with L(G) = L(M)

Without loss of generality we assume that M has a unique final state $q_{\text{accept}}$. (That is, $F = \{q_{\text{accept}}\}$) We also assume that the stack is empty when it accepts, and that each operation is a push or a pop, but not both. If a PDA does not satisfy these conditions it can be modified to meet them.

##### Rules:
1. For each $p, q, r, s \in Q, t \in \Gamma$, and $a, b \in \Sigma_\epsilon$, if $(r, t) \in \delta(p,a,\epsilon)$ and $(q,\epsilon) \in \delta(s,b,t)$,

![[Pasted image 20220217114322.png]]

We have the rule $$A_{p,q} \rightarrow aA_{r,s}b$$

2. For each $p,q,r \in Q$, we have the rule: $$A_{p,q} \rightarrow A_{p,r} A_{r,q}$$

3. For each $p \in Q$, we have the rule: $$A_{p,p} \rightarrow \epsilon$$

#### Lemma:
###### $A_{p,q}$ generates exactly the strings $x$ for which $$(p,x,\epsilon) \rightarrow^* (q,\epsilon,\epsilon)$$
that is, all strings that take M from state $p$ with empty stack to quick stack.

There are two directions:
- If $A_{p,q} \Rightarrow^* x$, then $(p,x,\epsilon) \rightarrow^* (q,\epsilon,\epsilon)$
- if $(p,x,\epsilon) \rightarrow^* (q,\epsilon,\epsilon)$, then $A_{p,q} \Rightarrow^* x$

_See book for induction proof._

---

## Examples

![[Pasted image 20220217115027.png]]
![[Pasted image 20220217115055.png]]
![[Pasted image 20220217115112.png]]
![[Pasted image 20220217115122.png]]
![[Pasted image 20220217115137.png]]

---

Next time COntext-free pumping lemma