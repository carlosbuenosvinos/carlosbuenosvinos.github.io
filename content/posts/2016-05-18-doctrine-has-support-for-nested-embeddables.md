---
title: Doctrine has support for nested embeddables
author: Carlos Buenosvinos
type: post
date: 2016-05-18T08:00:55+00:00
draft: true
url: /?p=915
featured_image: /posts/images/2016/05/368_960x3201.jpg
categories:
  - Databases
  - DDD
  - PHP
tags:
  - ddd
  - doctrine
  - domain-driven design
  - embeddables
  - orm
  - php
  - value objects

---
This is a quick post just to let you know that, in case you don&#8217;t know it already, since December 2015, Doctrine has support for nested embeddables. That&#8217;s really important for DDDers that can model their Domain using Value Objects with no restrictions. Just a small example:

<pre class="brush: javascript; gutter: true">Lw\Domain\Model\User\User:
  type: entity
  embedded:
    address:
      class: Lw\Domain\Model\User\Address

Lw\Domain\Model\User\Address:
  type: embeddable
  embedded:
    postalCode:
      class: Lw\Domain\Model\User\Cp
  fields:
    street: { type: string }
    city: { type: string }
    country: { type: string }

Lw\Domain\Model\User\Cp:
  type: embeddable
  fields:
    postalCode: { type: string }</pre>