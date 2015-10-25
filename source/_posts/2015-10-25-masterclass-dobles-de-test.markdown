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

Here's a mind map of the references used in this presentation:
[{% img center  https://raw.githubusercontent.com/rojoangel/mindmup-maps/df150730b51d386066cd08423b125b77cab34580/masterclass_dobles_de_test.png %}](https://atlas.mindmup.com/2015/10/fc0c3b005d340133db4d4147ed4fb840/masterclass_dobles_de_test/index.html)
