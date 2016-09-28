---
published: true
title: JOYENT DOCKER CONTAINER
layout: post
author: jungwook
category: Infra
tags:
- joyent
---

### Docker containers

Every data center in Triton (e.g us-east-1) is an easy to manage, elastic Docker host, while delivering enterprise grade networking and security to each Docker container.

There\`s no need to worry about Docker host. Just pull and deploy Docker images to the joyent data center of your choice. Use the joyent portal or the Docker client and CLI.

### Docker Installation on Ubuntu instance

I refer [doc.docker.com site](https://docs.docker.com/engine/installation/linux/ubuntulinux/) for this installation.

**Prerequisites**

Docker requires a 64-bit installation regardless of your Ubuntu version. Additionally, your kernel must be 3.10 at minimum. The latest 3.10 minor version or a newer maintained version are also acceptable.

Kernels older than 3.10 lack some of the features required to run Docker containers. These older versions are known to have bugs which cause data loss and frequently panic under certain conditions.

To check your current kernel version. open a terminal and use `uname -r` to display your kernel version:

```{.bash}
$ uname -r
3.11.0-15-generic
```

> Note: if you previously installed Docker using APT, make sure you update your APT sources to the new Docker repository.

**Update your apt sources**

Docker\`s `APT` repository contains Docker 1.7.1 and higher. To set `APT` to use packages from the new repository:

1. Log into your machine as a user with `sudo` or `root` privileges.
2. Open a terminal windows.
3. Update packge information, ensure that APT works with the `https` method, and that CA certificates and installed

   ```{.bash}
   $ sudo apt-get update
   $ sudo apt-get install apt-transport-https ca-certificates 
   ```

4. Add the new `GPG` key

   ```{.bash}
   $ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
   ````

5. Open the `/etc/apt/sources.list.d/docker.list` file in your favorite editor. If the file doesn\`t exist, create it.
6. Remove any existing entries.
7. Add an entry for your Ubuntu operating system.  
   The possible entries are:
   + On Ubuntu Precise 12.04[LTS]  

   ```{.bash}
   deb https://apt.dockerproject.org/repo ubuntu-precise main
   ```

   + On Ubuntu Precise 12.04[LTS]  

   ```{.bash}
   deb https://apt.dockerproject.org/repo ubuntu-trusty main
   ```

   + check another case in docker website.
8. Save and close the `/etc/apt/sources.list.d/docker.list` file.
9. Update the `ATP` package index. 

   ```{.bash}
   $ sudo apt-get update
   ```

9. Purge the old repo if it exists.

   ```{.bash}
   $ sudo apt-get purge lxc-docker
   ```

9. Verify that `APT` is pulling from the right repository.

   ```{.bash}
   $ apt-cache policy docker-engine
   ```

From now on when you run `apt-get upgrade`, `APT` pulls from the new repository.

**Prerequisites by Ubuntu version**

+ Ubuntu Xenial 16.04[LTS]
+ Ubuntu Wily 15.10
+ Ubuntu Trusty 14.04[LTS]

For Ubuntu Trusty, Wily, and Xenial. it\`s recomanded to install the `linux-image-extra-*` kernel packages. The `linux-image-extra-*` packages allows you use the aufs storage driver.

To install the `linux-image-extra-*` packages:

1. Open a terminal on your Ubuntu host.

2. Update your package manager.

   ```{.bash}
   $ sudo apt-get update
   ```

3. Install the recommanded packages.

   ```{.bash}
   $ sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
   ```

4. Go ahead and install Docker.

Note. When I run this command on joyent infra, ubuntu image don\`t have correct kernel version numbering. So it tried to update all kernel versions. Package size up to max storage size. In my opinion, don\`t need to do this.
