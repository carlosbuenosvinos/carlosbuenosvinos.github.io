---
title: Test Doubles
author: Carlos Buenosvinos
type: post
date: 2013-12-15T17:36:59+00:00
url: /test-doubles/
categories:
  - 'Agile &amp; Craftsmanship'
  - 'TDD &amp; Katas'
tags:
  - dummy
  - fake
  - mock
  - spy
  - stub
  - tdd
  - test doubles

---
Last week, another awesome <a href="http://cleancoders.com" target="_blank">cleancoders</a> episode was published. It comes in two parts. I really recommend to buy <a href="http://cleancoders.com/codecast/clean-code-episode-23-p1/show" target="_blank">it</a>. Let&#8217;s review the most important concept taught in the first one: Test Doubles.

<!--more-->

### **Favourite topic**

In the video you can learn different cool thing but I&#8217;m going to sum up the concept I liked the most, test doubles types.

Test doubles help us testing your application in order not to couple our tests to our implementation so our tests do not become fragile.

Different test doubles are: Dummy, Stub, Spy, Mock and Fake. Generally speaking, we tend to call to all of these mocks, but the correct term is test double.

#### Dummy

It&#8217;s an object its functions do nothing and return null (or what&#8217;s near to null). I mean, it does nothing and returns nothing useful.

Typically, it&#8217;s an object you have to pass as a parameter to a function that it&#8217;s not going to do nothing with it, specially for the pathway testing.

#### Stub

A stub is a dummy test double that does nothing and its functions return anything you like in order to force the pathway you are testing. Your stubs will be shared between your different tests.

#### Spy

A spy is a stub that remembers some data that can be accessed from your tests. Interesting things to remember are functions calls (argument, number of invocations, etc.). They watch and remember.

#### Mock

A mock is a spy that knows what to expected. Tests ask the mock about what to expected.

#### Fake

Fakes are objects, not related with any of the previous ones, that encapsulates some logic in order to behave as another object. It’s like a simulator. For integration tests, fakes can be really useful. As a general recommendation, you should avoid fakes as possible. For your unit tests, stubs and spies should be enough.

#### Book recommendation

<a href="http://www.amazon.com/xUnit-Test-Patterns-Refactoring-Code/dp/0131495054" target="_blank">xUnit Test Patterns: Refactoring Test Code</a>