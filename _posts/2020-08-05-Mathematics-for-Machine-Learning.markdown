---
layout: post
title: Mathematics for Machine Learning
date: 2020-07-27 10:04:00 +0800
catalog: true
header-style: text
mathjax: true
tags:
    - math
---

### Basics
* Axioms of Probability
  * Non-negativity:
    * for any event $E \in F, p(E) \geq 0$
  * All possible outcomes:
    * p($\Omega$)=1
  * Additivity of disjoint events:
    * For all events $E, E' \in F$ where $E \cap E' = \emptyset$,  
  $p(E \cup E') = p(E)+p(E')$  
* **A random variable X is a function that associates a number x with each outcome $O$ of a process.**
  * Common notation: $X(O)=x$, or just $X=x$
* Conditional probility  
  $p(X=x \mid Y=y)=\frac{p(X=x, Y=y)}{p(Y=y)}$
* Marginal Probility  
  $p(X=x)=\sum_{b=\text { all values of } Y} p(X=x, Y=b)$
* Rules
  * Complement Rule: Given: event A, which can occur or not  
  $p(\text { not } A)=1-p(A)$
  * Product Rule: Given: events A and B, which can co-occur (or not)  
  $p(A, B)=p(A \mid B) \cdot p(B)$
  * Rule of Total Probility: Given: events A and B, which can co-occur (or not)  
  $$
    \begin{aligned}
    p(A) &=p(A, B)+p(A, \operatorname{not} B) \\
    &=p(A \mid B) \cdot p(B)+p(A \mid \text { not } B) \cdot p(\text { not } B)
    \end{aligned}
  $$
  * Independence: Given: events A and B, which can co-occur (or not)  
  $p(A \mid B)=p(A) \quad \text { or } \quad p(A, B)=p(A) \cdot p(B)$
  ![img](/assets/images/prob-independence.png)
  * Bayes Rule: A way to find conditional probabilities for one variable when conditional probabilities for another variable are known.  
  $p(B \mid A)=\frac{p(A \mid B) \cdot p(B)}{p(A)}$  
  $p(B \mid A) \propto p(A \mid B) \cdot p(B)$  
  $\text{posterior probability} \propto \text{likelihood} \times \text{prior probability}$
    * Eg. In recent years, it has rained only 5 days each year in a desert. The weatherman is forecasting rain for tomorrow. When it actually rains, the weatherman has forecast rain 90% of the time. When it doesn't rain, he has forecast rain 10% of the time. What is the probability it will rain tomorrow?
* Estimate variance from actual samples:  
  $S^{2}=\frac{1}{n-1} \sum_{i=1}^{n}\left(X_{i}-\bar{X}\right)^{2}$  
  [Why n-1?](https://www.zhihu.com/question/20099757). $S^2$ is unbaised.
* **Unbiased Estimator: An estimator of a parameter is unbiased if the expected value of the estimate is the same as the true value of the parameters.**
* Covariance  
  * $$
        \begin{aligned}
        f\left(x_{i}\right) &=\left(x_{i}-\mu_{x}\right), \quad g\left(y_{i}\right)=\left(y_{i}-\mu_{y}\right) \\
        \operatorname{cov}(x, y) &=\sum p\left(x_{i}, y_{i}\right) \cdot\left(x_{i}-\mu_{x}\right) \cdot\left(y_{i}-\mu_{y}\right)
        \end{aligned}
    $$
  * Estimate covariance from actual samples:  
  $\operatorname{cov}(x, y)=\frac{1}{N-1} \sum_{i=1}^{N}\left(x_{i}-\mu_{x}\right)\left(y_{i}-\mu_{y}\right)$
* Pearson's correlation coefficient:  
  * $\operatorname{corr}(x, y)=\frac{\operatorname{cov}(x, y)}{\sigma_{x} \sigma_{y}}$
  * **Only reflects linear dependence**  
  ![img](/assets/images/corr.png)

## Maximum Likelihood Estimation (MLE)

### Main elements of MLE:  
* a sample $\xi$, that we use to make statements about the probability distribution that generated the sample;
* the sample $\xi$ is regarded as the realization of a random vector $\Xi$, whose distribution is unknown and needs to be estimated;
* there is a set $\Theta \subset \mathbb{R}^{p}$ of real vectors (called the parameter space) whose elements (called parameters) are put into correspondence with the possible distributions of $\Xi$; in particular:
  * if $\Xi$ is a continuous random vector, we assume that its joint probability density function $f_\Xi(\xi;\theta_0)$ belongs to a set of joint probability density functions $f_\Xi(\xi;\theta)$ indexed by the parameter $\theta$; **when the joint probability density function is considered as a function of $\theta$ for fixed $\xi$, it is called likelihood** and it is denoted by  
  $$L(\theta ; \xi)=f_{\Xi}(\xi ; \theta)$$

### Maximum likelihood estimator
A maximum likelihood estimator $\widehat{\theta}$ of $\theta_{0}$ is obtained as a solution of a maximization problem:  
$$\widehat{\theta}=\arg \max _{\theta \in \Theta} L(\theta ; \xi)$$  

In other words, $\widehat{\theta}$ is the parameter that maximizes the likelihood of the sample $\xi$. $\widehat{\theta}$ is called the maximum likelihood estimator of $\theta$.  
The logarithm of the likelihood is called log-likelihood and it is denoted by  
$$l(\theta ; \xi)=\ln [L(\theta ; \xi)]$$
The $\theta$ value that satisfies $\dfrac{dl(\theta ; \xi)}{d\theta}=\dfrac{d\ln f_{\Xi}(\xi ; \theta)}{d\theta}=0$ is the one with maximum likelihood. [Why?](https://www.zhihu.com/question/263423642)
<!-- **Information in equality**  [proof](https://www.statlect.com/fundamentals-of-statistics/maximum-likelihood)
$$\mathrm{E}\left[l\left(\theta_{0} ; \Xi_{n}\right)\right]>\mathrm{E}\left[l\left(\theta ; \Xi_{n}\right)\right], \forall \theta \neq \theta_{0}$$ -->

### Maximum A Posteriori Estimation (MAP)
* We assume that the parameters are a random variable, and we specify a prior distribution $p(θ)$.
* Employ Bayes' rule to compute the posterior distribution
  $$p\left(\theta \mid x_{1}, \ldots, x_{n}\right) \propto p(\theta) p\left(x_{1}, \ldots, x_{n} \mid \theta\right)$$
* Estimate paramenter $\theta$ by maximizing the posterior
  $$
  \begin{aligned}
  \hat{\theta}_{M A P} &=\arg \max _{\theta} p(\theta) p\left(x_{1}, \ldots, x_{n} \mid \theta\right) \\
  \hat{\theta}_{M A P} &=\arg \max _{\theta} \log p(\theta)+\sum_{i=1}^{n} \log \left(x_{i} \mid \theta\right)
  \end{aligned}
  $$

### Comparison MLE vs. MAP
* MLE: For which $\theta$ is $X_1, . . . , X_n$ most likely?  
* MAP: Which $\theta$ maximiszes $p(\theta\vert X_1, . . . ,X_n)$ with prior $p(\theta)$?  
* The prior can be regard as regularization -- to reduce the overfitting

### Vector
* 2-norm of vectors  
  $$\|\mathbf{x}\|_{2}=\sqrt{\sum_{i=1}^{n} x_{i}^{2}}$$
* Cauchy-Schwarz inequality  
  $$\mathbf{x} \cdot \mathbf{y} \leq\|\mathbf{x}\|_{2}\|\mathbf{y}\|_{2}$$
* A set of vectors is orthogonal if  
  $$\left\|\sum_{i=1}^{n} \mathbf{x}_{i}\right\|^{2}=\sum_{i=1}^{n}\left\|\mathbf{x}_{i}\right\|^{2}$$

### Matrix
* Rank of a matrix: the maximal number of linearly independent columns in the matrix
* The trace (often abbreviated to tr) of a square matrix $\mathbf{A}$ is defined to be the sum of elements on the main diagonal (from the upper left to the lower right) of $\mathbf{A}$.
* A square matrix $\mathbf{Q} \in \mathbb{R}^{n \times n}$ is orthogonal if
  $$\mathbf{Q}^{\top} \mathbf{Q}=\mathbf{I} \quad \text { i.e., } \mathbf{Q}^{\top}=\mathbf{Q}^{-1}$$
  i.e. the set of column vectors of $\mathbf{Q}$ is orthogonal.  
  * The processes of multiplication by an orthogonal matrices preserves geometric structure
  * Dot products are preserved  
  $$
  \begin{array}{l}
  (\mathbf{Q} \mathbf{x}) \cdot(\mathbf{Q} \mathbf{y})=\mathbf{x} \cdot \mathbf{y} \\
  (\mathbf{Q} \mathbf{x})^{\top}(\mathbf{Q} \mathbf{y})=\mathbf{x}^{\top} \mathbf{Q}^{\top} \mathbf{Q} \mathbf{y}=\mathbf{x}^{\top} \mathbf{y}
  \end{array}
  $$
  * Lengths of Vectors are preserved  
  $$\|\mathbf{Qx}\|=\|\mathbf{x}\|$$
  * Angles between vectors are preserved
  $$
  \cos \theta=\frac{(\mathbf{Q} \mathbf{x})^{\top}(\mathbf{Q} \mathbf{y})}{\|\mathbf{Q} \mathbf{x}\|\|\mathbf{Q} \mathbf{y}\|}=\frac{\mathbf{x}^{\top} \mathbf{y}}{\|\mathbf{x}\|\|\mathbf{y}\|}
  $$
* A matrix is invertible if it's a square matrix and is nonsingular.
* $\operatorname{det}\left(A^{-1}\right)=\frac{1}{\operatorname{det}(A)}$
* $A^{-1}=\frac{A^{*}}{|A|}$, $A^*$ is adjugate matrix.

### Norms
* Vector p-norms:  
  $$\|\mathbf{x}\|_{p}=\left(\left|x_{1}\right|^{p}+\left|x_{2}\right|^{p}+\cdots+\left|x_{n}\right|^{p}\right)^{1 / p}$$
  $$\|x\|_{\infty}=\max \left\{\left|x_{1}\right|,\left|x_{2}\right|, \ldots,\left|x_{n}\right|\right\}$$
* Matrix p-norms (operator norm):  
  $$\|\mathbf{A}\|_{p}=\max _{\mathbf{x} \neq 0} \frac{\|\mathbf{A} \mathbf{x}\|_{p}}{\|\mathbf{x}\|_{p}}$$
* Frobenius norm
  $$\|\mathbf{A}\|_{F}=\left(\sum_{i=1}^{m} \sum_{j=1}^{n}\left|a_{i j}\right|^{2}\right)^{1 / 2}$$
  * $$\|\mathbf{A}\|_{F}^{2}=\operatorname{trace}\left(\mathbf{A}^{\top} \mathbf{A}\right)$$
  * $$\|\mathbf{A} \mathbf{B}\|_{F}=\|\mathbf{A}\|_{F}\|\mathbf{B}\|_{F}$$
  * Invariance under orthogonal Multiplication, Q is an orthogonal matrix:
  $$\|\mathbf{Q} \mathbf{A}\|_{2}=\|\mathbf{A}\|_{2} \quad\|\mathbf{Q} \mathbf{A}\|_{F}=\|\mathbf{A}\|_{F}$$  

### Eigenvalue Decomposition
* For a square matrix $\mathbf{A} \in \mathbb{R}^{n \times n}$, we say that a nonzero vector $\mathbf{x} \in \mathbb{R}^n$ is an eigenvector of $\mathbf{A}$ corresponding to eigenvalue $\lambda$ if  
$$\mathbf{Ax}=\lambda \mathbf{x}$$
* An eigenvalue decomposition of a square matrix $\mathbf{A}$ is  
  $$\mathbf{A}=\mathbf{X} \mathbf{\Lambda} \mathbf{X}^{-1}$$
* $\mathbf{X}$ is nonsingular (rank=n), and consists of eigenvectors of $\mathbf{A}$
* $\mathbf{\Lambda}$ is a diagonal matrix with the eigenvalues of $\mathbf{A}$ on its diagonal.