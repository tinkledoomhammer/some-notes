# Coursera/Stanford Machine Learning
[https://www.coursera.org/learn/machine-learning/](https://www.coursera.org/learn/machine-learning/)

# 01 Introduction

## Forums and such

[How to use discussion forums](https://www.coursera.org/learn/machine-learning/supplement/66jBd/how-to-use-discussion-forums)
[discussion forum](https://www.coursera.org/learn/machine-learning/discussions?sort=lastActivityAtDesc&page=1)

[Honor Code](https://www.coursera.org/learn/machine-learning/supplement/nh65Z/machine-learning-honor-code)

[Lecture Slides](https://www.coursera.org/learn/machine-learning/supplement/d5Pt1/lecture-slides)

## Definitions

Two definitions of Machine Learning are offered. 

Arthur Samuel described it as: 
: >"the field of study that gives computers the ability to learn without being explicitly programmed."

This is an older, informal definition.

Tom Mitchell provides a more modern definition: 
: >"A computer program is said to learn from experience $E$ with respect to some class of tasks $T$ and performance measure $P$, if its performance at tasks in $T$, as measured by $P$, improves with experience $E$."

Example
: playing checkers.
* E = the experience of playing many games of checkers
* T = the task of playing checkers.
* P = the probability that the program will win the next game.

In general, any machine learning problem can be assigned to one of two broad classifications:

Supervised learning
: >In supervised learning, we are given a data set and already know what our correct output should look like, having the idea that there is a relationship between the input and the output.

* Examples

Regression
: >Given a picture of a person, we have to predict their age on the basis of the given picture

Classification 
: >Given a patient with a tumor, we have to predict whether the tumor is malignant or benign.

Unsupervised Learning
: >Unsupervised learning allows us to approach problems with little or no idea what our results should look like. We can derive structure from data where we don't necessarily know the effect of the variables. 
With unsupervised learning there is no feedback based on the prediction results.

* Examples

Clustering
: >Take a collection of 1,000,000 different genes, and find a way to automatically group these genes into groups that are somehow similar or related by different variables, such as lifespan, location, roles, and so on.

Non-clustering
: >The "Cocktail Party Algorithm", allows you to find structure in a chaotic environment. (i.e. identifying individual voices and music from a mesh of sounds at a  [cocktail party](https://en.wikipedia.org/wiki/Cocktail_party_effect)).


Learning Algorithm
: $$\boxed{\text{Training Set}} \to 
\boxed{\text{Learning algorithm}} \to \boxed h
$$

Hypothesis
: $$
\boxed x \to 
\boxed{h : X\to Y}\to
\boxed{\text{predicted } \hat y}
$$


## Model Representation

$x^{(i)}$
: "input" variables, aka "features."

$y^{(i)}$
: "output" or "target" variable.

$m$
: the length of the training set

$(x^{(i)},y^{(i)}); i=1 , \dots ,m$
: the training set

$X = Y = \R$
: the input and output spaces, real numbers in this example

$\theta$
: The parameter of the hypothesis

$h_\theta$, for linear regression
: $h_\theta(x^{(i)}) = \theta_0 + x^{(i)}\theta_1 = \hat y$



## Cost Function

Mean Squared error
$$
\begin{aligned}
J(\theta_0,\theta_1) &= 
	\frac1{2m}\sum_{i=1}^m
	 {(\hat y^{(i)}-y^{(i)})^2}
	 \\ &=
	\frac1{2m}\sum_{i=1}^m
	{(h_\theta(x^{(i)})-y^{(i)})^2}
	\\ &=
	 \frac1{2m}\sum_{i=1}^m
	 {( \theta_0 + x^{(i)}\theta_1-y^{(i)})^2}
	 \\ &=
	 \frac{\bar x}2
\end{aligned}
$$

* The $1/2$ is to cancel out the derivative in the gradient descent algorithm.

$\underset{\theta_0,\theta_1}{\text{minimize }}J(\theta_0,\theta_1)$
: find the $\theta$ that minimizes $J$ for all the given training set.

## Parameter Learning

Gradient Descent
: $$
\theta_j:=\theta_j-\alpha 
\frac\partial{\partial\theta_j}
J(\theta_0,\theta_1)
$$

$\alpha$
: Learning rate

$j=0,1$
: represents the feature or index
* Mind that all $\theta$s are updated simultaneously.


* $\frac d {d\theta}J(\theta) = 0$ at local optimums
* The steps get smaller as optimums are approached
* Setting $\alpha$ too low can lead to slow convergence.
* Setting $\alpha$ too high can cause a failure to converge or even divergence.
* 
The derivative $\frac\partial{\partial\theta}J(\theta_0,\theta_1)$
* $\theta_0 : \frac1 m\sum_{i=1}^m{(h_\theta(x_i)-y_i))}$
* $\theta_1: \frac1 m\sum_{i=1}^m{(h_\theta(x_i)-y_i)x_i)}$ 

Batch Gradient Descent
: Using the entire training set for each iteration.

## Linear Algebra Review


Matrix
: Rectangular array of numbers

$\R^{4\times2}$
: The set of all $4\times2$ matrices

Dimension of a Matrix
: number of rows x number of columns

$A_{ij} =$ "$i,j$entry" in the $i$^th^ row, $j$^th^ column.
* indexes start at 1

Vector
: An $n \times 1$ matrix. i.e. a column vector.

Dimension of a vector
: the number of elements, $n$ in the above definition.

$\R^n$
: The set of all $n$-dimensional vectors.

Indexes can start from $0$ or $1$.
* This course will use $1$ indexed vectors.

Capital letters for matrices and lower case for vector and scalar.


Matrix Addition
: $$
\begin{bmatrix} a&b\\c&d\end{bmatrix}+ 
\begin{bmatrix}w&x\\y&z\end{bmatrix}=
\begin{bmatrix}a+w&b+x\\c+y&d+z\end{bmatrix}
$$

Scalar Multiplication
: $$
\begin{bmatrix}a&b\\c&d\end{bmatrix} * x
=\begin{bmatrix}a*x&b*x\\c*x&d*x\end{bmatrix}
$$

Matrix Vector Multiplication
: $$
\begin{bmatrix}a&b\\c&d\\e&f\end{bmatrix}
\begin{bmatrix}w\\x\end{bmatrix}
=\begin{bmatrix}
a*w+b*x\\c*w+d*x\\e*w+f*x
\end{bmatrix}
$$

An $m\times n$ matrix $(A)\times$ an $n\times 1$ vector $(\vec x) =$ an $m$ dimensional vector $(\vec y)$.

* To get $y_i$, multiply the $A$'s $i$^th^ row with elements of $\vec x$, and add them up.
* The # of columns in the matrix $(n)$ must $=$ the dimension of the vector $(n)$. The resulting vector has dimension $=m$, the # of rows in the matrix.


Multiplication
: $$\underset {m\times n}A *
 \underset{n\times o}B
=\underset{m\times o}C$$
: $$C_{ij}=\sum_{k=1}^n(A_{ik}*B_{kj})$$

Multiplication Properties

Not Commutative
: $A \times B \ne B\times A$

Associative
: $$A\times B\times C 
\\=A\times (B\times C) 
\\=(A\times B)\times C$$

Multiplicative Identity $I$ or $I_{n\times n}$
: $$\begin{bmatrix}
	1&0&\dots&\dots&0\\
	0&1&0&\dots&\vdots\\
	\vdots&0&\ddots&\ddots&\vdots\\
	\vdots&\ddots&\ddots&\ddots&0\\
	0&\dots&\dots&0&1
	\end{bmatrix}$$
: $$\forall \underset{m\times n}A, 
	A \times \underset{n\times n}I  =
	\underset{m\times m}I\times A = A$$

Matrix Inverse
: $AA^{-1}=A^{-1}A=I$
: Only square matrices have an inverse
: Not all square matrices have an inverse
: Matrices that don't have an inverse are "singular" or "degenerate."
: `pinv(A)` in Octave

Matrix Transpose
: $(\underset{m\times n}{A^T})_{ij}=(\underset{n\times m}A)_{ji}$
: Rotate 90^o^ and reverse


# 02 Programming environment
[Coursera Link](https://www.coursera.org/learn/machine-learning/supplement/ks2m0/setting-up-your-programming-assignment-environment)
## Octave
[Download Octave](https://www.gnu.org/software/octave/download.html)
[Discussion Board](https://www.coursera.org/learn/machine-learning/discussions/vgCyrQoMEeWv5yIAC00Eog?page=2)
Octave [documentation pages](http://www.gnu.org/software/octave/doc/interpreter/).
* At the CLI use the `help` command i.e. `help plot`
 
 ## Multivariate Linear Regression
 
 Notation conventions
 * $n =$ number of features
 * $x^{(i)} = i$^th^ training example. A vector in $\R^n$
 * $x^{(i)}_j = j$^th^ value of the $i$^th^ example, $\in \R$
 * $m$ is the number of examples, as before
 * $\theta$ is a vector in $\R^{n+1}$, $0$ indexed.

Hypothesis
: $h_\theta(x) = \theta_0 + \sum_{i=1}^n(\theta_i x_i)$

With $x_0 =1$  i.e. $x^{(i)}\in \R^{n+1}$ 
: $h_\theta(x)=\sum_{i=0}^n(\theta_i x_i)$
: $h_\theta(x)=\theta^Tx$

Cost Function
: $$J(\theta)=\frac{1}{2m}\sum_{i=0}^m[
	h_\theta(x^{(i)})-y^{(i)}]^2$$ 
: $$=\frac{1}{2m}\sum_{i=0}^m[
	\theta^Tx^{(i)}-y^{(i)}]^2
$$

Gradient Descent
: $$\theta_j := \theta_j -\alpha
\frac\partial{\partial\theta_j}J(\theta)
$$
: Update simultaneously for every $j=0,\dots,n$
: Repeat until convergence



The derivative of the cost function
: $$\frac\partial{\partial\theta_j}J(\theta)=
\frac1m\sum_{i=1}^m[
h_\theta(x^{(i)})-y^{(i)}]x_j^{(i)}
$$

Feature Scaling
: $x_i:=\frac{x_i}{s_i}$
: * $s_i$ is $(\max-\min)$ on paper, or the standard deviation in the programming exercises
: * All the inputs will be in the range $0\le x_i\le1$

Mean normalization
: $x_i:=x_i - \mu_i$
: * $\mu_i$ is the average of all the values for the feature, $x_i$

Combined into the range $-0.5\le x_i\le 0.5$
: $x_i:=\frac{x_i-\mu_i}{s_i}$


### Making sure gradient descent is working correctly.

* Plot $J(\theta)$ vs No. of iterations to make sure that $J(\theta)$ is converging. It should decrease after every iteration.
	* Automatic convergence tests can be used, or visual inspection to see if the descent has flattened.
	* If $J(\theta)$ increases with more iterations,  then $\alpha$ is too high.
	* It may also increase, then decrease if $\alpha$ is too high.
* If $\alpha$ is too low, then convergence will be slower.
* Trying several different values (i.e. related by a factor of 2 or 3) can find an optimum $\alpha$

### Polynomial regression

$\hat y = \theta_0+\theta_1 x + \theta_2 x^2 + \theta_3 x^3\dots$
* Feature scaling is very important because the range will be much larger for $x^2$ than for $x$ (if $x>1$).
* The exponents are actual exponents
* Exponents$<1$ i.e. $\sqrt{x}$ can also be used
* The linear regression techniques work, but some of the features are powers of other features. i.e. size and $\sqrt{\text{size}}$ or size^2^.

### The normal equation (analytic solution)

Design Matrix
: $$\underset{m\times(n+1)}X=\begin{bmatrix}
	--&(x^{(1)})^T&--\\
	--&(x^{(2)})^T&--\\
	&\vdots\\
	--&(x^{(m)})^T&--
\end{bmatrix}$$
: * With $x_0=1$ and all other components of $x$ are features.

Solution
: $\theta=(X^T X)^{-1}X^T \vec y$
: $\vec y$ is the $m$ dimensional vector of training target values.
: In Octave: `pinv(X'*X)*X'*y`

This method does not require feature scaling or mean normalization.

Scales poorly with large $n$.
* $(X^T X)$ is $(n+1)\times (n+1)$ so it's inverse is hard to computer
* $n>\approx10000$ is about as large as it is practical.
* $\mathcal O(n^3)$ vs $\mathcal O(kn^2)$(for gradient descent)

Noninvertibility problem
* When there are redundant (linearly dependent) features, remove redundant features
* When $m\le n$ (i.e. more features than examples) then the matrix might not have an inverse, or the regression might otherwise not work correctly
	* Regularization can make the matrix invertible.
* Octave's `pinv()` function performs a 'pseudo-inverse' which will give the desired answer.  It also has a function `inv()` that will fail.

### Submitting programming assignments












> Written with [StackEdit](https://stackedit.io/).


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA4NTE4OTM5XX0=
-->