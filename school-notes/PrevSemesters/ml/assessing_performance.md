# Machine Learning
## Assessing Performance 
---
##### Lecture 7 (Feburary 2, 2022)
#Performance #Overfitting #Underfiting

Measuring Loss:
Loss Function:
$L(y, f_{\hat{w}}(x))$
- y is the actual value
- $f_{\hat{w}}(x)$ is the predicted value y^
- $\hat{w}$ is best parameter values


Assesing Loss:
Training error
- Define training data

EX: Fit quadratice to minimize RSS. $\hat{w}$ minimizes RSS of trainnig data.

        Computing Training Error
          1. Defiine Loss function
          2. Training Error = avg. loss on houses in training set.
                  1/N*(Sum )

        Generalization (True) Error
          Definition:
            generalization error = E_(x,y)[L(y,f^wCap(x))]
              - average over all possible (x,y) pairs weighted by how likely
                  each is
            = Integral (L(y,f^wCap(x) * p(x,y) dxdy))
            ** Cannot be calculated in reality because you cannot hava every
            pair.

        Test Error
          - Approximating the generalization error.
              (estimating loss over all pairs)
              Test set should be a proxy for "Everything you might see"
          - Definition
              Avg. Loss on houses in test set.
              = 1/N_test*(Sum)(L(y_i, f_wCap(x_i)))

Overfitting is something to look out for.
  1. Training Error < Test error
  2. True Error > Test Error

---

## End of Lecture 

##### Lecture 7 (Feburary 4th, 2022)

---
  Assessing Performance

Split data into a training set, and a test set. Must find optimal balance to minimize generalization errors. Too few for training. Research "Cross validations"

*** Idea ***********
  Would it be possible to run an script to optimize model's data sets.
  Gradiant Descent but instead of adjusting the parameters we are adjusting the
  data set.
********************

3 Sources of Error
1. Noise
2. Bias contribution
3. Variance contribution

        High Complexity -> High Variance Error
        High Complexity -> Low Bias Error

Data size:
The smaller data we have to train with the higher the true error would be this can be decreased by growing the data set but would decrease the true error but grow the training error.
## End of Lecture 
---
---


