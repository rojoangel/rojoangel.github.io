---
layout: post
title: "Misadventures with Property-Based TDD: A Lesson Learned - by Nat Pryce @ Mistaeks I Hav Made"
date: 2015-09-13 19:03:37 +0200
comments: true
categories: [tdd, best-practices, reading]
---
I've just read this post about TDD best practices & lessons learned: [Mistaeks I Hav Made: Misadventures with Property-Based TDD: A Lesson Learned](http://www.natpryce.com/articles/000800.html)

Through a TDD example it reaches the following conclusion:

> When working from examples, we start with specifics and then generalise, by adding contradictory examples. With property-based tests it seems better to start with very general properties and then specialise.

Interestingly the tests used in the example are based on [factcheck](https://github.com/npryce/python-factcheck)

> A simple but extensible implementation of QuickCheck for Python 2.7 and Python 3 that works well with Pytest.
