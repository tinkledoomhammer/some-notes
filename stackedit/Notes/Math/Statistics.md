
# Udacity intro to statistics

## Bayes Rule
$$P(A|B) = \frac{P(B|A) P(A)}{[P(B)
	= P(B|A) P(A) + P(B|\neg A) P(\neg A)]}$$
## Estimation
	
Empirical probability
: The measured probability
: $=\frac 1 n \sum_iX_i$

### Maximum Likelihood Estimator

Estimation problems are
* $Data \to P$
* $p\to(Data)$

The goal of MLE is to find
$p:\max P(Data)$


#### Example 1
Given
* $Data = {1,0,1}$ 
* $p=1/2$

Find $P(Data)$
$P({1,0,1})=P(1)P(0)P(1)$
$=\frac1{2^3}=0.125$

$p=1/3\implies P({1,0,1}) = \frac13\frac23\frac13
=\frac2{27}$
$p=\frac23\implies
 P({1,0,1})=\frac23\frac13\frac23 
 = \frac4{27}$
$p=0\implies P(Data) =0$
$p=1\implies P(Data) =0$
To find the maximum likelihood, find $p : \max P(Data)$.

#### Be Wicked
$Data = \{1\} \implies p=1$
$Data = \{0\} \implies p=0$

From a single data point, MLE will estimate that all future results will be the same. An odd number of coin tosses will always suggest that the coin is loaded. 

### Laplacian Estimator
Add Fake Data
$$Data \rightarrow Data + [Heads , Tails]$$
| Data | $MLE(Data)$ | $MLE(Data\cup\{1,0\})$ |
| ---- | ----------- | -------------------- |
| 1    | 1           | $\frac23$            |
| 0 0 1 | $\frac13$  | $\frac25$			|
| 1 0 1 0 | $\frac12$ | $\frac12$ |
| 1 1 | 1 | $\frac34$ |

Comparing the first and last cases, we can see that the fake data causes better estimates 
This is called Laplacian Estimator

#### Dice Example
|     |     |     |    |
| --- | --- | --- | -- |
| Data | Laplace | Combined |  |
| 1 2 3 2 | 1 2 3 4 5 6 | 1 1 2 2 2 3 3 4 5 6 |


| P(#) | MLE | Laplace |
| -- | ---| ---|
| 1 | $\frac14$ | $\frac2{10}=\frac15$ |
| 2 | $\frac24=\frac12$ | $\frac3{10}$ |

### Summary
Maximum Likelihood Estimator
: $\frac1N\sum X_i$
N : Number of Data
$X_i$ : Elements of data

Laplace Estimator
: $\frac1{N+k}(1+\sum X_i)$
k : Number of possible outcomes


## Averages

Data (House prices in $k) : {190, 170, 165, 180, 165}

* Total: 870
* N: 5
* mean $=\frac{870}5 = 174$
 
Mean
: $\mu=\frac1N\sum X_i$

Median
: Middle of sorted values
$=X_{n/2}$


Mode
: Most frequent value

### Party Example
Data
: 4 3 32 33 4 32 3 38 4
: $N=9$
: $\sum = 153$

Mean = $\frac{153}9 = 17$

Sorted Data
: 3 3 4 4 4 32 32 33 38
Median = 4

Two more parents (35,36) show up

New Sorted data
: 3 3 4 4 4 32 32 33 35 36 38
: $n=11$

New Median = 32
Mode = 4 (with or without the extra parents)

#### Three averages #2
Data
: 3 9 3 8 2 9 1 9 2 4
: $N=10$
: $\sum= 50$

Sorted
: 1 2 2 3 3 4 8 9 9 9

Mean = $\frac{50}{10}=5$
Median = $\frac{3+4}2 = 3.5$
Mode = 9

## Variance
### Quiz: Ages
Friends
: 17 17 18 19 19
$N=5$
$\sum=90$
Mean = $\frac{90}5=18$

Family
: 4 7 18 23 38
$n=5$
$\sum=90$
Mean $=\frac{90}5=18$

Variance
: $\sigma^2=\frac1N\sum(X_i-\mu)^2$

| $X_i$ | 17 | 19 | 18 | 17 | 19 |
| ---- | -- | -- | -- | -- | -- |
| $X_i-\mu$ | -1 | 1 | 0 | -1 | 1 |
| $(X_i-\mu)^2$ | 1 | 1 | 0 | 1 | 1 |
Variance $\sigma^2=\frac45$


| $X_i$ | 7 | 38 | 4 | 23 | 18 |
| ---- | -- | -- | -- | -- | -- |
| $X_i-\mu$ | -11 | 20 | -14 | 5 | 0 |
| $(X_i-\mu)^2$ | 121 | 400 | 196 | 25 | 0 |
Variance $=\frac{742}5=148.4$

### Standard Deviation
$\sigma = \sqrt{variance}=\sqrt{\frac1N{\sum(X_i-\mu)^2}}$

### Alternative Formula
Allows keeping running totals with a single pass. Just track $N$, $\sum X_i$, and $\sum X_i^2$

$\frac1N\sum(X_i-\mu)^2=\frac1N\sum(X_i^2-2X_i\mu+\mu^2)$
$=\frac1N\sum X_i^2 -\frac{2\mu}N\sum X_i+\mu^2$
$=\frac1N\sum X_i^2-\mu^2$
$=\frac1N\sum X_i^2-\frac1{N^2}(\sum X_i)^2$
$$\sigma^2=\frac{\sum X_i^2}N -\frac{\sum{(X_i)}^2}{N^2}$$

### Standard Score
$Z=\frac{X-\mu}\sigma$


## Programming Estimators

```
#data is a python list
sum(data)		#return the sum of elements of a list
len(data)		#return the length of the list
sorted(data)	#returns a sorted copy of a list
data.count(x)	#returns the # of times x occurs in data
data.append(x)	#modifies the list by adding
				#an element to the end

#Finding variance with list comprehension
def mean(data):
	return sum(data)/len(data)
def variance(data):
	mu=mean(data)
	return mean([(x-mu)**2 for x in data])
	
```

## Estimator problem set
### Deriving MLE
P(Data)$=\prod_i p^{X_i} (1-p)^{1-X_i}\rightarrow \max$
Maximize log P(Data)
$\sum_i  X_i\log(p)+(1-X_i)\log{(1-p)} \rightarrow\max$
Set first derivative to zero
$\frac\delta{\delta p}\log P(Data)=
	\sum_i\frac{X_i}p-\frac{1-X_i}{1-p}   =0$
Multiply by $p(1-p)$
$0=\sum_i (1-p)X_i-p(1-X_i)
=\sum_iX_i-pX_i-p+pX_i$
$0=\sum_i(X_i-p)=\sum(X_i)-Np$
$\Rightarrow \sum_iX_i=pN$
$\Rightarrow p=\frac1N\sum_iX_i$

## Outliers
### Quartiles
	1. Find Median
	2. Divide into two ranges (lower,upper)
	3. Find the median of each range
		* Upper quartile
		* Lower quartile
Interquartile range =upper - lower
Quartile discard mean - discard everything below the lower quartile or over the upper quartile


## Binomial distribution
$$\binom ab=\frac{a!}{b!(a-b)!}$$
$$\frac{n!}{k!(n-k)!}=\binom nk$$
For coin tosses, $\binom{tosses}{heads}$ gives the number of permutations that give that many heads.

### Loaded Coin

Flipping n coins and getting k heads where each toss has p chance of producing heads, there are $\binom n k$ permutations each with a probability of $p^k(1-p)^{n-k}$ 

Example: n=12, k=9, p=0.8
$\binom{12}9=\frac{12!}{9!(12-9=3)!}=\frac{10*11*12}{3*2}=10*11*2=220$
$p(9,3)=p^9(1-p)^3=0.8^9*0.2^3$
$=0.1342*.008=0.0010737$
$\Rightarrow\binom{12}{9}0.8^90.2^3=0.236$

So Remember:
$$P(\sum_iX_i=k)=\frac{n!}{k!(n-k)!}p^k(1-p)^{(n-k)}$$

## Central Limit Theorem
The limit of the distribution of many things.

Coin $:(0,1)$ with 
	$p(\sum_i X_i=k)
	=\frac{n!}{(n-k)!k!}2^{-n}$
As n increases, we get a Gaussian
### Programming Exercise
```
def flip(N):
	return [random.random() >0.5 for x in range(N)]
def sample(N):
	return [mean(flip(N)) for x in range(N)]
```

## Normal Distribution
Binomial Distribution $\rightarrow$ Central Limit Thm $\rightarrow$ Normal Distribution

The binomial distribution peaks at n=k/2

Normal distribution with mean $\mu$ and variance $\sigma^2$
For any outcome x $f(x)=(X-\mu)^2$. This is called a quadratic.
$f(x)=-\frac12\frac{(X-\mu)^2}{\sigma^2}$

$$f(x)=e^{-\frac12\frac{(X-\mu)^2}{\sigma^2}}$$ but to get the integral to 1

$$N(X,\mu,\sigma^2)
	=\frac1{\sqrt{2\pi\sigma^2}}
	\exp(-\frac12\frac{(X-\mu)^2}{\sigma^2})$$

### Summary
The binomial distribution better describes medium sized data sets (i.e. a few coin flips). The normal distribution is a good approximation for very large data sets.	

## Manipulating Normal Distributions
When adding 2 Gaussians: the means ($\mu$) and variances ($\sigma^2$) add but the standard deviation ($\sigma$) does not.

$(aX+b)$
$\implies \mu=a\mu_x+b$
$\implies \sigma=a\sigma_x$
$\implies \sigma^2=a^2\sigma_x^2$
$(X+Y)$
$\implies \mu=\mu_x+\mu_y$
$\implies \sigma^2=\sigma_x^2+\sigma_y^2$
$\implies \sigma=\sqrt{\sigma_x^2+\sigma_y^2}$


## Statistical Myths

* Most drivers believe they drive better than the average driver.

## Problem Set 4

Suppose you administer a survey where 1% of respondents are highly active users.  What are the odds that a sample of 10 surveys contains at least 2 highly active users.
* Use binomial thm
* $k=0: 1*0.01^0*(0.99)^{10})=0.99^{10} =0.99(0.99)^9$
* $k=1: 10!/9!*0.01^1*0.99^9$ $=10/100*0.99^9 = 0.1 (0.99)^9$
* Combine: $1-(0.99+0.1)(0.99^9) = 1.09(0.99)^9\approx 0.004266$

## Confidence Intervals

CI's are sort of an alternative to variance($\sigma^2$).
More trust = smaller CI. (i.e. more data) 
Size of CI $\propto \frac\sigma{\sqrt N}$

Coin tosses repeatedly
| Tosses | Mean $\sum X_i$ | Var $\sum X_i$ | Var $\frac1N\sum X_i$   | $\sigma$ | CI$=1.96\sigma$| 
| - | - | - | - | - | - |
|1 | 0.5 | 0.25 | 0.25 | 0.5 | 0.98
|2 | 1 | 0.5 | 0.125 | 0.35 | 0.686
|10 | 5 | 2.5 | 0.025 | 0.16 | 0.3136
|100| 50 | 25 | 0.0025 | 0.05 | 0.098

Loaded coin: p=0.1,  $\mu=p$, $\sigma^2=p(1-p)$
### Guided Proof
Var(X) = $p(1-p)^2 + (1-p)p^2$
$=p(1-2p+p^2)+p^2-p^3$
$=p-p^2=p(1-p)$
Still don't know where 1.96 comes from, but
$$CI= 1.96\frac{\sqrt{p(1-p)}}{\sqrt{N}}$$

## Magic Number
as N increases, $\mu$ approaches p.
Using the central limit theorem, assume a true average p, compute $P(\mu<a)$ (the area under the left end of the gaussian). The confidence interval $x : P(p<\mu-x)+P(p>\mu+x) >5$%. $x=1.96 \implies P(\mu-x<p<\mu+x)=0.95$

| CI range | Quantile |
| - | - |
|90%|1.64|
|95%|1.96|
|99| 2.58 |

### <30 samples
Degrees of freedom = N-1
Use a table.

## Hypothesis testing
$Sample\dots\overrightarrow{Statistics}\dots
Information\dots\overrightarrow{Statistics}\dots Decisions$

Example: Weight loss

* "90% Guaranteed"
* Sample: $\frac{11}4$ say yes. (73.3%)
* $H_0$ (Null Hypothesis): $p=0.9$
* $H_1$ (Alternate Hypothesis): $p<0.9$ ("Doesn't work **as well** as advertised.")

Assume $H_0$ and use a binomial to describe the distributions of samples size $N=11$. Determine the probability of a sample the same or worse.

The critical region (5%)
: is $\sum_{k=0}^\mu\binom N k p^k(1-p)^{N-k}<0.05$
Where $\mu$ is the empirical average.
Coming from the other direction it would be $\sum_{k=N}^\mu$

Example: Loaded coin
$H_0: p=0.3$
$H_1: p>0.3$
for $N=11$ the critical region is $\mu>6$

Example: Fair Coin
$H_0: p=0.5$
$H_1: p\ne0.5$
This is a two-tailed test. The critical region is split.
$N=14,k=3 \rightarrow_{0.95} 3<\mu<12$
The left-sided single-tailed test would be $N=14,k=3\rightarrow_{0.95}3<\mu<12$.

### Using confidence intervals

$$\frac1N \sum X_i \pm a \sqrt{ \frac{\sigma^2}N}$$
$\mu=\frac1n \sum_i X_i$
$\sigma^2=\frac1N \sum_i(X_i-\mu)$
get (from a table) $t(N-1,p)=a$. Be careful that you get he right number of tails.
Size $CI = a \sqrt \frac{\sigma^2}N$

### A significance test
Campaign A: 1%
Campaign B: 1.01%
10 Million samples
$(p_a-p_b)>a\sqrt \frac{\sigma^2}N$
$0.0001/{\sqrt{(0.99\times0.1+0.989\times0.0101)/10^7}}=2.24>1.96$

## Regression
Compares two related data sets to quantize the relationship.
### Linear Regression
$y=bx+a$
The task is to find (a,b).
To visually solve, find a first (y-intercept), then $b=\frac{rise}{run}$.
Minimize $\sum_i (X_i-a-bx)^2$
*This means that we are minimizing vertical error, not $\perp$

$$b=\frac{
	\sum_i[(x_i-\bar x)(y_i-\bar y)]}
	{\sum_i(x_i-\bar x)^2}
	=\frac{\sigma_{xy}}{\sigma_x^2}$$
and
$$a=\bar y-b\bar x$$

## Correlation
Correlation $r \in [-1\dots1]$
Measures how the two data are related. $r<0\implies b<0$ and vice-versa. $|r|>$when the data fits the relation well.$r=\pm1$ is perfect correlation.

$$r=\frac{\sum_i[(x_i-\bar x)(y_i-\bar y)] }
{\sqrt{\sum_i(x_i-\bar x)^2 \times \sum_i(y_i-\bar y)^2  }}$$

covariance $\sigma^2_{xy}=\sum_i(x-\bar x)(y_i-\bar y)$
normalizer = $\sqrt{\sigma_x^2\sigma_y^2}=\sigma_x\sigma_y$

doubling y doubles b but does not change r.
doubling both x and y changes neither.
doubling x but not y halves b and does not change r.
$$\frac br=\sqrt\frac{\sum(y-\bar y)^2}{\sum(x-\bar x)^2}
	=\frac{\sigma_y}{\sigma_x}
$$$$ r=b\frac{\sigma_x}{\sigma_y}
	, b=r\frac{\sigma_y}{\sigma_x}$$



Standard Scores
$z_x=\frac{x_i-\bar x}{\sigma_x}$
$z_y=\frac{y_i-\bar y}{\sigma_y}=bz_x+a
=a+b\frac{x_i-\bar x}{\sigma_x}$
$a=\bar y-b\bar x = 0 , b=r\sigma_y/\sigma_x,  -1\le b\le 1$
because standard scores subtract the mean.

$cov(x,y)=\frac1N\sum_i(x_i-\bar x)(y_i-\bar y)$

$$b=\frac
  {\sum x_iy_i-\frac1N \sum x_i \sum y_i}
  {\sum x_i^2 - \frac1N(\sum x_i)^2}$$



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI1MjgyOTk3MV19
-->