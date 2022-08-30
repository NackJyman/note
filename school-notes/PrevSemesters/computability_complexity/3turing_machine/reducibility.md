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

>#### Definition
>###### A function $f : \Sigma^* \rightarrow \Sigma^*$ is a _computable function_ if some Turning machine $M$, on every input $w$ halts with just f(w) on its tape.

>#### Definition
>###### Language $A$ is _mapping reducible_ to language $B$, written $A \ge_M B$, if there is a computable function $f : \Sigma^* \rightarrow \Sigma^*$, where for every $w$, $$w \in A \iff f(w) \in B$$
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

>N: On input w: 
>1. Compute $f(w)$.
>2. Run $M$ on input $f(w)$. 
>	- If $M$ accepts $f(w)$, accept. 
>	- If $M$ rejects $f(w)$, reject. 

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

### $HALT_{TM}$

The _halting problem_ for TMs:

$$HALT_{TM} = \{ \langle M, w \rangle \ | \ M \text{ is a TM that halts input string } w \}$$

>#### Theorem
>###### $HALT_{TM}$ is undecidable.

```ad-abstract
title: Proving $HALT_{TM}$ is undecidable
##### Proof by showing that $A_{TM} \le_m HALT_{TM}$:

We must design a computable function $f$ so that for any instance $\langle M,w \rangle$ of $A_{TM}$, $f(\langle M,w \rangle ) =\langle M', w' \rangle \in HALT_{TM}$.

The following Turing machine $F$ computes a reduction $f$. 
>$F$: On input $\langle M,w \rangle$: 
>1. Construct the following machine $M'$ 
>	-	$M'$ On input $x$: 
>		1. Run $M$ on $x$. 
>		2. If $M$ accepts, accept.
>		3. If $M$ rejects, enter an infinite loop. 
>2. Output $\langle M,w \rangle$. 
 
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

```

---

### $REGULAR_{TM}$

$$REGULAR_{TM} = \{ \langle M \rangle \ | \ M \text{ is a TM and } L(M) \text{ is a regulat language}\}.$$

>#### Theorem
>###### $REGULAR_{TM}$ is undecidable.

```ad-abstract
title: Prove that $REGULAR_{TM}$ is undecidable
##### Proof:

We will show that $A_{TM} \le_m REGULAR_{TM}$. Given a TM $M$ and a string $w$, define a TM $M_{(w)}$ as follows: 
$M_{(w)}$ : On input $x$: 
1. If $x$ has the form $0^n 1^n$ , accept. 
2. If $x$ does not have this form, run $M$ on input $w$ and accept if $M$ accepts $w$. 

- If $M$ accepts $w$, then $L(M_{(w)}) = \Sigma^*$, so $L(M_{(w)})$ is regular. 
- If $M$ does not accept $w$, then $L(M_{(w)}) = \{0^n 1^n \ | \ n \ge 0 \}$, so $L(M_{(w)})$ is not regular.

We define our reduction $f$ as 
$$f(\langle M,w \rangle) = \langle M_{(w)} \rangle.$$ 

Then $f$ is computable, 

$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(M_{(w)}) = \Sigma^* \\
& \Rightarrow & \langle M_{(w)} \rangle \in REGULAR_{TM}, 
\end{array}$$

and

$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(M_{(w)}) = \{0^n 1^n \ | \ n \ge 0\} \\
& \Rightarrow & \langle M_{(w)} \rangle \not \in REGULAR_{TM}, 
\end{array}$$

so $A_{TM} \le_m REGULAR_{TM}$ via $f$. 

Since $A_{TM}$ is undecidable, it follows that $REGULAR_{TM}$ is undecidable.

$\boxdot$
```

---

### $EQ_{TM}$

Equivalence Problem for TMs

$$EQ_{TM} = \{ \langle M_1, M_2 \rangle  | M_1 \text{ and } M_2 \text{ are TMs and } L(M_1) = L(M_2) \}$$
>#### Theorem 
>###### $EQ_{TM}$ is undecidable. 

```ad-abstract
title: Prove that $EQ_{TM}$ is undecidable

##### Proof: 
We will reduce $$E_{TM} = \{ \langle M \rangle \ | \ M \text{ is a TM and } L(M) = \emptyset\}$$ to $EQ_{TM}$. 

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
```

---

In fact, we can even show that $EQ_{TM}$ is not Turing-recognizable or co-Turing-recognizable. For this, we need the following theorem.

>#### Theorem 
>###### If $A \le_m B$ and $B$ is _Turing-recognizable_, then $A$ is _Turing-recognizable_

##### Proof:
Let $f$ be a mapping reduction of $A$ to $B$ and let $M$ be a recognizer for $B$. We describe a recognizer $N$ for $A$. 
>$N$: On input $w$: 
>1. Compute $f(w)$. 
>2. Run $M$ on input $f(w)$. 
>	- If $M$ accepts $f(w)$, accept. 
>	- If $M$ rejects $f(w)$, reject. 

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
Use the fact $A \le_m B \iff A^c \le_m B^c$ and the previous corollary

$\boxdot$

---

### $EQ_{TM}$ and Turing-Recognizable

>#### Theorem
>###### $EQ_{TM}$ is not Turing-recognizable.

```ad-abstract
title: Prove $EQ_{TM}$ is not Turing-recognizable.

##### Proof:
We showed that:
- $E_{TM}$ is not Turing-recognizable. 
- $E_{TM} \le_m EQ_{TM}$. 

The previous theorem implies that $EQ_{TM}$ is not Turing-recognizable.

We will reduce $A_{TM}$ to $EQ_{TM}$. Since $A_{TM}$ is not co-Turing-recognizable, this will establish that $EQ_{TM}$ is not co-Turing-recognizable as well.

For any TM $M$ and string $w$, define a TM $M_{(w)}$ as follows. 
$M_{(w)}$ : On input $x$: 
1. Run $M$ on input $w$. // Ignore input $x$. 
2. If $M$ accepts $w$, accept $x$. 

Consider any instance $\langle M,w \rangle$ of $A_{TM}$. 
- Suppose $\langle M,w \rangle \in A_{TM}$. Then $M$ accepts $w$. Therefore $L(M_{(w)}) = \Sigma^*$. 
- Suppose $\langle M,w \rangle \not \in A_{TM}$. Then $M$ does not accept $w$. Therefore $L(M_{(w)}) = \emptyset$.

Therefore we have 
- $\langle M,w \rangle \in A_{TM} \Rightarrow L(M_{(w)}) = \Sigma^*$
- $\langle M,w \rangle \not \in A_{TM} \Rightarrow L(M_{(w)}) = \emptyset$.

###### _Accepts everything or nothing - J.H_

Let $T$ be a TM with $L(T) = \Sigma^*$. We define our reduction $f$ as 
$$f (\langle M,w \rangle) = \langle M_{(w)} ,T \rangle.$$ 
Then $f$ is computable,
$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(M_{(w)}) = \Sigma^* \\
& \Rightarrow & L(M_{(w)}) = L(T) \\
& \Rightarrow & \langle M_{(w)}, T \rangle \in EQ_{TM}
\end{array}$$

and 
$$\begin{array}{rcl}
\langle M,w \rangle \not \in A_{TM} & \Rightarrow & L(M_{(w)}) = \emptyset \\
 
& \Rightarrow & L(M_{(w)}) \not = L(T) \\
& \Rightarrow & \langle M_{(w)} ,T \rangle \not \in EQ_{TM},
\end{array}$$

so $A_{TM} \le_m EQ_{TM}$ via $f$.

$\boxdot$
```

---

![[Pasted image 20220310120146.png]]

MISSING $EQ_{CFG}$

### $ALL_{DFA}$ is decidable

Define
$$ALL_{DFA} = \{\langle M \rangle \ | \ M \text{ is a DFA and } L(M) = \Sigma^* \},$$ 
$$ALL_{CFG} = \{\langle G \rangle \ | \ G \text{ is a CFG and } L(G) = \Sigma^* \}.$$ 
>#### Theorem 
>###### $ALL_{DFA}$ is decidable. 

```ad-abstract
title: Prove that $ALL_{DFA}$ is decidable.

##### Proof:

Use similar techniques to $E_DFA$ or reduce to $EQ_DFA$. 

We will show that $ALL_{CFG}$ is undecidable and as a corollary, that $EQ_{CFG}$ is also undecidable. 
$$EQ_{CFG} = \{ \langle G, H \rangle \ | \ G \text{ and } H \text{ are CFGs and } L(G) = L(H) \}$$
```

---

>#### Definition
>###### Let $M$ be a TM and $w$ be a string.
>- ###### An accepting computation history of $M$ on $w$ is a sequence of configurations $C_1, . . . , C_l$, where $C_1$ is the start configuration of $M$ on $w$, $C_l$ is an accepting configuration of $M$, and each $C_i$ follows from $C_{i−1}$ using $M$’s transition function. 
>- ###### A rejection computation history is defined similarly, except that $C_l$ is a rejecting configuration.

so show 

>#### Theorem
>###### $ALL_{CFG}$ is undecidable. 

```ad-abstract
title: Prove that $ALL_{CFG}$ is undecidable.

##### Proof: 
We will show that $A^c_{TM} \le_m ALL_{CFG}$. The proof uses computation histories. 

Given a TM and a string $w$, our goal is to construct a CFG $G$ so that 
- $M$ does not accept $w \Rightarrow L(G) = \Sigma^*$, 
- $M$ accepts $w \Rightarrow L(G) \not =  \Sigma^*$ . 

The idea is to design $G$ sot that $G$ generates all strings that do not encode accepting computation histories of $M$ on $w$.

The natural way to encode a computation history $C_1, . . . , C_l$ is as a string 
$$\#C_1\#C_2\#C_3\# · · · \#C_l\#.$$ 
However, we will encode the computation history as 
$$\#C_1\#C^R_2 \#C_3\#C^R_4 \# · · · \#C_l\#,$$ 
with every other configuration written in reverse. 

The language of our grammar will be 

$$L(G) = \left\{ \begin{array}{l} 
\ \ \ \ \ x \in \Sigma^* & | & x \text{ is not an accepting computation} \\
& | & \text{history of } M \text{ on } w\end{array}\right\}$$

That is, G generates all strings 
$$x = \#C_1\#C^R_2\#C_3\#C^R_4 · · · \#C_l\#$$

such that: 
1. $C_1$ is not the start configuration of $M$ on $w$, 
2. $C_l$ is not an accepting configuration, or 
3. for some $i, C_i$ does not yield $C_{i+1}$ under $M$’s transition function 

as well as all $x$ where 
4. $x$ is not of the form $\#C_1\#C^R_2 \#C_3\#C^R_4 · · · \#C_l\#$.

To make things simpler, we design a PDA $D$ first. An algorithm can convert $D$ into $G$.

$D$ starts by nondeterministically choosing one of the 4 conditions to check. Conditions 1, 2, and 4 are easy to check (can be done on a DFA).

For condition 3: 
- $D$ nondeterministically chooses two configurations $C_i$ and $C_{i+1}$ to check. 
- Then $D$ pushes $C_i$ onto its stack, and compares it to $C_{i+1}$. This is why we need the encoding of computation histories that writes every other configuration in reverse – $C_i$ will be pushed onto the stack in reverse. 
- Now $D$ can compare the two configurations, they should match except for the three cells around the head position of $C_i$. 
- $D$ accepts if there is a mismatch.

$\boxdot$
```

---

The proof that $ALL_{CFG}$ is undecidable reduces $A^c_{TM} \le_m ALL_{CFG}$. Because $A^c_{TM}$ is not Turing-recognizable, we actually have a stronger result: 

>#### Theorem 
>###### $ALL_{CFG}$ is not Turing-recognizable. 

However: 
>#### Theorem
>###### $ALL_{CFG}$ is co-Turing-recognizable.

We need to give a recognition algorithm for $ALL^c_{CFG}$. Let $s_1,s_2, . . .$ be an enumeration of $\Sigma^*$ . 

>$M$: On input $\langle G \rangle$: 
>for $i = 1, 2, . . .$ 
>-	Run the decision algorithm $S$ for $A_{CFG}$ on input $\langle G,s_i \rangle$. 
>	-	If it rejects, accept. 

Suppose that $\langle G \rangle \not \in ALL_{CFG}$. Then $L(G) \not = \Sigma^*$. Let $s_j$ be the least string that is not in $L(G)$. Then on the $j^{th}$ iteration of the loop, $S$ will reject $\langle G, s_j \rangle$. Therefore $M$ will accept $\langle G \rangle$. 

Now suppose that $\langle G \rangle \in ALL_{CFG}$. Then $L(G) = \Sigma^*$ , so $\langle G, s_i \rangle \in A_{CFG}$ for all $i$. Thus $S$ will accept in every iteration and $M$ will run forever on $\langle G \rangle$. 

Therefore $M$ recognizes $ALL^c_{CFG}$.

$\boxdot$

---
![[Pasted image 20220310121119.png]]

### $EQ_{CFG}$
Recall the _equivalence problem_ for CFGs

$$EQ_{CFG} = \{ \langle G,H \rangle \ | \ G \text{ and } H \text{ are CFGs and } L(G) = L(H) \},$$

>#### Theorem
>###### $EQ_{CFG}$ is undecidable.

##### Proof:

Show that $ALL_{CFG} \le_m EQ_{CFG}$.
$\boxdot$
>#### Theorem
>###### $EQ_{CFG}$ is co-Turing-recognizable.

Similar to $ALL_{CFG}$ is co-Turing-recognizable.
$\boxdot$

---

### $ALL_{TM}$

$$ALL_{TM} = \{ \langle M \rangle \ | \ G \text{ is a TM and } L(M) = \Sigma^*\}$$

>#### Theorem
>###### $ALL_{TM}$ is neither Turing-recognizable nor co-Turing-recognizable.

```ad-abstract
title: Prove that $ALL_{TM}$ is neither Turing-recognizable nor co-turing recognizable.
##### Proof:

It suffices to show that $EQ_{TM} \le_m ALL_{TM}$, since $EQ_{TM}$ is not Turing-recognizable and not co-Turing-recognizable.

Let $\langle M_1, M_2 \rangle$ be an instance of $EQ_{TM}$. Construct the following TM $M$:

![[Screen Shot 2022-04-01 at 1.10.56 PM.png]]

We clain that:

$$L(M_1) = L(M_2) \Rightarrow L(M) = \Sigma^*,$$
$$L(M_1) \not = L(M_2) \Rightarrow L(M) \not = \Sigma^*.$$

Suppose $L(M_1) = L(M_2)$. We want to show $L(M) = \Sigma^*$.
Let $x \in \Sigma^*$
- If $x \in 0^*$, then $M$ accepts $x$.
- Otherwise $x = 0^t1w$ for some $w \in \Sigma^*$ and $t \ge 0$.
	- If $M_1$ accepts $w$ within $t$ steps, then $M$ will run $M_2$ on $w$. Since $L(M_1) = L(M_2)$, $M_2$ will accept $w$. 
	Therefore $M$ accepts $x$.
	- Else if $M_2$ accepts $w$ within $t$ steps, then $M$ will run $M_1$ on $w$. Since $L(M_1) = L(M_2)$. $M_1$ will accept $w$.
	Therefore $M$ accepts $x$.
	- Otherwise $M_1$ does not accept $w$ within $t$ steps and $M_2$ does not accept $w$ within $t$ steps. Then $M$ accepts $x$.

In all cases, $M$ accepts $x$. Therefore $L(M) = \Sigma^*$.

Now suppose $L(M_1) \not = L(M_2)$. We want to show $L(M) \not = \Sigma^*$.

There is some $w \in L(M_1) \Delta L(M_2)$, where $\Delta$ denotes symmetric difference. 

Assume $w \in L(M_1) - L(M_2)$.
-	Let $t$ be the number of steps used by $M_1$ when it accepts $w$. 
-	We claim the string $0^t1w$ is not accepted by $M$.
	-	On input $0^t1w$, $M$ will find that $M_1$ accepts $w$ within $t$ steps. 
	-	$M$ will then run $M_2$ on $w$. 
	-	Since $M_2$ does not accept $w$, $M_2$ will either reject or not halt.
		-	If $M_2$ rejects $w$, $M$ will reject $0^t1w$
		-	If $M_2$ does not halt on $w$, $M$ will not halt.
		
		In either case, $M$ does not accept $0^t1w$.
- Therefore $L(M) \not = \Sigma^*$

Analogously, if $w \in L(M_2) - L(M_1)$, then $L(M) \not = \emptyset$.

We have shown 
$$L(M_1) = L(M_2) \Rightarrow L(M) = \Sigma^*,$$
$$L(M_1) \not = L(M_2) \Rightarrow L(M) \not = \Sigma^*.$$

Since the function $f( \langle M_1, M_2 \rangle ) = \langle M \rangle$ is computable, this shows $EQ_{CFG} \le_m ALL_{TM}.$ Therefore $ALL_{TM}$ is neither Turing-recognizable nor co-Turing-recognizable.
$\boxdot$
```

---

>## Rice's Theorem
>#### Let $P$ be any problem about Turing machines that satisfies the following two properties. $(P \subseteq \{ \langle M \rangle \ | \ M \text{ is a TM}\})$
>- #### For any two TMs $M_1$ and $M_2$, where $L(M_1) = L(M_2)$, we have $\langle M_1 \rangle \in P \iff \langle M_2 \rangle \in P$. In other words, the membership of $\langle M \rangle \in P$ depends only on $L(M)$.
>- #### There exist two TMs $M_1$ and $M_2$, where $\langle M_1 \rangle \in P$ and $\langle M_2 \rangle \not \in P$. In other words, $P$ is nontrivial - it holds for some, but not all TMs.
> #### Then $P$ is undecidable.

##### Proof:

Let $M_{empty}$ be a TM with $L(M_{empty}) = \emptyset$. We distinguish two cases. 

__Case 1:__ 
$\langle M_{empty} \rangle \not \in P$. In this  case, we show that $A_{TM} \le_m P$. Let $N$ be a TM with $\langle N \rangle \in P$. Such an $N$ exists because $P$ is nontrival. Then since $\langle N \rangle \in P$ and $\langle M_{empty} \rangle \not \in P$, we know that $L(N) \not = L(M_{empty})$, so $L(N) \not = \emptyset$.

For any TM $M$ and input $w$, define a new TM $S_{M,w}$ as follows.

$S_{M,w}$: On input $x$:
1. Simulate $M$ on input $w$.
2. If $M$ rejects $w$, reject.
3. If $M$ accepts $w$, simulate $N$ on $x$ and accept/reject according ro what $N$ does.

Our reduction $f$ is defined as 

$$f(\langle M,w \rangle) = \langle S_{M,w}\rangle$$

We have 

$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(S_{M,w}) = L(N) \\
& \Rightarrow & \langle S_{M,w} \rangle \in P \\
& \Rightarrow & f(\langle M,w \rangle ) \in P \end{array}$$

and

$$\begin{array}{rcl}
\langle M,w \rangle \in A_{TM} & \Rightarrow & L(S_{M,w}) = L(N) \\
& \Rightarrow & \langle S_{M,w} \rangle \not \in P \\
& \Rightarrow & f(\langle M,w \rangle ) \not \in P \end{array}$$

so $A_{TM} \le_m P$ via $f$. Therefore $P$ is undecidable.

**Case 2:**
$\langle M_{empty} \rangle \in P$. In this case, define $Q = P^c$. Since the decidable languages are closed under complement, it suffices to show that $Q$ is udecidable.

Since $\langle M_{empty} \rangle \in P$, $\langle M_{empty} \rangle \not \in Q$. Then Case 1 applies to $Q$ and we have $A_{TM} \le_m Q$. Therefore $Q$ is undecidable. 

$\boxdot$

### Applications of Rice's Theorem
##### $E_{TM} = \{\langle M \rangle \ | \ L(M) = \emptyset\}$

Rice's Theorem applies to show that $E_{TM}$ is undecidable:
1. For any $M_1$ and $M_2$ with $L(M_1) = L(M_2)$, we have $L(M_1) = \emptyset \iff L(M_2) = \emptyset$, so $\langle M_1 \rangle \in E_{TM} \iff \langle M_2 \rangle \in E_{TM}$.
2. For the second condition, let $M_1$ accept $\emptyset$ and $M_2$ accept $\Sigma^*$. Then $\langle M_1 \rangle \in E_{TM}$ and $\langle M_2 \rangle \not \in E_{TM}$

---

##### $REGULAR_{TM} = \{\langle M \rangle \ | \ L(M) \text{ is a regular language}\}.$

Rice's Theorem applies to show that $REGULAR_{TM}$ is undecidable:
1. The first condition is satisfied: if $L(M_1) = L(M_2)$ then $\langle M_1 \rangle \in REGULAR_{TM} \iff \langle M_2 \rangle \in REGULAR_{TM}$.
2. For the second condition, let $M_1$ accept $\Sigma^*$ and $M_2$ accept $\{0^n1^n \ | \ n \ge 0 \}$. Then $\langle M_1 \rangle \in REGULAR_{TM}$ and $\langle M_2 \rangle \not \in REGULAR_{TM}$.

---

##### $ALL_{TM} = \{\langle M \rangle \ | \ L(M) = \Sigma^*\}$

Rice's Theorem applies to show that $ALL_{TM}$ is undecidable.
1. The first condition is satisfied: if $L(M_1) = L(M_2)$ then $\langle M_1 \rangle \in ALL_{TM} \iff \langle M_2 \rangle \in ALL_{TM}$
2. For the second condition, let $M_1$ accept $\Sigma^*$ and $M_2$ accept $\emptyset$. Then $\langle M_1 \rangle \in ALL_{TM}$ and $\langle M_2 \rangle \not \in ALL_{TM}$

---
## End of Lecture Slides.