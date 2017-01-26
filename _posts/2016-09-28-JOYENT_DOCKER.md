---
published: true
title: JOYENT DOCKER CONTAINER
layout: post
author: jungwook
category: Cloud Infra
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

   + On Ubuntu Precise 14.04[LTS]  

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


**Install Docker**

Make sure you have installed the prerequisites for your Ubuntu version.

Then, install Docker using the following:

1. Log into your Ubuntu installation as a user with sudo privileges.

2. Update your APT package index.

   ```{.bash}
   $ sudo apt-get update
   ```

3. Install Docker

   ```{.bash}
   $ sudo apt-get install docker-engine
   ```

4. Start the `docker` daemon.

   ```{.bash}
   $ sudo service docker start
   ```

5. Verify `docker` is installed correctly.

   ```{.bash}
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints an informational message. Then, it exits.

> Note. there are some optional configurations. So I can refer <https://docs.docker.com/engine/installation/linux/ubuntulinux/> if I have time.

**Configure Docker for Triton**

Configuring Docker for Triton requires the `Triton CLI tool`. With that installed, you can connect Docker on your laptop to the Triton cloud using the `triton env` command in Triton CLI version 4.9 or newer:

>**Triton CLI tool and CloudAPI**
>
>The Triton CLI tool uses CloudAPI to manage infrastructure in Triton data centers. Many of the tasks that you can perform through the portal are also possible through the triton CLI tool and CloudAPI, including:
>
>+ Provision and manage compute instances, including infrastructure containers and hardware virtual machines
>+ Provision and manage networks
>+ Manage your account
>+ More
>
>In this page, you will learn how to install the triton command line tool. You can learn more about CloudAPI methods and resources here.
>
>Need a visual reference? Watch the screencast which covers how to install the Triton CLI tool and use CloudAPI to manage infrastructure in Triton data centers.
>
>**Installation**
>
>The CloudAPI tools require Node.js. You can find the latest version of Node.js for your operating system and architechture at nodejs.org.
>
> [nodejs and npm installation](http://programmingsummaries.tistory.com/374)
>
>>```{.bash}
>># 데비안 계열(우분투)에서 일반 사용자 계정인 경우,
>>$ sudo apt-get install curl
>>$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
>>$ sudo apt-get install -y nodejs
>>
>># 데비안 계열(우분투)에서 루트인 경우,
>>$ apt-get install curl
>>$ curl -sL https://deb.nodesource.com/setup_6.x | bash -
>>$ apt-get install -y nodejs
>>```
>
> Only difference between generic user and root user is sudo command in front of command
>


Once Node.js is installed, you can use `npm` to install the `triton` CLI tool:

```{.bash}
$ sudo npm install -g triton
. . .
/usr/local/bin/triton -> /usr/local/lib/node_modules/triton/bin/triton
triton@4.11.0 /usr/local/lib/node_modules/triton
├── bigspinner@3.1.0
├── assert-plus@0.2.0
├── extsprintf@1.0.2
├── wordwrap@1.0.0
├── strsplit@1.0.0
├── node-uuid@1.4.3
├── read@1.0.7 (mute-stream@0.0.6)
├── semver@5.1.0
├── vasync@1.6.3
├── once@1.3.2 (wrappy@1.0.2)
├── backoff@2.4.1 (precond@0.2.3)
├── verror@1.6.0 (extsprintf@1.2.0)
├── which@1.2.4 (isexe@1.1.2, is-absolute@0.1.7)
├── cmdln@3.5.4 (extsprintf@1.3.0, dashdash@1.13.1)
├── lomstream@1.1.0 (assert-plus@0.1.5, extsprintf@1.3.0, vstream@0.1.0)
├── mkdirp@0.5.1 (minimist@0.0.8)
├── sshpk@1.7.4 (ecc-jsbn@0.1.1, jsbn@0.1.0, asn1@0.2.3, jodid25519@1.0.2, dashdash@1.13.1, tweetnacl@0.14.3)
├── rimraf@2.4.4 (glob@5.0.15)
├── tabula@1.7.0 (assert-plus@0.1.5, dashdash@1.13.1, lstream@0.0.4)
├── smartdc-auth@2.3.0 (assert-plus@0.1.2, once@1.3.0, clone@0.1.5, dashdash@1.10.1, sshpk@1.7.1, sshpk-agent@1.2.0, vasync@1.4.3, http-signature@1.1.1)
├── restify-errors@3.0.0 (assert-plus@0.1.5, lodash@3.10.1)
├── bunyan@1.5.1 (safe-json-stringify@1.0.3, mv@2.1.1, dtrace-provider@0.6.0)
└── restify-clients@1.1.0 (assert-plus@0.1.5, tunnel-agent@0.4.3, keep-alive-agent@0.0.1, lru-cache@2.7.3, mime@1.3.4, lodash@3.10.1, restify-errors@4.2.3, dtrace-provider@0.6.0)
```


**Configuration**

The `triton` CLI uses "profiles" to store access information. Profiles contain the data center CloudAPI URL, your login name, and SSH key fingerprint so that you can switch between them conveniently. Profiles make it easy to connect to different data centers, or connect to the same data center as different users.

The `triton profile create` command steps through a series of questions to make profile setup and configuration easy:

```{.bash}
$ triton profile create

A profile name. A short string to identify a CloudAPI endpoint to the `triton` CLI.
name: us-sw-1

The CloudAPI endpoint URL.
url: https://us-sw-1.api.joyent.com

Your account login name.
account: jill

The fingerprint of the SSH key you have registered for your account. You may enter a local path to a public or private key to have the fingerprint calculated for you.
keyId: ~/.ssh/<ssh key name>.id_rsa
Fingerprint: 2e:c9:f9:89:ec:78:04:5d:ff:fd:74:88:f3:a5:18:a5

Saved profile "us-sw-1"
```

> When you use a SSH key, you need to use proper key. That is important.
>
>**How do I find my RSA key fingerprint?**
>
>```{.bash}
>$ ssh-keygen -lf ~/.ssh/id_rsa.pub
>```
>
>**SSH Key -- Converting PEM to PUB**
>
>1.Copy the private SSL key to ~/.ssh/id_ssl.
>
>```{.bash}
>$ cp ~/.ssh/TTS.pem ~/.ssh/id_ssl
>```
>
>2.Extract the public SSH key using ssh-keygen.
>
>```{.bash}
>ssh-keygen -y -f ~/.ssh/id_ssl > ~/.ssh/id_rsa.pub
>```
>
> Ref.1 : <https://ubuntuforums.org/showthread.php?t=1645352>
> Ref.2 : <http://stackoverflow.com/questions/9607295/how-do-i-find-my-rsa-key-fingerprint>
>

Select a CloudAPI endpoint URL from any of our [global data centers](https://docs.joyent.com/public-cloud/data-centers), or use a Triton-powered data center of your own (remember: it\`s [open source](https://github.com/joyent/sdc)).


To test the installation and configuration, let\`s use `triton info`:

```{.bash}
$ triton info
login: jill
name: Jill Example
email: jill@example.com
url: https://us-sw-1.api.joyent.com
totalDisk: 65.8 GiB
totalMemory: 2.0 GiB
instances: 2
running: 2
```

The `triton info` output above shows that Jill\`s account already has two instances running.

You can change the default profile with the `triton profile set` command:

```{.bash}
$ triton profile set us-east-1
Set "us-east-1" as current profile
```

**Configure Docker for Triton**

Configuring Docker for Triton requires the [Triton CLI tool](https://docs.joyent.com/public-cloud/api-access/cloudapi). With that installed, you can connect Docker on your laptop to the `Triton cloud` using the triton env command in Triton CLI version 4.9 or newer:

```{.bash}
eval $(triton env)
```

That command sets all the environment variables for Docker and Triton, including:

```{.bash}
export TRITON_PROFILE="us-sw-1"
export DOCKER_CERT_PATH=/Users/jill/.triton/docker/jill@us-sw-1_api_joyent_com
export DOCKER_HOST=tcp://us-sw-1.docker.joyent.com:2376
export DOCKER_TLS_VERIFY=1
export COMPOSE_HTTP_TIMEOUT=300
export SDC_URL="https://us-sw-1.api.joyent.com"
export SDC_ACCOUNT="jill"
unset SDC_USER
export SDC_KEY_ID="SHA256:bTh6bXMQAD2su4YOniwXsK/03Gzx8zD/hZK+X8oKUnQ"
unset SDC_TESTING
```

You can use [Triton CLI profiles](https://docs.joyent.com/public-cloud/api-access/cloudapi#using-profiles) for different data centers and different users. You can switch between profiles by specifying the profile name, as in: `triton env <profile name>`.


To test Docker, you can run docker info and see your account name in the output. We can see `jill`, our example user, in `SDCAccount: jill` here:

```{.bash}
$ docker info
Containers: 0
Images: 0
Storage Driver: sdc
 SDCAccount: jill
Execution Driver: sdc-0.3.0
Logging Driver: json-file
Kernel Version: 3.12.0-1-amd64
Operating System: SmartDataCenter
CPUs: 0
Total Memory: 0 B
Name: us-sw-1
ID: 65698e31-2754-4e86-9d05-bfc881037812
```

If your Triton profile was created before Triton CLI version 4.9, you\`ll have to update it for Docker:

```
triton profile docker-setup <profile name>
```

Please see the [Docker troubleshooting page](https://apidocs.joyent.com/docker/troubleshooting) or [contact support](https://docs.joyent.com/public-cloud/getting-started/support) if you encounter any difficulty.

Reference infomation from <https://docs.joyent.com/public-cloud/api-access/docker#configure-docker-for-triton>

