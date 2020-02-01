---
title: Accessing securely production services with SSH port forwarding
author: Carlos Buenosvinos
type: post
date: 2016-10-25T08:01:46+00:00
url: /accessing-securely-production-services-with-ssh-port-forwarding/
featured_image: /posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png
dsq_thread_id:
  - "5250964598"
categories:
  - DevOps
tags:
  - elastic
  - mysql
  - port forwarding
  - production
  - rabbitmq
  - security
  - ssh

---
We&#8217;ve already talked about <a href="https://carlosbuenosvinos.com/github-ansible-deploy-your-public-ssh-keys-to-your-server/" target="_blank">how to deploy your public SSH keys into your server using Ansible and GitHub</a>. This time, I just want to share a simple approach to have access to your production services (MySQL, Elastic, RabbitMQ, etc.) without exposing publicly your services. You have different alternatives, probably the most common are using a VPN or use SSH port forwarding. Let&#8217;s see an example of the last one.

<!--more-->

Consider that you have you cool side project running in Production. You are using some services such as MySQL, RabbitMQ and ElasticSearch. For debugging and having real data, you want to run some queries against MySQL, perform some queries to Elastic or spy some messages in RabbitMQ.

For accessing this production services from your development machine there are different options.

**A. Open the ports publicly**

This is not recommended because of the security risks. Anyone can just access your ElasticSearch and do whatever they want.

**B. Run the services in different ports and make them publicly**

Stop, please stop. I&#8217;m serious.

**C. Set up a VPN and use it to connect**

Now we&#8217;re talking. However, setting a VPN is a bit too much for me. If you&#8217;re interested, take a look to [&#8220;How To Set Up an OpenVPN Server on Ubuntu 16.04&#8221;][1]. The tutorial has lots of steps.

**D. Use SSH port forwarding**

The basic idea is to forward some of your local ports to a remote IP and ports using SSH. Imagine that you want to access your remote ElasticSearch that runs on port 9200. Let&#8217;s take a closer look to this option.

**Let&#8217;s run the some commands!**

If you make a curl to http://my-server.com:9200 an error page should be shown.

<pre>curl http://my-server.com:9200</pre>

[<img class="alignnone size-large wp-image-994" src="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?resize=620%2C260&#038;ssl=1" alt="screen-shot-2016-10-24-at-22-31-38" width="620" height="260" srcset="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?resize=1024%2C429&ssl=1 1024w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?resize=300%2C126&ssl=1 300w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?resize=768%2C322&ssl=1 768w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?w=1476&ssl=1 1476w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />][2]

And you should see the same if you are not running ElasticSearch in you local machine:

<pre>curl http://localhost:9200</pre>

<img class="alignnone size-large wp-image-993" src="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.36.png?resize=620%2C291&#038;ssl=1" alt="screen-shot-2016-10-24-at-22-34-36" width="620" height="291" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.36.png?resize=1024%2C481&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.36.png?resize=300%2C141&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.36.png?resize=768%2C361&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.36.png?w=1476&ssl=1 1476w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.36.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />

Now, open a new tab and try:

<pre>ssh -L 9200:localhost:9200 root@my-server.com</pre>

First of all, it looks like a ssh command. Cool. If you don&#8217;t have your SSH public key deployed in your server, the command will ask for the root password, as you are used to. After entering the password, you will have access to your remote server. What&#8217;s the difference? What about if we try the curl command again?

<pre>curl http://localhost:9200</pre>

[<img class="alignnone size-large wp-image-995" src="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?resize=620%2C349&#038;ssl=1" alt="screen-shot-2016-10-24-at-22-34-15" width="620" height="349" srcset="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?resize=1024%2C576&ssl=1 1024w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?resize=300%2C169&ssl=1 300w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?resize=768%2C432&ssl=1 768w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?w=1476&ssl=1 1476w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />][3]

Bam! Our ElasticSearch on Production. **What about if we want to include other services such as MySQL or RabbitMQ?**

<pre>ssh -L 9200:localhost:9200 -L 3306:localhost:3306 -L 15672:localhost:15672 root@my-server.com</pre>

The -L options maps local ports with remote ports. In the example, we are mapping our localhost:9200 to my-server.com:9200 (ElasticSearch). The same happens with MySQL (3306) and RabbitMQ Admin Plugin (15672).

**What about if you don&#8217;t need to get access to the server, just do the forwarding?**

<pre>ssh -nNT -L 9200:localhost:9200 -L 3306:localhost:3306 -L 15672:localhost:15672 root@my-server.com</pre>

The -nNT flags makes the command to wait until Control+C. If you don&#8217;t use it, the command still works but you will get prompt in the remote server.

**What about if you&#8217;re running already ElasticSearch on your local machine and you want to map to another port? **No problem.

<pre>ssh -nNT -L 9990:localhost:9200 -L 9991:localhost:3306 -L 9992:localhost:15672 root@my-server.com</pre>

[<img class="alignnone size-large wp-image-1001" src="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?resize=620%2C274&#038;ssl=1" alt="screen-shot-2016-10-25-at-09-31-35" width="620" height="274" srcset="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?resize=1024%2C452&ssl=1 1024w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?resize=300%2C133&ssl=1 300w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?resize=768%2C339&ssl=1 768w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?w=1476&ssl=1 1476w, https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?w=1240&ssl=1 1240w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" />][4]

Hope it helps!

If you want to know more about what you can do with SSH, take a look to this video &#8220;The Black Magic of SSH / SSH Can Do That?&#8221; by Bo Jeanes ;)

<div class="embed-vimeo" style="text-align: center;">
</div>

 [1]: https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04
 [2]: https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.31.38.png?ssl=1
 [3]: https://i2.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-24-at-22.34.15.png?ssl=1
 [4]: https://i0.wp.com/carlosbuenosvinos.com/posts/images/2016/10/Screen-Shot-2016-10-25-at-09.31.35.png?ssl=1