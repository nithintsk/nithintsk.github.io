---
title:  "Add sudo user and setup ssh config"
tags: [new user, useradd, wheel, ssh, config, passwd]
style: fill
color: dark
description: Setting up a user with root privileges on a linux machine which can only be accessed through a jumpbox. Also setting up passwordless ssh by modifying the ssh config file on your workstation.
---

### 1. Login in as root into the remote linux machine

We shall create a new user, set its passwd and add to wheel for sudo access

```
$ user add nithintsk
$ passwd nithintsk
$ usermod -aG wheel nithintsk
```

### 2. Modify your ssh config file on the local machine

Edit your ssh config file at ~/.ssh/config (Create a new one if you have to)

```
# Jump host
Host jump-box
    HostName jump-box.osu.edu
    User ntihintsk
    ForwardAgent yes
    IdentityFile ~/.ssh/id_rsa_PERSONAL

# Remote VM
Host remote-vm
    HostName 192.168.5.106
    ProxyJump jump-box
    User nithintsk
    IdentityFile ~/.ssh/id_rsa_PERSONAL
```

### 3. Enable passwordless ssh

Generate a new ssh key if you don't already have one

```
$ ssh-keygen
```

Copy over the key to your jump box and remote serve

```
$ ssh-copy-id jump-box
$ ssh-copy-id remote-vm
```

---
