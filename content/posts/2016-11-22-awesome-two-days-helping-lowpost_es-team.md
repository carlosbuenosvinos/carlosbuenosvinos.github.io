---
title: Awesome two days helping @Lowpost_es team
author: Carlos Buenosvinos
type: post
date: 2016-11-22T09:00:57+00:00
url: /awesome-two-days-helping-lowpost_es-team/
featured_image: /posts/images/2016/11/LowPostTeam.jpg
dsq_thread_id:
  - "5323266015"
categories:
  - 'Agile &amp; Craftsmanship'
  - DDD
  - PHP
  - Scrum
  - Talks
  - 'TDD &amp; Katas'
tags:
  - consultancy
  - ddd
  - drupal
  - hexagonal architecture
  - php
  - phpunit
  - silex
  - tdd
  - training
  - unit testing
  - workshop

---
I have recently visited Valencia in order to help my friends at <a href="http://lowpost.es/" target="_blank">Lowpost</a>. It was great and I had a lot of fun! I would like to tell you a bit about how it was.

### Who is Lowpost?

Lowpost is a cool start-up that are focused in Content Marketing. Lowpost connects companies that need interesting content with authors that can write about such topics. Authors bet for the open jobs and then they deliver the content to the final customer. Everything around such process is managed by the Lowpost platform.

As a start-up, they have grown quite fast, so their code. They started with Drupal, as many start-ups, and then they added a Silex application. You know that testing is difficult, however, doing unit testing for Drupal is a challenge.

### What we did?

<!--more-->

The main problem that companies that I help have is Coupled Legacy Code (CLC): Code without tests full of news, static calls, etc. So after signing a NDA, Lowpost sent me some long pieces of code of both applications. After reviewing in detail the code, the CLC problem was the root problem. The CTO and I finally planed a two-days training/workshop for the team.

In order to refactor CLC code, you need to add tests. However, when the code is coupled you need to use special patterns and techniques (Test Class, Mocks, Static Injection, etc.) to test such coupled code. Then you can refactor it more safely to become decoupled.

What about the new features? Hexagonal Architecture to the rescue! With Hexagonal Architecture and some Domain-Driven Design concepts, you can write decoupled and easy to test code.

One interesting thing is that we recorded all the training and shared with the team so they can replay any part each time they want.

### What was the agenda?

**Day #1 Morning:**
  
* Tips and Tricks to improve Scrum and Agile (2 hours)
  
* Development Cycle Overview path to Continuous Deployment (2 hours)

**Day #1 Afternoon:**
  
* TDD practice (1 hour)
  
* Testing patterns for coupled code (3 hours)

**Day #2 Morning:**
  
* Hexagonal Architecture Theory (1 hour)
  
* Refactor Lowpost Silex application to Hexagonal Architecture (3 hours)

**Day #2 Afternoon:**
  
* Refactor Lowpost Silex application to Hexagonal Architecture (2 hours)
  
* Testing Lowpost Drupal application (1 hours)
  
* Refactor Lowpost Drupal application to Hexagonal Architecture (1 hours)

### Final words

The Lowpost team was super engaged. They learnt really fast and after 2 days of training they were able to test the most difficult scenarios in their applications. I&#8217;m really proud of all you guys. I&#8217;m looking forward seeing the evolution of Lowpost code in the next weeks and months. Keep the good work! Thanks Edu, Sergio, Fernando, Andrés, Luís, Leissi, JuanJo, Sergio, Víctor and Nacho.