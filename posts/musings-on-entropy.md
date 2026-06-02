---
title = "Musings on Entropy"
date = 2026-06-02
draft = false
---

## Why?

I wanted to learn about how cross-entropy loss works in Machine Learning.

## What is a random variable X?

An assignment of _numbers_ to **outcomes** of a _random experiment_. The reason
we care about a random variable is that it quite elegantly maps to a probability
distribution, be it _discrete_ or _continuous_.

Regardless of the type, it describes how the probabilities are distributed over
the possible outcomes (random values, i.e `x_1, x_2 ... x_n`) of the random
variable, X.

## What is meant by the entropy of a distribution?

Intuitively: events that are rare are more surprising, and therefore more
valuable than 'common' ones in terms of the information they bring.

1. Likely events should have low information content (guaranteed event = no
   information)
2. The less probable an event is, the more surprising it is and the more
   information it yields.
3. Independent events are measured separately and should thus have additive
   information (since two successful independent occurrences carry twice as much
   information as one)

## Self-information

(AKA the information content/self-entropy) of an event `X = x` is:
`I(x) = -log(P(x))`, and is measured in 'nats' (assuming the log applied is a
natural log w/ base `e`; different bases output units other than nats).

The key takeaway here is that the self-entropy is simply the 'negative log
probability' of some arbitrary event _x_, from the random variable _X_.

## Why use the log-probability in particular?

> Log probability: Alternate representation of probabilities on a logarithmic
> scale `(-inf,0]` rather than b/w `[0,1]`.

Since it beautifully maps the desired requirements of our
information-accumulating system, or more pedantically, the three axioms required
to fulfill Shannon's definition of self information.

1. As the probabilities of independent events are generally _supposed to_
   multiply, and logarithms convert multiplication to addition, the _log_
   probabilities of independent events instead add!
   - This is _exactly_ in line w/ our third requirement from **B.**!
2. As the probability (input to log probability) decreases, the absolute
   magnitude of the logarithm increases, satisfying our second requirement i.e
   rare events should carry more information content.

## What is meant by the entropy of a distribution (02)?

> - Formally: Expected value of the self-information of a random variable.
> - Mathematically: Entropy _H(X)_ measures the average uncertainty or
>   information content in a probability distribution _p(x)_ over all possible
>   outcomes of a random variable _X_.
> - Intuitively: Expected amount of information in an event drawn from a
>   distribution; alternatively: how much information each observation provides.
>
> `H(X) = -sum(p(x) * I(x))` where x (event) belongs to X (random variable).

- As Discussed in **B.**, self information quantifies a single outcome's
  'surprise'
- Using the aforementioned concepts, we see that entropy can be extrapolated to
  the entire distribution.
- This is trivial to do by merely aggregating all the 'self informations,'
  weighted by their corresponding outcome's probability. A higher probability
  (bounded by 1) means that that outcome would have a lower log-probability,
  consequently resulting in a smaller multiplication result, thereby ultimately
  contributing less to the summation.

## Kullback-Leibler (KL) Divergence

KL Divergence is a metric of measure for how different two separate probability
distributions over the same random variable are.

I'm not going into the derivation of this expression, but it's not incredibly
challenging to comprehend, and Sebastian Hoenig's site in the acknowledgements
links has a decent proof, should you wish to dive deeper.

For now, just know that the KL-Divergence states:
`D_KL(P || Q) = sum( P(x) * log (P(x)/Q(x)) )` for all x within X, where P and Q
are different probability distributions.

Since the KL-divergence is a measure of distance, it is ostensibly sensible to
start w/ attempting to minimise this value in an attempt to minimise the
distance b/w the true distribution and our model's predicted distribution.

Replacing `P` and `Q` w/ `y` and `yhat` respectively, and expanding the log out
(as per the log division rule) out, we ultimately obtain:
`D_KL(y || yhat) = sum(y_i log y_i) - sum(y_i log yhat_i)`

That is to say:

`D_KL(y || yhat) = -H(y) + H(y, y_hat)`

FIXME:

Notice how the first summation term (i.e `-H(y)`) has no sign of `y_hat`. This
allows us to affirm that minimising the cross-entropy loss is equivalent to the
minimising the KL divergence, thereby bringing our prediction distribution,
closer to the true distribution.

## Acknowledgements

- <https://statgrades.berkeley.edu/~stark/SticiGui/Text/randomVariables.htm>
- <https://www.deeplearningbook.org/contents/prob.html>
- <https://en.wikipedia.org/wiki/Information_content#Definition>
- <https://en.wikipedia.org/wiki/Log_probability>
- <https://stackoverflow.com/a/63372831>
- <https://eoinmurray.info/aliens-and-self-information>
- <https://eoinmurray.info/primer-entropy-from-self-information>
- <https://sebastianhoenig.com/blog/cross-entropy/>
