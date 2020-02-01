---
title: 'GitHub + Ansible: deploy your public SSH keys to your servers'
author: Carlos Buenosvinos
type: post
date: 2016-09-14T16:22:39+00:00
url: /github-ansible-deploy-your-public-ssh-keys-to-your-server/
featured_image: /posts/images/2016/09/octocat_darkwood.jpg
dsq_thread_id:
  - "5144133721"
categories:
  - DevOps
tags:
  - ansible
  - github
  - provision
  - ssh key

---
When I work with some mates in a side project, I need to give them access to the server. This is a quick tip about how to easily do it using GitHub and Ansible. The solution is simple and elegant.

<!--more-->

If you and your mates work with GitHub, you may have added all your public SSH keys to GitHub. A really nice feature that GitHub provides is that you can access any user public SSH keys by visiting:

<pre>https://github.com/{{ item }}.keys</pre>

So, for example, if you want to take a look at mine, just visit:

<pre>https://github.com/carlosbuenosvinos.keys</pre>

Ok, so now we have a repository of SSH public keys, now it&#8217;s time to deploy them into your server. I love Ansible. The trick here is to use the <a href="http://docs.ansible.com/ansible/authorized_key_module.html" target="_blank">authorized_key</a> module and the **key** option. In the documentation, you can read:

> The SSH public key(s), as a string or (since 1.9) url (https://github.com/username.keys).

So, you can add in your provisioning roles a task to include all your friends public SSH keys. In this example, I allow my friends Christian (theUniC), Ricard (ricardclau) and myself.

<pre>tasks:
  - name: Ensure collaborators SSH keys are installed
    authorized_key: user=root key=https://github.com/{{ item }}.keys
    with_items:
      - carlosbuenosvinos
      - theUniC
      - ricardclau</pre>

Hope it helps!