---
title: 'Working at the same time in a project and its dependencies: Composer and path type repository'
author: Carlos Buenosvinos
type: post
date: 2016-03-24T15:36:10+00:00
url: /working-at-the-same-time-in-a-project-and-its-dependencies-composer-and-path-type-repository/
dsq_thread_id:
  - "4689841399"
categories:
  - DDD
  - PHP
tags:
  - composer
  - dependencies
  - monolithic repository
  - packagist

---
With the Domain-Driven Design, Microservices and API explosion, I see more teams working in a base project, such as a Web, and integrating other private packages they develop in a different repository. For example, working on the Web and an API client for a external REST service at the same time.

For example, at @AtrapaloEng, our sales development team (checkout process, purchases, orders, payments, etc.) integrates different payment methods into the web so users, specially Latam ones, can be happier using their preferred payment methods. They create a repository for each of the new payment methods we support as a external package. Sometimes a developer in the team must work with different projects at the same time, the Web and the payment method in development.

In this scenario, one option is work on the payment package, tag, push, go to the base project and update dependencies with Composer. As you can see, it&#8217;s a bit slower, how we can improve this process? Composer to the rescue!

<!--more-->

At <a href="https://github.com/restgames" target="_blank">RESTGames</a>, there is a battleship referee that makes two REST services play one against the other. It&#8217;s written in PHP and uses a helper PHP library with basic elements of Battleship game (Board, Ships, Shot, etc.) so you can create your own engine. Referee project depends upon the PHP library. When I improve the referee, sometimes I need to update the library. Sometimes it will be cool just to update the library in the &#8220;vendor&#8221; folder and commit the changes to the library from there.

Well, with Composer there is something similar for that. If you have both a project and its dependency cloned in your machine, you can define a Composer repository in your composer.json of &#8220;path&#8221; type that points to the library path. It&#8217;ll use a copy-and-paste o softlink (the cool one) strategy to &#8220;import&#8221; the dependency into the project.

From Composer&#8217;s web: &#8220;In addition to the artifact repository, you can use the path one, which allows you to depend on a relative directory. This can be especially useful when dealing with monolith repositories.&#8221;

Let&#8217;s take a look to the composer.json.

<pre class="brush: php; gutter: true">{
    "name": "restgames/battleship-client",
    ...
    "repositories": [
        {
            "type": "path",
            "url": "../battleship-php",
            "options": {
                "symlink": true
            }
        }
    ],
    "require": {
        "restgames/battleship-php": "dev-master",
    },
...</pre>

When running &#8220;composer install&#8221;, Composer will take a look to the folder. If the library exists, it will softlink it in your vendor folder. If it does not exist, it will try to find it in Packagist. Let&#8217;s see some snapshots.

<p style="text-align: center;">
  <strong>Library cloned in local</strong>
</p>

<a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.18.36.png?ssl=1" rel="attachment wp-att-819"><img class="size-large wp-image-819 aligncenter" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.18.36.png?resize=620%2C389&#038;ssl=1" alt="Screen Shot 2016-03-24 at 16.18.36" width="620" height="389" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.18.36.png?resize=1024%2C642&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.18.36.png?resize=300%2C188&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.18.36.png?resize=768%2C481&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.18.36.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

<p style="text-align: center;">
  <strong>Composer symlinked from local folder</strong>
</p>

<a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.04.png?ssl=1" rel="attachment wp-att-820"><img class="size-large wp-image-820 aligncenter" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.04.png?resize=620%2C389&#038;ssl=1" alt="Screen Shot 2016-03-24 at 16.21.04" width="620" height="389" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.04.png?resize=1024%2C642&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.04.png?resize=300%2C188&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.04.png?resize=768%2C481&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.04.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

<p style="text-align: center;">
  <strong>Library is renamed so Composer is not going to find it</strong>
</p>

<p style="text-align: center;">
   <a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.40.png?ssl=1" rel="attachment wp-att-821"><img class="alignnone size-large wp-image-821" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.40.png?resize=620%2C389&#038;ssl=1" alt="Screen Shot 2016-03-24 at 16.21.40" width="620" height="389" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.40.png?resize=1024%2C642&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.40.png?resize=300%2C188&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.40.png?resize=768%2C481&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.21.40.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>
</p>

<p style="text-align: center;">
  <strong>Composer downloads from Packagist ;)</strong>
</p>

<p style="text-align: center;">
  <a href="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.22.03.png?ssl=1" rel="attachment wp-att-822"><img class="alignnone size-large wp-image-822" src="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.22.03.png?resize=620%2C389&#038;ssl=1" alt="Screen Shot 2016-03-24 at 16.22.03" width="620" height="389" srcset="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.22.03.png?resize=1024%2C642&ssl=1 1024w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.22.03.png?resize=300%2C188&ssl=1 300w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.22.03.png?resize=768%2C481&ssl=1 768w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-24-at-16.22.03.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>
</p>

The only issue, is that all developers working in the project and the library needs to have cloned the library in the same folder name, not a big deal for such a benefit.

You can find more info at Composer documentation about Repositories (<a href="https://getcomposer.org/doc/05-repositories.md#path" target="_blank">https://getcomposer.org/doc/05-repositories.md#path</a>)

### Bonus Point (requested via twitter)

You can define as many repositories as you want. Componer will try to find the package respecting the order defined in the composer.json file. For example, given the following composer.json:

<pre class="brush: javascript; gutter: true">...
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/restgames/battleship-php"
        },
        {
            "type": "path",
            "url": "../battleship-php",
            "options": {
                "symlink": true
            }
        }
    ],
...</pre>

Composer will try to find first on github without packagist, then in local, then in Packagist.