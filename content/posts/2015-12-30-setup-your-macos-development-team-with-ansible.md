---
title: Setup your MacOS (or other OS) development team with Ansible
author: Carlos Buenosvinos
type: post
date: 2015-12-30T10:41:28+00:00
url: /setup-your-macos-development-team-with-ansible/
featured_image: /posts/images/2015/12/ansible_circleA_red.png
categories:
  - DevOps
tags:
  - ansible
  - brew
  - local environment
  - macos
  - php

---
Imagine that a new developer joins your team, installing everything needed for developing, including the application being developed is not an easy o fast task. I&#8217;m sure you have a wiki process, a markdown file in your repo or something similar. For a normal PHP web application, there is so much to do: installing PHP, composer and global tools, npm and/or bower, mysql, redis, etc. or setting up you Docker/Vagrant environment if you choose an isolated environment.

I remember when GitHub released <a href="https://github.com/boxen" target="_blank">Boxen</a> in 2012. A tool for managing Mac development boxes with love. It&#8217;s based in Puppet. The main idea is use Puppet for provisioning not remote machines or servers but you local Mac. Let&#8217;s see a silly idea about how to do it with Ansible.

<!--more-->

### tl;dr

For set up MacOS local development environments, you can use <a href="http://www.ansible.com/" target="_blank">Ansible</a> with local connections (ansible_connection=local in your inventory file) and the <a href="http://docs.ansible.com/ansible/homebrew_module.html" target="_blank">homebrew</a> module.

### Introduction

On a Mac, and also on other OS, there are different options for developing: local development with built-in web servers, Docker or Vagrant. The first one is the fastest one, but has its pitfalls (polluting your local environment with things you need for a specific project, routing tricks or lack of HTTPS support), the second one is still fast if you&#8217;re not in MacOS (that uses a VirtualBox machine for running Docker due to its lack of containers support) and the last one, the more flexible in all the environments but slower in performance due to its low sharing capabilities (Vbox Share folders, NFS, or Rsync).

### Set the environment up

What it&#8217;s a common issue is set up everything on your machine. You can do it step by step following a guide somewhere in your repo or wiki, or help the process using a provisioning tool such as Ansible. The trick is using Ansible, with local connection (in order to perform all the tasks in your local machine) and the Homebrew module. Let&#8217;s see a silly example about how to use all of this.

#### Inventory File

<pre class="brush: php; gutter: true">localhost ansible_connection=local</pre>

#### Playbook

<pre class="brush: php; gutter: true">---
- name: Ensure local development is ready
  hosts: all
  tasks:
    - name: REDIS | Ensure REDIS is present
      homebrew: name=redis state=latest
    - name: REDIS | Ensure REDIS will run as daemon
      replace: dest=/usr/local/etc/redis.conf regexp=&#039;^daemonize no$&#039; replace=&#039;daemonize yes&#039;
    - name: REDIS | Ensure REDIS will run just in memory
      replace: dest=/usr/local/etc/redis.conf regexp=&#039;^save (.*)$&#039; replace=&#039;#save \1&#039;
    - name: REDIS | Ensure REDIS is stopped
      command: launchctl unload /usr/local/opt/redis/homebrew.mxcl.redis.plist
      ignore_errors: yes
    - name: REDIS | Ensure REDIS is running
      command: launchctl load /usr/local/opt/redis/homebrew.mxcl.redis.plist
      ignore_errors: yes

    - name: PHP | Tap repositories
      command: brew tap {{ item }}
      with_items:
        - "homebrew/dupes"
        - "homebrew/versions"
        - "homebrew/homebrew-php"
    - name: PHP | Ensure PHP is present
      homebrew: name=php56 state=latest install_options=with-fpm
    - name: PHP | Ensure PHP extensions are present
      homebrew: name={{ item }} state=latest
      with_items:
        - php56-apcu
        - php56-geoip
        - php56-imagick
        - php56-intl
        - php56-mcrypt
        - php56-msgpack
        - php56-opcache
        - php56-pdo-pgsql
        - php56-ssh2
        - php56-tidy
        - php56-twig
        - php56-xdebug
    - name: PHP | Ensure PHP will run as daemon
      replace: dest=/usr/local/etc/php/5.6/php-fpm.conf regexp="^daemonize = no$" replace=";daemonize = no"
    - name: PHP | Ensure PHP is stopped
      command: launchctl unload /usr/local/opt/php56/homebrew.mxcl.php56.plist
      ignore_errors: yes
    - name: PHP | Ensure PHP is running
      command: launchctl load /usr/local/opt/php56/homebrew.mxcl.php56.plist
      ignore_errors: yes

    - name: NGINX | Ensure NGINX is present
      homebrew: name=nginx state=latest
    - file: path=/usr/local/etc/nginx/conf.d state=directory
    - file: path=/usr/local/etc/nginx/sites-enabled state=directory
    - file: path=/usr/local/etc/nginx/logs state=directory
    - template: src=templates/nginx-fpm/nginx.nginx.conf dest=/usr/local/etc/nginx/nginx.conf
    - name: NGINX | Ensure sites are installed
      template: src=templates/nginx-fpm/sites-available/{{ item.src }} dest=/usr/local/etc/nginx/sites-enabled/{{ item.dest }}
      with_items:
        - { src: myproject.nginx.local.conf, dest: myproject.local.conf }
    - name: NGINX | Ensure Nginx is stopped
      command: nginx -s stop
      ignore_errors: yes
      sudo: yes
    - name: NGINX | Ensure Nginx is running
      command: nginx
      sudo: yes

    - name: Setting /etc/hosts file
      lineinfile: dest=/etc/hosts regexp="^127\.0\.0\.1 {{ item }}$" line="127.0.0.1 {{ item }}" insertbefore="BOF"
      sudo: yes
      with_items:
        - myproject.local

    - name: PHP | Ensure PHP configuration is set up properly
      ini_file: dest=/etc/php5/{{ item[0] }}/php.ini section={{ item[1].section }} option={{ item[1].option }} value={{ item[1].value }}
      with_nested:
       - [ "fpm", "cli" ]
       - [
            { option: "memory_limit",                     value: "512M",            section: "PHP" },
            { option: "date.timezone",                    value: "Europe/Madrid",   section: "PHP" },
            { option: "opcache.fast_shutdown",            value: "1",               section: "opcache" },
            { option: "opcache.memory_consumption",       value: "360",             section: "opcache" },
         ]
    - name: PHP | Ensure XDebug is configured properly
      lineinfile: dest=/usr/local/etc/php/5.6/conf.d/ext-xdebug.ini line="{{ item }}"
      with_items:
        - "xdebug.default_enable = 1;"
        - "xdebug.remote_enable = 1;"
        - "xdebug.remote_connect_back = 1;"
        - "xdebug.remote_autostart = 1;"
        - "xdebug.remote_port = 9000;"
        - "xdebug.profiler_enable = 0;"
        - "xdebug.profiler_enable_trigger = 1;"
        - "xdebug.profiler_output_dir = /tmp/xdebug;"
</pre>

And now, you can just run your environment with:

<pre>ansible-playbook setup-my-local-environment.yml -i local-inventory -v --ask-sudo-pass
</pre>

### Conclusion

You know, automate all the things. Including also your local development environment. If your playbook is checked in your project repository, a new developer just needs to install brew, ansible, checkout the project and run the playbook. I prefer Vagrant, but in some cases, this trick can be useful. Hope it helps!