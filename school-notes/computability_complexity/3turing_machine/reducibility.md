# Reducibility

###### Computability and Complexity

---
HW Note:

Prooving Decidable:
1. $A \in CFL$ and $B \in REG \Rightarrow (A \cap B) \in CFL$
2. $E_{CFG}$ is decidable
---
$L(M) = \Sigma^* M$ accepts every string, similar to $E_{DFA}$
#### Mapping Reducibility

A function $f : \Sigma^* \rightarrow \Sigma^*$ is a _computable function_ if some Turning machine $M$, on every input $w$ halts with just f(w) on its tape.

>#### Definition
>###### Language $A$ is _mapping reducible_ to language $B$, written $A \ge_M B$, id there is a computable function $f : \Sigma^* \rightarrow \Sigma^*$, where for every $w$, $$w \in A \iff f(w) \in B$$
>###### The function $f$ is called a _reduction_ of $A$ to $B$.

The reduction maps positive instances to positive instances: 
$$w \in A \Rightarrow f (w) \in B $$
and negative instance to negative instances: 
$$w \not \in A \Rightarrow f (w) \not \in B.$$

The reduction converts questions about membership in $A$ to questions about membership in $B$. 

We can combine a reduction with an algorithm that decides $B$ to obtain an algorithm that decides $A$.

---

>#### Theorem
>###### If $A \le_m B$ and $B$ is decidable, then $A$ is decidable.

##### Proof:

Let $f$ be a mapping reduction of $A$ to $B$ and let $M$ be a decider for $B$. We describe a decider $N$ for $A$. 

N: On input w: 
1. Compute $f(w)$.
2. Run $M$ on input $f(w)$. 
	- If $M$ accepts $f(w)$, accept. 
	- If $M$ rejects $f(w)$, reject. 

We claim that $N$ decides $A$. 
- If $w \in A$, then $f(w) \in B$ because $f$ reduces $A$ to $B$. Therefore $M$ accepts $f(w)$ and $N$ accepts $w$. 
- If $w \not \in A$, then $f(w) \not \in B$ because $f$ reduces $A$ to $B$. Therefore $M$ rejects $f(w)$ and $N$ rejects $w$.

---

>#### Corollary
>###### If $A \le_m B$ and $A$ is undedicable, then $B$ is undecidable.
>###### $$B \text{ decidable } \Rightarrow A \text{ decidable }$$
>###### $$A \text{ undecidable } \Rightarrow B \text{ undecidable }$$

##### Proof:
Assume the hypothesis and suppose that $B$ is decidable. Then A is decidable by the theorem above, a contradiction. $\boxdot$

---

### Relating back to the Halting Problem

The _halting problem_ for TMs:

$$HALT_{TM} = \{ \langle M, w \rangle \ | \ M \text{ is a TM that halts input string } w \}$$

>#### Theorem
>###### $HALT_{TM}$ is undecidable.

The following Turing machine $F$ computes a reduction $f$. 
$F$: On input $\langle M,w \rangle$: 
1. Construct the following machine $M'$ 
	-	$M'$ On input $x$: 
		1. Run $M$ on $x$. 
		2. If $M$ accepts, accept.
		3. If $M$ rejects, enter an infinite loop. 
2. Output $\langle M,w \rangle$. 
 
We claim that $f$ is a mapping reduction of $A_{TM}$ to $HALT_{TM}$

$$\begin{array}{rcl} 
\langle M,w \rangle \in A_{TM} & \Rightarrow & M \text{ accepts } w \\
& \Rightarrow & M' \text{ accepts } w \\ 
& \Rightarrow & M' \text{ halts on } w \\
& \Rightarrow & \langle M',w \rangle \in HALT_{TM}
\end{array}$$

If $\langle M,w \rangle \not \in A_{TM}$, then either $M$ rejects $w$ or $M$ does not halt on $w$. We consider these two cases separately. 

$$\begin{array}{rcl}
M \text{ rejects }w & \Rightarrow & M' \text{ on }w \text{ goes into an infinite loop in step 3} \\ 
& \Rightarrow & M' \text{ does not halt on } w\\
& \Rightarrow & \langle M',w \rangle \not \in HALT_{TM}
\end{array}$$

$$\begin{array}{rcl}
M \text{ does not halt on }w & \Rightarrow & M' \text{ simulates } M \text{ on } w \text{ forever in step 1}\\
& \Rightarrow & M'\text{ does not halt on }w\\
& \Rightarrow & \langle M',w \rangle \not \in HALT_{TM}\\
\end{array}$$

Therefore $\langle M,w \rangle \in A_{TM} \Rightarrow \langle M' ,w \rangle \in HALT_{TM}$.

We have proved 

$$\langle M,w \rangle \in A_{TM} \iff f(M,w) = \langle M',w \rangle \in HALT_{TM},$$ 

so $A_{TM} \le_m HALT_{TM}$ via $f$. Since $A_{TM}$ is undecidable, it follows that $HALT_{TM}$ is undecidable.

$\boxdot$

---

### Regularity Problem for TMs

$$REGULAR_{TM} = \{ \langle M \rangle \ | \ M \text{ is a TM and } L(M) \text{ is a regulat language}\}.$$

>#### Theorem
>###### $REGULAR_{TM}$ is undecidable.

##### Proof:

We will show that $A_{TM} \le_m REGULAR_{TM}$. Given a TM $M$ and a string $w$, define a TM $M_{(w)}$ as follows: 
$M_{(w)}$ : On input $x$: 
1. If $x$ has the form $0^n 1^n$ , accept. 
2. If $x$ does not have this form, run $M$ on input $w$ and accept if $M$ accepts $w$. 

- If $M$ accepts $w$, then $L(M_{(w)}) = \Sigma^*$, so $L(M_{(w)})$ is regular. 
- If $M$ does not accept $w$, then $L(M_{(w)}) = \{0^n 1^n \ | \ n \ge 0 \}$, so $L(M_{(w)})$ is not regular.

We define our reduction $f$ as 
$$f(\langle M,w \rangle) = \langle M(w) \rangle.$$ 

Then $f$ is computable, 

$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(M_{(w)}) = \Sigma^* \\
& \Rightarrow & \langle M(w) \rangle \in REGULAR_{TM}, 
\end{array}$$

and

$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(M_{(w)}) = \{0^n 1^n \ | \ n \ge 0\} \\
& \Rightarrow & \langle M(w) \rangle \not \in REGULAR_{TM}, 
\end{array}$$

so $A_{TM} \le_m REGULAR_{TM}$ via $f$. 

Since $A_{TM}$ is undecidable, it follows that $REGULAR_{TM}$ is undecidable.

$\boxdot$

---

### Equivalence Problem for TMs


$$EQ_{TM} = \{ \langle M_1, M_2 \rangle  | M_1 \text{ and } M_2 \text{ are TMs and } L(M_1) = L(M_2) \}$$
>#### Theorem 
>###### $EQ_{TM}$ is undecidable. 

##### Proof: 
We will reduce $$ETM = \{ \langle M \rangle \ | \ M \text{ is a TM and } L(M) = \emptyset\}$$ to $EQ_{TM}$. 

Let $R$ be a TM that immediately rejects all inputs. Then $L(R) = \emptyset$. Our reduction $f$ is defined by 

$$f(\langle M \rangle) = \langle M, R \rangle.$$

Then 

$$
\begin{array}{rcl}
\langle M \rangle \in E_{TM} & \iff & L(M) = \emptyset \\
& \iff & L(M) = L(R) \\
& \iff & \langle M, R \rangle \in EQ_{TM} \\
& \iff & f(\langle M \rangle) \in EQ_{TM},
\end{array}$$
so $E_{TM} \le_m EQ_{TM}$ via $f$. Since $E_{TM}$ is undecidable, it follows that $EQ_{TM}$ is undecidable.

$\boxdot$

---

In fact, we can even show that $EQ_{TM}$ is not Turing-recognizable or co-Turing-recognizable. For this, we need the following theorem.
$
>#### Theorem 
>###### If A ≤m B and B is Turing-recognizable, then A is Turing-recognizable

##### Proof:
Let $f$ be a mapping reduction of $A$ to $B$ and let $M$ be a recognizer for $B$. We describe a recognizer $N$ for $A$. 
$N$: On input $w$: 
1. Compute $f(w)$. 
2. Run $M$ on input $f(w)$. 
	- If $M$ accepts $f(w)$, accept. 
	- If $M$ rejects $f(w)$, reject. 

We claim that $N$ recognizes $A$. 
- If $w \in A$, then $f(w) \in B$ because $f$ reduces $A$ to $B$. Therefore $M$ accepts $f(w)$ and $N$ accepts $w$. 
- If $w \not \in A$, then $f(w) \not \in B$ because $f$ reduces $A$ to $B$. Therefore $M$ either rejects $f(w)$ or does not halt on $f(w)$ and $N$ does the same on $w$.
 
$\boxdot$

---

>#### Theorem 
>###### If $A \le_m B$ and B is Turing-recognizable, then A is Turing-recognizable. 
>
>#### Corollary 
>###### If $A \le_m B$ and A is not Turing-recognizable, then B is not Turing-recognizable. 
>#### Corollary 
>###### If $A \le_m B$ and A is not co-Turing-recognizable, then B is not co-Turing-recognizable. 

##### Proof:
Use the fact A ≤m B ⇐⇒ A c ≤m B c and the previous corollary

$\boxdot$

---

>#### Theorem
>###### $EQ_{TM}$ is not Turing-recognizable.

##### Proof:
We showed that:
- ETM is not Turing-recognizable. 
- ETM ≤m EQTM. 

The previous theorem implies that EQTM is not Turing-recognizable.

We will reduce ATM to EQTM. Since ATM is not co-Turing-recognizable, this will establish that EQTM is not co-Turing-recognizable as well.

For any TM M and string w, define a TM M(w) as follows. 
M(w) : On input x: 
1. Run M on input w. // Ignore input x. 
2. If M accepts w, accept x. 

Consider any instance hM,wi of ATM. 
- Suppose hM,wi ∈ ATM. Then M accepts w. Therefore L(M(w) ) = Σ∗ . 
- Suppose hM,wi ∈/ ATM. Then M does not accept w. Therefore L(M(w) ) = ∅.

Therefore we have 
- hM,wi ∈ ATM ⇒ L(M(w) ) = Σ∗ 
- hM,wi ∈/ ATM ⇒ L(M(w) ) = ∅.

_Accepts everything or nothing - J.H_

We have 
- hM,wi ∈ ATM ⇒ L(M(w)) = Σ∗ 
- hM,wi ∈/ ATM ⇒ L(M(w)) = ∅. 

Let T be a TM with L(T) = Σ∗ . We define our reduction f as $$f (hM,wi) = hM(w) ,Ti.$$ 
Then f is computable, 
hM,wi ∈ ATM ⇒ L(M(w)) = Σ∗ 
⇒ L(M(w)) = L(T) 
⇒ hM(w) ,Ti ∈ EQTM 

and 

hM,wi ∈/ ATM ⇒ L(M(w)) = ∅ 
⇒ L(M(w)) 6= L(T) 
⇒ hM(w) ,Ti ∈/ EQTM, 

so ATM ≤m EQTM via f.

$/boxdot$

---

![[Pasted image 20220310120146.png]]

MISSING $EQ_{CFG}$

Define
ALLDFA = {hMi | M is a DFA and L(M) = Σ∗ }, 
ALLCFG = {hGi | G is a CFG and L(G) = Σ∗ }. 
>#### Theorem 
>###### ALLDFA is decidable. 

##### Proof:

Use similar techniques to EDFA or reduce to EQDFA. 

We will show that ALLCFG is undecidable and as a corollary, that EQCFG is also undecidable. 
$$EQ_{CFG} = \{hG, Hi \ | \ G \text{ and } H \text{ are CFGs and } L(G) = L(H) \}$$

---

>#### Definition
>###### Let $M$ be a TM and $w$ be a string.
>- ###### An accepting computation history of M on w is a sequence of configurations C1, . . . , Cl , where C1 is the start configuration of M on w, Cl is an accepting configuration of M, and each Ci follows from Ci−1 using M’s transition function. 
>- ###### A rejection computation history is defined similarly, except that Cl is a rejecting configuration.

Show 

>#### Theorem
>###### ALLCFG is undecidable. 
##### Proof: 
We will show that A c TM ≤m ALLCFG. The proof uses computation histories. 

Given a TM and a string w, our goal is to construct a CFG G so that 
- M does not accept w ⇒ L(G) = Σ∗ , 
- M accepts w ⇒ L(G) 6= Σ∗ . 

The idea is to design G sot that G generates all strings that do not encode accepting computation histories of M on w.

The natural way to encode a computation history C1, . . . , Cl is as a string 
$$\#C1\#C2\#C3\# · · · \#Cl\#.$$ 
However, we will encode the computation history as 
$$\#C1\#C R 2 \#C3\#C R 4 \# · · · \#Cl\#,$$ 
with every other configuration written in reverse. 

The language of our grammar will be 

```ad-error
title: need fix this format holy fuck
$$L(G) =  x \in Σ ∗     x is not an accepting computation history of M on w $$
```

That is, G generates all strings 
$$x = \#C1\#C R 2 \#C3\#C R 4 · · · \#Cl\#$$

such that: 
1 C1 is not the start configuration of M on w, 
2 Cl is not an accepting configuration, or 
3 for some i, Ci does not yield Ci+1 under M’s transition function 

as well as all x where 
4 x is not of the form $\#C1\#C R 2 \#C3\#C R 4 · · · \#Cl\#$.

To make things simpler, we design a PDA D first. An algorithm can convert D into G.
D starts by nondeterministically choosing one of the 4 conditions to check. Conditions 1 , 2 , and 4 are easy to check (can be done on a DFA).

For condition 3 : 
- D nondeterministically chooses two configurations Ci and Ci+1 to check. 
- Then D pushes Ci onto its stack, and compares it to Ci+1. This is why we need the encoding of computation histories that writes every other configuration in reverse – Ci will be pushed onto the stack in reverse. 
- Now D can compare the two configurations, they should match except for the three cells around the head position of Ci . 
- D accepts if there is a mismatch.

$\boxdot$

---

The proof that $ALL_{CFG}$ is undecidable reduces A c TM ≤m $ALL_{CFG}$. Because A c TM is not Turing-recognizable, we actually have a stronger result: 

>#### Theorem 
>###### $ALL_{CFG}$ is not Turing-recognizable. 

However: 
>#### Theorem
>###### $ALL_{CFG}$ is co-Turing-recognizable.

We need to give a recognition algorithm for $ALL^c_{CFG}$. Let $s_1,s_2, . . .$ be an enumeration of $\Sigma^*$ . 

M: On input $\langle G \rangle$: 
for i = 1, 2, . . . 
Run the decision algorithm S for ACFG on input hG,sii. 
If it rejects, accept. 

Suppose that $\langle G \rangle$ ∈/ $ALL_{CFG}$. Then L(G) 6= Σ∗ . Let sj be the least string that is not in L(G). Then on the j th iteration of the loop, S will reject $\langle G, s_j \rangle$. Therefore M will accept hGi. 

Now suppose that $\langle G \rangle \in ALL_{CFG}$. Then $L(G) = \Sigma^*$ , so $\langle G, s_i \rangle \in A_{CFG}$ for all $i$. Thus $S$ will accept in every iteration and $M$ will run forever on $\langle G \rangle$. 

Therefore $M$ recognizes $ALL^c_{CFG}$.

$\boxdot$

---
![[Pasted image 20220310121119.png]]

### $EQ_{CFG}$

Define:

$$ALL_{DFA} = \{ \langle M \rangle \ | \ M \text{ is a DFA and } L(M) = \Sigma^*\},$$
$$ALL_{CFG} = \{ \langle G \rangle \ | \ G \text{ is a CFG and } L(G) = \Sigma^*\},$$

>#### Theorem
>###### $ALL_{DFA}$ is decidable.

##### Proof:

Use similar techniques to $E_{DFA}$ or reduce to $EQ_{DFA}$
 
We will show that $ALL_{CFG}$ is undecidable and as a corollary, that $EQ_{CFG}$ is also undecidable.
 
$$EQ_{CFG} = \{ \langle G, H \rangle \ | \ G \text{ and } H \text{ are CFGs and } L(G) = L(H)\}$$
 
>#### Definition
>###### Let $M$ be a TM and $w$ be a string.
>* ###### Let accepting computation history of M on w is a sequence of configurations $C_1, ... , C_l$ where $C_1$ is the start configuration of $M$ on $w$, $C_l$ is an accepting configuration of $M$, and each $C_i$ follows from $C_{i-1}$ using $M$'s transition function.
>* ###### A _rejection computation history_ is defined similarly, except $C_l$ is a rejecting configuration
 
>#### Theorem
>###### $ALL_{DFA}$ is decidable.

##### Proof:

We will show that $A^c_{TM} \le_m ALL_{CFG}$. The proof uses computation histories.

Given a TM and a string $w$, our goal is to construct a CFG G so that
- $M$ does not accept $w \Rightarrow L(G) = \Sigma^*$
- $M$ accepts $w \Rightarrow L(G) \not = \Sigma^*$

The idea is to design G so that G generates all strings that do not encode accepting computation histories of M on w.

slide 72

---
...

---
slide 105

Let $\langle M_1, M_2 \rangle$ be an instance of $EQ_{TM}$. Construct the following TM $M$:

...
see slide 114
...

We claim that:
$$L(M_1) = L(M_2) \Rightarrow L(M) = \Sigma^*$$
$$L(M_1) \not = L(M_2) \Rightarrow L(M) \not = \Sigma^*$$

---

Slide 136
## Rice's Theorem