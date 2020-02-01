---
title: 3 commands for working with remote machines
author: Carlos Buenosvinos
type: post
date: 2015-12-14T08:00:47+00:00
url: /3-commands-for-working-with-remote-machines/
featured_image: /posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png
categories:
  - DevOps
tags:
  - cluster
  - csshx
  - docker
  - multiple terminals
  - nmap
  - raspberry
  - rasperry pi
  - remote machines
  - ssh-copy-id

---
These days I&#8217;m playing a bit with 5 Raspberries 2B (4 cores and 1 GB of RAM) in order to build a Docker cluster. There are different options: Docker Swarm over Consul/Etcd/Zookeper, Kubernetes by Google, etc. I just want to share some interesting commands for working with remote machines that have helped me to build such a cluster: ssh-copy-id, nmap and csshx.

<!--more-->

### 1. ssh-copy-id

Because I&#8217;m accessing my raspberries via SSH, I don&#8217;t want to enter password all the time, so I want to use public/private keys to do so. ssh-copy-id to the rescue.

Installs your public key in a remote machine&#8217;s authorized_keys. For MacOS, you will need to install it, for example, using &#8220;brew install ssh-copy-id&#8221;.

ssh-copy-id is a script that uses ssh to log into a remote machine (presumably using a login password, so password authentication should be enabled, unless you&#8217;ve done some clever use of multiple identities) It also changes the permissions of the remote user&#8217;s home, ~/.ssh, and ~/.ssh/authorized\_keys to remove group writability (which would otherwise prevent you from logging in, if the remote sshd has StrictModes set in its configuration). If the -i option is given then the identity file (defaults to ~/.ssh/id\_rsa.pub) is used, regardless of whether there are any keys in your ssh-agent. Otherwise, if this: ssh-add -L provides any output, it uses that in preference to the identity file. If the -i option is used, or the ssh-add produced no output, then it uses the contents of the identity file. Once it has one or more fingerprints (by whatever means) it uses ssh to append them to ~/.ssh/authorized_keys on the remote machine (creating the file, and directory, if necessary).

<a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png" rel="attachment wp-att-740"><img class="size-large wp-image-740 aligncenter" src="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52-1024x554.png?resize=620%2C335" alt="Screen Shot 2015-12-13 at 13.51.52" width="620" height="335" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png?resize=1024%2C554&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png?resize=300%2C162&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png?resize=768%2C416&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png?w=1240&ssl=1 1240w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.51.52.png?w=1860&ssl=1 1860w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

### 2. nmap

After pluging my Rasberry PIs in, I don&#8217;t exactly know what are their IPs, so I need a way to discover that. Nmap to the rescue.

Nmap (&#8220;Network Mapper&#8221;) is a free and open source ([license][1]) utility for network discovery and security auditing. Many systems and network administrators also find it useful for tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. It was designed to rapidly scan large networks, but works fine against single hosts. Nmap runs on all major computer operating systems, and official binary packages are available for Linux, Windows, and Mac OS X. In addition to the classic command-line Nmap executable, the Nmap suite includes an advanced GUI and results viewer ([Zenmap][2]), a flexible data transfer, redirection, and debugging tool ([Ncat][3]), a utility for comparing scan results ([Ndiff][4]), and a packet generation and response analysis tool ([Nping][5]). On MacOS, install it using &#8220;brew install nmap&#8221;.

Because I have my Mac running as a DHCP server on 192.168.2.1 net, I can discover Raspberry IPs, pluging and scanning.

<a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12.png" rel="attachment wp-att-742"><img class="size-large wp-image-742 aligncenter" src="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12-1024x554.png?resize=620%2C335" alt="Screen Shot 2015-12-13 at 13.59.12" width="620" height="335" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12.png?resize=1024%2C554&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12.png?resize=300%2C162&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12.png?resize=768%2C416&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12.png?w=1240&ssl=1 1240w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-13.59.12.png?w=1860&ssl=1 1860w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

### 3. csshx

Sometimes, I need to perform a task in all the PIs (install python for running ansible, for example, check some configuration, etc.). csshX is a Cluster SSH tool using Mac OS X Terminal.app

csshX is a tool to allow simultaneous control of multiple ssh sessions. host1, host2, etc. are either remote hostnames or remote cluster names. csshX will attempt to create an ssh session to each remote host in separate Terminal.app windows. A master window will also be created. All keyboard input in the master will be sent to all the slave windows. To specify the username for each host, the hostname can be prepended by user@. Similarly, appending :port will set the port to ssh to. You can also use hostname ranges, to specify many hosts. &#8220;brew install csshx&#8221;.

<a href="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06.png" rel="attachment wp-att-744"><img class="size-large wp-image-744 aligncenter" src="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06-1024x554.png?resize=620%2C335" alt="Screen Shot 2015-12-13 at 14.06.06" width="620" height="335" srcset="https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06.png?resize=1024%2C554&ssl=1 1024w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06.png?resize=300%2C162&ssl=1 300w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06.png?resize=768%2C416&ssl=1 768w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06.png?w=1240&ssl=1 1240w, https://i2.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.06.06.png?w=1860&ssl=1 1860w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>

<a href="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48.png" rel="attachment wp-att-743"><img class="size-large wp-image-743 aligncenter" src="https://i0.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48-1024x640.png?resize=620%2C388" alt="Screen Shot 2015-12-13 at 14.07.48" width="620" height="388" srcset="https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48.png?resize=1024%2C640&ssl=1 1024w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48.png?resize=300%2C188&ssl=1 300w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48.png?resize=768%2C480&ssl=1 768w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48.png?w=1240&ssl=1 1240w, https://i1.wp.com/carlosbuenosvinos.com/posts/images/2015/12/Screen-Shot-2015-12-13-at-14.07.48.png?w=1860&ssl=1 1860w" sizes="(max-width: 620px) 100vw, 620px" data-recalc-dims="1" /></a>
  
You can type in the red terminal and all the keys will be repeated into all the terminals. If you want to perform something in a specific terminal, just click on it and do it. Then go back into the red terminal.

Hope it helps!

 [1]: https://nmap.org/data/COPYING
 [2]: https://nmap.org/zenmap/
 [3]: https://nmap.org/ncat/
 [4]: https://nmap.org/ndiff/
 [5]: https://nmap.org/nping/