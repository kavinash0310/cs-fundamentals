# Chapter 6 — Probability and Statistics

## 1. Probability rules

For finite equally likely outcomes, P(A)=|A|/|S|. Complement: P(Aᶜ)=1-P(A). Addition: P(A∪B)=P(A)+P(B)-P(A∩B).

Conditional probability P(A|B)=P(A∩B)/P(B), when P(B)>0. Independence means P(A∩B)=P(A)P(B); equivalently conditioning on B does not change A's probability.

Bayes theorem:
~~~text
P(A|B)=P(B|A)P(A)/P(B)
~~~
Use a tree or table to avoid confusing P(A|B) with P(B|A).

## 2. Random variables and expectation

A random variable maps outcomes to numbers. For discrete X, E[X]=Σx P(X=x). Variance is E[(X-μ)²]=E[X²]-μ²; standard deviation is square root of variance.

Linearity: E[aX+bY]=aE[X]+bE[Y], even without independence. For independent X,Y, Var(X+Y)=Var(X)+Var(Y); otherwise covariance matters.

## 3. Distributions

- Bernoulli(p): one success/failure trial; mean p, variance p(1-p).
- Binomial(n,p): successes in n independent Bernoulli trials; mean np, variance np(1-p).
- Geometric(p): trials until first success; with support 1,2,..., mean 1/p.
- Poisson(λ): count of events in a fixed interval under a rate model; mean and variance λ.
- Uniform discrete: each value in a finite set is equally likely.
- Normal: continuous bell-shaped distribution with mean μ and variance σ².

Know assumptions, not only formulas. A binomial model needs fixed n and independent trials with common p.

## 4. Descriptive statistics

Mean is sum/count; median is middle after sorting; mode is most frequent. Range is max-min. Variance measures squared spread around the mean. Outliers can shift mean more than median.

For data 1,2,2,9: mean=3.5, median=2, mode=2. The mean is pulled upward by 9.

## 5. Worked Bayes example

A test detects a condition with sensitivity 0.9 and false-positive probability 0.1. Prevalence is 0.01. In 10,000 people, expect 100 with condition; 90 test positive. Of 9,900 without it, 990 test positive. Thus among 1,080 positives, 90 truly have the condition: P(condition|positive)=90/1080≈0.0833. High sensitivity does not imply a positive result is highly predictive when prevalence is low.

## 6. Questions and practice

**Q1.** If A and B are independent, what is P(A∩B)? **A:** P(A)P(B).
**Q2.** A fair die shows an even number. Probability? **A:** 3/6=1/2.
**Q3.** Binomial mean and variance? **A:** np and np(1-p).
**Q4.** When does Var(X+Y) add? **A:** For independent variables (or zero covariance).
**Q5.** What does median resist better than mean? **A:** Extreme outliers.
**Practice:** Compute a conditional probability from a two-way table; calculate expectation of a small discrete variable; choose a distribution for repeated independent trials; solve a Bayes diagnostic problem.

## Notebook work

Draw a probability tree, a histogram, and a normal curve. Label mean, median, variance, and conditional probability.


## Distribution and Bayes worked examples

**Binomial:** For 5 independent fair coin flips, X=head count has mean 2.5 and variance 1.25. P(X=3)=C(5,3)/2⁵=10/32.

**Poisson:** For X~Poisson(λ), P(X=k)=e^-λ λ^k/k!, E[X]=Var(X)=λ. It models counts in a fixed interval under a constant-rate/independence approximation.

**Exponential:** For waiting time T with rate λ, P(T>t)=e^-λt for t≥0; mean=1/λ. It is memoryless: P(T>s+t | T>s)=P(T>t). State units for λ.

**Normal:** Standardize X~N(μ,σ²) with Z=(X-μ)/σ. This converts questions to standard-normal probabilities/table values.

**Conditional example:** If 30 of 100 students study both A and B, 50 study B, then P(A|B)=30/50=0.6. This differs from P(B|A) unless denominators match.

**Practice:** For Binomial(10,.2), mean=2 and variance=1.6. For Poisson(3), P(X=0)=e^-3. For an exponential rate .5/hour, mean waiting time is 2 hours. Always identify the random variable and model assumptions.

## Extended lesson: conditional probability and distributions

Conditional probability is P(A|B)=P(A∩B)/P(B), for P(B)>0. Bayes' rule reverses the conditioning:
P(A|B)=P(B|A)P(A)/P(B).

**Medical-test style example.** Prevalence is 1%; sensitivity is 99%; false-positive rate is 5%. Among 10,000 people, about 100 have the condition and 9,900 do not. Expected positives: 99 true positives plus 495 false positives. So the probability a positive result is a true case is 99/(99+495)=1/6≈16.7%, not 99%. The base rate matters.

For a discrete random variable X, E[X]=ΣxP(X=x). Variance is E[X²]−E[X]². A fair die has E[X]=(1+2+3+4+5+6)/6=3.5 and E[X²]=91/6; variance=91/6−(3.5)²=35/12.

### Binomial and Poisson models

If n independent trials each have success probability p, X~Binomial(n,p), with mean np and variance np(1-p). Example: n=10, p=0.2 gives expected successes 2 and variance 1.6.

For a Poisson count with rate λ per interval, P(X=k)=e^-λ λ^k/k!, and mean and variance both equal λ. It approximates rare independent events over many opportunities when np=λ is moderate. Check assumptions before choosing a distribution.

### Descriptive statistics

Mean is sensitive to extreme values; median is the middle ordered observation and is more robust. Variance measures squared spread around the mean; standard deviation is its square root and returns to the original units. Correlation measures linear association, not causation, and may miss nonlinear relationships.

### Questions with answers

1. If A and B are independent, what is P(A∩B)? P(A)P(B).
2. Does a 99%-sensitive test imply a positive result has a 99% chance of disease? No; specificity and prevalence also matter.
3. For Bernoulli(p), mean and variance? p and p(1-p).
4. When is median preferable to mean? When outliers/skew make the average unrepresentative.
