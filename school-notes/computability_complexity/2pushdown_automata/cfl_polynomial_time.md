# Context-Free Languages in Polynomial-Time
##### Computability and Complexity
###### Feburary 22, 2022
---
###### [[pa_home | unit homepage]]
---
#### Definition
###### A context-free grammar is in _Chomsky normal form_ if every rule is of the form $$A \rightarrow BC$$ or $$A \rightarrow a$$ where $a$ is any terminal and $A,B,$ and $C$ are any variable - except $B$ and $C$ may not be the start variable. In addition, the rule $S \rightarrow \epsilon$ is permitted, where $S$ is the start variable.

To prove this, we show how to convert any CFG into Chomsky normal form: 
- We add a new start variable.
- We elimate all $\epsilon$-rules of the form $A \rightarrow \epsilon$.
- We also elimainate all unit rules of the form $A \rightarrow B$.
- We convert all remaining rules to the proper form by breaking into multiple rules.

#### Proof
First, add a new start variable $S_0$ and the rule $S_0 \rightarrow S$, where $S$ was the original start variable. This ensures the start variable is never on the right-hand side of a rule.

Second, we take care of $\epsilon$-rules. Let $A \rightarrow \epsilon$ be an $\epsilon$-rule in the grammar.
- Remove the rule $A \rightarrow \epsilon$.
- For any rule of the form $R \rightarrow uAv$, we add the rule $R \rightarrow uv$ with $A$ removed.
- If there are multiple occurrences of $A$ in a rule, for example $R \rightarrow uAvAw$, then add the $R \rightarrow uvw$ with all occurences of $A$ removed.

Repeat this until all $\epsilon$-rules are removed.

Third, we take care of all unit rules. Let $A \rightarrow B$ be a unit rule
- Remove the rule $A \rightarrow B$.
- For any rule $B \rightarrow u$, we add the rule $A \rightarrow u$.

Repeat this until all unit rules are removed.

Finally, we convert all remaining rules into proper form. We replace each rule $$A \rightarrow u_1 u_2 ... u_k,$$ where $k \ge 3$ and each $u_i$ is a variable or a terminal, with the rules 
$$\begin{array}{rcl} A & \rightarrow & u_1 A_1 \\ A_1 & \rightarrow & u_2 A_2 \\ A_2 & \rightarrow & u_3 A_3 \\ & . & \\ & . & \\ A_{k-2} & \rightarrow & u_{k-1} A_k \end{array}$$
Here $A_1, A_2,$ . . $, A_{k-2}$ are new variables. We replace any terminal $u_i$ in these rules with a new variable $U_i$ and add the rule $$U_i \rightarrow u_i.$$
This completes the construction. $\boxdot$

---

### _Chomsky Normal Form_
```ad-example
title: __Example:__ Convert a grammar to _Chomsky Normal Form._

$B = \{ a^n b^m c^l \vert n = m \text{ or } m = l \}$
$$\begin{array}{rcl} 
S & \rightarrow & LC \ | \ AR \\
L & \rightarrow & aLb \ | \ \epsilon \\
R & \rightarrow & bRc \ | \ \epsilon \\
A & \rightarrow & Aa \ | \ \epsilon \\
C & \rightarrow & Cc \ | \ \epsilon\end{array}$$
Convert this grammar to _Chomsky Normal Form_.

---

Add new start variable:
$$\begin{array}{rcl}
S_0 & \rightarrow & S \\
S & \rightarrow & LC \ | \ AR \\
L & \rightarrow & aLb \ | \ \epsilon \\
R & \rightarrow & bRc \ | \ \epsilon \\
A & \rightarrow & Aa \ | \ \epsilon \\
C & \rightarrow & Cc \ | \ \epsilon\end{array}$$
Remove $\epsilon$-rules (_First pass_):
$$\begin{array}{rcl}
S_0 & \rightarrow & S \\
S & \rightarrow & LC \ | \ L \ | \ C \ | \ AR\ | \ A\ | \ R\ | \ \epsilon \\
L & \rightarrow & aLb \ | \ ab \\
R & \rightarrow & bRc \ | \ bc \\
A & \rightarrow & Aa \ | \ a \\
C & \rightarrow & Cc \ | \ c\end{array}$$
Remove $\epsilon$-rules (_Second pass_):
$$\begin{array}{rcl}
S_0 & \rightarrow & S \ | \ \epsilon \\
S & \rightarrow & LC \ | \ L \ | \ C \ | \ AR\ | \ A\ | \ R\\
L & \rightarrow & aLb \ | \ ab \\
R & \rightarrow & bRc \ | \ bc \\
A & \rightarrow & Aa \ | \ a \\
C & \rightarrow & Cc \ | \ c\end{array}$$
Remove unit rules ($S \rightarrow S_0$):
$$\begin{array}{rcl}
S_0 & \rightarrow &  LC \ | \ L \ | \ C \ | \ AR\ | \ A\ | \ R\ | \ \epsilon \\
L & \rightarrow & aLb \ | \ ab \\
R & \rightarrow & bRc \ | \ bc \\
A & \rightarrow & Aa \ | \ a \\
C & \rightarrow & Cc \ | \ c\end{array}$$
Remove unit rules ($S \rightarrow L \ | \ C \ | \ A \ | \ R$):
$$\begin{array}{rcl}
S_0 & \rightarrow &  LC \ | \ aLb \ | \ ab \ | \ Cc \ | \ c \ | \ AR\ | \ Aa \ | \ a \ | \ bRc \ | \ bc\ | \ \epsilon \\
L & \rightarrow & aLb \ | \ ab \\
R & \rightarrow & bRc \ | \ bc \\
A & \rightarrow & Aa \ | \ a \\
C & \rightarrow & Cc \ | \ c\end{array}$$
Convert remaining rules $(S_0 \rightarrow aLb \ | \ bRC; L \rightarrow aLb; R \rightarrow bRc)$:
$$\begin{array}{rcl}
S_0 & \rightarrow &  LC \ | \ ab \ | \ Cc \ | \ c \ | \ AR\ | \ Aa \ | \ a \ | \ bc\ | \ \epsilon \\
S_0 & \rightarrow & aA_1 \\
A_1 & \rightarrow & Lb \\
S_0 & \rightarrow & bA_2 \\
A_2 & \rightarrow & Rc \\
L & \rightarrow & aA_1 \ | \ ab \\
R & \rightarrow & bA_2 \ | \ bc \\
A & \rightarrow & Aa \ | \ a \\
C & \rightarrow & Cc \ | \ c\end{array}$$
Replace terminals with rules:
$$\begin{array}{rcl}
S_0 & \rightarrow &  LC \ | \ U_1 U_2 \ | \ C U_3 \ | \ c \ | \ AR\ | \ A U_1 \ | \ a \ | \ U_2 U_3\ | \ \epsilon \\
S_0 & \rightarrow & U_1 A_1 \\
A_1 & \rightarrow & L U_2 \\
S_0 & \rightarrow & U_2 A_2 \\
A_2 & \rightarrow & R U_3 \\
L & \rightarrow & U_1 A_1 \ | \ U_1 U_2 \\
R & \rightarrow & U_2 A_2 \ | \ U_2 U_3 \\
A & \rightarrow & A U_1 \ | \ a \\
C & \rightarrow & C U_3 \ | \ c \\
U_1 & \rightarrow & a \\
U_2 & \rightarrow & b \\
U_3 & \rightarrow & c \end{array}$$
This is now in _Chomsky Normal Form._
```

---
### Context-Free Languages are Decidable in Polynomial Time.

We will use _Dynamic Programming_ to show that every CFL is decidable in polynomial time.

Let $P$ be the class of problems decidable in polynomial time.

#### Theorem
###### CFL $\subseteq P$.

This is called Cocke-Kasami-Younger (CKY) Algorithm.

```ad-abstract
title: Dynamic Programming
___Recursion___ is top-down: we start with the full problem, divide it into subproblems, and recurse until we reach base cases. 

___Dynamic programming___ is bottom-up: we start with the base cases and build up from there to solve larger and larger subproblems, until we have solved the full problem.

Standard examples of dynamic programming problems are longest common subsequence and edit distance.
```


#### Subproblems
Let $G$ be a CFG in _Chomsky Normal Form_. Let $w = w_1$ . . $w_n$ be an input string. We wish to determine whether $G$ generates $w$.

The idea is to determine for each $i \le j$, whether the substring $$w[i, j] = w_i w_{i+1} \ . \ .\  w_j$$
is generated by $G.$

---
### Dynamic Programming Table

We will have a table $table(·, ·)$ where $table(i, j)$ includes all variables from which $w[i, j]$ can be derived. We start with the smallest subproblems and work our way up. 
- The base cases are when $i = j$. For any rule $A \rightarrow w_i$ , we put $A$ in $table(i, j)$. 
- Now let $i < j$. Note $A \Rightarrow^* w[i, j]$ if and only if for some rule $A \rightarrow BC$ there is a splitting position $k, i \le k < j$, such that $$B \in table(i, k) \text{ and }C \in table(k + 1, j).$$ In this case we put $A$ in $table(i, j)$. 

At the end, we just need to check whether the start variable $S$ is in $table(1, n)$.

---
### Cocke-Kasmi-Younger (CKY) Algorithm

```ruby
On input w = w1 · · · wn: 
	if (w =  and S → ) is a rule, accept 
	else, reject 
	
	for i = 1 to n 
		for each variable A 
			if A → wi is a rule 
				put A in table(i, i) 

// l is the length of the substring
// i is the start position of the substring
// j is the end position of the substring 
// k is th split position for each rule A → BC 
	for l = 2 to n 
		for i = 1 to n − l + 1 
			let j = i + l − 1 
			for k = i to j − 1 
					if [ B ∈ table(i, k) and 
						and C ∈ table(k + 1, j) ], 
					then put A in table(i, j) 
	
	if S ∈ table(1, n), accept 
	else, reject
```

Let
$$\begin{array}{rcl} n & = & \text{the length of the input,} \\
v & = & \text{the number of variables,} \\ 
r & = & \text{the number of rules.} \end{array}$$
Note that for a fixed content-free language $v$ and $r$ are fixed constants.
- the first for loop take $O(nv) = O(n)$ time.
- The nested for loops take $O(n^3 r) = O(n^3)$ time.

The total run time is $O(n^3)$.

Therefore this polynomial-time algorithm and CFL $\subseteq P. \boxdot$

---
### Summary
- Every CFG may be converted into _Chomsky normal form._
- Membership in _Chomsky Normal Form_ grammat may be decided in $O(n^3)$ time by the CKY algorithm
- $REG \subseteq CFL\subseteq P$