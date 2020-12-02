---
layout: post
title:  "Monte Carlo estimation"
date:   2020-11-25 11:03:35 -0500
categories: notes
---

Statistics deals with two basic problems: probabilty and inference. 
- *Probability*: Given a data generating process, what are the characteristics of the outcomes
- *Inference*: Given the outcomes, what are the characteristics of the underlying data generating process

In this article, we will discuss a statistical inference technique called Monte Carlo (MC) estimation. It is a method that uses a set of samples to estimate the parameters (i.e. characteristics) of the underlying stochastic systems, e.g. mean, variance, etc.


## Mean Estimation
Suppose our goal is to estimate $$\mu = \mathbb E [f(X)]$$ where $$X \sim p$$. MC proceeds as follows:

1. generate i.i.d. samples $$X_1, X_2, \cdots,X_n$$

2. collect observations $$Y_1, Y_2, \cdots, Y_n$$ where $$Y_i = f(X_i)$$

3. compute MC estimator $$\hat \mu_n = \frac{1}{n} \sum_{i=1}^n Y_i$$.



This procedure gives us an unbiased estimator by the following theorem.


**Theorem 1**: MC estimator described above is unbiased.
<details>
  <summary>Proof</summary>
  
 We want to show the MC estimator is unbiased, i.e. $$\mathbb E [\hat \mu_n] - \mu = 0.$$ Directly applying the linearity of expectation

$$
\begin{align}
\mathbb E [\hat \mu_n] - \mu &= \mathbb E \left[ \frac{1}{n} \sum_{i=1}^n Y_i \right] - \mu \\
&= \mathbb E \left[ \frac{1}{n} \sum_{i=1}^n f(X_i) \right] - \mu\\
&= \frac{1}{n} \sum_{i=1}^n \mathbb E \left[f(X_i) \right] - \mu\\
&= \mu - \mu \\
&= 0
\end{align}
$$

completes the proof.

</details>



More samples gives a more precise estimator, shown by the following theorem.


**Theorem 2**: The variance of MC estimator $$\hat \mu_n$$ shrinks at a rate of $$O(n^{-1})$$.
<details>
  <summary>Proof</summary>

$$
\begin{align}
\text{Var}(\hat \mu_n) &= \text{Var} \left( \frac{1}{n} \sum_{i=1}^n Y_i \right) \\
&=  \frac{1}{n^2} \sum_{i=1}^n \text{Var} \left( Y_i \right) \\
&=  \frac{1}{n^2} \sum_{i=1}^n \text{Var} \left( f(X_i) \right) \\
&=  \frac{1}{n} \underbrace{\text{Var} \left( f(X_i) \right)}_{\text{constant}} \\
&= O(n^{-1})
\end{align}

$$

</details>


(Todos: Add plots showing unbiasedness and variance shrinkages.)



These basic results are quite useful because many problems can be cast as mean estimation problem.

## Probability Estimation

Suppose we are interested in estimating $$P(X \in \mathcal E)$$ for some set $$\mathcal E$$ and  $$X \sim p$$. Then, define indicator function 

$$
\begin{align}
f(X) = \mathbb{1}(X \in \mathcal E) = 
\begin{cases} 
1, & X \in \mathcal E \\
0, & X \not \in \mathcal E
\end{cases}.
\end{align}
$$

So we have

$$
\begin{align}
P(X \in \mathcal E) = \mathbb E[f(X)]
\end{align}
$$


## Integral Estimation

Suppose we are interested in calculating
$$
 \int_a^b f(x) dx.
$$ 
Then, define, for instance $$X \sim \text{Uniform}(a, b)$$ in which case the pdf of $$X$$ is 

$$
p(x) = 
\begin{cases}
\frac{1}{b-a}, & x \in [a, b] \\
0, & x \not \in [a, b]
\end{cases}.
$$

Hence,

$$
\begin{align}
\int_a^b f(x)dx &=  \int_a^b f(x) \left( \frac{b-a}{b-a} \right) dx \\
&= (b-a)\int_a^b f(x) \frac{1}{b-a} dx \\
&= (b-a)\int f(x) p(x)dx \\
&= (b-a) \mathbb E [f(X)].
\end{align}
$$


Note: MC assumes the ability to generate observations, i.e. to easily simulate the process: generate random inputs and observe the correponding random outputs. This means, we need to have access to the sampling distribution $$p$$ and the underlying process $$f$$.


The above procedure is ofted called *naive* MC technique. More advanced techniques have been developed to achieve better convergence rate, such as Importance Sampling (IS).

### Importance Sampling

The basic idea for IS is to use another distribution $$\tilde p$$ to generate the samples $$X_1, X_2, \cdots, X_n$$. To maintain unbiasedness, each output $$Y_i = f(X_i)$$ is multiplied with importance weight $$W_i$$,  which is often computed as the likelihood ratio 

$$
W_i = \frac{p(X_i)}{\tilde p(X_i)}
$$

where $$p(X_i)$$ and $$\tilde p(X_i)$$ denotes the probability density of $$p$$ and $$q$$ evaluated at $$X_i$$.
With this construction, if $$\tilde p$$ is absolutely continuous w.r.t $$p$$, then IS estimator 

$$
\hat \mu_n = \frac{1}{n} \sum_{i=1}^n Y_i W_i
$$

is also unbiased.

(Proof:)

More importantly, if $$\tilde p$$ is designed properly, IS estimator can have much smaller variance than MC estimator. In fact, if we use $$\tilde p^* (x)= \frac{p(x)}{\mu}$$, the IS estimator have zero variance!

(Is this true only for rare event probability problem?)

(Proof:)

Of course, it is impractical to use $$\tilde p^*$$ because it involves $$\mu$$, which is the unknown value we want to estimate in the first place.
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
