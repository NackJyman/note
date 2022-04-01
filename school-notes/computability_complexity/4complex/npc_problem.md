# NPC Problems
##### Computability and Complexity
---

### Polynomial-Time Reductions

>#### Definition
>###### We say that $A$ is polynomial-time many-one reducible to $B$, and write $A \le_P B$, if ther is a polynomial-time computable function $f : \Sigma^* \rightarrow \Sigma^*$ such that for all $w \in \Sigma^*$
>###### $$w \in A \iff f(w) \in B$$

Two Implications:

...

### NP-Completeness

...

>#### Theorem
>###### The following are equivalent
>###### 1. Some NP-Complete problem is in P.
>###### 2. Every NP-Complete problem is in P.
>###### 3. P = NP

##### Proof: 

...

>#### Corollary
>###### The following are equivalent
>###### 1. Some NP-Complete problem is not in P.
>###### 2. Every NP-Complete problem is not in P.
>###### 3. P $\not=$ NP

---

### Satisfiability

A propositional formula is a sentence made from variables, operators ...

...

A _3CNF_ formula is a propositional formula consisting of the conjuction og a number of disjunctive clauses with at most 3 literals. An example of this is 

...

.
.
...

---

>#### Cook-Levin Theorem (1971, 1973)
>###### 3SAT is NP-Complete

This requires showing that for every $A \in NP, A \le_P 3SAT$.

- There is a polynomial-time nondeterminstic Turing machine for A.
- ...
- ...

##### Proof:
Let $A \in NP$. THere is a nondeterministic TM that decides $A$ in $n^k$ time for some constant $k$.

A tableau for $N$ on $w$ is an $n^k \times n^k$ table whose rows are the configureations of a branch of the computations of $N$ on input $w$.

![[Pasted image 20220329115733.png]]

- For convenience later, we assume each configuration starts and ends with \#.
- The first row of the tableau is that start configuration of $N$ on $w$.
- ...
- ...
- ...

On reduction $f$ will take an input $w$ and produce a formula ...
- Let $C = Q \cup \Gamma \cup \{\#\}$, where $Q$ is the set of states and $\Gamma$ is the tape alphabet.
- For each $s \in C$ and $1 \le i,j \le n^k$ we have a variable $x_{i,j,s}$.
- ...
- ...
- ...

Our formula .. will 
..

This slide (30-38) is a nightmare to recreate here so im not going to.

---
###### March 31st, 2022

>#### Lemma 
>###### $\le_p$ is transitive. That is, if $A \le_p B$ and $B \le_p C$, then $A \le_p C$.

##### Proof:

Let $f$ be the reduction from $A$ to $B$ and let $g$ be the reduction from $B$ to $C$. Then
- $l \in A \iff f(l) \in B$ for all instances $l$.
- $J \in B \iff g(J) \in C$ for all instances $J$.
- $f$ is computable in some polynomial $p(n)$ time and 
- $g$ is computable in some polynomial $q(n)$ time.

Define
$$h(l) = g(f(l))$$
Then for every instance of $l$, 
$$\begin{array}{rcl}
i \in A & \iff & f(l) \in B \\
& \iff & g(f(l)) \in C \\
& \iff & h(l) \in C
\end{array}$$

...

---

>#### Corollary 
>###### Let $A, B \in NP$. If $A$ is NP-complete and $A \le_p B$, then $B$ is also NP-complete.

##### Proof:
Because $A$ is NP-complete, $C \le_p A$ for every $C \in NP$. By transitivity and $A \le_p B$, we have $C \le_p B$ for every $B \in NP$. 
$\boxdot$

### NP-Completeness Proofs

Here is the method of showing a problem $B$ is NP-complete.
1. Show that $B \in NP$. It is usually easy to show this but it is a step that shouldn't be overlooked.
2. Choose some NP-Complete problem $A$ that is a candidate for reduction to $B$. Often we try to find a problem that is similar ot $B$. 
3. Reduce $A$ to $B$.

For this, we need to define a mapping of instances $l$ of $A$ to instances $l'$ of $B$. 

$$l \longmapsto l'$$

Then we need to prove that for every instance of $l$ of $A$,

$$l \in B \Rightarrow l' \in B$$

This involves showing two implicationsL $l \in A \Rightarrow l' \in B$ and $l' \in B \Rightarrow l \in A$.
-	To show that $l \in A \Rightarrow l' \in B$, assume that $l \in A$. Then there is some witness $w$ for $l$...

![[Screen Shot 2022-03-31 at 11.14.27 AM.png]]

### Clique

...
...

>#### Theorem
>###### CLIQUE is NP-Complete 

##### Proof:

We will show 3SAT $\le_p$ CLIQUE. Let $\phi = C_1 \wedge ... \wedge C_k$ be a 3CNF formula, where each $C_i$ is a clause with at most 3 literals. We construct a graph $G$ as follows.

For each clause $C_i = (l_1^{(i)} \vee l_2^{(i)} \vee l_3^{(i)})$ we put vertices $l_1^{(i)}, \ l_2^{(i)}, l_3^{(i)}$ in $G$. (If $C_i$ has 1 or 2 literals, we put 1 or 2 vertices in $G$.) We put an edge between $v_a^{(i)}$ and $v_b^{(j)}$ if the following two conditions hold.
1. $i \not = j$ In other words, $v_a^{(i)}$ and $v_b^{(j)}$ correspond to literals in different clauses.
2. $l_a^{(i)} \not =\neg l_b^{(j)}$. In other words, the corresponding literals are consistent.

##### For example, consider the formula

$$\phi = (x_1 \vee \neg x_2 \vee x_3) \wedge (x_1 \vee x_2) \wedge(\neg x_1 \vee x_2)$$

The graph $G$ appears below (Maybe? slide 69$_{lol}$). The three vertices at the top correspond to the first clause, the two at the left are for the second clause, and the two at the right are for the third clause.

![[Screen Shot 2022-03-31 at 11.28.55 AM.png]]

Suppose that φ is satisfiable. Then there is an assignment to the variables that makes at least one literal true in each clause. Choose one such literal from each clause and let V 0 be the corresponding set of k vertices. Then V 0 is a clique of size k because our construction ensures there is an edge between each vertex in V 0 . This shows φ ∈ 3SAT ⇒ hG, ki ∈ CLIQUE.

Conversely, suppose that $G$ has a clique of size $k$. Let $V'$ be this clique. 

Then $V'$ must contain a vertex from each of the $k$ groups that correspond to the clauses. This is because there cannot be edges between vertices in the same group.

From $V'$ we obtain a satisfying assignment for $\phi$ by setting the variables to make each of the corresponding literals true. 
- We can do this because there is no edge between a literal and its negation, thus we won’t ever try to set a variable to both true and false. 
- If a variable does not correspond to a vertex in V 0 , then we can set it arbitrarily. 

This assignment satisfies at least one literal in each clause of φ. Therefore, hG, ki ∈ CLIQUE ⇒ φ ∈ 3SAT. 

Since the mapping φ 7→ hG, ki can be computed in polynomial time, we have that 3SAT ≤P CLIQUE.

---

### Vertex Cover

A vertex cover of a graph $G = (V, E)$ is subset $V' \subseteq V$ such that if (u,v) \in E, then u \in V' or V' (or both). In other words, a vertex cover is a subset of the vertices that touches all edges in the graph. The decision problem is 

$$VERTEX-COVER = \{ \langle G,k \rangle \ | \ G \text{ has a vertex cover of size }k\}$$

Note that $V$, the set of all vertices in the graph, is always a vertex cover. Also, if $V'$ is vertex cover and $V'' \supseteq V'$ Then $V''$ is also a vertex cover. THe goal is to determine if small vertex covers exist.

>#### Theorem
>###### VERTEX-COVER is NP-Complete

##### Proof:

To see that $VERTEX-COVER \in NP$, note that $\langle G,k \rangle \in VERTEX-COVER \iff (\exists V') | V' | \le k$ and $V'$ is a vertex cover for $G$. The set $V'$ is the witness and all we have to do is check $V'$ covers all the edges, which can be done in polynomial time.

...
...

---

Suppose that G has a clique V 0 with |V 0 | = k. We claim that V − V 0 is a vertex cover in G¯. 

To see this, let (u, v) ∈ E¯ be any edge in G¯. Then (u, v) ∈/ E, so at least one of the two vertices is not in the clique: at least one of u, v is not in V 0 . Therefore at least one of u or v is in V − V 0 . 

Since (u, v) ∈ E¯ was arbitrary, we have shown that V − V 0 is a vertex cover in G¯. 

Since |V − V 0 | = |V| − k, this shows hG, ki ∈ CLIQUE ⇒ hG¯, |V| − ki ∈ VERTEX-COVER.

Now suppose that G¯ has a vertex cover V 0 with |V 0 | = |V| − k. Then for all u, v ∈ V, we have 

(u, v) ∈ E¯ ⇒ u ∈ V 0 or v ∈ V 0 .

If we take the contrapositive of the implication, we have that for all u, v ∈ V, 

u ∈/ V 0 ∧ v ∈/ V 0 ⇒ (u, v) ∈/ E¯.

This is equivalent to saying that for all u, v with u 6= v, 

u, v ∈ V − V 0 ⇒ (u, v) ∈ E, 

which means that V − V 0 is a clique in G. Notice that 

$$|V − V'| = |V| − |V'| = |V| − |V| + k = k$$
so G has a clique of size k. This shows 

hG¯, |V| − ki ∈ VERTEX-COVER ⇒ hG, ki ∈ CLIQUE

We hace shown that dor any graph $G = (V,E)$ and number $k$, 

$$\langle G,k \rangle \in CLIQUE \iff \langle G, |V| - k \rangle \in VERTEX-COVER$$

Since the mapping $\langle G,k \rangle \longmapt \langle G, |V| - k \rangle$ can be computed in polynomial time, we have $CLIQUE \le_p VERTEX-COVER$

$\boxdot$


---
## Part II

### Hamiltonian Cycle

Recall that a Hamiltonian cycle in a graph is a cycle that cisits all the vertices without repetition. Not all graphs have a Hamiltonian xycle, and determining if a graph has asuch a cycle is NP-Complete.

The decision problem is 

$$HC = \{ G \ | \ G \text{ has a Hamiltonian cycle} \}$$

>#### Theorem
>###### HC is NP-complete.

##### Proof:

...

---

### Traveling Salesman Problem

In the traveling salesman problem, a salesman wishes to visit a collection of cities at the minimum possible cost.
- For each pair of cities, $A$ and $B$, there is a cost associated with traveling from $A$ to $B$. 
- The goal is to find a tour of the cities of the minimum total cost.

We model an instance of this problem as follows:
- $n$ = number of cities. The cities are number from 1 to $n$. 
- $c:\{1,...,n\}\times \{ 1,...,n \} \rightarrow N^+$ is a cost function. $c(i,j)$ is the cost of traveling from city $i$ to city $j$.

A tour of the $n$ cities is a sequence $(a_1,..., a_n, a_{n+1})$ where
- $a_i \in \{1,...,n\}$ for all $1 \le i \le n$,
- $a_{n+1} = a_1$, and
- for each $j \in \{1,...,n\}$, there is some $i \in \{1,...,n\}$ such that $a_i = j$.

The cost of the tour is

$$\SIGMA$$

Sometimes this is also called the length of the tour.

The traveling salesman decision problem is

...

It is easy to see that TSP \in NP. THe witnesses are the tours. We can easily check if the cost of a tour is at most $k$.

>#### Theorem
>###### TSP is NP-complete

Before we prove this theorem, let's discuss the consequences. 

In the TSP decision problem, we only have to decide of a tour with cost at most $k$ exists. In practice what we really want is to find a minimum cost tour.

>#### Corollary
>###### If $P \not = NP$, then there is no polynomial-time algorithm that finds shortest TSP tours.

##### Proof:

...



