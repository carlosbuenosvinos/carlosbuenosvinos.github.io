---
title: 'Legacy Code and Teams Series: Composer'
author: Carlos Buenosvinos
type: post
date: 2014-07-29T07:00:23+00:00
url: /legacy-code-and-teams-series-composer/
categories:
  - 'Agile &amp; Craftsmanship'
  - PHP
tags:
  - composer
  - legacy code
  - legacy teams
  - pear
  - php

---
<p class="p1">
  After talking about <a title="Legacy Code and Teams Series: Training Sessions and Technical Committee" href="http://carlosbuenosvinos.com/legacy-code-and-teams-series-training-sessions-and-technical-committee/" target="_blank">Technical Committee and Training Sessions</a> in the previous post of the series, today, I&#8217;ll carry on with adding Composer to legacy projects without pain.
</p>

<p class="p1">
  The day I left Emagister, I was sat down with <a href="https://twitter.com/eberhm" target="_blank">Eber</a> and <a href="https://twitter.com/theUniC" target="_blank">Christian</a> having beers and talking about all the cool things we did there. We were reviewing each of the changes, it cost, benefits, feelings in the team, etc. When I said, &#8220;what about composer? that was cool!&#8221;, Eber said, <strong>&#8220;Composer is not just cool, it&#8217;s a must.&#8221;</strong>. As always, he was too right. It has been also the first thing done at Atrápalo.
</p>

<p class="p1">
  Starting with a brand-new project using <a href="https://getcomposer.org/doc/03-cli.md#create-project" target="_blank">create-project command</a> it&#8217;s easy, but, what about those developers dealing with a big legacy project, with custom autoloader, PEAR dependencies, hardcoded fixes in a external library, non PSR-0 code, etc. Don&#8217;t worry, you can also take benefit of Composer. However, there are some tricks you should take care of in order to face those issues.
</p>

<!--more-->

### My code does not follow PSR-0 {.p1}

<p class="p1">
  The autoloading options for Composer are really well defined in its <a href="https://getcomposer.org/doc/04-schema.md#autoload" target="_blank">documentation</a>. People just don&#8217;t read them. If your code follows PSR-0 or PSR-4, you have to define in your composer.json the &#8220;psr-0&#8221; or &#8220;psr-4&#8221; directive and specified what prefixes point to what directories. Easy.
</p>

<p class="p1">
  But, if your code does not follow any of those standards, you can always use the <a href="https://getcomposer.org/doc/04-schema.md#classmap" target="_blank">&#8220;classmap&#8221; option</a>.
</p>

<pre class="brush: javascript; gutter: true">{
    "autoload": {
        "classmap": ["src/", "lib/", "Something.php"]
    }
}</pre>

<p class="p1">
  When running <em>composer install</em> or <em>composer update</em>, it will go through all the folders and files identifying any class definition. It will generate a file ./<em>vendor/composer/autoload_classmap.php</em> with an array with all the class names and their corresponding path files. So, when you require a file, composer goes to this dictionary, find the class by name and requires the file associated with it.
</p>

<p class="p1">
  <span style="text-decoration: underline;">Cons:</span> What it&#8217;s annoying is that if any developer in your team adds a new class in those folders, you will need to run <em>composer dump-autoload</em> in order to regenerate the classmap. If you want to update the classmap automatically, you can add a hook in your Composer, but it&#8217;s an approach I just don&#8217;t like.
</p>

### I already have a custom autoloader {.p1}

You may have an existing autoloader that you want to preserve. However, if the previous autoloader trick does not fix your situation, you really have something strange such as classes defined with the same name in different files you load dynamically. My suggestion is to use just one autoloader. So, while you move to composer ;) you may need to live with both autoloaders in your application. Is there an option to do it? Of course! You can achieve this mixing your current  &#8220;spl\_auto\_register&#8221; and composer&#8217;s autoload require famous code line:

<pre class="brush: actionscript3; gutter: true">&lt;?php
// Somewhere in your bootstrap file
spl_auto_register([&#039;MyCustomAutoloader&#039;, &#039;load&#039;]);
require __DIR__ . &#039;/../../vendor/autoload.php&#039;; 
//...
</pre>

<p class="p1">
  If you need to live with both autoloaders, you may need to run your autoloader <strong>before</strong> Composer&#8217;s. In order to do it, you will have to take a look to &#8220;prepend-autoloader&#8221; Composer option. By default, Composer will always prepend its autoloader to any others registered.
</p>

<p class="p1">
  <em><strong>&#8220;prepend-autoloader:</strong> Defaults to <code>true</code>. If false, the composer autoloader will not be prepended to existing autoloaders. This is sometimes required to fix interoperability issues with other autoloaders.&#8221;</em>
</p>

<p class="p1">
  The reason, take a look to /vendor/composer/ClassLoader.php:
</p>

<pre class="brush: php; gutter: true">/**
 * Registers this instance as an autoloader.
 *
 * @param bool $prepend Whether to prepend the autoloader or not
 */
public function register($prepend = false)
{
    spl_autoload_register(array($this, &#039;loadClass&#039;), true, $prepend);
}</pre>

<p class="p1">
  So probably, you will need to play with this configuration if you need to tune what are the order you need to make your autoloader and Composer&#8217;s live together. Tricks apart, it&#8217;s possible to mix them, so don&#8217;t worry. Do it while you move to composer.
</p>

### I have dependencies directly in my code {.p1}

<p class="p1">
  For each library you have in your project, find it in <a href="https://packagist.org/" target="_blank">Packagist</a>. If it&#8217;s there, great! just define the library in your <em>require </em>configuration section. Everything is explain in Composer doc. If the library is in other place rather than Packagist, you can define your own <a href="https://getcomposer.org/doc/05-repositories.md" target="_blank">repository</a> in Composer and require your library normally. Let&#8217;s see an example.
</p>

<p class="p1">
  If you want to import the Google Analytics API library, you&#8217;ll need to define your own repository.
</p>

<pre class="brush: javascript; gutter: true">"repositories": [
    {
        "type": "package",
        "package": {
            "name": "gapi/gapi",
            "version": "1.3.0",
            "dist": {
                "url": "https://gapi-google-analytics-php-interface.googlecode.com/files/gapi-1.3.zip",
                "type": "zip"
            },
            "source": {
                "url": "http://gapi-google-analytics-php-interface.googlecode.com/svn",
                "type": "svn",
                "reference": "trunk/"
            },
            "autoload": {
                "files": [
                    "gapi.class.php"
                ]
            }
        }
    },
    ...
],
"require": {
    "gapi/gapi": "1.3.0",
    ...
</pre>

<p class="p1">
  For more information, just take a look to how to <a href="https://getcomposer.org/doc/05-repositories.md" target="_blank">create a custom repository Composer documentation</a>.
</p>

### I have some dependencies that are not in Packagist {.p1}

<p class="p1">
  If some libraries are not there, you can just create your own library in Packagist. Register your account, put your code in GitHub and link your project. I&#8217;m sure you are not the only one that needs that library in Packagist. Community power!
</p>

### I have some dependencies I cannot update due to own modifications {.p1}

Bad, bad, bad. This is something you should never do. Try always to extend a library but not to modify it directly. You may get stuck in that version and updates may be imposible to apply. So, the options you have are: move your version to Packagist as the previous tip, try to use the official Packagist version extending the library you are using or take your modifications and make a pull request to library&#8217;s owner.

### I still use some PEAR libraries {.p1}

Some of the most typical PEAR libraries have been ported to Packagist. Just <a href="https://packagist.org/search/?q=pear" target="_blank">search for &#8220;pear&#8221;</a>.

### Sum up {.p1}

<p class="p1">
  Just use Composer, seriously. More tips are welcome.
</p>