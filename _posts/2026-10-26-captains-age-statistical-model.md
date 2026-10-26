---
title: "Estimating the Captain's Age: A Step-by-Step Statistical Model"
date: 2026-10-26
author: Mauro Ghiani
category: Odds
tags: [Statistics, Bayesian, Probability, Riddle, Python, Modeling]
excerpt: "A boat carries a captain and 35 sheep. How old is the captain? A Bayesian model gives 50, but only because of where the analyst put the thresholds: the model returns whatever the analyst puts in."
---

*Version 3: the prior mean is no longer fixed; Section 11 reports the
posterior for every prior setting.*

This document uses **plain-text formulas** so they remain readable in
Markdown viewers that do not render LaTeX.

## 1. The original riddle

A boat carries a captain and 35 sheep. How old is the captain?

Taken literally, the information is insufficient to determine the
captain's age. The number of sheep on the boat does not, by itself, tell
us the captain's age.

To explore a possible statistical interpretation, we construct a
hypothetical model using:

- a prior distribution for the captain's age;
- a step function for age-related experience;
- a survival model for the 35 sheep, driven by that experience level.

The model is illustrative. Its assumptions are not established facts
about the captain or the sheep.

## 2. Prior distribution for the captain's age

Assume that the captain's age, A, follows a normal distribution:

**A ~ Normal(mean = 47, standard deviation = 6.36)**

Equivalently, using variance as the second parameter:

**A ~ N(47, 6.36²)**

The variance is:

**6.36² = 40.4496 years²**

The prior density is proportional to:

**f₀(a) ∝ exp(−(a − 47)² / (2 × 6.36²))**

Here:

- `a` is a possible captain age;
- 47 years is the assumed prior mean;
- 6.36 years is the assumed prior standard deviation.

The value 47 is the center of the assumed prior distribution; it is not
inferred from the 35 sheep. **It is an arbitrary choice**: it may feel
intuitively plausible, but nothing in the riddle supports it. Sections
2–10 use it as a worked example; Section 11 drops it and recomputes the
posterior for every prior mean μ and standard deviation σ.

## 3. A step function for role-related experience

Rather than assuming experience grows linearly with age, we use fixed
age bands. These cutoffs are modeling choices, not universal rules.

**E(a) =**

- **0**, if a < 25;
- **1**, if 25 ≤ a < 35;
- **2**, if 35 ≤ a < 50;
- **3**, if a ≥ 50.

Interpretation:

- Below 25: excluded by this illustrative model.
- Ages 25–34: initial professional experience.
- Ages 35–49: established experience.
- Age 50 and above: potentially advanced experience.

The levels are ordinal categories. They do not imply that a person in
one band has a proportional amount of experience compared with someone
in another band.

A step function alone does not define a probability model. Age affects
the likelihood **only through** the experience level: the next section
assigns a survival probability to each level, so that q(a) = g(E(a)).
The numerical values 0–3 are labels and never enter the calculation
directly.

## 4. Modeling sheep survival

Let `g(e)` be the assumed probability that one sheep survives for one
year when cared for by a captain with experience level `e`, and write
`q(a) = g(E(a))`. For illustration, assume:

| Captain age `a` | Experience level `E(a)` | Assumed annual survival probability `q(a)` |
|-----------------|:-----------------------:|:------------------------------------------:|
| a < 25          | 0                       | 0                                          |
| 25 ≤ a < 35     | 1                       | 0.90                                       |
| 35 ≤ a < 50     | 2                       | 0.95                                       |
| a ≥ 50          | 3                       | 0.97                                       |

**These probabilities are hypothetical, not empirical estimates.** There
is no evidence here that a captain's age determines sheep survival at
these rates. Actual survival would depend on animal health, food, water,
weather, husbandry skill, and conditions aboard the boat.

Assume, solely for this example:

1. All 35 sheep are alive at the start.
2. Their survival outcomes are independent, conditional on the assumed
   probability.
3. We observe that all 35 survive for one year.

Then the probability that all 35 survive is:

**L(D | a) = q(a)^35**

Here, `D` is the observation that all 35 sheep survive for one year.

The exponent 35 makes the group size relevant: the probability that all
animals survive is the individual survival probability raised to the
35th power. It is also the **only** place where the number of sheep
enters the model.

The original riddle does not state that the sheep survive for a year,
nor does it provide their ages or health conditions. This survival
observation is an extra assumption.

## 5. Combining the prior and likelihood

Under the illustrative assumptions, Bayes' rule gives a posterior
density proportional to:

**f(a | D) ∝ f₀(a) × L(D | a)**

Substituting the formulas:

**f(a | D) ∝ exp(−(a − 47)² / (2 × 6.36²)) × q(a)^35**

A normalizing constant is needed to make this a probability density
integrating to one, but it does not affect the location of the maximum.

The maximum a posteriori (MAP) estimate is:

**a_MAP = argmax over a of f(a | D)**

This is the age with the highest posterior density under the chosen
model.

## 6. Why the illustrative model gives 50 years

Within each age band, `q(a)` is constant. Therefore, within each band,
the likelihood factor `q(a)^35` is constant, and the posterior density
is proportional to the prior density. The candidate maximum in each band
is therefore the point of that band closest to 47.

**Band a < 25.** The likelihood is 0, so the posterior is 0. Excluded.

**Band 25 ≤ a < 35.** The prior increases toward 35, but 35 itself is
not in this band, so the posterior has no maximum here; its supremum is
approached as a → 35. Compared with age 47:

**f(35⁻ | D) / f(47 | D) = exp(−(35 − 47)² / (2 × 6.36²)) × (0.90 / 0.95)^35**

≈ 0.1686 × 0.1507 ≈ **0.025**

This band is strongly disfavored.

**Band 35 ≤ a < 50.** The maximum is at age 47, the prior mean.

**Band a ≥ 50.** The prior decreases as age moves away from 47, so the
maximum is at the boundary, age 50. Because this band includes 50
(a ≥ 50), the maximum is actually attained there.

Comparing the two remaining candidates:

**f(50 | D) / f(47 | D) = exp(−(50 − 47)² / (2 × 6.36²)) × (0.97 / 0.95)^35**

- The prior-density factor is approximately **0.8947**.
- The likelihood ratio is approximately **2.0734**.
- Their product is approximately **0.8947 × 2.0734 ≈ 1.855**.

Since 1.855 is greater than 1, the posterior density at age 50 exceeds
the density at age 47, and therefore exceeds the posterior density at
every other age:

**MAP estimate: a_MAP = 50 years**

This is the result of the chosen illustrative assumptions, not a fact
established by the riddle.

## 7. Sensitivity of the result

The MAP moves from 47 to 50 only if the likelihood ratio outweighs the
prior penalty, that is, if:

**(q₃ / 0.95)^35 > 1 / 0.8947**

where `q₃` is the survival probability assumed for the band a ≥ 50.
Solving for `q₃`:

**q₃ > 0.95 × exp((9 / (2 × 6.36²)) / 35) ≈ 0.9530**

In other words, any value of `q₃` larger than about 0.953 (just three
thousandths above the 0.95 of the previous band) makes 50 the MAP
estimate; any value below it leaves the MAP at 47. The exponent 35
amplifies tiny differences in assumed survival probabilities, so the
result is driven by the choice of inputs rather than by any data.

## 8. Extension: the captain can die too

So far the captain is assumed to survive the whole year. A natural
refinement is to let the captain's own mortality depend on age, since
an older captain is more likely to die during the voyage and leave the
sheep without care.

### 8.1 Captain mortality

Let `m(a)` be the probability that a captain of age `a` dies during the
year. Adult human mortality grows roughly exponentially with age
(Gompertz law), so for illustration we use:

**m(a) = 0.003 × 2^((a − 50) / 8)**

This gives about 3 deaths per 1,000 at age 50, doubling every 8 years.
These values are only of the right order of magnitude for Italian men;
**they are not taken from the ISTAT life tables**, and a real analysis
should replace them with the official age-specific death probabilities.

| Age `a` | m(a)    |
|--------:|--------:|
| 47      | 0.00231 |
| 50      | 0.00300 |

### 8.2 New likelihood

If the captain survives, each sheep survives with probability `q(a)` as
before. If the captain dies, the sheep are left without care and each
survives with an assumed probability of 0.5. The likelihood becomes:

**L(D | a) = (1 − m(a)) × q(a)^35 + m(a) × 0.5^35**

Since 0.5^35 ≈ 2.9 × 10⁻¹¹, the second term is negligible and:

**L(D | a) ≈ (1 − m(a)) × q(a)^35**

### 8.3 Effect on the estimate

The new factor in the comparison between ages 50 and 47 is:

**(1 − 0.00300) / (1 − 0.00231) ≈ 0.9993**

so the posterior ratio changes from about 1.855 to about **1.854**.
**The MAP estimate remains 50 years.**

Within the 35–49 band, `m(a)` now makes the posterior slightly
decreasing in age even above the prior mean, which moves the local
maximum from 47 to about 46.992 years, a shift of roughly three days.

The effect is negligible because, between 40 and 60, annual mortality
is a few per thousand, while the differences in sheep survival, raised
to the 35th power, change the likelihood by a factor of about two.
Captain mortality would matter only for much older captains or much
longer voyages.

### 8.4 Consequence for the independence assumption

The more important consequence is conceptual. Once the captain can die,
the 35 sheep are **no longer independent**: they share a common fate,
because if the captain dies they are all affected at once. Their
outcomes are independent only *conditional on* the captain surviving.
The original model's assumption of unconditional independence ignored
this common cause, which is a typical source of error in models that
multiply probabilities as if events were independent.

## 9. From a point estimate to an interval

A single number hides how uncertain the model is. This section reports
ranges of ages together with their probability.

### 9.1 Variance versus standard deviation

The variance, 40.4496, is measured in **years²**, so it cannot be used
directly as a range of ages. Intervals are built with the standard
deviation, its square root: **6.36 years**.

### 9.2 Prior intervals (before looking at the sheep)

For a normal distribution:

| Interval              | Ages              | Probability |
|-----------------------|-------------------|------------:|
| 47 ± 1 × 6.36         | 40.6 – 53.4 years | ≈ 68.3%     |
| 47 ± 1.96 × 6.36      | 34.5 – 59.5 years | ≈ 95%       |

These intervals are symmetric around 47 and say nothing about the sheep.

### 9.3 Posterior intervals (after the sheep observation)

The posterior of Section 8 (step likelihood plus captain mortality) is
not normal, so its intervals were computed numerically on a grid of
ages with a step of 0.001 years. The table reports the **highest
posterior density (HPD) intervals**, the shortest ranges of ages that
contain the stated probability; in this model each is a single
contiguous interval.

| Summary                    | Value             |
|----------------------------|------------------:|
| MAP (mode)                 | 50.0 years        |
| Posterior mean             | ≈ 49.1 years      |
| Posterior median           | ≈ 50.0 years      |
| P(age ≥ 50)                | ≈ 50.1%           |
| 68.3% HPD interval         | 43.0 – 55.7 years |
| 95% HPD interval           | 36.5 – 60.0 years |

Compared with the prior, the posterior intervals are **shifted upward**
and are **no longer symmetric**: the step in the likelihood at age 50
pushes probability mass above the threshold. Mode, median and mean all
differ, which is typical of a skewed distribution with a jump.

## 10. Hierarchical variant: an uncertain prior mean

So far the prior mean, 47, has been treated as known. If we are also
unsure about it, we can make it a random variable:

**μ ~ N(47, τ²)**
**A | μ ~ N(μ, 6.36²)**

Integrating out μ, the marginal prior for the captain's age is still
normal, with the same mean and a larger spread:

**A ~ N(47, 6.36² + τ²)**

For illustration, take **τ = 3 years**. The total standard deviation
becomes:

**√(6.36² + 3²) = √49.4496 ≈ 7.03 years**

The prior penalty for moving from 47 to 50 becomes weaker:

**exp(−(50 − 47)² / (2 × 49.4496)) ≈ 0.913** (instead of 0.895)

so the posterior ratio between ages 50 and 47 grows and the MAP estimate
**remains 50 years**. The intervals widen:

| Summary                    | Base model (sd 6.36) | Hierarchical (τ = 3) |
|----------------------------|---------------------:|---------------------:|
| MAP (mode)                 | 50.0                 | 50.0                 |
| Posterior mean             | ≈ 49.1               | ≈ 49.5               |
| Posterior median           | ≈ 50.0               | ≈ 50.3               |
| P(age ≥ 50)                | ≈ 50.1%              | ≈ 52.4%              |
| 68.3% HPD interval         | 43.0 – 55.7          | 42.7 – 56.5          |
| 95% HPD interval           | 36.5 – 60.0          | 35.8 – 61.0          |

Admitting uncertainty about the prior mean does not move the point
estimate, but it makes the stated ranges wider and therefore more
honest. The value τ = 3 is itself an assumption.

## 11. All the distributions: dropping the fixed prior mean

The number 47 was chosen by hand, and the hierarchical variant of
Section 10 still centers everything on it. Rather than defending any
single value, this section treats the prior mean μ and the prior
standard deviation σ as free parameters and reports the posterior for
all of them:

**A ~ N(μ, σ²)**, with μ from 25 to 70 years and σ from 2 to 12 years.

The likelihood is the one of Section 8 (survival steps plus captain
mortality). All results were computed numerically on a grid of ages.

### 11.1 A closed-form rule for the MAP

Let **R ≈ 2.072** be the likelihood ratio between the band a ≥ 50 and
the band 35 ≤ a < 50, evaluated at their best points (Section 8.3:
2.0734 × 0.9993). For a prior mean between 35 and 50, the MAP jumps to
the threshold 50 whenever the prior penalty for moving from μ to 50 is
smaller than R:

**exp(−(50 − μ)² / (2σ²)) × R > 1  ⟺  μ > 50 − σ × √(2 ln R) ≈ 50 − 1.21σ**

Therefore, approximately:

- if **μ ≥ 50**: MAP ≈ μ (slightly below it, because of captain mortality);
- if **50 − 1.21σ < μ < 50**: MAP = **50**, the threshold;
- if **35 ≤ μ ≤ 50 − 1.21σ**: MAP = μ;
- if **μ < 35**: MAP = 35 (the lower threshold) unless σ is small, in
  which case the mode stays inside the 25–34 band.

| σ (years) | Range of μ that gives MAP = 50 |
|----------:|-------------------------------:|
| 4         | 45.2 – 50                      |
| 6.36      | 42.3 – 50                      |
| 9         | 39.1 – 50                      |

The answer "50" is thus not a property of the data: it is produced by a
window of prior means just below the threshold, and the window widens
as the prior becomes more vague.

### 11.2 Full table for σ = 6.36

The 95% intervals here are **equal-tailed** (2.5% of probability on each
side). Unlike the HPD intervals of Section 9, they are always a single
range, whereas for some priors the HPD region splits into two pieces
around the jumps at 35 and 50.

| μ  | MAP  | Mean | Median | P(age ≥ 50) | 95% interval |
|---:|-----:|-----:|-------:|------------:|-------------:|
| 30 | 35.0 | 36.2 | 36.6 | 0.6%  | 26.0 – 45.7 |
| 32 | 35.0 | 37.4 | 37.4 | 1.2%  | 26.7 – 47.4 |
| 34 | 35.0 | 38.6 | 38.2 | 2.4%  | 27.8 – 49.8 |
| 36 | 36.0 | 39.9 | 39.3 | 4.5%  | 29.3 – 51.4 |
| 38 | 38.0 | 41.2 | 40.5 | 8.1%  | 31.3 – 53.0 |
| 40 | 40.0 | 42.7 | 42.0 | 13.7% | 33.5 – 54.7 |
| 42 | 42.0 | 44.4 | 43.8 | 21.6% | 35.1 – 56.3 |
| 44 | 50.0 | 46.3 | 46.0 | 32.0% | 35.7 – 58.1 |
| 46 | 50.0 | 48.2 | 48.6 | 43.9% | 36.5 – 59.8 |
| 48 | 50.0 | 50.1 | 50.7 | 56.2% | 37.7 – 61.5 |
| 50 | 50.0 | 51.9 | 52.1 | 67.7% | 39.3 – 63.3 |
| 52 | 52.0 | 53.6 | 53.6 | 77.5% | 41.2 – 65.0 |
| 54 | 54.0 | 55.2 | 55.1 | 85.2% | 43.3 – 66.8 |
| 56 | 56.0 | 56.9 | 56.7 | 90.8% | 45.4 – 68.7 |
| 58 | 58.0 | 58.6 | 58.4 | 94.7% | 47.5 – 70.6 |
| 60 | 60.0 | 60.4 | 60.2 | 97.1% | 49.5 – 72.5 |
| 62 | 62.0 | 62.2 | 62.1 | 98.5% | 50.8 – 74.4 |
| 64 | 64.0 | 64.1 | 64.0 | 99.3% | 52.2 – 76.4 |

### 11.3 Other prior widths

| μ  | MAP (σ = 4) | 95% interval (σ = 4) | MAP (σ = 9) | 95% interval (σ = 9) |
|---:|------------:|---------------------:|------------:|---------------------:|
| 35 | 35.0 | 29.9 – 43.8 | 35.0 | 28.5 – 57.0 |
| 40 | 40.0 | 35.1 – 48.5 | 50.0 | 32.3 – 60.7 |
| 45 | 45.0 | 37.6 – 53.9 | 50.0 | 35.5 – 64.6 |
| 50 | 50.0 | 42.9 – 58.3 | 50.0 | 37.2 – 68.8 |
| 55 | 55.0 | 48.4 – 62.9 | 55.0 | 40.6 – 73.2 |
| 60 | 60.0 | 52.4 – 67.8 | 59.9 | 45.2 – 77.8 |

### 11.4 No preferred age at all: a flat prior

If we refuse to pick any center and give every age between 25 and an
upper limit the same prior weight, the posterior is driven only by the
likelihood. Its maximum is then always at **50**, the start of the best
survival band, where captain mortality is lowest within that band. But
the rest of the distribution depends entirely on the arbitrary upper
limit:

| Flat prior on | MAP | Mean | Median | P(age ≥ 50) | 95% interval |
|--------------:|----:|-----:|-------:|------------:|-------------:|
| 25 – 70       | 50  | 54.7 | 56.0 | 71% | 34.6 – 69.3 |
| 25 – 80       | 50  | 59.9 | 60.9 | 79% | 35.4 – 79.0 |
| 25 – 90       | 50  | 64.9 | 65.6 | 83% | 35.9 – 88.7 |

The flat prior shows most clearly where "50" comes from: not from the
sheep, but from the threshold placed at 50 in the step function.

### 11.5 Figures

![Posterior distributions for six prior means](/assets/images/fig1_posteriors_by_prior_mean.png)

*Figure 1. Posterior densities for μ = 35, 40, …, 60 (σ = 6.36). Every
curve jumps at the thresholds 25, 35 and 50; for μ = 45 and 50 the
highest point is the spike at 50.*

![MAP, median and 95% interval versus prior mean](/assets/images/fig2_summaries_vs_prior_mean.png)

*Figure 2. MAP, median and 95% interval as functions of μ (σ = 6.36).
The MAP follows the prior mean except on two plateaus: at 35 for small
μ and at 50 for μ between about 42.3 and 50.*

![MAP over all combinations of prior mean and standard deviation](/assets/images/fig3_map_heatmap.png)

*Figure 3. MAP for every combination of μ and σ. The dashed line is the
switch rule μ = 50 − 1.21σ; the uniform band between it and μ = 50 is
the region where the answer is exactly 50. In the lower-left corner
(small σ, μ below about 31) the mode stays inside the 25–34 band.*

### 11.6 What the sweep shows

Across all prior settings, the posterior mostly **follows the prior**:
the MAP equals μ for most values, and the 95% intervals are between
about 14 and 53 years wide. The only systematic effects of the sheep data are a
shift of probability toward ages above 50 and the two plateaus created
by the thresholds. A result that so closely tracks an assumption chosen
by hand is a property of the model, not knowledge about the captain.

## 12. Limitations and interpretation

1.  **The original information does not identify the captain's age.**
    Knowing only that there are 35 sheep aboard does not determine the
    captain's age.
2.  **The prior is assumed, not observed.** The normal distribution
    centered at 47 was stipulated for this exercise; Section 11 shows
    that the answer changes with almost any other choice.
3.  **The age bands are subjective modeling choices.** The thresholds
    25, 35, and 50 are not universal requirements for being a captain.
4.  **The survival probabilities are hypothetical.** The values 0.90,
    0.95, and 0.97 were selected to demonstrate how a stepwise
    likelihood can shift a posterior mode.
5.  **The survival observation is an extra assumption.** The original
    riddle does not state that all 35 sheep survive for a year.
6.  **The hyperparameter τ is assumed.** The hierarchical variant
    replaces one arbitrary number (the certainty of 47) with another
    (τ = 3).
7.  **Age is not the same as experience or care quality.** A captain's
    age alone would not justify the assumed survival probabilities.
8.  **The result is highly sensitive to the inputs.** As shown in
    Section 7, a change of a few thousandths in one assumed probability
    is enough to move the estimate.
9.  **Independence holds only conditionally.** As Section 8 shows,
    the sheep share a common dependence on the captain, so their
    outcomes are independent only given the captain's fate.
10. **A maximum is model-dependent.** A sufficiently specified model can
    return a numerical result, but that does not mean the result is
    supported by the original evidence.

For a scientifically defensible model, we would need data about sheep
survival under different husbandry conditions, a justified model linking
care to survival, information about the sheep and observation period,
and a defensible prior for captains' ages.

## 13. Numerical check

The figures above can be reproduced with a few lines of Python:

```python
import math

var = 6.36 ** 2
prior_50 = math.exp(-(50 - 47) ** 2 / (2 * var))   # ≈ 0.8947
lr_50 = (0.97 / 0.95) ** 35                         # ≈ 2.0734
print(prior_50 * lr_50)                             # ≈ 1.855

prior_35 = math.exp(-(35 - 47) ** 2 / (2 * var))   # ≈ 0.1686
lr_35 = (0.90 / 0.95) ** 35                         # ≈ 0.1507
print(prior_35 * lr_35)                             # ≈ 0.025

print(0.95 * math.exp((9 / (2 * var)) / 35))        # threshold ≈ 0.9530

# Extension with captain mortality (Section 8)
m = lambda a: 0.003 * 2 ** ((a - 50) / 8)
L = lambda a, q: (1 - m(a)) * q ** 35 + m(a) * 0.5 ** 35
print(prior_50 * L(50, 0.97) / L(47, 0.95))         # ≈ 1.854
```

The posterior summaries and HPD intervals of Sections 9 and 10 can be
reproduced on a grid (requires NumPy):

```python
import numpy as np

def summarize(sd):
    a = np.arange(15, 95, 0.001)
    q = np.select([a < 25, a < 35, a < 50], [0, 0.90, 0.95], 0.97)
    m = 0.003 * 2 ** ((a - 50) / 8)
    post = np.exp(-(a - 47) ** 2 / (2 * sd ** 2)) * ((1 - m) * q ** 35 + m * 0.5 ** 35)
    post /= post.sum()
    cdf = post.cumsum()
    order = np.argsort(-post)
    mass = post[order].cumsum()
    hpd = {lvl: (a[order[:np.searchsorted(mass, lvl)]].min(),
                 a[order[:np.searchsorted(mass, lvl)]].max())
           for lvl in (0.683, 0.95)}
    return {"mode": a[post.argmax()], "mean": (a * post).sum(),
            "median": a[np.searchsorted(cdf, 0.5)],
            "P(a>=50)": post[a >= 50].sum(), "HPD": hpd}

print(summarize(6.36))                      # base model
print(summarize(np.sqrt(6.36**2 + 3**2)))   # hierarchical, tau = 3
```

The sweep of Section 11 (equal-tailed intervals) and the switch rule:

```python
import math
import numpy as np

a = np.arange(15, 100, 0.001)
q = np.select([a < 25, a < 35, a < 50], [0, 0.90, 0.95], 0.97)
m = 0.003 * 2 ** ((a - 50) / 8)
L = (1 - m) * q ** 35 + m * 0.5 ** 35

def sweep(prior):
    post = prior * L
    post /= post.sum()
    cdf = post.cumsum()
    qt = lambda p: a[np.searchsorted(cdf, p)]
    return a[post.argmax()], (a * post).sum(), qt(0.5), post[a >= 50].sum(), qt(0.025), qt(0.975)

for mu in range(30, 66, 2):                              # Section 11.2
    print(mu, sweep(np.exp(-(a - mu) ** 2 / (2 * 6.36 ** 2))))

for hi in (70, 80, 90):                                  # Section 11.4
    print(hi, sweep(((a >= 25) & (a <= hi)).astype(float)))

R = (0.97 / 0.95) ** 35 * (1 - 0.003) / (1 - 0.003 * 2 ** (-3 / 8))
for sd in (4, 6.36, 9):                                  # Section 11.1
    print(sd, 50 - sd * math.sqrt(2 * math.log(R)))
```

## 14. Final conclusion

There are two different answers:

-   **For the original riddle:** the captain's age cannot be determined
    from the information provided.
-   **For the illustrative Bayesian model with the prior centered at
    47:** the MAP estimate is **50 years**, conditional on the assumed prior, age thresholds, survival
    probabilities, independence assumptions, and hypothetical
    observation that all 35 sheep survive for one year. Adding an
    age-dependent mortality for the captain does not change this
    result. Expressed as a range, the 95% highest posterior density
    interval is about 36.5 – 60.0 years (35.8 – 61.0 years if the prior
    mean is itself uncertain with τ = 3), which shows how little the
    model really pins down.
-   **Across all prior settings (Section 11):** the MAP is 50 only when
    the prior mean lies roughly between 50 − 1.21σ and 50, or when the
    prior is flat. Otherwise the estimate essentially reproduces the
    prior mean. The model returns whatever the analyst puts in, which
    is the statistical version of the riddle's lesson.

The value 50 is an example of how a stepwise likelihood can move a
posterior mode. It is not a statistically justified answer to the
original riddle.
