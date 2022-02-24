# Machine Learning
## Linear Classifiers 
---
##### Lecture 14 (Feburary 16, 2022)

End of Supervised Learning Lectures

#### Example
Descriptors of words and it's asociated meaning

| Feature          | Coefficient |
| ---------------- | ----------- |
| good ($h_0$)     | 1.0         |
| great ($h_1$)    | 1.2         |
| awesome ($h_2$)  | 1.7         |
| bad ($h_3$)      | -1.0        |
| terrible ($h_4$) | -2.1        |
| awful ($h_5$)    | -3.3        |

The above table an example of the matrix $H$. 
A Linear Classifier scores  

#### Generic Definition
Model: $$\hat y = \text{sign(Score)}(x_i)$$

$$\text{Score}(x_i) = w_0 h_0 (x_i) + w_1 h_1 + ... + w_D h_D (x_i)$$
$$= \Sigma_{j=0}^D w_j h_j ...$$

---

| Input       | Coefficient | Value |
| ----------- | ----------- | ----- |
| -           | $w_0$       | 0.0   |
| # Awesome   | $w_1$       | 1.0   |
| # Something | $w_2$       | -1.7  |


Score = $w_0 + w_1$ * (Numebr of occurences of that word)

---

### Using probablity in Classification

### Learning Conditional Probabilities from data