# Machine Learning
## Linear Regression
---
##### Lecture 4 (January 26th, 2022)
#LinearRegression 

```ad-abstract
title: Linear Regression Equation Format
Predicted Output: $\hat{y}$
Input:  $x$
Desired Output: $y$

Model Equation: $$\hat{y} = wx + b$$
Loss Function: $$L = (y - \hat{y})^{2}$$

```

---
### Ordinary Least Squares
At the root of the loss function is ordinary least squares.
$$y \approx x w$$
$$\left[\begin{array}
{r}
y_1 \\
y_2 \\
... \\
y_{n}
\end{array}\right] \approx 
\left[\begin{array}
{r}
\overrightarrow{x_1^T} \\
\overrightarrow{x_2^T} \\
... \\
\overrightarrow{x_n^T}
\end{array}\right] *
\left[\begin{array}
{r}
w_1 \\
w_2 \\
... \\
w_{d}
\end{array}\right]
$$

```ad-note
$w$ is length $d$ and so is each element of $x$.
```
---
### Four Levels for ML Problems
1. Data & Application
	- Looking at  '$y \approx x w$' we need to find the best $w$.
2. Model
	$$y \approx x w$$
	$$\hat{y} = x w$$
3. Optimization Problem
	$$L = (y - \hat{y})^{2}$$
4. Optimization Algorithm
	$$(x^T x) \hat{w} = x^T y$$
---
However, not everything is linear. So we need to be able to correctly predict using quadratics as well.

**Quadratic Model:**
$$\hat{y}_i = w_2 x_i^2 + w_1 x_i + w_0$$
With Features $\{1, x, x^2\}$

**Polynomial Model:**
$$\hat{y}_i = \sum_{j = 0}^{p} w_j x_i^j$$
With Features $\{1, x, x^2, x^3 ... x^p\}$

---
Regression Model: 
$$y_i =f(x_i) + error$$ 
```ad-note
error will be described as $\epsilon$
```

Simple linear regression Model:
 $$y_i = w_0 + w_1 + x_i + \epsilon_i$$
 $$f(x) = w_0 + w_1 x$$

$w_0, w_1$ are parameters: Regression Coefficients

---
Quadratic Regression Model:
$$f(x) = w_0 + w_1 x + w_2 x^2 + ... + w_p x^p$$
Polynomial Regression:
$$y_i = w_0 + w_1 x_i + w_2 x_i^2 + ... + w_p x_i^p + \epsilon_i$$

Treat these $x$'s as differeent features.

| Feature 1 = 1 (Constant) | Parameter 1 = $w_0$     |
| ------------------------ | ----------------------- |
| Feature 2 = $x$          | Parameter 2 = $w_1$     |
| Feature 3 = $x^2$        | Parameter 3 = $w_2$     |
| ...                      | ...                     |
| Feature $p+1$ = $x^p$    | Parameter $p+1$ = $w_p$ |

### Generic Basis Expansion
Model:
$$y_i = w_0 h_0(x_i) + w_1 h_1(x_i)+ ... + w_Dh_D(x_i) + \epsilon_i$$
$$= \sum_{j = 0}^{D} w_j h_j(x_i)+\epsilon_i$$

- $h_j$ is the $j^{th}$ feature in the model.
- $w_j$ is the $j^{th}$ regression coefficient or weight.

The model gets better the more inputs and parameters we have

### Fitting the Linear Regression Model
Rewrite  $\sum_{j = 0}^{D} w_j h_j(x_i)+\epsilon_i$ in matrix notation
$$y_i = w^T h(x_i) + \epsilon_i$$
$$y_i = h^T (x_i) w + \epsilon_i$$
$$\mathbf{Y} = \mathbf{H} \mathbf{W} + \epsilon$$
```ad-note
title: About $\mathbf{H}$...

This matrix is "n x D" accounts for the features ($h$) and the inputs ($\overrightarrow{x}$). Each row in $\mathbf{H}_{x_i}$ are the data points of the model.
```

---
Compute the cost of a given line (Residual sum of squares, RSS):

$$RSS(w_0, w_1) = \sum_{i=1}^N (y_i - [w_0+w_1 x_i])^2$$
$$RSS = \sum_{i=1}^N (y_i - \hat{y}_i)^2$$

RSS for multiple Regression:

$$RSS(w) = \sum_{i=1}^N (y_i - h^T (x_i) w)^2$$
$$= (y - H w)^T(y - H w)$$

Gradient of RSS:
$$\Delta RSS(w) = \Delta [(y - H w)^T(y - H w)$$
$$= -2 H^T(y - H w)$$

---
### Minimiziing RSS
#### Approach 1: Set the gradient to 0
##### Closed form solution:
Solve for w:
$$\Delta RSS(w) = -2 H^T(y - H w) = 0$$
$$\hat{w} = (H^T H)^{-1} H^T y$$

Invertible if:
$$N > 0$$
Complexity of inverse:
$$O(D^3)$$

---
#### Approach 2: Gradient Descent
#GradiantDescent

While not converged:
$$\eta = \text{Learning rate OR step size}$$
$$w^{t+1} \leftarrow w^{t} - \eta \Delta RSS(w^t)$$
$$\Delta RSS(w) = -2 H^T(y - H w)$$
$$w^{t+1} \leftarrow w^{t} - \eta -2 H^T(y - H w)$$
$$\hat{y} = H w$$
$$w^{t+1} \leftarrow w^{t} + 2 \eta H^T(y - \hat{y})$$

##### Interpreting Elementwise
Update to $j^{th}$ feature weight:
$$w_j^{t+1} \leftarrow w_j^{t} + 2 \eta \sum_{i=1}^N h_j (x_i)  (y_i - \hat{y}_i(w^t))$$

- $(y_i - \hat{y}_i(w^t))$ will always be positive
- $y_i$ is the target value.
- $w_j^{t+1} > w_j^t$  means increase

```ad-abstract
title: Summary of Gradient Descent for Multiple Regression

init $w^1 = 0, t = 1$
while || $\Delta RSS(w^t)$ || $> \epsilon$
For j = 0,...,D {
	partial[j] $= -2  \sum_{i=1}^N h_j (x_i)  (y_i - \hat{y}_i(w^t))$
	$w_j^{t + 1} \leftarrow w_j^t - \eta$ partial[j]
}
$t \leftarrow t + 1$
```

---
### Assuming Gaussian Noise
  Model for $\epsilon_i$:
  $$y_i = f(x_i) + \epsilon_i$$
      = h^T(x_i)w + epsilon_i  /// h^T(x_i)w  is a constant
      = NormalDistricution(h^T(x_i), std_error^2)

#### Maximize likelihood estimate of params
  Maximize log-likelihood wrt w
    ln p*(D | w, std_error) = ln(1/std_error*sqrt(2pi))^N ... idk any of the symbols after this part
               N - Sum(from i=1 to N) y_i - Sum_j w_j h_j (x_j)^2
      => Anymax w (Sum from i=1 to N)(y_i - Sum_j w_j h_j (x_j)^2)

      This is "Maximum L Error for Gaussian Error"
          Leads to min RSS!

#### Interpreting the Coefficients 
  - Simple Linear Regression: $\hat{y}^* = \hat{w}_0 + \hat{w}_1 x$
      	- $w_1$ is the slope $(\frac{\Delta price}{ \Delta sq_.feet})$
      	- $w_0$ is the y-intercept

Two Linear Features:
$$\hat{y} = \hat{w}_0 + \hat{w}_1 x[1] + \hat{w}_2 x[2]$$

Multiple Linear Features:
$$\hat{y} = \hat{w}_0 + \hat{w}_1 x[1] + \hat{w}_2 x[2] + ... + \hat{w}_j x[j] + ... + \hat{w}_d x[d]$$

 Polynomial Regression:
$$\hat{y} = \hat{w}_0  + \hat{w}_1 x + ... + \hat{w}_j x^j + ... + \hat{w}_j x^p$$

---

What I am appearantly able to do now:
- Describe Polynomial Regressino
- Write a regression model using multiple inputs or features thereof
- Cast both polynomial regression and regression with multiple inputs as
  regression with multiple features.
- Calculate a goodness-of-fit metric (e.g., RSS)
- Estimate model parameters of a general multiple regression model ot minimize RSS:
      In closed Form
      Using an iterative gradient descent algorithm
- Interpret the coefficients of a non-featureized multiple regression fit
- Exploit the estimated model to form predictions

#### End of Lecture 

---

## Lecture 6 (January 31st, 2022)

TA Problem:
  Linear Regression Exercise

$y = w_1*x + w_0$

$w_1$ is the slope, and $w_0$ is the intercept.

| Weight (x) | 140 | 155 | 159 | 179 | 192 | 200 | 212 |
| ---------- | --- | --- | --- | --- | --- | --- | --- |
| Height (y) | 60  | 62  | 67  | 70  | 71  | 72  | 75  |


$w_1 = [n * (2*x*y)-(Sum(x)*Sum(y))] / [n*Sum(x^2)-Sum(x)^2]$
$w_0 = [2*y - (w_1*Sum(x))] / n$

yCAP = CHECK PACKBACK FOR TRUE yCAP


### End of Lecture 
---

