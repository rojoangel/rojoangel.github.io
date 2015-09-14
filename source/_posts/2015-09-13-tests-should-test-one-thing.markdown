---
layout: post
title: "Tests Should Test One Thing (?) - by Jason Gorman @ Software People Inspiring"
date: 2015-09-13 19:55:16 +0200
comments: true
categories: [tdd, best-practices, reading]
hidden: true
---
I've just read this post about TDD best practices:  [Tests Should Test One Thing (?)](http://codemanship.co.uk/parlezuml/blog/?postid=1325)

The author states that

> unit tests should have only one reason to fail

and there are several good reasons for this:

- Small tests tend to be easier to pass, facilitating "baby steps" approach to development
- Small tests tend to be easier to understand, serving as clearer specifications
- Small tests When failing, make it easier to pinpoint the problem
