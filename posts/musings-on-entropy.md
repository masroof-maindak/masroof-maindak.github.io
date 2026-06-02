---
title = "Musings on Entropy" date = 2026-06-02
---

## A. What is a random variable X?

- An assignment of _numbers_ to **outcomes** of a _random experiment_.

## B. What is meant by the entropy of a distribution?

- Intuitively: events that are rare are more surprising and therefore more
  valuable than 'common' ones in terms of the information they bring.

1. Likely events should have low information content (guaranteed event = no
   information)
2. The less probable an event is, the more surprising it is and the more
   information it yields.
3. Independent events are measured separately and should thus have additive
   information (since two successful independent occurrences carry twice as much
   information as one)

## C. Self-information

(AKA the information content/self-entropy) of an event x = _x_ is:
`I(_x_) = -log(P(_x_))`, and is measured in nats (assuming log is a natural log
w/ base `e`; different bases output different units -- same thing, different
scale)

- We observe that the self-entropy is shrimply the 'negative log probability' of
  an arbitrary event _x_.
- Now this begs the question;

## D. Why do we use the log-probability in particular?

> Log probability: Alternate representation of probabilities on a logarithmic
> scale (-inf,0] rather than b/w [0,1].

- Since it beautifully maps the desired requirements of our
  information-accumulating system, or more pedantically, the three axioms
  required to fulfill Shannon's definition of self information.

1. As the probabilities of independent events are generally _supposed to_
   multiply, and logarithms convert multiplication to addition, the _log_
   probabilities of independent events instead add!
   - This is _exactly_ in line w/ our third requirement from **B.**!
2. As the probability (input to log probability) decreases, the absolute
   magnitude of the logarithm increases, satisfying our second requirement i.e
   rare events should carry more information content.

D. What is meant by the entropy of a distribution?

> **Entropy**
>
> - Formally: Expected value of the self-information of a random variable.
> - Mathematically: Entropy _H(X)_ measures the average uncertainty or
>   information content in a probability distribution _p(x)_ over all possible
>   outcomes of a random variable _X_.
> - Intuitively: Expected amount of information in an event drawn from a
>   distribution
>
> `H(X) = - sum(p(x) * I(x))` where x (event) belongs to X (random variable).

- AKA Shannon Entropy
- Self information quantifies a single outcome's 'surprise'
- It can be spanned over the entire distribution via Shannon Entropy AKA entropy
- Intuitively speaking, it is trivially the aggregation of all the 'self
  informations,' weighted by each event's probability. Higher probability
  (bounded by 1) => lower log-probability => smaller multiplication result =>
  lesser contribution in summation.

---

The Kullback-Leibler (KL) Divergence is a metric of measure for how different
two separate probability distributions over the same random variable are.

I'm not going into the derivation of this expression, but it's not incredibly
challenging to comprehend, and the first site (sebastian hoenig) in the attached
links has a decent proof.

For now, just know that the KL-Divergence states:
`D_KL(P || Q) = sum( P(x) * log (P(x)/Q(x)) )` for all x within X.

Since the KL-divergence is a measure of distance, it makes sense to start w/
attempting to minimise this in an attempt to minimise the distance b/w the true
distribution and our model's predicted distribution.

Replacing `P` and `Q` w/ `y` and `yhat`, and expanding some log garbage, we get:

`D_KL(P || Q) = sum(y_i log y_i) - sum(y_i log yhat_i)`

i.e

`D_KL(P || Q) = - H(y) + H(y, y_hat)`

As the first summation term (i.e `-H(y)`) has no sign of `y_hat`'s
participation, minimising the cross-entropy loss is equivalent to the minimising
the KL divergence, thereby bringing our prediction distribution, closer to the
true distribution.

## Acknowledgements

- https://www.deeplearningbook.org/contents/prob.html
- https://en.wikipedia.org/wiki/Information_content#Definition
- https://en.wikipedia.org/wiki/Log_probability
- https://stackoverflow.com/a/63372831
- https://eoinmurray.info/aliens-and-self-information
- https://eoinmurray.info/primer-entropy-from-self-information
- https://sebastianhoenig.com/blog/cross-entropy/
- https://www.deeplearningbook.org/contents/prob.html
