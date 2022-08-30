# Intro to Math Modeling
### Intro
##### August 23rd, 2022
---

## Deterministic Analysis of Observations

Take an arbitrary amount of data and come to conclusions about them. Finding lines and/or quadratic equations to aproximate the data.

#### Motivation

	A. Simple Problems
		- 1. Motion of astrial objects (stars, planets, galaxies...)
			- Agriculture, Navigation
		- 2. Energy Supplies
			- Current use, and future needs
		- 3. Daily Situations: distace to car in front of you
	B. Complicated Problems
		- 1. Global Warming
		- 2. Technical Developments
			- Optimization of flow and chemical reactions
			- Flow around wind turbines
		- 3. Weather Prediction
			- Blizzards, Hurricanes, Tornados

---
## Data Transformations: Linear Models
### Concept
*Insert non-linear plot here*

#### Idea: $y= ae^{bx}$

Take $ln(y) = bx + ln(a)$
And rewrite it as $Y = AX + B$

Now *it is a* linear function of big $X$.

#### Does this work?

Plot the new *seemingly* linear equation to see if it appears linear.

#### $A,B \rightarrow a,b$
You can calculate $a, b$ from $A, B$

#### Test Model in the original equation/setup
Use the original model, with $a,b$ from previous step. 

#### Calculate the Model Error
Relative Difference between model data and the original set (normalize in some way).

#### Apply Model
Come to conclusions about the model, identify relationships.

---

##### August 25th, 2022

## 1.2.3 Kepler's Third Law
### k1
Planets orbit in eliptical orbit

### k2
Discovered that $\frac{dA}{dt}$ is constant. 

### k3
$T_p = f(r)$
$r$: Distance from sun
$T_p$: Orbital period in earth years

| Planets | r($10^9 km$) | $T_p$    |
| ------- | ------------ | -------- |
| Mercury | 0.00579      | 0.2408   |
| Venus   | 0.1082       | 0.6152   |
| Earth   | 0.1496       | 1.0000   |
| Mars    | 0.2280       | 1.8808   |
| Jupiter | 0.7785       | 11.8618  |
| Saturn  | 1.4335       | 29.4566  |
| Uranus  | 2.8718       | 84.0107  |
| Neptune | 4.4948       | 164.7858 |

```chart
type: line
labels: [Mercury,Venus,Earth,Mars,Jupiter,Saturn,Uranus,Neptune]
series:
  - title: T_p
    data: [0.2408,0.6152,1.0000,1.8808,11.8618,29.4566,84.0107,164.7858]
width: 90%
beginAtZero: true
```


#### Idea: $$T_p = Ar^B$$

Turn the equation into an analysis problem

#### $$ln(T_p) = B*ln(r) + ln(A)$$
```chart
type: line
labels: [-3,-2,-1,0,1,2]
series:
  - title: Linearized Data
    data: [-2,0,2,4,6]
tension: 0.77
width: 90%
labelColors: false
fill: false
beginAtZero: false
```

#### $$ln(T_p) = 2.8490 + 1.4996*ln(r)$$

Where 2.8490 = $ln(A)$ and 1.4996 = $B$. Therefore

#### $$T_p = e^{2.849} r^{1.4996}$$
#### $$T_p = e^{2.849} r^{\frac{3}{2}}$$
#### $$T_p = \sqrt{\frac{4\pi^2}{Gs}r^3}$$

$Gs$ is a standard gravitational parameter, equal to $1.3291 * 10^{20} \frac{m^3}{s^2}$

```chart
type: line
labels: [1,2,3,4,5]
series:
  - title: T_p
    data: [10, 20, 50,100, 170]
tension: 0.2
width: 90%
labelColors: false
fill: false
beginAtZero: false
```

```chart
type: line
labels: [1,2,3,4,5]
series:
  - title: Error
    data: [0, -.2, -.3, .1, 0]
tension: 0.2
width: 80%
labelColors: false
fill: false
beginAtZero: false
```
$|e| < 0.6\%$ is acceptable.

Look at the plots to verify the validity of data.

---
### 1.2.4 Vehicular Stopping Distance
###### A new problem

Given some cars and they have some velocity. Determine an appropriate stopping distance between the two.

```chart
type: line
labels: [20,40,60,80,100]
series:
  - title: D / Feet
    data: [35,180,380,650,1100]
tension: 0.52
width: 93%
labelColors: false
fill: false
beginAtZero: false
```
_ Not linear_

Using the assumption that D is porportional to V, we can assume that the B is a constant. We can take D/V and this should be a constant value. 

```chart
type: line
labels: [20,40,60,80,100]
series:
  - title: D / V
    data: [2,4,6,8,10]
tension: 0.52
width: 93%
labelColors: false
fill: false
beginAtZero: false
```
_Linear_

#### $$D = (2.2 + \frac{v}{21}) * v$$
Do some rounding.
#### $$D = (2 + \frac{v}{20}) * v$$

```chart
type: line
labels: [20,40,60,80,100]
series:
  - title: Error (%)
    data: [5,2,.5,.2, 0]
tension: 0.52
width: 93%
labelColors: false
fill: false
beginAtZero: false
```
---
# End of week
---