---
title: Remove non used PHP files in your project
author: Carlos Buenosvinos
type: post
date: 2013-09-26T07:40:56+00:00
url: /remove-non-used-php-files-in-your-project/
categories:
  - PHP
tags:
  - clean code
  - opcache
  - symfony
  - zend

---
When a project gets bigger, specially when using a framework, it&#8217;s mandatory to have some processes that help you in removing code that is not used anymore such as models, classes, template files, etc.

That is not an easy task, however the benefits or removing non used coded are tons (code coverage, development speed, bugs, etc.). But what&#8217;s the best approach to detect that a template file is not used anymore in your PHP project?

<!--more-->

We discuss about that at <a href="http://www.emagister.com" target="_blank">Emagister</a> with <a href="http://twitter.com/eberhm" target="_blank">Eber Herrera</a> and we get to two different approaches that might help you.

## **Code analysis approach**

For each type of object you want to remove, you can create a command to do it. However, the main algorithm is:

  1. Get a list of items to check (list of models, view helpers, etc.): <a href="http://symfony.com/doc/current/components/finder.html" target="_blank">Symfony Finder Component</a> can help you
  2. Iterate on your code and grep it or parse it: <a href="https://github.com/nikic/PHP-Parser" target="_blank">PHP Parser</a> can do the job
  3. Report the object that is candidate to be removed
  4. Manual review, remove and feel like a star!

It&#8217;s possible that every type of object needs a different strategy. However, this process can be applied in most cases. For example, ViewHelpers in Zend Framework that are not called, TPLs that are not included, etc.

What&#8217;s the problem? Some objects usage depends on an application request so you should check your access logs. But there&#8217;s another approach.

## **Opcode cached files approach**

And there&#8217;s the tricky one! If you have played a bit with opcache (if not you should because it&#8217;s faster than APC), you will see that is easy to get a list of all the files cache by the opcache.

At Emagister, we have developed a command, that connects to our opcache, gets the cached files, stores that info in a redis and the computes the difference with the files in our project. We run this process frequently.

Benefits are that it does not affect the performance, it&#8217;s base on real usage of your site (so let the command work and check from time to time the differences) and it affects any object because it checks the file usage, not the objects contained in it, so no parsing needed.

<pre class="brush: php; gutter: true">$status = opcache_get_status();
foreach($status[&#039;scripts&#039;] as $key=&gt;$data) {
    $dirs[dirname($key)][basename($key)]=$data;
}

asort($dirs);

foreach($dirs as $dir =&gt; $files) {
    foreach ($files as $file =&gt; $data) {
        // $data["hits"] holds hits for the script
        // $file has the filename
        // $dir has the path
    }
}</pre>

And remember children! Keep your clean code. You will be a happier developer and you&#8217;ll make others happier.