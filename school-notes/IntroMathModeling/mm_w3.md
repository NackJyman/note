# Intro to Math Modeling
---
##### September 6th, 2022
---

#### Conceptual Models covered so far
- Exponetial model
- Logistic model

### 1.4.2 Polynomial Models

```chart
type: line
labels: [1700,1800,1900,2000,2100]
series:
  - title: P
    data: [.01,.01,.1,.3,.45]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtZero: true
```

#### 1. Quadratic Polynomial
$P = P_1 \frac{(t - t_2)(t - t_3)}{(t_1 - t_2)(t_1 - t_3)} + P_2\frac{(t - t_1)(t - t_3)}{(t_2 - t_1)(t_2 - t_3)} + P_3\frac{(t - t_1)(t - t_2)}{(t_3 - t_1)(t_3 - t_2)}$

#### 2. Linear Model 

$P = 0.28 + \frac{t -2000}{322} = \frac{t - 1970}{322}$

### 1.4.3 Model Evaluation

|                                      | Properties of Reasonable Models                                        | Properties of Optimal Models                                          |     |     |
| ------------------------------------ | ---------------------------------------------------------------------- | --------------------------------------------------------------------- | --- | --- |
| Support Via Observations             | Agreement with trend of observations.                                  | Accurate agreement with observations.                                 |     |     |
| Range of Agreement with Observations | Agreement with observations over a **certain** range of **one** case.  | Accurate agreement with all observations of one class of problems.    |     |     |
| Additional Info                      | Information in addition to observations results from using a function. | Information in addition to observations results from using a concept. |     |     |
| Adavantage of Model                  | Interpolation and extrapolation of info given by observations.         | Valuable new insight into the mechanism of observed processes.        |     |     |

###### Conceptual Models:
Pros
- Based on Concept

Cons
- Only support for certain range
- Valuable insight: No

###### Polynomial Models
Pros
- Range of applicability

Cons
- No explanations of mechanism

### 1.5 Advantage of Modeling: Global Warming Modeling

The goal of modeling is to explain and understand the data and why it happens. Looking for understanding. This example is focusing on the greenhouse effect.

#### 1.5.1 Greenhouse Effect

With greenhouse gasses: **+14 C** (_mean earth temperature_)

Without greenhouse gases: **-18 C**

Let's start with defining a formula to calculate temperature and $CO_2$ levels with respect to time
$$T = T(t) \rightarrow T = [t(CO_2)]$$
$$CO_2 = CO_2(t) \rightarrow t = f(CO_2)$$

