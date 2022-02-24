# Non-Context-Free Languages
##### Computability and Complexity
###### Feburary 22, 2022
---
###### [[pa_home | unit homepage]]
---
#### Pumping Lemma for Context-Free Languages

###### If $A$ is a context-free language, then there is a number $p$ (the pumping constant) such that for evert $w \in A$ with $|w| \ge p, w$ can be divided into five pieces $w = uvxyz$ satisfying the following conditions:
1. ###### For all $i \ge 0, ub^ixy^iz \in A$.
2. ###### $|xy| > 0.$
3. ###### $|vxy| \le p$.

##### Comments:
- Condition (2) says that $v \not = \epsilon \text{ or } y \not = \epsilon$
- Condition (1) says that $uxz, \ uvxyz, \ uvvxyyz, \ uvvvxyyyz, \ . \ . \ ,$ are all in $A.$

---

Proof. 
Let $G$ be a CFG for $A$. 

Let $b$ be the max $\#$ symbols on the RHS of a rule (assume $b \ge 2$).

In any parse tree for $G$, a node can have no more than $b$ children. 
- At most $b$ leaves are 1 step from the start variable. 
- At most $b^2$ leaves are within 2 steps from the start variable
- . . 
- . . 
- At most $b^h$ leaves are within $h$ steps from the start variable. 
 
Therefore, if the height of the parse tree is at most $h$, the length of the string generated is at most $b^h$. 

Conversely, if a generated string is at least $b^h + 1$ long, each of its parse trees must be at least $h + 1 $high.

---

Recall $|V|$ is the $\#$ of variables in $G$. We set the pumping constant $p$ to be $$p = b^{|V|+1}.$$ 
If a string $s \in A$ and $|s| \ge p$, any parse tree for s must be at least $|V| + 1$ high, because $$p = b^{|V |+1} ≥ b^{|V |} + 1.$$

---
Let τ be a parse tree for $s \in A, s \ge p$. If there are multiple parse trees, choose τ to be one with the smallest number of nodes. 
We know: 
- τ must be at least $|V| + 1$ high 
⇒ ∃ a path from the root to a leaf of length $\ge |V| + 1$
- this path has $\ge |V| + 2$ nodes: one terminal, the others variables 
- therefore there are $\ge |V| + 1$ variables 
- there are only $|V|$ variables in $G$, so some variable appears more than once on the path 
- let $R$ be a variable that repeats among the lowest $|V| + 1$ variables on the path

---

![[Pasted image 20220223002813.png]]

![[Pasted image 20220223002831.png]]

---
### Examples
```ad-example
title: Prove $A = \{ a^n b^n c^n | n \ge 0 \}$ is not context-free

 Suppose that $A$ is context-free, and let $p$ be the pumping constant for $A$. Let $w = a^pb^p c^p$. Then let $uvxyz = w$ such that $|vy| > 0$ and $|vxy| \le p$. 
 
 We will consider two cases, based whether $v$ and $y$ contain more than one type of alphabet symbol. 
 1. Suppose $v$ and $y$ each contain only one type of alphabet symbol. Then the string $uv^2 xy^2 z$ will not contain equal numbers of $a$’s, $b$’s, and $c$’s (because it will have more of the symbols that are present in $v$ and $y$ – but this does not include all three symbols). Therefore it is not a member of $A$. 
 2. Suppose that $v$ or $y$ contains more than one type of alphabet symbol. Then the string $uv^2 xy^2 z$ will not be a member of $a^∗ b^∗  c^∗$ and therefore also not a member of $A$. 
 
 In either case, we have a contradiction of the context-free pumping lemma. Therefore the assumption that $A$ is context-free must be false, so $A$ is not context-free. $\boxdot$
```

```ad-example
title: Prove $D = \{ ww \ | \ w \in \{0,1\}^* \}$ is not context-free

Suppose that $D$ is context-free, and let p be the pumping constant for $D$. Let $s = 0^p1^p0^p1^p$. Then let $uvxyz = s$ such that $|vy| > 0$ and $|vxy| \le p$. 
We consider three cases: 
1. Suppose $vxy$ lies in the first half of $s$. Then pumping $v$ and $y$ will move a 1 into the first position of the second half. Therefore $uv^2 xy^2 z \not \in D$. 
2. Similarly, if $vxy$ lies in the second half of $s$, $uv^2 xy^2 z$ will have a 0 in the last position of the first half, so $uv^2 xy^2 z \not \in D$. 
3. The remaining case is when $vxy$ straddles the midpoint of $s$. In this case, pumping down, we get $uxz = 0^p 1^i 0^j 1^p$, where $i < p$ or $j < p$. Hence $uxz \not \in A$.
 
This contradicts the context-free pumping lemma, and therefore $D$ is not context-free. $\boxdot$
```

### Closure Properties of CFL

The Context-Free Languages are Closed Under:
- Union
- Concatenation
- Star

##### Intersection is special though

$$A = \{a^n b^n c^n | n ≥ 0\} \not \in CFL.$$ 
Let $$B = \{a^n b^n c^m | n, m ≥ 0\},$$ 
$$C = \{a^n b^mc^m | n, m ≥ 0\}.$$ Then $A = B \cap C$ and $B, C \in CFL$:

| CFG for B                              | CFG for C                             |
| -------------------------------------- | ------------------------------------- |
| $S \rightarrow LR$                     | $S \rightarrow LR$                    |
| $L \rightarrow aLb \ \vert \ \epsilon$ | $L \rightarrow aL \ \vert \ \epsilon$ | 
| $R \rightarrow cR \ \vert \ \epsilon$                     | $R \rightarrow bRc \ \vert \ \epsilon$                    |

Therefore $CFL$ is _not_ closed under intersection

---

However it can be shown that the context-free languages are closed inder intersection with the regular languages:
#### Theorem
###### If $A \in CFL$ and $B \in REG$, then $A \cap B \in CFL$.

##### Proof:
HWHWHWHW HAHAH HEEEHEE ITS HW HA :)

---

### $CFL$'s are not closed under complement.

##### Proof 1:
Suppose that $CFL$ is closed under complement.
Let 
$$A = \{a^i b^j c^k | i \not = j \text{ or } j \not = k\}.$$
Then $A \in CFL$, so $A^c \in CFL$ by assumption. Because $CFL$ is closed under intersection with $REG$, we also have 
$$A^c ∩ a^∗ b^∗ c^∗ = \{a n b n c n | n \ge 0\} \in CFL,$$
a contradiction. 

##### Proof 2:
 Let $$B = {ww | w ∈ {0, 1}^∗ }.$$ 
 Then $B \not ∈ CFL$. However, we can show that
 $$\begin{array}{rcl} B^c & = & \{ xy \vert \ |x| = |y| \text{ and } x \not = y \} \\ & & \cup \ \{ x ∈ \{0, 1\}^∗ \ \vert \  |x| \text{ is odd} \}\end{array}$$ is context-free!
 Here is a CFG for $B^c$: 
 $$\begin{array}{rcl} S & \rightarrow & AB \ | \ BA \ | \ A \ | \ B \\
 A & \rightarrow & CAC \ | \ 0 \\
 B & \rightarrow & CBC \ | \ 1 \\
 C & \rightarrow & 0 \ | \ 1
 \end{array}$$

1. Starting with $S \rightarrow A$ or $S \rightarrow B$, we can derive all strings of odd length. 
2. Starting with $S \rightarrow AB$, we can derive all strings of the form $$r0su1v$$ where $|r| = |s|$ and $|u| = |v|$.
3. Starting with $S \rightarrow BA$, we can derive all strings of the form $$u1vr0s$$ where $|u| = |v|$ and $|r| = |s|$. 

If $w = xy$ with $|x| = |y|$ and $x \not = y$, then $w$ can be derived in the second or third forms above. $\boxdot$

---

![[Pasted image 20220223010330.png]]

---

### Summary
$CFL$ is closed under:
- Union
- Concatenation
- Star

$CFL$ is not closed under:
- Intersection
- Complement