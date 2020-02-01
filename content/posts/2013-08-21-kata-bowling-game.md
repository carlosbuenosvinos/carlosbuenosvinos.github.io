---
title: 'Kata: Bowling Game'
author: Carlos Buenosvinos
type: post
date: 2013-08-21T10:10:12+00:00
url: /kata-bowling-game/
switch_like_status:
  - "1"
categories:
  - 'TDD &amp; Katas'
tags:
  - bowling scoring
  - coding
  - kata
  - tdd

---
## Context

[<img class="alignnone size-medium wp-image-141" alt="Bowling" src="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2013/08/Bowling-300x29.png?resize=300%2C29" width="300" height="29" srcset="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2013/08/Bowling.png?resize=300%2C29&ssl=1 300w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2013/08/Bowling.png?w=776&ssl=1 776w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />][1]

Scoring Bowling: The game consists of 10 frames as shown above.  In each frame the player has two opportunities to knock down 10 pins.  The score for the frame is the total number of pins knocked down, plus bonuses for strikes and spares.

<!--more-->

<span style="line-height: 1.5;">A spare is when the player knocks down all 10 pins in two tries.  The bonus for </span>that frame is the number of pins knocked down by the next roll.  So in frame 3 above, the score is 10 (the total number knocked down) plus a bonus of 5 (the number of pins knocked down on the next roll.)

A strike is when the player knocks down all 10 pins on his first try.  The bonus for that frame is the value of the next two balls rolled.

In the tenth frame a player who rolls a spare or strike is allowed to roll the extra balls to complete the frame.  However no more than three balls can be rolled in tenth frame.

## <span style="line-height: 1.3;">Level</span>

Easy / 40 minutes<img style="font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;" title="More..." alt="" src="https://i2.wp.com/carlosbuenosvinos.com/wp-includes/js/tinymce/plugins/wordpress/img/trans.gif?w=620" data-recalc-dims="1" />

## Requirements

<div>
  <div>
    Write a class named “Game” that has two methods:
  </div>
  
  <div>
    <ul>
      <li>
        roll(pins : int) is called each time the player rolls a ball.  The argument is the number of pins knocked down.
      </li>
      <li>
        score() : int is called only at the very end of the game.  It returns the total score for that game.
      </li>
    </ul>
  </div>
</div>

<div>
  <h2>
    Goal
  </h2>
  
  <p>
    Resolve applying TDD, 100% code coverage is possible. When finish, comment on this post with your GitHub repository (or similiar) solution so others can check.
  </p>
  
  <h2>
    Origin
  </h2>
  
  <p>
    Adapted from <a href="http://butunclebob.com/ArticleS.UncleBob.TheBowlingGameKata">http://butunclebob.com/ArticleS.UncleBob.TheBowlingGameKata</a><a href="http://butunclebob.com/ArticleS.UncleBob.ThePrimeFactorsKata" target="_blank"><br /> </a>
  </p>
</div>

 [1]: https://i2.wp.com/carlosbuenosvinos.com/posts/images/2013/08/Bowling.png