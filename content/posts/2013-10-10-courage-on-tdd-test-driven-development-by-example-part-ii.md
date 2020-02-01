---
title: 'Courage on TDD (“Test Driven Development: By Example” – Part II)'
author: Carlos Buenosvinos
type: post
date: 2013-10-10T21:28:03+00:00
url: /courage-on-tdd-test-driven-development-by-example-part-ii/
categories:
  - 'Agile &amp; Craftsmanship'
  - 'TDD &amp; Katas'
tags:
  - kent beck
  - tdd

---
La segunda parte del prólogo de [&#8220;Test Driven Development: By Example&#8221;][1] es casi aún mejor (podéis ver la primera parte [aquí][2]). 34$ bien invertidos.

<!--more-->

Test-driven development is a way of managing fear during programming. I don&#8217;t mean fear in a bad way—pow widdle prwogwammew needs a pacifiew—but fear in the legitimate, thisis-a-hard-problem-and-I-can&#8217;t-see-the-end-from-the-beginning sense. If pain is nature&#8217;s way of saying &#8220;Stop!&#8221; then fear is nature&#8217;s way of saying &#8220;Be careful.&#8221; Being careful is good, but fear has a host of other effects.

  * Fear makes you tentative.
  * Fear makes you want to communicate less.
  * Fear makes you shy away from feedback.
  * Fear makes you grumpy.

None of these effects are helpful when programming, especially when programming something hard. So the question becomes how we face a difficult situation and,

  * Instead of being tentative, begin learning concretely as quickly as possible.
  * Instead of clamming up, communicate more clearly.
  * Instead of avoiding feedback, search out helpful, concrete feedback.
  * (You&#8217;ll have to work on grumpiness on your own.)

Imagine programming as turning a crank to pull a bucket of water from a well. When the bucket is small, a free-spinning crank is fine. When the bucket is big and full of water, you&#8217;re going to get tired before the bucket is all the way up. You need a ratchet mechanism to enable you to rest between bouts of cranking. The heavier the bucket, the closer the teeth need to be on the ratchet.

The tests in test-driven development are the teeth of the ratchet. Once we get one test working, we know it is working, now and forever. We are one step closer to having everything working than we were when the test was broken. Now we get the next one working, and the next, and the next. By analogy, the tougher the programming problem, the less ground that each test should cover.

Readers of my book Extreme Programming Explained will notice a difference in tone between Extreme Programming (XP) and TDD. TDD isn&#8217;t an absolute the way that XP is. XP says, &#8220;Here are things you must be able to do to be prepared to evolve further.&#8221; TDD is a little fuzzier. TDD is an awareness of the gap between decision and feedback during programming, and techniques to control that gap. &#8220;What if I do a paper design for a week, then test-drive the code? Is that TDD?&#8221; Sure, it&#8217;s TDD. You were aware of the gap between decision and feedback, and you controlled the gap deliberately.

That said, most people who learn TDD find that their programming practice changed for good. Test Infected is the phrase Erich Gamma coined to describe this shift. You might find yourself writing more tests earlier, and working in smaller steps than you ever dreamed would be sensible. On the other hand, some software engineers learn TDD and then revert to their earlier practices, reserving TDD for special occasions when ordinary programming isn&#8217;t making progress.

There certainly are programming tasks that can&#8217;t be driven solely by tests (or at least, not yet). Security software and concurrency, for example, are two topics where TDD is insufficient to mechanically demonstrate that the goals of the software have been met. Although it&#8217;s true that security relies on essentially defect-free code, it also relies on human judgment about the methods used to secure the software. Subtle concurrency problems can&#8217;t be reliably duplicated by running the code.

Once you are finished reading this book, you should be ready to

  * Start simply
  * Write automated tests
  * Refactor to add design decisions one at a time

 [1]: http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530
 [2]: http://carlosbuenosvinos.com/test-driven-development-by-example/ "Test Driven Development: By Example"