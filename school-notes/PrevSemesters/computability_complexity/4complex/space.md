# Space
##### Computability & Complexity
---

## Deterministic Time and Space Complexity

>![[Screen Shot 2022-04-13 at 9.38.01 AM.png]]

### Time and Space Bounded TMs 

>#### Definition 
>###### Let $s, t : N \rightarrow N$, and let $M$ be a TM. Then $M$ is 
>- ###### $t(n)$_-time-bounded_ if for all $x \in \{0,1\}^*, time_M(x) \le t(|x|)$.
>- ###### $s(n)$_-space-bounded_ if for all $x \in \{0,1\}^*, space_M(x) \le s(|x|).$
>- ###### $O(t)$_-time-bounded_ if $M$ is $f(n)$-time-bounded for some $f(n)=O(t(n)).$
>- ###### $O(s)$_-space-bounded_ if $M$ is $f(n)$-space-bounded for some $f(n)=O(s(n)).$

### Complexity Classes

>#### Definition 
>###### Given $t,s : N \rightarrow N$, define the following complexity classes. 
>###### $$DTIME(t) = \{L(M) \ | \ M \text{ is an } O(t) \text{-time-bounded } TM\},$$
>###### $$DSPACE(s) = \{L(M) \ | \ M \text{ is an } O(s) \text{-space-bounded } TM\},$$

Then 