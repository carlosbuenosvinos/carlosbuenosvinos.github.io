---
title: “Hexagonal Architecture with PHP” was published in phparch|magazine
author: Carlos Buenosvinos
type: post
date: 2014-07-25T17:41:22+00:00
url: /hexagonal-architecture-with-php-was-published-in-phparch-magazine/
categories:
  - 'Agile &amp; Craftsmanship'
  - PHP
tags:
  - ddd
  - hexagonal architecture
  - magazine
  - php
  - phparch

---
This week, the php|architect magazine has <a href="http://www.phparch.com/magazine/2014-2/july/" target="_blank">published</a> an article of mine about &#8220;Hexagonal Architecture with PHP&#8221;. Here&#8217;s the introduction:

&#8220;With the rise of DDD (domain-driven development), architectures promoting domain-centric designs are becoming more popular. This is the case with <b style="color: #000000;">Hexagonal Architecture</b>, also known as <b style="color: #000000;">Ports and Adapters</b>, that seems to have been rediscovered recently by PHP developers. Invented in 2005 by Alistair Cockburn, one of the Agile Manifesto authors, the Hexagonal Architecture allows an application to be equally driven by users, programs, automated tests, or batch scripts, while being developed and tested in isolation from its eventual run-time devices and databases. This results in infrastructure-agnostic web applications that are easier to test, write, and maintain. Let’s see how to apply it using real PHP examples.&#8221;

I would like to share with you some thought, the cover and the code repository.

<!--more-->

My main goal was to show how to apply hexagonal architecture in the real world migrating from a legacy and coupled Zend Framework v1 application using MySQL to a decoupled domain-centric application using Use Cases and abstractions. I show how to refactor the Zend Framework web app to use UseCases, add Silex support as a REST application and Symfony Console for command line application.

Everything is unit tested with 100% coverage showing when to mock and how to w/o mockery. I also show how to easily switch persistence strategy from MySQL to Redis when everything is decoupled. At last, I cover how to use Symfony Service Container component to make easier the process to generate the dependence chain. 14 pages of full refactoring. If you are interested in the whole article and a bunch of greater ones, you can <a href="http://www.phparch.com/magazine/2014-2/july/" target="_blank">buy July Issue</a> of the magazine. The code repository with fixed typos can be found <a href="https://github.com/carlosbuenosvinos/phparchitect-hexagonal-architecture-article" target="_blank">here</a>.

If you have any comment, please send me an email or tweet. Hope you like it. I would like to thank everyone in the phparch team for asking me to write this article and giving support during the process.

[<img class="alignnone size-large wp-image-494" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/07/HexagonalArchitectureWithPHPCover-781x1024.png?resize=620%2C812" alt="HexagonalArchitectureWithPHPCover" width="620" height="812" srcset="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2014/07/HexagonalArchitectureWithPHPCover.png?resize=781%2C1024&ssl=1 781w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2014/07/HexagonalArchitectureWithPHPCover.png?resize=228%2C300&ssl=1 228w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2014/07/HexagonalArchitectureWithPHPCover.png?w=1230&ssl=1 1230w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />][1]

 [1]: https://i2.wp.com/carlosbuenosvinos.com/posts/images/2014/07/HexagonalArchitectureWithPHPCover.png