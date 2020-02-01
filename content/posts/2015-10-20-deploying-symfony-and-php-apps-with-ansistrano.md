---
title: Deploying Symfony (and PHP) apps with Ansistrano
author: Carlos Buenosvinos
type: post
date: 2015-10-20T08:00:30+00:00
url: /deploying-symfony-and-php-apps-with-ansistrano/
featured_image: /posts/images/2015/10/ansistrano-flow.png
categories:
  - 'Agile &amp; Craftsmanship'
  - PHP
tags:
  - ansible
  - ansistrano
  - capistrano
  - deploy
  - deployment
  - rollback
  - silex
  - symfony

---

For a long time, <a href="http://capifony.org/" target="_blank">Capifony</a> was, without any doubt, the facto option for deploying Symfony2 applications. Capifony is a ruby gem based in </span><a href="https://github.com/capistrano/capistrano" target="_blank">Capistrano</a> v2, an open source tool for running scripts on multiple servers with a deployment flow built-in. It’s primary use is for easily deploying applications. While it was built specifically for deploying <a href="http://rubyonrails.org/" target="_blank">Rails</a> apps, it’s pretty simple to customize it to deploy other types of applications. At that time, alternatives were shell scripting or Fabric. Now there&#8217;s a better one, Ansistrano.

<!--more-->

Capistrano v3 were released but without the features of its previous version. v2 was not maintained anymore, so that was a problem for the ones using Capistrano v2 for deploying. Then the whole DevOps started with Puppet, Chef and Ansible. With this provisioning systems, executing remote commands is so easy, specially if you are not Sysadmin, that thinking about using another tool is not worthy. So, why don&#8217;t put together the best of both worlds? Why don&#8217;t put together the power of Ansible and its community and the easy flow of Capistrano v2 for deploying apps?

Today, I’m happy to announce <a href="https://github.com/ansistrano" target="_blank">Ansistrano</a>, an <a href="http://www.ansible.com/" target="_blank">Ansible</a> role for deploying and rollbacking your PHP (and another scripting language Ruby, Python, NodeJS, etc.) apps. It has almost 200 stars on Github and some companies already using it. Ricard Clau (<a href="https://twitter.com/ricardclau" target="_blank">@ricardclau</a>), project&#8217;s cofounder, is showing us how easy can be deploying <a href="http://silex.sensiolabs.org/" target="_blank">Silex</a> applications with Ansistrano. Please, help us spread the love.

{{< youtube x94rCIUz5c0 >}}