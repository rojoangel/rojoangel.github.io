---
layout: post
title: "Are They The Same? Kata Java Solution"
date: 2015-09-02 22:47:19 +0200
comments: true
categories: [kata, java]
---
Today I was practicing with the [Are they the same?](http://www.codewars.com/kata/are-they-the-same) kata from [www.codewars.com](http://www.codewars.com)

Here's a summary of the steps I followed to solve it.

#### Description
The kata goal is

> Given two arrays a and b write a function comp(a, b) (compSame(a, b) in Clojure) that checks whether the two arrays have the "same" elements, with the same multiplicities. "Same" means, here, that the elements in b are the elements in a squared, regardless of the order.

The full description can be found at [www.codewars.com](http://www.codewars.com/kata/are-they-the-same)
<!--more-->
#### The initial code

The initial code has one class and a test.

{% codeblock lang:java %}
public class AreSame {

	public static boolean comp(int[] a, int[] b) {
    return null;
  }
}
{% endcodeblock %}
{% codeblock lang:java %}
import static org.junit.Assert.*;
import org.junit.Test;


public class AreSameTest {

	@Test
	public void test1() {
		int[] a = new int[]{121, 144, 19, 161, 19, 144, 19, 11};
		int[] b = new int[]{121, 14641, 20736, 361, 25921, 361, 20736, 361};
		assertEquals(AreSame.comp(a, b), true);
	}
}
{% endcodeblock %}
The kata focuses on implementing the ```AreSame``` class so it passes the tests provided by the ```AreSameTest``` class.

It also seems a good idea to enhance the tests in ```AreSameTest``` to further test our implementation.

#### Solution steps

##### Iteration 1 - true use case
My goal for this first iteration was to make ```test1``` pass / green.

The ```comp``` method will

- sort array ```a```
- sort array ```b```
- raise ```a``` elements to the second power
- compare the elements in ```a``` and ```b```

This is the resulting code:

{% codeblock lang:java %}
public static boolean comp(int[] a, int[] b) {
    Arrays.sort(a);
    Arrays.sort(b);
    for (int i = 0; i < a.length; i++) {
        if (a[i]*a[i] != b[i]) {
            return false;
        }
    }
    return true;
}
{% endcodeblock %}
This code has a couple of problems that will be dealt with in next iterations:

- ```Arrays.sort()``` is modifying the input parameters, which can be considered as a code smell.
- The ```for``` loop used to compare both arrays adds a lot of code and is not expressive enough, I'm sure there are better ways to achieve the same result

##### Iteration 2 - false use case
The tests provided are only covering the scenario where ```comp``` return ```true```. Let's add a test for ```comp``` returning false.
{% codeblock lang:java %}
@Test
public void test2() {
    int[] a = new int[]{122, 144, 19, 161, 19, 144, 19, 11};
    int[] b = new int[]{121, 14641, 20736, 361, 25921, 361, 20736, 361};
    assertEquals(AreSame.comp(a, b), false);
}
{% endcodeblock %}
The new test is also satisfied by the current ```comp``` implementation, so there's no need to modify the code at this stage.

#### Iteration 3 - null / empty use case
The kata description mentions that we have to deal with null and empty arrays:
> If a or b are nil (or null or None), the problem doesn't make sense so return false.
>
> If a or b are empty the result is evident by itself.

Let's add two new tests to cover for these scenarios:
{% codeblock lang:java %}
@Test
public void testNull() {
    assertEquals(AreSame.comp(null, null), false);
}

@Test
public void testEmpty() {
    int[] a = new int[]{};
    int[] b = new int[]{};
    assertEquals(AreSame.comp(a, b), true);
}
{% endcodeblock %}
When running the test suite, ```testEmpty``` is passing, but ```testNull``` is failing: ```comp``` needs to be modified to deal properly with null arrays an to bring the test suite back to green.

A check for null arguments is added to the ```comp``` method to turn the tests back to pass / green.

{% codeblock lang:java %}
if (a == null || b == null) return false;
{% endcodeblock %}
and the resulting ```comp``` method is as follows:
{% codeblock lang:java %}
public static boolean comp(int[] a, int[] b) {
    if (a == null || b == null) return false;

    Arrays.sort(a);
    Arrays.sort(b);
    for (int i = 0; i < a.length; i++) {
        if (a[i]*a[i] != b[i]) {
            return false;
        }
    }
    return true;
}
{% endcodeblock %}
#### Iteration 4 - Different sizes use case
How will ```comp``` deal with arrays of different sizes? Let's add a couple of tests to check this scenarios:
{% codeblock lang:java %}
@Test
public void testBLonger() {
    int[] a = new int[]{121, 144, 19, 161, 19, 144, 19, 11};
    int[] b = new int[]{121, 14641, 20736, 361, 25921, 361, 20736, 361, 2073600};
    assertEquals(AreSame.comp(a, b), false);
}

@Test
public void testALonger() {
    int[] a = new int[]{121, 144, 19, 161, 19, 144, 19, 11, 14400};
    int[] b = new int[]{121, 14641, 20736, 361, 25921, 361, 20736, 361};
    assertEquals(AreSame.comp(a, b), false);
}
{% endcodeblock %}
Just by adding a couple of big numbers to the end of ```a``` and ```b``` we are making our code fail.

In order to bring the tests back to pass / green a guard to check the size of the arrays can be added:
{% codeblock lang:java %}
if (a.length != b.length) return false;
{% endcodeblock %}
and the resulting ```comp``` method is as follows:
{% codeblock lang:java %}
public static boolean comp(int[] a, int[] b) {

    if (a == null || b == null) return false;
    if (a.length != b.length) return false;

    Arrays.sort(a);
    Arrays.sort(b);
    for (int i = 0; i < a.length; i++) {
        if (a[i]*a[i] != b[i]) {
            return false;
        }
    }
    return true;
}
{% endcodeblock %}
#### Iteration 5 - Negative numbers use case
There's a scenario that hasn't been tested yet: what if ```a````contains negative numbers?

A new test is added to cover for this scenario:
{% codeblock lang:java %}
@Test
public void testNegativeNumbers() {
    int[] a = new int[]{121, -144, 19, 161, 19, 144, 19, 11};
    int[] b = new int[]{121, 14641, 20736, 361, 25921, 361, 20736, 361};
    assertEquals(AreSame.comp(a, b), true);
}
{% endcodeblock %}
When the tests are run we see this one failing miserably: the code needs to be changed to bring the test suite back to green.

The error is caused by the fact that ```comp``` sorts ```a``` and then raises its values to the second power, which doesn't work well for negative numbers.

At a first stage I considered working with absolute values from the ```a``` array. In order to apply ```Math.abs()``` to the values in the ```a``` array it seemed inevitable to add a new ```for``` loop, but I felt lazy so I after googling a bit I found that ```lambdas``` may come in to rescue.

The following code snippet returns a new array with the ```abs``` function applied to all its elements:
{% codeblock lang:java %}
Arrays.stream(a).map(x -> Math.abs(x)).toArray();
{% endcodeblock %}
Arrays can be also sorted by calling:
{% codeblock lang:java %}
Arrays.stream(b).sorted().toArray();
{% endcodeblock %}
Here's the resulting ```comp``` function after making use of ```Arrays```, ```IntStreams``` and ```lambdas```:
{% codeblock lang:java %}
import java.util.Arrays;

public class AreSame {

    public static boolean comp(int[] a, int[] b) {

        if (a == null || b == null) return false;
        if (a.length != b.length) return false;

        int[] sortedA = Arrays.stream(a).map(Math::abs).sorted().toArray();
        int[] sortedB = Arrays.stream(b).sorted().toArray();
        for (int i = 0; i < a.length; i++) {
            if (sortedA[i]*sortedA[i] != sortedB[i]) {
                return false;
            }
        }
        return true;
    }
}
{% endcodeblock %}
And that brought the tests back to pass / green. Now it's time for some refactoring!

#### Iteration 5 - Refactoring
By using ```IStreams``` the original arrays passed in to the ```comp``` method remain untouched so we have already dealt with one of the concerns raised in our first iteration.

Let's see how the ```comp``` method can be refactored, and if we can get rid of the ```for``` loop in it used for array comparison.

Now that the ```comp``` method uses ```lambdas``` to apply a function to all the elements of an array, why not use ```lambdas``` to square the elements of ```a```? By doing so ```Arrays.equals``` could be used to compare the resulting array with ```b``` and save our code for all the overhead in the ```for``` loop used for comparison.

The following code snippet applies ```abs```, squares and sorts the elements in the ```a``` array:
{% codeblock lang:java %}
Arrays.stream(a).map(x -> Math.abs(x)).map(x -> x*x).sorted().toArray();
{% endcodeblock %}
Wait a minute! If we raise to the second power, ```abs``` is redundant, so we can get rid of it:
{% codeblock lang:java %}
Arrays.stream(a).map(x -> x*x).sorted().toArray();
{% endcodeblock %}
And putting it all together here is the ```comp``` method final version:
{% codeblock lang:java %}
public static boolean comp(int[] a, int[] b) {

    if (a == null || b == null) return false;
    if (a.length != b.length) return false;

    int[] sortedA = Arrays.stream(a).map(x -> x*x).sorted().toArray();
    int[] sortedB = Arrays.stream(b).sorted().toArray();
    return Arrays.equals(sortedA, sortedB);
}
{% endcodeblock %}

Here are the final versions of the classes of this kata:

- [AreSame.java](https://github.com/rojoangel/codewars/blob/master/java/are-they-the-same/src/AreSame.java)
- [AreSameTest.java](https://github.com/rojoangel/codewars/blob/master/java/are-they-the-same/tests/AreSameTest.java)
