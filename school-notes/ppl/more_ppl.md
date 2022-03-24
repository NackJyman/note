# Introduction to Semantics
##### Principals of Programming Languages
###### March 10th, 2022
---

This marks the end of covering type checkers. This is supposed to show that types are good.

Have you heard of a Scheme.

---
# $\uranus$
### Semantics and Interpreters
###### We doin big-step semantics
...

When evaluating an identifier expression, we look up the identifier ....

...

It should be clear that the structure of an interpreter that implements these semantics will be extremely similar to the structure fo the type checker implementing the type system! The typing rule vs. the evaluation rule?

$$\frac{\Gamma \vdash_e e_1 : int, y : }{\Gamma e }$$

##### Side Effects

There is one very important aspect of semantics that we didn't have to worry about in our type systems. Evalusation can have side effects, meaning it can do things other than just return a value. 

The most typical side effect is changing the environment. The...

So we need to change out evaluation relation to not only relate expressions and evironments to values, but also to updated environments! We now write
$$\gamma \vdash_e$$

...

We can now form the rule for assignment expressions: 
$$\gamma ...$$

...

Operational semantics makes the difference between pre- and post-increment and decrement operators very clear. In post-increment, the incremented value is only put in the environment and the result is the original value. In pre-increment....

...

It may seem like the only expressions which have side effects are those which have side effects themselves, but since expressions can contain other expressions, we must track side effects in all expressions!

For example, the expression:

$$x ++ - ++ x $$

is abominable, yet correct. The evaluation rule for substraction just needs to account for the side effects of its subexpressions:

$$\frac{\gamma...}{\gamma...}$$

So What is the value of the following expression in the environment x:= 1?

$$x ++ - ++ x $$

Turns out we can calculate it by building a proof tree! Let's do that.

$$\frac{???}{x := 1 \vdash_e x ++ \Downarrow \langle ???, ??? \rangle}

idk_Fhow_Fto_Fmake_FProof_F Tree_F$$

...

---
### Extra Resources
