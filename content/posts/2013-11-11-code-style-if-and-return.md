---
title: 'Code Style: If and return'
author: Carlos Buenosvinos
type: post
date: 2013-11-11T19:09:42+00:00
url: /code-style-if-and-return/
categories:
  - 'Agile &amp; Craftsmanship'
  - PHP
tags:
  - check style
  - clean code
  - coding standard
  - if
  - return
  - style

---
When contributing to opensource code at GitHub, I found some mistakes, or improvements when writing code, that are repetead over and over again for newbies and not so newbies developers. I&#8217;ll try to write some posts about this, but for not, let&#8217;s start with some &#8220;if&#8221; and &#8220;return&#8221; statements cases.

<!--more-->

### Return #1: If + big code + small else

When you have and if-else, I&#8217;d prefer to have the shortest block first so I can see more information at once. Specially, the first flow related with the conditional cases.

Bad:

<pre>public function foo($user)
{
    if ($user-&gt;hasAvatar()) {
        // tons of amazing code that make you scroll to the infinity
        // ...
    } else {
        // typical check
        return false;
    }
}</pre>

Better:

<pre>public function foo($user)
{
    if (!$user-&gt;hasAvatar()) {
        return false;
    } else {
        // tons of code that you should refactor :)
        // btw, check the next one for this case
    }    
}</pre>

### Return #2: If + return + useless else

If you return something in the first case, &#8220;else&#8221; is useless. Removing it reduces lines of code, cyclomatic complexity and so on.

Bad:

<pre>public function foo($user)
{
    if (!$user-&gt;hasAvatar()) {
        return false;
    } else {
        // do something...
    }
}</pre>

Better:

<pre>public function foo($user)
{
    if (!$user-&gt;hasAvatar()) {
        return false;
    }

    // do something...
}</pre>

### Return #3: If + return true + else return false

I don&#8217;t know why, but this is typical. If you are returning booleans, please, return the conditional evaluation.

Bad:

<pre>public function foo($user)
{
    if ($user-&gt;hasAvatar()) {
        return true;
    } else {
        return false;
    }
}</pre>

Better:

<pre>public function foo($user)
{
    return $user-&gt;hasAvatar();
}</pre>

### Return #4: If + return A + else return B

Exactly the same with other types.

Bad:

<pre>public function foo($user)
{
    if ($user-&gt;hasAvatar()) {
        return &#039;yeah!&#039;;
    } else {
        return &#039;woo!&#039;;
    }
}</pre>

Better:

<pre>public function foo($user)
{
    return $user-&gt;hasAvatar() ? &#039;yeah!&#039; : &#039;woo!&#039;;
}</pre>

### Return #5: If + else that holds a default value

One variation for case #2.

Bad:

<pre>public function foo($user)
{
    if ($user-&gt;hasAvatar()) {
        $result = &#039;yeah!&#039;;
    } else {
        $result = null;
    }

    // Do whatever with $result...
    return $result;
}</pre>

Better:

<pre>public function foo($user)
{
    $result = null;
    if ($user-&gt;hasAvatar()) {
        $result = &#039;yeah!&#039;;
    }

    // Do whatever with $result...
    return $result;
}</pre>

Feel free to comment for adding other examples that may help others. Please, make your mates live easier. Happy Clean Coding!