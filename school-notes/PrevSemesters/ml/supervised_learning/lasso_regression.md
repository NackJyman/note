# Machine Learning
## Lasso Regression
---
##### Lecture 10 (Feburary 11, 2022)
#LassoRegression

Used for Feature Correction

if $\hat w$ is sparse (many zeros), computation only depends of very few features.

$$\hat y_i = \Sigma_{\hat w \not = 0} \hat w_j h_j (x_j)$$

#### An exhaustive approach

Consider all possible models, using each subset of features. This would be about $O(2^D)$. Which would be typically computationally infeasible.

### Choosing Model Complexity
#### Greedy Algorithms
##### Forward Stepwise
Starting from a simple model and iteratvely add features most useful to fit.

##### Backward Stepsize
Start with full model and iteratively remove features least useful to fit

##### Combining forward and backward steps
In forward algorithm, insert steps to remove features no longer as important.
#### Feature Selection
Utiilizing Ridge Regression: $L_2$ regularized regression 

Using regularization fo feature selection

instead of searching over a discrete set of solutions we can use regularization.
- ???
- ???

Thresholding ridge coefficients, a good way of thinking of this is looking at two features that are closly tied together. In the class example we see '# of bathrooms' thresholded with '# of showers.' 

Instead of ridge...

##### Lasso Regression

???

Lasso regression: $L_1$ regularized regression

$$RSS(w) + \lambda||w||_1$$
if $\lambda$ = 0:
$$\hat w_{LASSO} = \hat w_{ls}$$
if $\lambda$ ???:
$$?????$$

The Lasso Regression method requires a cut off point as $\lambda$ increases. As $\lambda$ approaches infinity, all the weights will go to zero. So it is important to stop at a certain point to prevent all the features being zero/null/nothing.

### Fitting the Lasso Regression Model (For a given $\lambda$)
##### Step 1: ???
##### Step 2: ???
##### Step 3: ???

Optimizing the lasso objective

Lasso total cost: $$RSS(w) + \lambda||w||_1$$

There is no closed form solution here so we must use gradiant descent. 

### Coordinate Descent

Goal: Minimize some function g

Coordinate descent
> initialize $\hat w = 0$ (or smartly...)
> while not converged:
>> pick a coordinate j
>> $\hat w_j \leftarrow \text{min }w_j ( \hat w_0, ... w_{j-1}, \hat w_{j-1}, \hat w_{j-1}, ..., ... \hat w_D)$

There is no step size to use. Converges to optimum in some cases, "Strongly convex" function. The coordinate descent is also utilized in the Lasso Regression model.

### Normalizing Features

Scale training columns (not rows) as:
$$h_j(x_k)=\frac{h_j (x_k)}{\sqrt{\Sigma_{i=1}^{N} h_j (x_i)^2}}$$

Normalization important for hw1. 
Normalizer: $Z_j$
$$\sqrt{\Sigma_{i=1}^{N} h_j (x_i)^2}$$

### Coordinate descent for lasso
> initialize $\hat w = 0$ (or smartly...)
> while not converged:
> for j = 0,1,...,D:
>> Compute: $p_j = \Sigma_{i = 1}^{N} \underline h_j (x_i) (y_i - \hat y_i (\hat w_{\not j}))$
>> set: PIECE WISE FUNCTION YOU NEED TO PULL FROM SLIDES$

#### Convergence Criteria

As Coordinate Descent progresses the steps get smaller and smaller, and if the change in step is less then a specified value or *threshold*. Stop and exit the descent.

#### Coordinate Descent for lasso with normalized features

Precompute thing

### Choosing $\lambda$

Fit $\hat w_\lambda$ to the training set
Test performance of $\hat w_\lambda$ to select the best $\lambda$ ($\lambda^*$) 
Then use the test set to assess generalization error of $\hat w_{\lambda^*}$

### Conclusion
Cons: Random Selection

---
---

hw 1 comments 

d o it on ur oWn
numpy is ok

determine coefficient wit gradient descent
polyfit() is ok i think

##### coordinate descent

normalized h needs computed
##### cross-validation

K-fold cross validation 

---