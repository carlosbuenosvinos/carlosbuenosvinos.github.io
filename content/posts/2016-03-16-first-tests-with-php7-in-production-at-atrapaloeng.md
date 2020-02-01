---
title: 'First tests with #PHP7 in production at @AtrapaloEng'
author: Carlos Buenosvinos
type: post
date: 2016-03-16T09:00:03+00:00
url: /first-tests-with-php7-in-production-at-atrapaloeng/
featured_image: /posts/images/2016/03/php7.png
dsq_thread_id:
  - "4666934191"
categories:
  - DevOps
  - PHP
tags:
  - migration
  - performance
  - php
  - php7

---
On Monday, Badoo blogged about its migration to PHP7 (<a href="https://techblog.badoo.com/blog/2016/03/14/how-badoo-saved-one-million-dollars-switching-to-php7/" target="_blank">https://techblog.badoo.com/blog/2016/03/14/how-badoo-saved-one-million-dollars-switching-to-php7/</a>). Those are great results! At @AtrapaloEng, we&#8217;re running already tests in production to perform the same step. We could have started some months before, but we&#8217;ve been struggling with the php-msgpack extension and its (un)support for PHP7. We hope to deploy PHP7 in all our server during this week but we would like to share with you what we have seen so far. What we have done is adding another FPM node with the same capabilities as the current ones running in production with PHP 5.6. The new node is getting the same amount of traffic as the other ones. No special configurations or tweaks such as Huge Pages, just PHP7 upgrade. Data after more than 24 hours running.
  
<!--more-->


  
**Purple: FPM, PHP 7.0.4 (ngx24)**

PHP 7.0.4-1~dotdeb+8.1 (cli) ( NTS )
  
Copyright (c) 1997-2016 The PHP Group
  
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
  
with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2016, by Zend Technologies

**Pink: FPM, PHP 5.6.14 **(ngx23)****

PHP 5.6.14-0+deb8u1 (cli) (built: Oct 4 2015 16:13:10)
  
Copyright (c) 1997-2015 The PHP Group
  
Zend Engine v2.6.0, Copyright (c) 1998-2015 Zend Technologies
  
with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2015, by Zend Technologies

### Load average: Cut by half

### <a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.50.49.png?ssl=1" rel="attachment wp-att-807"><img class="alignnone size-large wp-image-807" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.50.49.png?resize=620%2C150&#038;ssl=1" alt="Screen Shot 2016-03-16 at 09.50.49" width="620" height="150" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.50.49.png?resize=1024%2C247&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.50.49.png?resize=300%2C72&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.50.49.png?resize=768%2C185&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.50.49.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

### Memory Usage: Cut by half

### <a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.51.24.png?ssl=1" rel="attachment wp-att-808"><img class="alignnone size-large wp-image-808" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.51.24.png?resize=620%2C147&#038;ssl=1" alt="Screen Shot 2016-03-16 at 09.51.24" width="620" height="147" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.51.24.png?resize=1024%2C242&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.51.24.png?resize=300%2C71&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.51.24.png?resize=768%2C182&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-16-at-09.51.24.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

After more than 24 hours of running, memory usage is cut by more than a half.

### Response time: Reduced by 25%-40%

At Atrápalo, we have tons of calls to external providers for getting content (hotel providers such as Booking and airlines companies), so most of the time we&#8217;re waiting to external resources, not pure PHP execution. Also consider some legacy code running too many queries in a single request. So considering that, global response time (including external resources and queries) with PHP7 improves between 25%-40%.

### Unit Tests

We&#8217;ve got information about running our Unit Tests.

<a href="https://twitter.com/atrapaloeng/status/709399349478989825" target="_blank" rel="attachment wp-att-799"><img class="alignnone size-large wp-image-799" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-15-at-22.13.27.png?resize=620%2C320&#038;ssl=1" alt="Tweet Unit Testing PHP7" width="620" height="320" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-15-at-22.13.27.png?resize=1024%2C528&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-15-at-22.13.27.png?resize=300%2C155&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-15-at-22.13.27.png?resize=768%2C396&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/03/Screen-Shot-2016-03-15-at-22.13.27.png?w=1194&ssl=1 1194w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

### Conclusion

It looks like there is no reasons to go for it. ;)