---
title: “new” and fluent interfaces
author: Carlos Buenosvinos
type: post
date: 2013-11-21T14:39:54+00:00
url: /new-and-fluent-interfaces/
dsq_thread_id:
  - "2772953217"
categories:
  - PHP
tags:
  - fluent interface
  - new
  - oop

---
Last week, I was with some friends and we were talking about fluent interfaces and I show them something that they didn&#8217;t know. I got surprised about it because it was introduced in PHP 5.4.

<!--more-->

**Code from friends:**

<pre class="brush: php; gutter: true">$user = new User();
$user
    -&gt;setName(&#039;Carlos&#039;)
    -&gt;setLastname(&#039;Buenosvinos&#039;)
;</pre>

**What they don&#8217;t know it can be done:**

<pre class="brush: php; gutter: true">$user = (new User())
    -&gt;setName(&#039;Carlos&#039;)
    -&gt;setLastname(&#039;Buenosvinos&#039;)
;</pre>

Silly, but cool, isn&#8217;t it?