# Machine Learning
## Ridge Regression
---
##### Lecture 8 (Feburary 7, 2022
#RidgeRegression #Overfitting 
Regulating overfitting when using many features

Overfitting happens when the training error is low but the true error is really high.

If there exists an $w'$ such that the training error of $w'$ is greater than $\hat{w}$'s training error. Same with the true error. Both of these conditions need to exist for it to be overfitting.

training error vs data points
- graphs like a log function
-  The error is made from bias and noise

---
### Overfitting of a Polynomial 

Overfitting can happen easily when we are using very large approximations (Many inputs/features).

Few Observations
- Rabidly overfit as model complexity increases.

Many Observations
- Harder to overfit

With large numbers of inputs we can easily over complicate our model because we need to evaluate every possible permuation of the endpoints.

#### Desired total cost format

Want to balance
- How well the function fits the data. 
- The magnitude of coefficients

Total Cost = Measure of fit + measure of magnitude of coefficients 

```ad-caution

if measure of fit is a small number then ?????

if the measure of magnitude is small, then the data is underfit.
```

```ad-hint
title: What Summary # is indicative of size of regression coefficients?

Sum?
$$\{w_0 + w_1 x_i = y_i \} \rightarrow w_1 + w_0$$

Sum of absolute Value?
$$\sum_{j=0}^D|w_j| = ||w||_1$$
$$(L_1 \text{ Norm})$$

Sum of squares ($L_2$ Norm)?
$$\sum_{j=0}^D w_j^2 = ||w||_2$$
$$(L_2 \text{ Norm})$$
```
---
#### Introducing $\lambda$

Consider specific total cost
Total Cost = Measure of fit + measure of magnitude of coefficients 

$$RSS(w) + \lambda (L_1 \text{ OR } L_2)$$
$$RSS(w) + \lambda ||w||_2$$
$\lambda$ is a tuning parameter (Balance of fit and amgnitude)
>
If $\lambda = 0$:
$$\text{Cost: } RSS(w)$$
>
If $\lambda = \infty$:
$$\hat{w} \not = 0 \rightarrow \text{Cost: } \infty$$
$$\hat{w} = 0 \rightarrow \text{Cost: } RSS(0)$$
>
If $\lambda = 0$:
$$\hat{w} = ??? \rightarrow \text{Cost: } RSS(????)$$
	
Ridge Regressuin is $L_2$ ?????

$\lambda$ controls model complexity

Large $\lambda$:
- High bias, low variance

Small $\lambda$:
- low bias, high variance

As $\lambda$ increases the number of coeefficients for each input decreases. 

## Fitting the ridge regression model (for a given $\lambda$ value)

1. Rewrite the total cost in matrix notation
2. Compute the gradiant
3. Minimize RSS
	1. Set the gradient = 0
	2. Gradient descent

$$y = Hw + \epsilon$$

```ad-abstract
title: Matrix Dimensions
y will be "n x 1"
H is "n x D"
w is "D x 1"
$\epsilon$ will be "n x 1"
```

Rewrite magnitude of coefficients in vector notation
???

---
Ridge closed form solution 

$$Cost(w) = -2H^T(y-Hw) + 2 \lambda I w$$
Solve for W

$$\hat w = (H^TH+\lambda I)^{-1} H^T y$$

Interpreting ridge closed-form solution

if $\lambda = 0$: $\hat w =$ Least squared 

if $\lambda = \infty$: $\hat w = 0$

---

When compared to linear regression, $\hat w$ is invertable always if $\lambda > 0$ even if N < D.

Complexity of inverse:
	$O(D^3)$
	
---
#### Gradient Descent returns

Elementwise ridge regression gradient descent algorithm

$$\Delta cost(w) = -2H^T(y-Hw)+2\lambda w$$

init $$w^{(1)} = 0, t = 1$$

>While $||\Delta RSS(w^{(t)})|| > \epsilon$
for $j=0,...,D$
???

---
### How to choose $\lambda$

1. Model Selection
Need to choose tuning parameters $\lambda$ controlling model complexity
2. Model Assessment
???

##### Hypothetical Implementation
Model Selection:
For each considered $\lambda$
i.	Estimate parameters $\hat w_\lambda$
ii. ???


Model Assessment
Compute test error of $\hat w_\lambda$, $\lambda^*$ was selected to minimize *test error*
???

##### Practical Implementation
1. Divide the data into three sets, Training Set, Validation Set, and Test Set. Select $\lambda^*$ such that $\hat w _{\lambda^*}$ minimizes error on validation set
2. ???

```ad-note
title: Data Set

fit $\hat w_\lambda$ to the training set
On the Validation set, Test performance of $\hat w_\lambda$ to slect $\lambda^*$
Test Set: assess generalization of error of $\hat w_{\lambda^*}$
```

Typical Splits of Data:

| Training Set | Validation Set | Test Set |
| ------------ | -------------- | -------- |
| 80%          | 10%            | 10%      |
| ???          | ???            | ???      |

---

HW Questions:
	Due Feb. 16
	
Q1, given a LR.csv file 4 collumns, collumn 4 is the independent variable
Fit a simple linear regression to predict y from x

Use Cross Validation
Multiple Regression

Write code so u finally understand what your doing.

regularzation

LASSO Regression
```py
import pandas as pd
```

turn in a pdf and code folder.

---
---