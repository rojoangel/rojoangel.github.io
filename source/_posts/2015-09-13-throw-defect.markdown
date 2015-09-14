---
layout: post
title: "Throw Defect - by Nat Pryce @ Mistaeks I Hav Made"
date: 2015-09-13 19:27:48 +0200
comments: true
categories: [java, exceptions, reading]
---
I've just read this post about an interesting way to make explicit programmer errors: [Throw Defect](http://www.natpryce.com/articles/000739.html)

This can be particularly useful to catch errors that are not expected to happen. Here's an example from the original post:

{% codeblock lang:java %}
Template template;
try {
    template = new Template(getClass().getResource("data-that-is-compiled-into-the-app.xml"));
}
catch (IOException e) {
    // should never happen
}
{% endcodeblock %}

The 'should never happen' comment block can be replaced - thus making the error explicit - in the following way:

{% codeblock lang:java %}
Template template;
try {
    template = new Template(getClass().getResource("data-that-is-compiled-into-the-app.xml"));
}
catch (IOException e) {
    throw new Defect("could not load template", e);
}
{% endcodeblock %}
