---
title: Fixing commit TYPOs in git history with filter-branch
author: Carlos Buenosvinos
type: post
date: 2014-05-12T13:21:12+00:00
url: /fixing-commit-typos-in-git-history-with-filter-branch/
categories:
  - 'Agile &amp; Craftsmanship'
tags:
  - filter-branch
  - git
  - github
  - rewriting history
  - typo commits

---
Last week, we tried at Atrapalo to migrate from our own git repository to Github. That should be easy, is just a matter of three commands already explained in <a href="https://help.github.com/articles/importing-an-external-git-repository" target="_blank">Github&#8217;s guide</a>. The main problem appears when you have some sort of issue with your commits, so messages like &#8220;invalid author email&#8221; raises when trying to push your changes.

<!--more-->

For checking if your repository is ok, you can run **git fsck**. If everything is ok, nice, however, in our case, some error messages where there.

[<img class="size-large wp-image-418 aligncenter" src="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.11.10-1024x344.png?resize=620%2C208" alt="Screen Shot 2014-05-12 at 15.11.10" width="620" height="208" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.11.10.png?resize=1024%2C344&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.11.10.png?resize=300%2C100&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.11.10.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />][1]

Let&#8217;s check those commits with issues with **git log bca5308fa04b1d43e47de77fc264a770ba9d2522**

[<img class="size-large wp-image-419 aligncenter" src="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.13.48-1024x377.png?resize=620%2C228" alt="Screen Shot 2014-05-12 at 15.13.48" width="620" height="228" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.13.48.png?resize=1024%2C377&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.13.48.png?resize=300%2C110&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.13.48.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />][2]

Look at the author. There is an additional &#8220;**<**&#8221; that should not be there. That&#8217;s causing the problem. So we need to rewrite the history to update those commits. There are <a href="http://git-scm.com/book/en/Git-Tools-Rewriting-History" target="_blank">different options</a>, but the ones that avoid remerging are based on **filter-branch** but cloning again your repo is a must. If you look to StackOverflow or Google, most of the scripts that are suppose to work are like:

<pre class="brush: bash; gutter: true">$ git filter-branch --commit-filter &#039;
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi&#039; HEAD</pre>

But it didn&#8217;t work for us. So after talking with the GitHub guys, here&#8217;s the solution that worked like a charm.

<pre class="brush: bash; gutter: true">git filter-branch -f -- --all</pre>

If take a closer look to git-filter-branch there is an interesting comment in the description:

> <span style="color: #4e443c;">If you specify no filters, the commits will be recommitted without any changes, which would normally have no effect. Nevertheless, this may be useful in the future for compensating for some Git bugs or such, therefore such a usage is permitted</span>

The old commits will still be there, but pushing will push just the new ones. If you&#8217;re migrating and try to **push &#8211;mirror**, everything is pushed so it won&#8217;t work due to this old commits. An alternative could be **git push &#8211;all -u origin**

Hope it helps somebody.

 [1]: https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.11.10.png
 [2]: https://i1.wp.com/carlosbuenosvinos.com/posts/images/2014/05/Screen-Shot-2014-05-12-at-15.13.48.png