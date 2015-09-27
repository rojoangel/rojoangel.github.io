---
layout: post
title: "What to look for in a Code Review - by Trisha Gee @ JetBrains Upsource Blog"
date: 2015-09-27 20:24:13 +0200
comments: true
categories: [reading, code-review]
hidden: true
---
I've just read this post about [What to look for in a Code Review](http://blog.jetbrains.com/upsource/2015/07/23/what-to-look-for-in-a-code-review/).

These are notes to myself.

Automatize these:
- Formatting
- Style
- Naming
- Test coverage

Instead look for:

**Design**
- Fit with the overall architecture?
- SOLID principles, Domain Driven Design
- Design patterns used. Are these appropriate?
- Does this new code follow the current practices? Is the code migrating in the correct direction?
- Is the code in the right place?
- Could the new code have reused something in the existing code? Does the new code provide something we can reuse in the existing code? Does the new code introduce duplication?
- Is the code over-engineered? YAGNI?

**Readability & Maintainability**
- Do the names actually reflect the thing they represent?
- Can I understand what the code does by reading it?
- Can I understand what the tests do?
- Do the tests cover a good subset of cases? Do they cover happy paths and exceptional cases? Are there cases that haven’t been considered?
- Are the exception error messages understandable?
- Are confusing sections of code either documented, commented, or covered by understandable tests (according to team preference)?

**Functionality**
- Does the code actually do what it was supposed to do? Do the tests really test the code meets the agreed requirements?
- Does the code look like it contains subtle bugs, like using the wrong variable for a check, or accidentally using an and instead of an or?

**Have you thought about…?**
- Security
- Regulatory requirements that need to be met?
- Does the new code introduce avoidable performance issues
- Does the author need to create public documentation, or change existing one?
- Have user-facing messages been checked for correctness?
- Are there obvious errors that will stop this working in production?
