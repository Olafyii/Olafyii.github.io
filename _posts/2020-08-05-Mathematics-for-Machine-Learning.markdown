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

* Axioms of Probability
  * Non-negativity:
    * for any event $E \in F, p(E) \geq 0$
  * All possible outcomes:
    * p($\Omega$)=1
  * Additivity of disjoint events:
    * For all events $E, E' \in F$ where $E \cap E' = \emptyset$,
    
      $p(E \cup E') = p(E)+p(E')$  
* A random variable X is a function that associates a number x with each outcome $O$ of a process.
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