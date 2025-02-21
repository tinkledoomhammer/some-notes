# Calculus 2

From [Coursera](https://www.coursera.org/learn/advanced-calculus/home/welcome)

## Sequences
[Week 1](https://www.coursera.org/learn/advanced-calculus/home/week/1)


### Definition / Description

$a:\mathbb{N}\mapsto\mathbb{R}$

How to describe an infinite list:

* By formula: $a_n=n^2$
* Recursively i.e. Fibonacci: 
	* $F_0=0,F_1=1, F_n=F_{n-1}+F_{n-2}$
	* $F_2=F_1+F_0=1$
	* $F_3=F_2+F_1=2$
	* $F_4=F_3+F_2=3$

Equality
: Two sequences $a_n$ and $b_n$ are equal when they start with the same $n$ and $a_n=b_n\forall n$. i.e:

	* $b: b_0=1,b_{n+1}=2*b_n$
	* $a: a_n=2^n$
	* $\implies a=b$

### Progression

Sequences can be defined using other sequences:

* Tribinacci sequence:
	* $a_0=a_1=a_2=1$
	* $a_n=a_{n-1}+a{n-2}+a{n-3}$
* Define $b: b_n=\frac{a_{n+1}}{a_n}$
	* $lim_{n\rightarrow\inf}b_n\approx1.89$

**Arithmetic progression**

* Sequence with a common difference between terms
* i.e. $a_n=5+7n$
* each term is the arithmetic mean of its neighbors
	* $a_n=\frac12(a_{n-1}+a_{n+1})$

**Geometric Progression**

* A sequence with a common ration between terms.
* $\forall (n>0):\frac{a_n}{a_{n-1}}
	 =\frac{a_{n+1}} {a_{n+2}}$
* i.e. $a_n=3\times2^n$
* general form: $a_n=a_0r^n$
* Elements are the geometric mean of its neighbors.
	* $a_n^2=a_{n-1}\times a_{n+1}$


### Limit of a Sequence

 $\forall(\epsilon>0\in\mathbb{R})\exists (N\in\mathbb{Z}):
  n\ge N\rightarrow|a_n-L|<\epsilon$

Nifty collection of series:

* $a:a_n=r\times a_{n-1}(1-a_{n-1})$
* depending on r, it may or may not converge

Quantifying N

* For some $\epsilon$, how big do we need N to be. i.e. How many elements of the sequence do we need to be within $\epsilon$ of the convergence?
*  For $\epsilon=1/100, \ a+n=\frac{n+1}{n+1}, \ L=1$
	* Find $N, \ n\ge N, |a_n-1| < 1/100$
	* $\frac{99}{100}<\frac{n+1}{n+2}<\frac{101}{100}$
	* $0.99<\frac{n+1}{n+2}\rightarrow 99(n+2)<100(n+1)$
	* $99n+198<100n+100\rightarrow n>98$



### Why do we care
$x_1=1\ ;\ \ \ x_{n+1}=x_n^{-1}+x_n/2\ \ 
\ \implies\ \  L=\sqrt2$

### Other properties of Sequences
#### Bounds
$a_n$ is "bounded above" means $\exists(M\in\mathbb{R}):
\forall n\ge0, a_n\le M$
bounded below:$\exists(M\in\mathbb{R}):\forall n\ge0, a_n\ge M$
#### Increasing
$a_n$ is increasing whenever $m>n\implies a_m>a_n$
i.e. $\forall (n\in\mathbb{N}):a_n<a_{n+1}$
Decreasing: $a_n>a_{n+1}\forall n$ 
Non-decreasing$(a_n): m>n\implies a_m\ge a_n$.
Monotone: increasing, non-decreasing, decreasing, or non-increasing.
#### Monotone convergence Theorem
Any sequence that is both bounded and monotone converges.

## Series
[Week 2](https://www.coursera.org/learn/advanced-calculus/home/week/2)

$$\sum_{k=1}^\infty a_k=\lim_{n\rightarrow\infty}s_n
	=\lim_{n\rightarrow\infty}\sum_{k=1}^na_k$$
	
$s_n=\sum_{k=1}^n$ is called a partial sum.

$\exists \lim_{n\rightarrow\infty}s_n$
: $a_n$ converges

$\not\exists \lim_{n\rightarrow\infty}s_n$
: $a_n$ diverges

### Geometric Series
$\sum_{k=0}^\infty r^k=\frac1{1-r}\ ;\ \  s_n=\sum_{i=0}^nr^i$

* $r=1/2\implies L=1$
* $r=1$ diverges 
* $r=-1\implies a_n$ alternates between 1 and -1
	* $s_n$ alternates between 0 and 1
* for $r\ne1$
	* $(1-r)s_n=(1-r)(r^0+r^1\dots+r^n)$
	* $=s^n-(r^1+r^2\dots+r^{n+1})=1-r^n$
	* $\implies s_n=\frac{1-r^n}{1-r}$
* $\lim_{n\rightarrow\infty}s_n$
	* $=\frac{1-\lim(1-r^{n+1})}{1-r}$
	* $\lim r^{n+1}$ is finite when $-1<r<1$
	* otherwise $lim_{n\rightarrow\infty}r^n=0$
	* and $\lim s_n=\frac{1-0}{1-r}=(1-r)^{-1}$

$\sum_{K=m}^\infty r^k$

* if it converges then $c\sum a_k=\sum(ca_k)$
* so this is only true when $|r|<1$
* $\sum_{K=m}^\infty r^k=\frac{r^m}{1-r}$

### Telescoping Series
$$\sum_{k=1}^\infty(k^2+k)^{-1}=\sum_k^\infty\frac1k-\frac1{k+1}=\frac11$$

* $(k^2+k)^{-1}=1/k-1/(k+1)$
* When expanded, the 1/k term cancels the previous k's 1/(k+1)
* this leaves $s_n=1-1/(k+1)$
* $\lim_{n\rightarrow\infty} s_n=1-\lim(1/(n+1)=1$
* more generally $\sum_k^\infty [f(k)-f(k+1)]=\sum f(k)-\sum(f(k+1))$
	* $=f(1)-\lim f(k+1)$
* Another example: $s_n=\frac k{(k+1)!}$
	* $f(k)=1/k!$
	* $f(k)-f(k+1)=\frac 1{k!}-\frac1{(k+1)!}$
	* $=\frac{k+1}{(k+1)!}-\frac1{(k+1)!}=\frac k{(k+1)!}$
	* $s_n=\frac1{1!}-\frac1{(n+1)!}$
	* $\implies\lim\sum=1$
	* 
### Convergence and divergence

$\exists\sum^\infty a_k\implies\lim_{n\to\infty}a_n=0$
$\lim_{n\to\infty}a_n\ne0\implies\sum$ diverges.
If the limit of the sequence is 0, the series may or may not diverge.

### More complicated series
#### The harmonic series
$\sum_{n=1}^{\infty}\frac1n$ Diverges even though $\lim s_n=0$. This can be seen by breaking any s_n into parts that are all $\ge 1/2$ (the number of terms in each is a power of 2).

### The comparison test
$0\le\sum_{k=0}^\infty\frac{\sin^2k}{2^k}$ is $\le \sum_{k=0}^\infty1/2^k
\therefore \sum\frac{sin^2k}{2^k}$ converges
remember that $\sum_{k=0}^\infty1/2^k=2$

Using 2 series: $\sum a_k$ and $\sum b_k$ 
Given that 

* $\sum b_k$ converges. 
* $0\le a_k\le b_k\ \  \forall k\ge0$

Then

* $s_n=\sum_{k=0}^n a_k\le \sum_{k=0}^nb_k$
* $s_n$ is bounded above
* $s_n$ is monotone ($m>n\to s_m\ge s_n$)
	* because $s_m=s_n+\sum_{k=n+1}^ma_n$ and $a_n>0\forall n$

Given: $0\le a_k\le b_k$
$\sum b_k$ converges $\to \sum a_k$ converges
$\sum a_k$ diverges $\to \sum b_k$ diverges

### Cauchy Condensation
Given $a_k$ is decreasing and $a_k>0 \ \ \forall k$ then
* $\sum_{k=1}^\infty a_k < \sum_{k=0}^\infty 2^ka_{2^k}$
* $\sum 2^ka_{2^k}$ is called the condensed series.
* The original series converges iff the condensed series converges.

## More convergence tests

### The ratio test

$a_n=n^5/4^n$
Does it converge?

* Look at the ratio between adjacent terms
* If it is constant, then the series is geometric and converges when the ratio $|r|<1$.
* If the ratio has a constant limit, then you can use the comparison test between the original series and a geometric series.
* in the example$\lim_{n\to\infty}\frac{a_{n+1}}{a_n}=\frac14$

In general, $\sum_{n=1}^\infty a_n$ (where $a_n>0$) converges if $\lim_{n\to\infty}<1$

* let $L=\lim_{n\to\infty}\frac{a_{n+1}}{a_n}<1$
	* The test fails if L>=1
* Pick a small $\epsilon: L+\epsilon<1$
* Find $N: n\ge N \implies \frac{a_{n+1}}{a_n}<L+\epsilon$
* then $a_{N+k}<(L+\epsilon)^k a_N\forall k\ge0$
* This is just the comparison test combined with a geometric series.
* $L>1\to$ divergence
* $L=1$ doesn't help
	* $a_n=1\to$ divergence
	* $a_n=1/n^2$ converges

### Definition of e

$$e=\lim_{n\to\infty}(\frac{n+1}n)^n
=\lim_{n\to\infty} \frac{(n+1)^n}{n^n}$$

This is useful for testing series that contain factorials and exponents.

### The Root test

If $\forall n, a_n>0$ then let $L=
	\lim_{n\to\infty}\sqrt[n]{a_n}$

* if $L<1 , \ \sum_{n=1}^\infty a_n$ converges
* if $L>1 , \ \sum_{n=1}^\infty a_n$ diverges
* If $L=1$ inconclusive.

### P series
For which p does $\sum_{n=1}^\infty\frac1{n^p}$ converge?
By Cauchy condensation or integral test, it converges when $p>1$ and diverges when $p<1$.

Does $\sum_{n=4}^\infty\frac1{n\log n}$ converge?

* The terms are between $1/n^2$ and $1/n$
* Condensed: $\sum_{n=2}^\infty\frac{2^n}{2^n*\log(2^n)}$
* Cancel the $2^n$'s and pull out $\log2$ and you get the harmonic series $\sum1/n$.
* Using the integral test, $\frac d{dx}\log(\log x)=\frac1{\log(x)x}$ (chain rule)

What about $\sum_{n=4}^\infty\frac1{n(\log n)^2}$

* Condensation: $\sum_{n=2}^\infty\frac{2^n}{2^n*(\log(2^n))^2}$ 
* $=\sum\frac1{(n\log2)^2} =(\frac1{\log2})^2\sum\frac1{n^2}$ 
* That converges

And finally$\sum_{n=4}^\infty\frac1{n(\log n)(\log\log n)}$

* The condensation is $\sum\frac{2^n}{2^n(\log(2^n))(\log\log2^n)}$
* $=\sum\frac1{n(\log2)\log(n\log2)}$
* let $y=n\log2$, then the above $=\sum\frac1{y\log y}$ which diverges.


## Series with negative terms

If we drop the assumption that $a_n\ge0$, then the comparison test can no longer be used.  All of the tests in the previous section use the comparison test which in turn uses monotone convergence.

### Convergence of series with negative terms

Absolute convergence
: The series $\sum_{n=1}^\infty a_n$ converges absolutely if the series $\sum_{n=1}^\infty |a_n|$ converges.

Theorem: If $\sum_{n=1}^\infty|a_n|$ converges then $\sum_{n=1}^\infty a_n$ converges.


* assume $\sum_{n=1}^\infty|a_n|$ converges
* so $\sum_{n=1}^\infty2|a_n|$ converges
* $0\le a_n+ |a_n| \le 2|a_n|$
	* actually, $a_n+|a_n|$ either $=0$ or $=2|a_n|$
* so $\sum_{n=1}^\infty(a_n+|a_n|)$ converges
* $\sum_{n=1}^\infty a_n=\sum_{n=1}^\infty(a_n+|a_n|)-\sum_{n=1}^\infty|a_n|$
* $\therefore \sum_{n=1}^\infty a_n$ converges.


Prove $\sum_{n=1}^\infty\frac{(-1)^n}{n^2}$ converges.

* $\sum_{n=1}^\infty|\frac{(-1)^n}{n^2}|=\sum_{n=1}^\infty\frac{1^n}{n^2}$
* This is a p-series, p=2, which converges.



Def: $\sum_{n=1}^\infty a_n$ is conditionally convergent if
: $\sum_{n=1}^\infty a_n$ converges
but $\sum_{n=1}^\infty |a_n|$ diverges
i.e. the series converges but not absolutely

Example: $\sum_{n=1}^\infty \frac{(-1)^n}n$

* it's absolute value is the harmonic series
* It converges to $-\log2$

Def: $\sum_{n=1}^\infty a_n$ is an alternating series
: if $a_n=(-1)^nb_n$ and all the $b_n$ are the same sign.
i.e. $\sum_{n=1}^\infty\frac{(-1)^{n+1}}n$

	* $\frac11-\frac12+\frac13-\frac14\dots$

	i.e. $\sum_{n=1}^\infty\frac{(-1)^n\sin^2n}n$

	* $\sin^2n>=0$
	
	Not all series with both positive and negative terms are alternating.
	
	* $\sum_{n=1}^\infty\frac{(-1)^n\sin n}n$
	* $\sum_{n=1}^\infty\frac{(-1)^{n(n+1)/2}}n$

Alternating series test
: If $a_n$ is decreasing and $a_n>0$ and $\lim_{n\to\infty} a_n=0$ then $\sum_{n=1}^\infty(-1)^{n+1}a_n$ converges.
If $\lim_{n\to\infty} a_n\not=0$ then $\sum_{n=1}^\infty(-1)^{n+1}a_n$ diverges.
This works because the odd partial sums over-estimate while the even partial sums under-estimate.
 $\forall n:\ \ s_{2n}\le\lim_{x\to\infty}s_x\le s_{2n-1}$

Standard process for finding limits of series.

1. Limit Test - If it's not 0, it diverges.
2. Absolute convergence?
	1. Root test
	2. Ratio test
	3. Comparison Test
3. Alternating Series?
	4. Alternating series test
4. Otherwise, you're screwed.


Why is $e$ irrational?
 

Limit comparison test: If $a_n\ge0$ and $b_n\ge0$ and $\lim_{n=\to\infty}\frac{a_n}{b_n}=L>0$  
: then $\sum_{n=1}^\infty b_n$ converges $\implies\sum_{n=1}^\infty a_n$ converges



## Taylor series


$f(x)=f(a)+f^\prime(x)(x-a)+\frac{f^{\prime\prime}(a)}{2!}(x-a)^2+\frac{f^{(3)}(a)}{3!}(x-a)^3$
$=f(a)+\sum_{n=1}^\infty\frac{f^{(n)}(a)}{n!}(x-a)^n$
$=\sum_{n=0}^\infty\frac{f^{(n)}(a)}{n!}(x-a)^n$

so with $\frac d{dx}\arctan x=\frac1{1+x^2}$, find $\arctan(\frac12)$ to within $\frac1{33}$


Rearrangement Theorem

: Let $L\in\mathbb{R}$ and $\sum_{n=1}^\infty a_n$ be conditionally convergent. Then $a_n$ can be rearranged to form $b_n$ so that $\sum_{n=1}^\infty b_n=L$.
Therefore order matters.


## Power Series

Def: $\sum_{n=0}^\infty a_n x^n$ is a power series
: for any $a_n$ (that is independent of x?)??
i.e. $a_n=2^n \to \sum_{n=0}^\infty(2^nx^n)$

* $\sum_{n=0}^\infty x^n=\frac1{1-x}$
* $\therefore \sum_{n=0}^\infty(2x)^n=\frac1{1-2x}$
* $=1+2x+4x^2+8x^3\dots$

Convergence depends on x

* Given $S=\sum_{n=0}^\infty a_nx^n$
	* If it converges absolutely for $x=a$ then it must also converge absolutely when $|x|\le a$

Def: Interval of convergence
: $C=\{x\in\mathbb{R} | \sum_{n=0}^\infty a_nx^n$conv$\}$

Thm: If $\sum_{n=0}^\infty a_nx^n$ conv when $x=x_0$
: Then the series converges absolutely when $|x|<=x_0$
Because the series is bounded and its terms have lim $L=0$

* With $|a_nx_0^n|\le M$ and $x\in(-|x_0| , |x_0|)$
* $|a_nx^n|=|a_nx_0^n|\times|\frac{x^n}{x_0^n}|$
* $\le M\times|\frac{x}{x_0}|^n$ which is a geometric series with $r=|\frac{x}{x_0}|$
* $r<1$ because $|x|<|x_0|$ by the definition of $x$

Def: Radius of Convergence for $S=\sum_{n=0}^\infty a_nx^n$
: $\exists R: x\in(-R,R)\implies S$ converges absolutely
and $x\not\in(-R,R)\implies S$ diverges.

* This tells us nothing about absolute convergence when $x=R$ or $x=-R$


Find the interval of convergence for $\sum_{n=1}^\infty |\frac{x^n}n|$

* First find the radius with the ratio test
	* $\lim_{n\to\infty}|\frac{x^{(n+1)}/(n+1)}{x^n/n}|$
	* $=\lim_{n\to\infty}|x|\times|\frac{n}{n+1}|=|x|$
	* so S converges when $x\in(-1,1)$
* but it may or may not converge when $x\in\{-1,1\}$
	* but $x=1$ gives the harmonic series
	* and $x=-1$ give the alternating harmonic series, which converges conditionally. 
* so the interval of convergence is $x\in [-1,1)$

Power series centered on $a$
: $\sum_{n=0}^\infty c_n (x-a)^n$

* Converges when $\lim_{n\to\infty}|\frac{c_{n+1}}{c_n}|\ |x-a| <1$
* i.e. $|x-a| < |\frac{c_n}{c_{n+1}}|$
* and the Radius of convergence $R=\frac{c_n}{c_{n+1}}$


Thm $\frac d{dx}\sum_{n=0}^\infty a_nx^n$
: $=\sum_{n=1}^\infty n a_n x^{n-1}$

* This has the same interval of convergence as the original series and each term of the derivative is the derivative of the corresponding term of the original series. 

Thm $\int_0^t\sum_{n=0}^\infty a_nx^n dx$
: $=\sum_{n=0}^\infty \frac{a_nt^{n+1}}{n+1}$  





> Written with [StackEdit](https://stackedit.io/).



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3OTczMDMyODBdfQ==
-->