---
title: "From Weak Learners to French Cuisine"
date: 2026-02-21
author: Mauro Ghiani
category: AI
tags: [EnsembleLearning, MachineLearning, Statistics, Gestalt]
excerpt: "What would you do with ML models that are only slightly better than random guessing? Combine them—and the whole becomes something other than the sum of its parts."
---

I am reminded of a Dilbert strip:

> **Boss:** We won a government contract to measure ocean temperatures.
> **Dilbert:** Which part of the ocean?
> **Boss:** The whole ocean.
> **Dilbert:** We can't put sensors everywhere in the ocean. It's too big.
> **Boss:** We can measure a bunch of places and estimate the rest.
> **Dilbert:** So... you want me to measure 1% of the ocean's temperature and estimate the other 99%? I don't know how to do that.
> **Boss:** Try using math.
> **Dilbert:** Wouldn't it be cheaper to measure nothing and just estimate the whole thing?
> **Boss:** Every now and then, you come up with a great idea.

If something is useless, why not be simple, invent things, and be arbitrary?

### What Would You Do with Models Barely Better Than Guessing?

In the old Gestalt school, the whole is something other than the sum of its parts.

Imagine tossing a biased coin—on average, 51% of the coin lands heads and 49% tails. If you toss it just once, you only have a 51% chance of seeing heads. But if you increase the number of tossings to 1,000, the probability of obtaining a majority of heads climbs to roughly 75%. And if you reach 10,000 tosses, the probability of a heads majority jumps to over 97%. As the number of trials increases, the ratio of heads approaches the **true** probability, 51%.

### Ensemble Learning

In machine learning, **ensemble** learning is created by combining several "weak learners"—models that are only slightly better than random guessing—to create a "strong learner" with high accuracy.

To create a "strong learner" with high accuracy, it is possible as long as there is a sufficient number of weak learners and the learners are sufficiently diverse.

And if all classifiers are perfectly **independent**, uncorrelated errors are made. In practice, because models are often trained on the same data, they tend to make similar mistakes. This correlation reduces the overall accuracy of the ensemble compared to the theoretical ideal.

### The Recipe

Similarly, humans organize patterns rather than isolated experiences.

Adding singular insignificant ingredients to a recipe making, out of necessity, the final dish French **cuisine**.
