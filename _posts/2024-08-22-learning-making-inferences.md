---
title: "Learning Is Making Inferences"
date: 2024-08-22
author: Mauro Ghiani
category: AI
tags: [PrimeNumbers, NeuralNetworks, Mersenne, Fermat, Empiricism]
excerpt: "When Marin Mersenne was ordained into the Minim Order, he took vows of poverty and perpetual abstinence. But the Minims valued knowledge."
---

When **Marin Mersenne** (1588–1648) was ordained into the **Minim Order**, he took vows of poverty and perpetual abstinence. But the Minims valued knowledge. Mersenne became one of the great intellectual connectors of his age, corresponding with Descartes, Fermat, Pascal, and Galileo.

And he left behind a particular obsession: prime numbers of a special form.

## Mersenne Primes

A **Mersenne prime** is a prime number of the form:

**M_p = 2^p - 1**

where **p** itself must be prime.

The first eight Mersenne primes are:

| p  | M_p          |
|----|--------------|
| 2  | 3            |
| 3  | 7            |
| 5  | 31           |
| 7  | 127          |
| 13 | 8191         |
| 17 | 131071       |
| 19 | 524287       |
| 31 | 2147483647   |

As of today, **51 Mersenne primes** are known. They grow extraordinarily large — the largest known primes are almost always Mersenne primes.

But not every prime **p** yields a Mersenne prime. Take **n = 11**:

**2^11 - 1 = 2047 = 23 × 89**

Composite. The pattern breaks.

## Fermat Numbers

Pierre de Fermat proposed another family of numbers:

**F_n = 2^(2^n) + 1**

The first few Fermat numbers are:

- F_0 = 3
- F_1 = 5
- F_2 = 17
- F_3 = 257
- F_4 = 65537

All prime. Fermat conjectured that all numbers of this form would be prime.

He was wrong.

**F_5 = 4294967297 = 641 × 6700417**

Euler proved this in 1732. No Fermat number beyond F_4 has ever been shown to be prime.

## Fermat's Little Theorem

Fermat also gave us a tool for testing primality. **Fermat's Little Theorem** states:

If **p** is prime and **a** is not divisible by **p**, then:

**a^(p-1) ≡ 1 (mod p)**

This gives us a probabilistic test. If the equation doesn't hold, the number is definitely composite. If it does hold, the number is *probably* prime.

But there are exceptions: **pseudoprimes** — composite numbers that pass the Fermat test for certain bases.

And worse: **Carmichael numbers**. The smallest is **561 = 3 × 11 × 17**. Carmichael numbers pass the Fermat test for *every* base coprime to them. They are the ultimate deceivers.

## The Limits of Empiricism

Fermat numbers illustrate a deep problem with **empiricism**. The first five Fermat numbers are all prime. An empiricist, observing this pattern, might reasonably conclude that all Fermat numbers are prime. The data supports it.

But the data is misleading. F_5 shatters the conjecture.

This is not unlike the debate between **Noam Chomsky** and **Slavoj Žižek** on the nature of knowledge. Chomsky argues for innate structures of understanding — grammar, logic, mathematical reasoning — that go beyond what experience alone can provide. Žižek pushes back on the limits of formal systems themselves.

Empiricism gives us patterns. But patterns are not proofs. Observation is not understanding.

## Teaching a Neural Network About Primes

What happens when we train a neural network to predict primality?

It goes **awfully**.

The model can learn surface-level patterns. It quickly discovers that **even numbers greater than 2 aren't prime**. It picks up on some divisibility heuristics.

But it doesn't *understand* what makes a number prime. It doesn't grasp the underlying reasoning — the relationship between a number and its factors, the structure of divisibility, the fundamental theorem of arithmetic.

The neural network is an empiricist. It sees data points and finds correlations. It cannot construct a proof. It cannot reason about why 2047 is composite while 2^13 - 1 is prime.

## Learning Patterns Is Not Understanding Mathematics

A model trained on examples of primes and composites is doing something fundamentally different from a mathematician who *proves* that a number is prime.

The model learns to approximate. The mathematician learns to deduce.

Learning patterns is not understanding mathematics. Inference from data is not the same as inference from axioms. And no amount of training data will bridge that gap.
