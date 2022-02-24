# Machine Learning HW1
### Jack Nyman
---
##### Note To Grader:
I really struggled with this assignment dispite the classwide extentions (which truely did help). The code implementations here are really really rough. I struggled alot with the implementation of these algorithms, as you will see. I spent alot of time trying to organize and do things in different ways that made the end product really disorganized and hard to follow. I was hoping to implement everything I wanted to (correctly) and then go back over and organize everything nicely. I unfortunately was unable to do so, espcially with Lasso Regression. I should have reached out and asked questions but I was not even sure what to ask specifically. I do apologize for the lack of quality in this assignment.

## Linear Regression
### Simple Linear Regression
| Dataset              | x1                | x2                 | x3                 |
| -------------------- | ----------------- | ------------------ | ------------------ |
| Figure               | ![[Figure_2.png]] | ![[Figure_3.png]]  | ![[Figure_4.png]]  |
| General Error | 9.653805515747994 | 38.469036926556115 | 60.321185725647695 |

For collecting error, I saved the y-intercept and slope of each linear regression to construct $\hat y$, such that $\hat y = f(x_i) = b_0 + c_0 x$. For each model I computed $(\hat y - y)^2$ for each data point in the saved the error as the sum.

---
### Polynomial Regression
#### x1

| $$p = 2$$                 | $$3$$                     |
| --------------------- | --------------------- |
| ![[polyReg_x1-2.png]] | ![[polyReg_x1-3.png]] |
| $$p=4$$                   | $$5$$                     |
| ![[polyReg_x1-4.png]] | ![[polyReg_x1-5.png]] |

###### Error for x1
| p     | 2        | 3        | 4        | 5        |
| ----- | -------- | :------: | -------- | -------- |
| Error | 33.56886 | 21.60946 | 20.80479 | 5.478342 |

#### x2
| $$p = 2$$                 | $$3$$                     |
| --------------------- | --------------------- |
| ![[polyReg_x2-2.png]] | ![[polyReg_x2-3.png]] |
| $$p=4$$                   | $$5$$                     |
| ![[polyReg_x2-4.png]] | ![[polyReg_x2-5.png]] |

###### Error for x2
| p     | 2        | 3        | 4        | 5        |
| ----- | -------- | -------- | -------- | -------- |
| Error | 62.04830 | 68.50858 | 68.38507 | 64.25906 |

#### x3
| $$p = 2$$                 | $$3$$                     |
| --------------------- | --------------------- |
| ![[polyReg_x3-2.png]] | ![[polyReg_x3-3.png]] |
| $$p=4$$                   | $$5$$                     |
| ![[polyReg_x3-4.png]] | ![[polyReg_x3-5.png]] |

###### Error for x3
| p     | 2        | 3        | 4        | 5        |
| ----- | -------- | -------- | -------- | -------- |
| Error | 60.39633 | 54.20528 | 54.62547 | 52.45366 |

For this section I collected the model of each x-dataset and saved it to a matrix. I then iterated through each colum represented by $p$ and row which was related to whatever x-dataset being operated on. I then compared the input to these models to y for each dataset. I have come to the following conclusion:

| Data Set     | $x_1$ | $x_2$ | $x_1$ |
| ------------ | ----- | ----- | ----- |
| Best fit $p$ | 5     | 2     | 3     |

This was determined by selecting the 'p' value that resulted in the lowest squeared error.

---

### Multiple Regression

![[MulReg_x1.png]]
R squared: 80.35
Mean Absolute Error: 0.8920249634747442
Mean Square Error: 1.1016003173111828
Root Mean Square Error: 1.049571492234418

---

## 2 Regularization Constants

### 2.1 LASSO Regression

Loss Function under LASSO regression is:

$$E_L = \Sigma_{i=1}^n (\textbf{Y}_i - (\hat w + \textbf{X}_i \hat w))^2 + \lambda || \hat w_i ||$$
and $\lambda$ is our regularization constant.And $$\lambda ||\hat w ||_1 = \lambda \Sigma_{i=1}^d |\hat w_i|$$
#### 1.	Suppose $\lambda$ is too small, how will this affect the magnitude of:
##### The Error on the training set?

If $\lambda$ is too small the adjustments during optimization will be much too small and there would be minimal feature selection. So the error on the training set would increase or just not change much at all. In fact if it is so low then we are just doing linear regression.

##### The Error on the testing set?

The error on the testing set would be affected similarly to the error on the training set.

##### $\hat w$?

Will be the same as $\hat w_ls$ ($\hat w$ from linear regression). 
	
##### The number of nonzero elements of $\hat w$?

Will stay the same through training, no nonzero elements will be found through feature minimization.
	
#### 2. Suppose instead that we overestimated $\lambda$. What do expect the magnitude of:
##### The Error on the training set?

The training error would become significant very fast with a high $\lambda$. Every feature would likely be minimized to zero and the model will not see any input as significant and the squared error would be the sum of the real y-values squared.
	
##### The Error on the testing set?

Very similar to the training set as the model will not find any significance in any feature so the training set error would be the sum of the testing y values squared.
	
##### $\hat w$?

$\hat w$ would go to zero.
	
##### The number of nonzero elements of $\hat w$?

Every feature would go to zero, so the every element would go to zero. So there would be no nonzero elements of $\hat w$.

### 2.2 Ridge Regression
Loss Function under LASSO regression is:

$$E_R = \Sigma_{i=1}^n (\textbf{Y}_i - (\hat w_0 + \textbf{X}_i \hat w))^2 + \lambda || \hat w_i ||_2^2$$
and $\lambda$ is our regularization constant. And $$\lambda||w||_2^2 = \lambda \Sigma_{i=1}^d (\hat w_i)^2$$

###### Calculate the partial derivative of the penalty term in $E_L$.

$$\frac{\partial ||\hat w_i||_1}{\partial \hat w_i} = \lambda$$

###### Calculate the partial derivative of the penalty term in $E_R$.

$$\frac{\partial ||\hat w_i||^2_2}{\partial \hat w_i} = \lambda \Sigma_{i=1}^d 2(\hat w_i)$$

###### When $|\hat w_i| < 1/2$, which method "pushes away" more, Ridge or Lasso?

Ridge regression pushes more because squaring the penalty term will dramatically increase the "push" rather than the absolute value. As shown by the above pertial derivatives.

---
## 3. Lasso Regression

![[LassoRegression.png]]
This is so wrong and bad and im so sorry but it looks cool :).

I was unable to get this section working in any real capacity, apologies.

---
---


## Python Code Files
###### Do not include in PDF
```ad-hint
title: Python Code for hw1_p1
linearRegression.py
```py
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt

# Probably Illegal (Use until you can do it right)
import statsmodels.api as sm
import seaborn as sns
# Simple Linear Regression
#   Things to do:
#       1. Read in data from LR.csv
#       2. Run simple Linear Regression to predict y from x
#         - Report found linear model
#         - Predict value of y for new x-values 1, for 2, and for 3.
#       3. Use Cross-Validation to predict generalization error
#         - Describe Procedure and Analysis
#       4. Run Polynomial Regression for p = 2,3,4,5
#         - Report models for each:
#            - For each model predict new y value for new x of 1, for 2, for 3
#       5. Use Cross-Validation
#         - Describe Procedure and Analysis
# Multiple Regression
#       1. Run Linear Regression sumultaneously using all three
#               explanatory Variables.
#        - Predict the value of y for new (x1, x2, x3) Values (1, 1, 1), for
#                   (2, 0, 4), and for (3,2,1)
#        - Use Cross Validation with single data point (x1,x2,x3,y) from model f

##### Helper Functions
# Create and save a plot of given data points and their interpelation.
def createScattlerInterpPlot(xData, xLabel, yData, yLabel, yHat, pngDir):
    plt.scatter(xData, yData)
    fig = plt.plot(xData, yHat, lw=4, c='orange', label='Regression Line')
    plt.xlabel(xLabel, fontsize = 20)
    plt.ylabel(yLabel, fontsize = 20)
    plt.savefig(pngDir)
    plt.close()

# Returns the generalization error of the predicted output to the actual output
def genError(input, predictedOut, yhat):
    error = 0
    for x in input:
        for y in predictedOut:
            error =+ (yhat(x) - y)**2
    return error



### Load in CSV into respective matrix
df = pd.read_csv('LR.csv', names=('x1','x2','x3','y'))
data = df.to_numpy()
print(data)
x1 = data[:, 0]
x2 = data[:, 1]
x3 = data[:, 2]
y = data[:, 3]

xDatas = [x1, x2, x3]


print("-- Linear Regression --")
### Linear Regression from x1 -> y
# is 'sm' illegal code ?
x = sm.add_constant(x1)
results = sm.OLS(y,x).fit()
b0 = results.params[0]
b1 = results.params[1]
# Graph Linear Regression from x1 to y
yhat = b0 + b1*x1
createScattlerInterpPlot(x1, 'x1', y, 'y', yhat, "figures/linReg_x1.png")

### Linear Regression from x2 -> y
x2s = sm.add_constant(x2)
results = sm.OLS(y,x2s).fit()
b02 = results.params[0]
b12 = results.params[1]
# Graph Linear Regression from x2 to y
yhat = b02 + b12*x2
createScattlerInterpPlot(x2, 'x2', y, 'y', yhat, "figures/linReg_x2.png")

### Linear Regression from x3 -> y
x3s = sm.add_constant(x3)
results = sm.OLS(y,x3s).fit()
b03 = results.params[0]
b13 = results.params[1]
# Graph Linear Regression from x3 to y
yhat = b03 + b13*x3
createScattlerInterpPlot(x3, 'x3', y, 'y', yhat, "figures/linReg_x3.png")

### Linear Regression Cross Validation
# yhat Functions
yhat1 = lambda x: b0 + b1*x
yhat2 = lambda x: b02 + b12*x
yhat3 = lambda x: b03 + b13*x

yhats = [yhat1, yhat2, yhat3]

print("Generalized Error of x1 -> y: " + str(genError(x1, y, yhat1)))
print("Generalized Error of x2 -> y: " + str(genError(x1, y, yhat2)))
print("Generalized Error of x3 -> y: " + str(genError(x1, y, yhat3)))



### Polynomial Regression for p = 2,3,4,5
# Repeat for x1,x2,x3
print("-- Polynomial Regression --")
# Write function to do polyfit stuff
def polyGraph(x1Data, x2Data, x3Data, yData, p):
    mymodelx1 = np.poly1d(np.polyfit(x1Data, yData, p))
    myline = np.linspace(0, 5, 50)
    plt.scatter(x1Data, yData)
    plt.plot(myline, mymodelx1(myline))
    plt.xlabel('x1', fontsize = 20)
    plt.ylabel('y', fontsize = 20)
    plt.savefig("figures/polyReg_x1-" + str(p) + ".png")
    plt.close()
    mymodelx2 = np.poly1d(np.polyfit(x2Data, yData, p))
    myline2 = np.linspace(0, 5, 50)
    plt.scatter(x2Data, yData)
    plt.plot(myline2, mymodelx2(myline2))
    plt.xlabel('x2', fontsize = 20)
    plt.ylabel('y', fontsize = 20)
    plt.savefig("figures/polyReg_x2-" + str(p) + ".png")
    plt.close()
    mymodelx3 = np.poly1d(np.polyfit(x3Data, yData, p))
    myline3 = np.linspace(0, 5, 50)
    plt.scatter(x3Data, yData)
    plt.plot(myline3, mymodelx3(myline3))
    plt.xlabel('x3', fontsize = 20)
    plt.ylabel('y', fontsize = 20)
    plt.savefig("figures/polyReg_x3-" + str(p) + ".png")
    plt.close()
    print("Saved 3 figures with p=" + str(p))
    return([mymodelx1, mymodelx2, mymodelx3])

ps = [0,1,2,3]
polyModels = [[], [], [], []]
# polyModels will be a 4x3 Matrix
#   Where each column is represents p       (p = 2,3,4,5)
#            Each row is represents x-group (x1)
#                                           (x2)
#                                           (x3)

for p in ps:
    polyModels[p] = polyGraph(x1, x2, x3, y, (p+2))

# Cross Validate each model
gErrorXp2 = [0,0,0]
gErrorXp3 = [0,0,0]
gErrorXp4 = [0,0,0]
gErrorXp5 = [0,0,0]

for curModel in polyModels[0]: # Iterates through each xN yhat Function
    gErrorXp2[0] = genError(x1, y, curModel)
    gErrorXp2[1] = genError(x2, y, curModel)
    gErrorXp2[2] = genError(x3, y, curModel)

for curModel in polyModels[1]: # Iterates through each xN yhat Function
    gErrorXp3[0] = genError(x1, y, curModel)
    gErrorXp3[1] = genError(x2, y, curModel)
    gErrorXp3[2] = genError(x3, y, curModel)

for curModel in polyModels[2]: # Iterates through each xN yhat Function
    gErrorXp4[0] = genError(x1, y, curModel)
    gErrorXp4[1] = genError(x2, y, curModel)
    gErrorXp4[2] = genError(x3, y, curModel)

for curModel in polyModels[3]: # Iterates through each xN yhat Function
    gErrorXp5[0] = genError(x1, y, curModel)
    gErrorXp5[1] = genError(x2, y, curModel)
    gErrorXp5[2] = genError(x3, y, curModel)

polyGenError = [gErrorXp2,gErrorXp3,gErrorXp4,gErrorXp5]

print("- x1")
p=2
for xpN in polyGenError:
    print("| p = " + str(p) + " | " + str(xpN[0]) + "|")
    p += 1

print("- x2")
p=2
for xpN in polyGenError:
    print("| p = " + str(p) + " | " + str(xpN[1]) + "|")
    p += 1

print("- x3")
p=2
for xpN in polyGenError:
    print("| p = " + str(p) + " | " + str(xpN[2]) + "|")
    p += 1

### Multiple Regression
print("-- Multiple Regression --")

x = df[['x1', 'x2', 'x3']]
y = df['y']
df.head()
#Splitting the dataset
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = 0.3, random_state = 100)

train_test_split(x, y, test_size = 0.3, random_state = 100)

from sklearn.linear_model import LinearRegression

mlr = LinearRegression()
mlr.fit(x_train, y_train)

print("Intercept: ", mlr.intercept_)
print("Coefficients:", mlr.coef_)
list(zip(x, mlr.coef_))

# Make Graph in plt
mulRegress  = lambda x: mlr.intercept_ + mlr.coef_[0]*x + mlr.coef_[1]*x +  mlr.coef_[2]*x

xline = np.linspace(0, 5, 50)
plt.scatter(x1, y)
plt.scatter(x2, y)
plt.scatter(x3, y)
plt.plot(xline, mulRegress(xline))
plt.xlabel('x', fontsize = 20)
plt.ylabel('y', fontsize = 20)
plt.savefig("figures/MulReg_x1.png")
plt.close()


y_pred_mlr= mlr.predict(x_test)
print("Prediction for test set: \n {}".format(y_pred_mlr))

mlr_diff = pd.DataFrame({'Actual value': y_test, 'Predicted value': y_pred_mlr})
mlr_diff.head()

from sklearn import metrics

meanAbErr = metrics.mean_absolute_error(y_test, y_pred_mlr)
meanSqErr = metrics.mean_squared_error(y_test, y_pred_mlr)
rootMeanSqErr = np.sqrt(metrics.mean_squared_error(y_test, y_pred_mlr))

print('R squared: {:.2f}'.format(mlr.score(x,y)*100))
print('Mean Absolute Error:', meanAbErr)
print('Mean Square Error:', meanSqErr)
print('Root Mean Square Error:', rootMeanSqErr)
```

```ad-hint
title: Python Code for hw1_p3

lassoRegression.py
```py
import numpy as np
import pandas as pd
import matplotlib as mpl
import matplotlib.pyplot as plt

# Code go here

# Dataset is local crime statistics for 1,994 US Communities.
#   y, the dependent variable is the crime rate. "ViolentCrimesPerPop"
#           First column of df_train
#   There are 95 independent variables (x's)

# Implement Coordinate descent LASSO algorithm.
class LassoRegression():
    def __init__( self, learning_rate, iterations, l1_penality ):
        self.learning_rate = learning_rate
        self.iterations = iterations
        self.l1_penality = l1_penality

    # Function for model training
    def fit( self, X, Y ):
        # no_of_training_examples, no_of_features
        self.m, self.n = X.shape
        # weight initialization
        self.W = np.zeros( self.n )
        self.b = 0
        self.X = X
        self.Y = Y
        # gradient descent learning // Needs to be Coordinate Descent (while not converged)
        for i in range( self.iterations ) :
            self.update_weights()
        return self

    # Helper function to update weights in gradient descent
    def update_weights( self ):
        Y_pred = self.predict( self.X )
        # calculate gradients
        dW = np.zeros( self.n )
        for j in range( self.n ) :
            if self.W[j] > 0 :
                dW[j] = ( - ( 2 * ( self.X[:, j] ).dot( self.Y - Y_pred ) )
                    + self.l1_penality ) / self.m
            else :
                dW[j] = ( - ( 2 * ( self.X[:, j] ).dot( self.Y - Y_pred ) )
                    - self.l1_penality ) / self.m
        db = - 2 * np.sum( self.Y - Y_pred ) / self.m
        # update weights
        self.W = self.W - self.learning_rate * dW
        self.b = self.b - self.learning_rate * db
        return self

    # Hypothetical function h( x )
    def predict( self, X ) :
        return X.dot( self.W ) + self.b


def main() :
    # Importing dataset
    df_train = pd.read_table("crime-train.txt")
    df_test = pd.read_table("crime-test.txt")
    X_train = df_train.drop("ViolentCrimesPerPop", axis = 1).values
    Y_train = df_train["ViolentCrimesPerPop"].values
    X_test = df_test.drop("ViolentCrimesPerPop", axis = 1).values
    Y_test = df_test["ViolentCrimesPerPop"].values
    # Splitting dataset into train and test set
    # X_train, X_test, Y_train, Y_test = train_test_split( X, Y, test_size = 1 / 3, random_state = 0 )

    # Model training
    model = LassoRegression( iterations = 5000, learning_rate = 0.01, l1_penality = 600 )
    model.fit( X_train, Y_train )
    # Prediction on test set
    Y_pred = model.predict( X_test )
    print( "Predicted values ", np.round( Y_pred[:5], 2 ) )
    print( "Real values	 ", Y_test[:5] )
    print( "Trained W	 ", round( model.W[0], 2 ) )
    print( "Trained b	 ", round( model.b, 2 ) )
    # Visualization on test set
    # plt.scatter( X_test, Y_test, color = 'blue' )
    plt.plot( X_test, Y_pred) #, color = 'orange' )
    plt.title( 'Violent Crimes Per Population' )
    plt.xlabel( 'X Data' )
    plt.ylabel( 'Y Data' )
    plt.show()

if __name__ == "__main__" :
    main()
```
