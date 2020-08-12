---
title:  "Generating entropy to create gpg keys"
tags: [entropy, gpg, spack]
style: fill
color: success
description: Generating entropy for gpg keys can sometimes cause massive wait times if your system is idle. I faced this problem and had to figure out a way to invoke false workloads to increase system utilization. Here are my methods following a quick web search.
---

It's been over a month under lockdown due to Covid-19. I try to spend my time being as productive as possible. However, I find myself unable to keep track of the progress I'm making given that I don't document any of it. So, below is the first in a series of posts to keep a record of at least One New Thing I learn everyday.

## Generating entropy to create gpg keys
While trying to create a new gpg key for a spack mirror, I found that it was taking way too long.
{% highlight bash %}
$ spack gpg create "Nithin Kumar" senthilkumar.16@osu.edu
$ spack gpg init
{% endhighlight %}

The reason it was taking so long was because there was not enough entropy in the system to generate the required number of bits.
This entropy can be created by using the system actively. For eg., moving the mouse around a lot, having a lot of chrome tabs open, etc.
Surely there should be a way to generate randomness by running background processes to simulate high cpu usage and high disk IO. 
After a little googling, I came across the below snippets which I could run parallely in other shells while the gpg key was being created.

#### 1. Read/Write to a whole lot of files
Generate IO using any of the below two commands
{% highlight bash %}
$ (find / | xargs file) &> /dev/null &
$ dd if=/dev/zero of=/dev/null
{% endhighlight %}

#### 2. Increase CPU utilization
Run the command as many times as the number of cores
{% highlight bash %}
$ yes > /dev/null &
$ killall yes
{% endhighlight %}

#### 3. Using RNG
Install rng-tools based on the system you are running and launch it.
{% highlight bash %}
$ sudo apt-get install rng-tools
$ sudo yum install rng-tools
$ sudo rngd -r /dev/urandom
{% endhighlight %}

Once the gpg key has been generated kill any of these commands if you have pushed them to the background.
