---
layout: post
title: "Masterclass - Dobles de Test - by Xavi Gost @ Devscola"
date: 2015-10-25 10:34:41 +0100
comments: true
categories: [reading, presentation, tdd, test doubles, xavi gost]
hidden: true
---
I've just watched this presentation by Xavi Gost about [Masterclass - Dobles de Test](https://www.youtube.com/watch?v=S4Ueg64xfXQ).

These are notes to myself taken while watching the video:

- The type of test double to use should be dictated by the intention of use.
- Test doubles:
  - Dummy: we only need it to be present.
  - Stub: returns fixed values.
  - Mock: more complex scenarios.
- Mocks are evil. When using a mock you are acknowledging a lack of ability / know-how in the design / architecture.
- Mocks introduce complexity in a part of system (tests) that doesn't deliver value to the customer.
