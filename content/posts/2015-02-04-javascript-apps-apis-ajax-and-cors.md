---
title: Javascript Apps, APIs, AJAX and CORS
author: Carlos Buenosvinos
type: post
date: 2015-02-04T08:30:57+00:00
url: /javascript-apps-apis-ajax-and-cors/
categories:
  - Javascript
tags:
  - ajax
  - CORS
  - ionic framework
  - javascript

---
If you are developing Javascript applications that make AJAX calls directly to an API that you don&#8217;t control, you will probably experiment issues with CORS. If you take a look to your console, the following message will appear:

> XMLHttpRequest cannot load http://foo/bar. No &#8216;Access-Control-Allow-Origin&#8217; header is present on the requested resource. Origin &#8216;http://localhost:8100&#8217; is therefore not allowed access.

<!--more-->

How to fix it, easy, developing with Chrome, just install the extension &#8220;<a href="https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi" target="_blank">Allow-Control-Allow-Origin: *</a>&#8221; and you will be able to disable the CORS protection in any Chrome tab. Done. Working. Enjoy.

**UPDATE:** You can also run Chrome with a parameter to disallow all the security checks. For OSX, open Terminal and run: &#8220;open -a Google\ Chrome &#8211;args &#8211;disable-web-security&#8221;