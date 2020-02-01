---
title: RESTGames – More than katas
author: Carlos Buenosvinos
type: post
date: 2016-03-02T09:00:06+00:00
url: /restgames-more-than-katas/
featured_image: /posts/images/2016/03/1453093897_Brain-Games-grey.png
dsq_thread_id:
  - "4625403461"
categories:
  - 'Agile &amp; Craftsmanship'
  - 'TDD &amp; Katas'
tags:
  - battleship
  - clojure
  - games
  - kata
  - php
  - rest
  - silex

---
Today, Christian (<a href="https://twitter.com/theUniC" target="_blank">@theUniC</a>) and I (<a href="https://twitter.com/buenosvinos" target="_blank">@buenosvinos</a>) would like to introduce you &#8220;<a href="https://github.com/restgames" target="_blank">REST Games</a>&#8220;: Games based in REST Services for learning and practicing coding.

Its goal is to provide some coding challenges that go beyond katas. You will have to implement a small JSON REST API that will play a well known multiplayer game, such as battleship, connect-4, tic-tac-toe, etc. The best part comes when two mates develop the same JSON REST API and you can use our referees programs two make them play one against the other. Who will win?

<!--more-->

### Why REST?

For building a REST service in any language, you will have to deep into different REST frameworks, dependency management, etc. It can be challenging when starting to learn a new language. REST also allows to make any service play together, whether is developed in PHP, Scala, Clojure, Node, etc.

### Why games?

Games have algorithms, strategies and some sort of Artificial Intelligence. They are more difficult than katas. Some multiplayer games are not mathematically resolved, so, you can win even playing with the best engine.

### Battleship: The first game

Battleship is a popular game about sinking ships in a war style. The rules are the official ones from the Hasbro original board game. You can find then here: <http://www.hasbro.com/common/instruct/Battleship.PDF>. A part of the original rules, if your REST APIs does not work properly (connectivity issues, returning wrong values, etc.) you will loose the game.

### Where to start?

Visit the battleship referee for making two players play Battleship. You will find it at <a href="https://github.com/restgames/battleship-client" target="_blank">https://github.com/restgames/battleship-client</a>. You&#8217;ll find rules, skeletons, suggestions and much more.

### Battleship: Skeletons

If you feel a bit lazy about to start, we&#8217;ve created some skeleton applications you can use to start your own project. For now, there are <a href="https://github.com/restgames/battleship-rest-clojure-skeleton" target="_blank">Clojure</a> and <a href="https://github.com/restgames/battleship-rest-silex-skeleton" target="_blank">PHP</a> skeleton applications for playing Battleship. You can find them, here.

Feel free to use REST Games in your meet ups or trainings. Let us know if you have suggestions, comments or contributions.