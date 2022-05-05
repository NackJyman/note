# Computational Complexity
##### Computability and Complexity

---
Beginning of the fourth section of the course.

### A Motivating Dichotomy

Given a weight graph $G$, consider the following problems:
- Computing the shortest path between two vertices.
- Compute the shortest tour that visits each vertex once (a traveling salesman tour)

Shortest paths cna be computed efficiently. There is a _polynomial-time_ algorithm.

On the other hand, there is no known efficient algoirthm for computing the shortest tour. We can solve this problem via exhaustive search, which takes _exponential time_. No one knows how to do better than this!

Why is computing shortest tours more difficult than computing shortest paths?

This question is addressed by _Computational Complexity Theory_.

---

###### Computational Complexity Theory is the study of the resources needed to solve problems on a computer. Problems are classified into "complexity classes" according to the computational resources than can be used to solve them.

The most basic and important resources are
- time (running time of an algorithm and)
- space (memory used by an algorithm).

Complexity theory also introduces a number of theoretical resources including
- Randomness,
- Nondeterminism,
- Alternation,
- Circuit-size,
- Counting Complexity,
- Quantum Complexity,
- and many others.

The most fundamental open question in computational complexity is the P vs NP problem.
- P: problems which solutions can be found efficiently
- NP: problems for which solutions can be _verified efficiently_

P means "deterministic polynomial-time"
NP means "nondeterministic polynomial-time"

>![[Pasted image 20220324114544.png]]

Major open problems in computational complexity: 
- Does P = NP?
- Does NP = EXP?

##### Computational Complexity Theory History

**Origin: 1960s**
- Cobham (1964) and Edmonds (1965):
	polynomial-time algorithm $\equiv$ efficient
- Harmanis and Stearns (1965):
	"On the Computational Complexity of Algorithms"
	-	Formal definitions of time and space complexity
	-	Hierarchy Theorems
	-	Won the Turing Award in 1993

**NP-completeness: 1970s**
- Cook (1971, Harvard grad student): Satisfiability is NP-complete
- Karp (1972): 21 NP-complete problems won the Turing award in 1985
- Levin (1973, Student of Kolmogorov at Moscow University) tiling problem is NP-complete.

---

### Asymptotic Notation

>#### Definition
>###### Let $f,g : N \rightarrow R$
>###### We say that $f(n) = O(g(n))$ if
>###### $$(\exists c, n_0) (\forall n \ge n_0) f(n) \le c \cdot g(n)$$

Read "f(n) is big-oh of g(n)"
Big-oh notation ignores constant factors and lower order terms.

Examples:
- $3n + 6 = O(n)$
- $n^2 + 2n + 4 = O(n^2)$
- $2n$ log $n = O(n$ log $n)$

>#### Definition
>###### Let $M$ be a Turing machne that halts on all inputs. The _running time_ or _time complexity_ of $M$ is the function $f : N \rightarrow N$ where $f(n)$ is the maximum number of steps that $M$ uses on any input of size $n$.
>###### We Say that $M$ runs in $f(n)$ time and that $M$ is an $f(n)$-time-bounded Turing machine.
>###### A Turing machine $M$ is polynomial-time-bounded if $M$ runs in $O(n^c)$ time for some constant $c$.

>#### Definition
>###### $P$ is the class of all languages that are decidable by a polynomial-time-bounded Turing Machine.

### Polynomial-Time Church-Turing Thesis

There is an extension of the Church-Turing Thesis for polynomial-time computation.

>#### Polynomial-Time Church-Turing Thesis
>###### Let $M$ be any _reasonable_ deterministic computational model (such as random access machines or multitape TMs)
>###### Then any langauge which can be decided in polynomial-time by a single-tape TM, and vice versa.

Therefore the definition of $P$ does not depend on the model of computation

>#### Theorem
>###### A _k_-tape TM running in time $t(n)$ can be simulated by a single-tape TM running in time $O(t(n)^2)$.

_k_ tapes: $O(n^3) \Rightarrow$ One Tape: $O(n^6)$

>#### Theorem
>###### A _k_-tape TM running in time $t(n)$ can be simulated by a two-tape TM running in time $O(t(n) \text{ log } t(n))$.

_k_ tapes: $O(n^3) \Rightarrow$ Two Tapes: $O(n^3 \text{ log } n)$

### Verifiers and NP

>#### Definition
>###### A _verifier_ for a language A is an algorithm $V$ such that
>###### $$A \ \{ w \ | \ V \text{ accepts } \langle w,c \rangle \text{ for some string } c\}.$$

$c$ is called a _certificate_ (or _witness_) that $w$ belongs to $A$.

>#### Definition
>###### NP is the class of languagese that have polynomial-time verifiers.

##### NP Examples:

COMPOSITES = $\{n \ | \ n \text{ is a composite number}\}.$

We know that $n$ is composite if there are two numbers $p,q > 1$ such that $n = pq$.

Such a pair $\langle p,q \rangle$ is a _certificate_ that $n$ belongs to COMPOSITES.

A verifier can efficiently check that $n = pq$.

V: On input $\langle n, \langle p,q \rangle \rangle$:
1. Multiply $pq$.
2. If $n = pq$, accept. Otherwise, reject.

- If $n$ is composite, there is a certificate $\langle p,q \rangle$ that causes $V$ to accept
- If $n$ is not composite ($n$ is prime), there is no certificate $\langle p,q \rangle$ that causes $V$ to accept.

A _clique_ in a graph $G$ is subset of vertices that is fully connected.

$$CLIQUE = \{ \langle G,k \rangle \ | \ G \text{ is a graph with a clique of size } k\}$$

The clique is the certificate.

V: On input $\langle G, k, c \rangle$:
1. Test whether $c$ is a set of $k$ vertices in $G$.
2. Test whether $G$ contains all edges connecting vertices in $c$.
3. If both tests are passed, accept. Otherwise, reject.

### NP Examples

A _Hamiltonian Cycle_ in a graph $G$ is a cycle that visits every vertex in $G$ without repetition.

$$HAMCYCLE = \{ \langle G \rangle \ | \ G \text{ has a Hamiltonian cycle} \}.$$

A certificate that G has a Hamiltonian cycle is a listing of the vertices in a cycle.

...  slide 39

lecture ended at 48

---

At this point Hitchcock updated the slides so... idk

---
### Satisfiability (3SAT)

...

### Polynomial-Time Reductions

>#### Definition
>###### We say that $A$ is _polynomial-time_ ...

---

>#### Theorem
>###### If $A \le_P B$ and $B \in P$, then $A \in P$

##### Proof:

Let $f$ be the reduction from $A$ to $B$ and let $M$ be a polynomial-time decision algorithm for $B$.

- $w \in A \iff f(w) \in B$ for all instances $w$.
- $f$ is computable in some polynomial $p(n)$ time.
- $x \in B$ if and only if $M$ accepts $x$,
- $M$ runs in some polynomial $q(n)$ time.

The idea is to apply the reduction $f$ and then use the decision algorithm $M$ on the reduced instance.

Algorithm N for A: 
on input w, 
1. Compute f (w). 
2. Run M on f (w). 
	- If M accepts, accept. 
	- If M rejects, reject.

For ever instance $I$, we have

$$w \in A \Rightarrow f(w) \in B$$

...

---
>*Whiteboard:*
$$p(n) = n^2$$
$$q(n) = n^3$$
$$q(p(n)) = q(n^2) = (n^2)^3 = n^6$$

---

### NP-Completeness

>#### Definition
>###### A language $B$ is _NP-complete_ if
>###### 1. $B$ is in NP, and
>###### 2. every $A$ in NP is polynomial-time reducible to $B$.

---

### Verifiers Versus Nondeterminism

The original definition of NP used polynomial-time nondeterministic Turing Machines (NTM).

The definition...

#### Nondeterminism

The computation of an NTM can be represented as a tree.
- At each step, the machine may have multiple configurations it can go to next.
- In the end, we will in general have many final configurations, and each one is either accepting or rejecting.
- An NTM  accepts its input if there is at lease one accepting computation path.
- An NTM rejects its input if all computation paths are not accepting (rejecting assuming they halt).

For an NTM $N$, we define the language accepted by $N$ as 

$$L(N) \{x \in \{0,1\}^* \ | \ N \text{ accepts } x \}$$

---

### Time-Bounded NTMs and NP

...

---
>#### Theorem
>###### The following are equivalent for every language $A$.
>###### 1. $A$ is accepted by a polynomial-time NTM
>###### 2. $A$ has a polynomial-time verifier.

##### Proof:

$2 \Rightarrow 1$:
Suppose $A$ has a polynomial-timer verifier $V$. Then for all $w, w \in A \iff (\exists c) \ V$ accepts $\langle w,c \rangle$.
Consider the following NTM:

>NTMN
	>Input w;
	>nondeterministically choose a certificate c;
			>accept w if V accepts $\langle w,c \rangle$;
			>otherwise, reject;

This algorithm uses a polynomial amount of time to choose $c$, and checking if $V$ accepts $\langle w,c \rangle$ also takes a polynomial amount of time. Therefore $N$ is a polynomial-time-bouded NTM and $L(N) = A$

_Valid path = Valid Certificate_

$2 \Rightarrow 1$:
Suppose the $A$ is accepted by an NTM $N$. We will use accepting computation paths of $N$ as certificates for our verifier.

> Verifier V
	> Input $\langle w,c \rangle$
	> If c encodes an accepting computation path of N on w
		> accept
	> Other wise, reject.

Since $N$ is polynomial-time-bounded, all computation paths have length bounded by a polynomial. Thus $V$ is a polynomial-time verifier for $A$

$\boxdot$

---

### Quantifiers and Predicates

We cna rephrase the verifier definition of NP using a polynomial-time predicate with polynomial-size witnesses:

>#### Theorem
>###### $A \in$ NP if and only if there is a $D \in P$ and a polynomial $p$ such that for all $x \in \Sigma^*$, 
>###### $$x \in A \iff (\exists w \ \in \{0,1\}^{\le_{p(n)}}) \langle x,w \rangle \in D$$

This is analogous to the $\Sigma_1$ definition of Turing-recognizable.

Let EXP be the class of problems that are decidable by a $2^{O(n^k)}$-time Turing machine for some $k \ge 1$.

>#### Theorem
>###### NP $\subseteq$ EXP.

##### Proof:
...

![[Pasted image 20220329114034.png]]

![[Pasted image 20220329114056.png]]

