# Intro to Math Modeling
### Week 2
---
##### August 30th, 2022
---

## 1.3 Model Development

### 1.3.1 Lagrangian Form of Polynomials
- ##### Linear Polynomials
	- Imagine two points ($x_1, y_1$) and  ($x_2, y_2$) with a straight line crossing the points. Assume that $y = a_0 + a_1 x$, assume $a_0, a_1$ are unknown constants. Define the function in terms of ($x_1, y_1$) and  ($x_2, y_2$). $y_1 = a_0 + a_1 x_1$ and $y_2 = a_0 + a_1 x_2$
	- $y = a_0 + a_1 x \rightarrow$ 
	- ###### $P_1(x) = y_1 \frac{x - x_2}{x_1 - x_2} + y_2 \frac{x - x_1}{x_2 - x_1}$
	- $P_1(x_1) = y_1, \ \ P_1(x_2) = y_2$
- ##### Quadradic Polynomials
	- Imagine three points on a parabola, ($x_1, y_1$), ($x_2, y_2$), ($x_3, y_3$). Here we consider the equation $y = a_0 + a_1x + a_2 x^2$.
	- We must construct a system of equations:
		- $y_1 = a_0 + a_1 x_1 + a_2 x_1^2$
		- $y_2 = a_0 + a_1 x_2 + a_2 x_2^2$
		- $y_3 = a_0 + a_1 x_3 + a_2 x_3^2$
	- ###### $P_2(x) = y_1 \frac{(x-x_2)(x-x_3)}{(x_1 - x_2)(x_1 - x_3)} + y_2 \frac{(x-x_1)(x-x_3)}{(x_2 - x_1)(x_2 - x_3)} + y_3 \frac{(x-x_1)(x-x_2)}{(x_3 - x_1)(x_3 - x_2)}$ 

##### Lagrangian Form of Polynomials:

($n + 1$) data $\rightarrow$ polynomial of order $n$.
$P_n(x) = y_0 L_0(x) + y_1 L_1(x) + ... + y_n L_n(x)$
$L_k(x) = \Pi_{i = o, i \not = k}^n \frac{x - x_i}{x_k - x_i} = \frac{(x-x_0)(x-x_1)...(x-x_{k-1})(x - x_{k+1})...(x-x_n)}{(x_k-x_0)(x_k-x_1)...(x_k-x_{k-1})(x_k - x_{k+1})...(x_k-x_n)}$

### 1.3.2 Properties of Polynomials
#### Exact Polynomial Models
##### Advantages:
- Can be easily integrated and differentiated. 
- Easy to find minimum and maximum.
- Easy to find global values. (Work, Probability)

##### Erronious Data:
1. Landitions for experiments change
2. Characteristic errors of measurements
3. Round-off errors
4. Incorrect measurements

###### Round-Off Error Exmaple
| x     | (a) y  | (b) y | (c) y | (d) y | (e) y     | (f) y |
| ----- | ------ | ----- | ----- | ----- | --------- | ----- |
| 1     | 1.0001 | 1.001 | 1.01  | 1.05  | 1.00      | 1.00  |
| 2     | 1.9998 | 1.998 | 1.98  | 1.90  | 2.00      | 2.00  |
| 3     | 3.0003 | 3.003 | 3.03  | 3.15  | 3.00      | 3.00  |
| 4     | 3.9996 | 3.996 | 3.96  | 3.8   | 4.04      | 4.80  |
| 5     | 5.0005 | 5.005 | 5.05  | 5.25  | 5.00      | 5.00  |
| 6     | 5.9994 | 5.994 | 5.94  | 5.70  | 6.00      | 6.00  |
| 7     | 7.0007 | 7.007 | 7.07  | 7.35  | 7.00      | 7.00  |
| 8     | 7.9992 | 7.992 | 7.92  | 7.60  | 8.00      | 8.00  |
|       |        |       |       |       |           |       |
| Noise | 0.01%  | 0.1%  | 1%    | 5%    | 1% Errror | 20% Error   |
|       |        |       |       |       |           |       |

```chart
type: line
labels: [1,2,3,4,5,6,7,8]
series:
  - title: (a) y
    data: [1.0001,1.9998,3.0003,3.9996,5.0005,5.9994,7.0007,7.9992]
  - title: (b) y
    data: [1.001,1.998,3.003,3.996,5.005,5.994,7.007,7.992]
  - title: (c) y
    data: [1.01,1.98,3.03,3.96,5.05,5.94,7.07,7.92]
  - title: (d) y
    data: [1.05,1.90,3.15,3.8,5.25,5.70,7.35,7.60]
  - title: (e) y
    data: [1.00,2.00,3.00,4.04,5.00,6.00,7.00,8.00]
  - title: (f) y
    data: [1.00,2.00,3.00,4.80,5.00,6.00,7.00,8.00]
width: 100%
beginAtZero: true
```

### 1.3.3 Application of Exact Polynomials

Energy Consumption Example (not what he wrote on board):

```chart
type: line
labels: [20,40,60,80,100]
series:
  - title: Energy Consumption
    data: [1,4,6,8,10]
tension: 0.52
width: 93%
labelColors: false
fill: false
beginAtZero: false
```

If we try and fit this model to go through every point available the model can be overfit and lose it's meaning when we use it to extrapolate. 

---
##### September 1st, 2022
---

### 1.4 Model Evaluation: Population Modeling

###### Example of US Population 1790-2050
```chart
type: line
labels: [1700, 1800, 1900, 2000, 2100]
series:
  - title: Population
    data: [0,.05,.2,.45,.7]
tension: 0.43
width: 80%
labelColors: false
fill: false
beginAtZero: false
```

L(&@#$) Driven Approachs:

Idea[Law] $\rightarrow$ model

Data Driven Approachs:

function $\rightarrow$ model

#### 1.4.1 Conceptual Models

##### a. Exponetial Growth [Malthusian Law]

$P = P_0 e^{r(t - t_0)}$

We want to linearize this.

In order to do that, we will utilize natural logs.

$ln(P) = ln(P_0) + r(t - t_0)$

The given values in this example are $P_0, t_0$
Once we linearize the equation imagine the output looks like this

```chart
type: line
labels: [1700, 1800, 1900, 2000, 2100]
series:
  - title: Population (Ln P)
    data: [-5,-5,-2,-1.5,-1.25]
tension: 0.09
width: 80%
labelColors: false
fill: false
beginAtZero: false
```
From this graph we can see that the model is not fitting the data in a linear fashion so something is off.

$ln(P) = ln(0.0039) + \frac{t - 1790}{35} \rightarrow P = 0.0039 e^{\frac{t - 1790}{35}}$

*insert expoential growth graph cover 1780 - 1900, looks like * $x^2$

When the data points are so close to the model we can expect the error of the model to be less than 1%.

##### b. Logistic Growth

This is an alternate approach to differential equations.

$P = P_c(1 + \frac{1 - e^{\frac{t - t_c}{\tau}}}{1+e^{-\frac{t - t_c}{\tau}}}) = \frac{P_c}{1 + e^{-\frac{r-t_c}{\tau}}}$

as population grows to infinity, $P_{\infty} = 2P_c$ 

*insert drawing of some kinda line that grows and levels out.*

$ln(\frac{2P_c}{P} - 1) = -\frac{t -t_c}{\tau}$

$P_c$ is an unknown and we will not be able to plot the left hand side because of that. So to provide this information, we must provide a reasonable guess that

$P_c = .06$ Which is the point where the population levels off. There is a level of uncertainty with this. 

Is there a way to determine if a selected value works? This comes with the modeling process. The line will be different based off the setting of $P_c$. What is a reasonable way to ask if the $P_c$ is the best possible $P_c$. 

When we plot this, the point where the model begins to fail and not to follow the data anymore, It is also benefitual to consider multiple $P_c$ points and compare the linear relation between the right hand side and $t$. We will look at the simularity of setting $P_c$.

$ln(\frac{0.42}{P} - 1) = -\frac{t - t_c}{\tau}$

When the model is not linear, the model fails and we need to get rid of it. An error of 5% is usually a solid thershold to consider the model seriously. The important aspect of it all is to linearly represent the trends within the data.

$ln(\frac{0.12}{P} - 1) = -\frac{t - 1890}{29} \rightarrow \frac{0.12}{1+e^{-\frac{-1890}{29}}}$

Then plot the model again uses new thing (1780 - 1900). With a curve that goes up and then levels off, not exponetial but different. 

---


