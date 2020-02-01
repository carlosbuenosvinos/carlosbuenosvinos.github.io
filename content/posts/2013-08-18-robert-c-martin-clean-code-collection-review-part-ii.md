---
title: Robert C. Martin Clean Code Collection Review (Part II)
author: Carlos Buenosvinos
type: post
date: 2013-08-18T17:07:51+00:00
url: /robert-c-martin-clean-code-collection-review-part-ii/
featured_image: /posts/images/2013/08/CleanCodeCollection-500x270.jpg
switch_like_status:
  - "1"
categories:
  - 'Agile &amp; Craftsmanship'
tags:
  - agile
  - clean code
  - robert c martin
  - tdd

---
Hace una semana, publicaba un pequeño resumen y mis apuntes sobre el libro &#8220;Clean Coder&#8221; de Robert C. Martin [(ver parte I)][1]. Hoy hago lo propio con el resumen del libro de la misma colección, &#8220;Clean Code&#8221;.

Os dejo cosas que subrayé y anoté del libro que me parecieron interesantes, espero que lo disfrutéis.

<!--more-->

> In software, 80% or more of what we do is quaintly called “maintenance”: the act of repair.
> 
> &nbsp;
> 
> In about 1951, a quality approach called Total Productive Maintenance (TPM) came on the Japanese scene. Its focus is on maintenance rather than on production. One of the major pillars of TPM is the set of so—called 5S principles. 58 is a set of disciplines— and here I use the term “discipline” instructively. These 58 principles are in fact at the foundations of Lean
> 
> &nbsp;
> 
> The 5S philosophy comprises these concepts:
> 
> &#8211; Seiri, or organization (think “sort” in English). Knowing where things are—using approaches such as suitable naming—is crucial. You think naming identifiers isn’t important? Read on in the following chapters.
> 
> &#8211; Seiton, or tidiness (think “systematize” in English). There is an old American saying: A place for everything, and everything in its place. A piece of code should be where you expect to find it—and, if not, you should re-factor to get it there.
> 
> &#8211; Seiso, or cleaning (think “shine” in English): Keep the workplace free of hanging wires, grease, scraps, and waste. What do the authors here say about littering your code with comments and commented-out code lines that capture history or wishes for the future? Get rid of them.
> 
> &#8211; Seiketsu, or standardization: The group agrees about how to keep the workplace clean. Do you think this book says anything about having a consistent coding style and set of practices within the group? Where do those standards come from? Read on.
> 
> &#8211; Shutsuke, or discipline (self-discipline). This means having the discipline to follow the practices and to frequently reflect on one’s work and be willing to change.
> 
> &nbsp;
> 
> There are two parts to learning craftsmanship: knowledge and work. You must gain the knowledge of principles, patterns, practices, and heuristics that a craftsman knows, and you must also grind that knowledge into your fingers, eyes, and gut by working hard and practicing.
> 
> &nbsp;
> 
> Learning to write clean code is hard work.
> 
> &nbsp;
> 
> You must see them agonize over decisions and see the price they pay for making those decisions the wrong way.
> 
> &nbsp;
> 
> You are reading this book for two reasons. First, you are a programmer. Second, you want to be a better programmer. Good. We need better programmers.
> 
> &nbsp;
> 
> LeBlanc’s law: Later equals never.
> 
> &nbsp;
> 
> But the fault, dear Dilbert, is not in our stars, but in ourselves. We are unprofessional.
> 
> &nbsp;
> 
> So too it is unprofessional for programmers to bend to the will of managers who don’t understand the risks of making messes.
> 
> &nbsp;
> 
> The only way to make the deadline—the only way to go fast—is to keep the code as clean as possible at all times.
> 
> &nbsp;
> 
> Clean code is focused.
> 
> &nbsp;
> 
> Leave the campground cleaner than you found it.
> 
> &nbsp;
> 
> If a name requires a comment, then the name does not reveal its intent.
> 
> &nbsp;
> 
> We should avoid words whose entrenched meanings vary from our intended meaning.
> 
> &nbsp;
> 
> Spelling similar concepts similarly is information. Using inconsistent spellings is disinformation.
> 
> &nbsp;
> 
> _If a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly name. _
> 
> &nbsp;
> 
> One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king. Professionals use their powers for good and write code that others can understand.
> 
> &nbsp;
> 
> A class name should not be a verb.
> 
> &nbsp;
> 
> The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.
> 
> &nbsp;
> 
> So, another way to know that a function is doing more than “one thing” is if you can extract another function from it with a name that is not merely a restatement of its implementation
> 
> &nbsp;
> 
> A long descriptive name is better than a long descriptive comment.
> 
> &nbsp;
> 
> Flag arguments are ugly. Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. It does one thing if the flag is true and another if the flag is false!
> 
> &nbsp;
> 
> We should never ignore any part of code. The parts we ignore are where the bugs will hide.
> 
> &nbsp;
> 
> In general output arguments should be avoided. If your function must change the state of something, have it change the state of this owning object.
> 
> &nbsp;
> 
> Functions should either do something or answer something, but not both.
> 
> &nbsp;
> 
> But never forget that your real goal is to tell the story of the system, and that the functions you write need to fit cleanly together into a clear and precise language to help you with that telling.
> 
> &nbsp;
> 
> “Don ’t comment bad code— rewrite it.&#8221;
> 
> &nbsp;
> 
> The proper use of comments is to compensate for our failure to express ourself in code
> 
> &nbsp;
> 
> Comments are always failures
> 
> &nbsp;
> 
> Truth can only be found in one place: the code.
> 
> &nbsp;
> 
> Objects hide their data behind abstractions and expose functions that operate on that data. Data structure expose their data and have no meaningful functions.
> 
> &nbsp;
> 
> Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.
> 
> &nbsp;
> 
> The Law of Demeter says that a method f of a class C should only call the methods of these:
> 
> &#8211; An object created by f
> 
> &#8211; An object passed as an argument to f
> 
> &#8211; An object held in an instance variable of C
> 
> The method should not invoke methods on objects that are returned by any of the allowed functions. In other words, talk to friends, not to strangers.
> 
> &nbsp;
> 
> In learning tests we call the third-party API, as we expect to use it in our application. We’re essentially doing controlled experiments that check our understanding of that API. The tests focus on what we want out of the API.
> 
> &nbsp;
> 
> Three laws of TDD
> 
> First Law: You may not write production code until you have written a failing unit test.
> 
> Second Law: You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
> 
> Third Law: You may not write more production code than is sufficient to pass the currently failing testing.
> 
> &nbsp;
> 
> _Test code is just as important as production code._
> 
> &nbsp;
> 
> _F.I.R.S.T._
> 
> Fast: Tests should be fast.
> 
> Independent: Tests should not depend on each other.
> 
> Repeatable: Tests should be repeatable in any environment.
> 
> Self-Validating: The tests should have a boolean output. Either they pass or fail.
> 
> Timely: The tests need to be written in a timely fashion.
> 
> &nbsp;
> 
> The problem is that too many of us think that we are done once the program works.
> 
> &nbsp;
> 
> The startup process of object construction and wiring is no exception. We should modularize this process separately from the normal runtime logic and we should make sure that we have a global, consistent strategy for resolving our major dependencies.
> 
> &nbsp;
> 
> According to Kent, 21 design is “simple” if it follows these rules:
> 
>   * Runs all the tests
>   * Contains no duplication
>   * Expresses the intent of the programmer
>   * Minimizes the number of classes and methods
> 
> The rules are given in order of importance.

 [1]: http://carlosbuenosvinos.com/robert-c-martin-clean-code-collection-review-part-i/ "Robert C. Martin Clean Code Collection Review (Part I)"