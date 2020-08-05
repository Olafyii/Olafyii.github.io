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

## Basics
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
  [Why n-1?](https://www.zhihu.com/question/20099757)
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