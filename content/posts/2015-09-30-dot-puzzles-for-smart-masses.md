---
title: 'Dot: Puzzles for smart masses'
author: Carlos Buenosvinos
type: post
date: 2015-09-30T03:00:40+00:00
url: /dot-puzzles-for-smart-masses/
featured_image: /posts/images/2015/09/destacada-1024x500.png
categories:
  - Javascript
  - Mobile
tags:
  - apache cordova
  - backtracking
  - impactjs
  - ionic
  - javascript
  - php
  - value objects

---
I have always been interested in games, just for fun, nothing serious. A game is IMHO the most difficult piece of software to write. Think for a second. It needs support for user input devices (mouse, keywords, gamepads, etc.), graphics (2D or 3D), sounds and music, physics, AI, networking when playing with more players and it has to perform really, really fast. That&#8217;s not the typical PHP web, is it? Today, I would like to present my first really small and silly game, and probably the last one :) for iOS/Android and the technologies behind it. Hope you like it.
  
<!--more-->

<a href="http://dot.carlosbuenosvinos.com" target="_blank"><strong>&#8220;Dot: Puzzles for smart masses&#8221;</strong></a> is a Web-based technologies game, developed using Javascript and embedded into iOS and Android using Apache Cordova. 

### Technologies used

Let&#8217;s review all the projects and technologies used.

### ImpactJS

<a href="http://impactjs.com/" target="_blank">Impact</a> is a JavaScript Game Engine that allows you to develop stunning HTML5 Games for desktop and mobile browsers. It&#8217;s not super really well maintained, but it&#8217;s really powerful. There are other interesting options such as <a href="http://phaser.io" target="_blank">Phaser.io</a>.

### Apache Cordova

<a href="https://cordova.apache.org/" target="_blank">Apache Cordova</a> is a set of device APIs that allow a mobile app developer to access native device function such as the camera or accelerometer from JavaScript. Combined with a UI framework such as jQuery Mobile or Dojo Mobile or Sencha Touch, this allows a smartphone app to be developed with just HTML, CSS, and JavaScript.

### Ionic framework

<a href="http://ionicframework.com/" target="_blank">Ionic</a> is the beautiful, open source front-end SDK for developing hybrid mobile apps with web technologies. As a killer feature, &#8220;ionic resources&#8221; that generates icons and splash screens for all devices and device sizes with a single command.

### PHP

I&#8217;m generating the levels automatically using the <a href="http://symfony.com/doc/current/components/console/introduction.html" target="_blank">Symfony Console Component</a> and a <a href="https://en.wikipedia.org/wiki/Backtracking" target="_blank">backtracking algorithm</a>. In such algorithms, using <a href="https://leanpub.com/ddd-in-php/read" target="_blank">Value Objects</a> is especially interesting for keeping immutable complex data structures such as trees.

Hope you like it.