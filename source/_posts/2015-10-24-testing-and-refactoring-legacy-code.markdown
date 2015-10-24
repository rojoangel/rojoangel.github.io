---
layout: post
title: "Testing and Refactoring Legacy Code - by Sandro Mancuso"
date: 2015-10-24 23:38:16 +0200
comments: true
categories: [reading, presentation, kata, refactoring, legacy]
hidden: true
---
I've just watched this presentation by Sandro Mancuso about [Testing and Refactoring Legacy Code](https://www.youtube.com/watch?v=_NnElPO5BU0).

In this video the [trip-service-kata](https://github.com/sandromancuso/) is used.

These are notes to myself taken while watching the video:

- Wrap code with tests before modifying it. If the code is not covered by tests perform IDE auto refactorings only.
- Start testing from the lower nesting level to the deepest.
- Start refactoring from the deepest nesting level to the shorter.
- In the case of static methods, singletons or instances being created inside a method: create seams to make code testable. Seams can be overriden in order to obtain an intermediate testable class.
- When working with legacy code use code coverage in order to verify that your tests cover the code they are created for.
- Use builders to make tests more readable.
- Refactor staying on the green side of red-green-refactor cycle as much as you can.
- Try to convert conditionals to guards.
- Initially get rid of variables whenever possible without considering potential performance issues caused by calling methods multiple times. Variables can be reintroduced later on if needed.
- Your code should reflect the language in your tests if possible.
- The language in your tests and code should be the domain language.
- If the design is wrong - when adding tests we are perpetuating the wrong design!
- Wrap static methods in instance methods. In your code base start replacing static calls to instance calls, until the static call is not used anymore: then you can get rid of the static method.
