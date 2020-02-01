---
title: 'Test Driven Development: By Example'
author: Carlos Buenosvinos
type: post
date: 2013-10-08T21:58:56+00:00
url: /test-driven-development-by-example/
dsq_thread_id:
  - "2778310989"
categories:
  - 'TDD &amp; Katas'
tags:
  - kent beck
  - tdd

---
Siguiendo con mi andadura de lecturas técnicas, esta semanas voy a estar con [&#8220;Test Driven Development: By Example&#8221;][1]. El enfoque del libro es totalmente práctico pero el prólogo me ha parecido tan simple, claro y delicioso que no he podido hacer otra cosa que compartirlo con vosotros para que os animéis con TDD y a la lectura de este fantástico libro.

<!--more-->

Clean code that works, in Ron Jeffries&#8217; pithy phrase, is the goal of Test-Driven Development (TDD). Clean code that works is a worthwhile goal for a whole bunch of reasons.

  * It is a predictable way to develop. You know when you are finished, without having to worry about a long bug trail.
  * It gives you a chance to learn all of the lessons that the code has to teach you. If you only slap together the first thing you think of, then you never have time to think of a second, better thing.
  * It improves the lives of the users of your software.
  * It lets your teammates count on you, and you on them.
  * It feels good to write it.

But how do we get to clean code that works? Many forces drive us away from clean code, and even from code that works. Without taking too much counsel of our fears, here&#8217;s what we do: we drive development with automated tests, a style of development called Test-Driven Development (TDD). In Test-Driven Development, we:

  * Write new code only if an automated test has failed
  * Eliminate duplication

These are two simple rules, but they generate complex individual and group behavior with technical implications such as the following.

  * We must design organically, with running code providing feedback between decisions.
  * We must write our own tests, because we can&#8217;t wait 20 times per day for someone else to write a test.
  * Our development environment must provide rapid response to small changes.
  * Our designs must consist of many highly cohesive, loosely coupled components, just to make testing easy.

The two rules imply an order to the tasks of programming.

  1. Red— Write a little test that doesn&#8217;t work, and perhaps doesn&#8217;t even compile at first.
  2. Green— Make the test work quickly, committing whatever sins necessary in the process.
  3. Refactor— Eliminate all of the duplication created in merely getting the test to work.

Red/green/refactor—the TDD mantra.

Assuming for the moment that such a programming style is possible, it further might be possible to dramatically reduce the defect density of code and make the subject of work crystal clear to all involved. If so, then writing only that code which is demanded by failing tests also has social implications.

If the defect density can be reduced enough, then quality assurance (QA) can shift from reactive work to proactive work.

If the number of nasty surprises can be reduced enough, then project managers can estimate accurately enough to involve real customers in daily development.

If the topics of technical conversations can be made clear enough, then software engineers can work in minute-by-minute collaboration instead of daily or weekly collaboration.

Again, if the defect density can be reduced enough, then we can have shippable software with new functionality every day, leading to new business relationships with customers.

So the concept is simple, but what&#8217;s my motivation? Why would a software engineer take on the additional work of writing automated tests? Why would a software engineer work in tiny little steps when his or her mind is capable of great soaring swoops of design? **Courage**

 [1]: http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530