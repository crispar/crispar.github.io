---
published: true
title: How to install Docker?
layout: post
author: jungwook
category: docker
tag:
- docker
- container
---

### 사전확인 내역

최소 3.10 이상의 버젼이어야 정상적으로 Docker운영이 가능.

Proxy 환경의 경우

/etc/environment 파일에

```{.bash}
no_proxy="xxx.xxx.xxx.xxx,localhost"
http_proxy="http://xxx.xxx.xxx.xxx:xxxx/"
https_proxy="http://xxx.xxx.xxx.xxx:xxxx/"
ftp_proxy="ftp://xxx.xxx.xxx.xxx:xxxx/"
socks_proxy="socks://xxx.xxx.xxx.xxx:xxxx/"
```

해당 내역을 추가 ( 환경 변수를 추가함. )


그리고

```{.bash}
sudo apt-key adv --keyserver-options http-proxy=http://xxx.xxx.xxx.xxx:xxxx/ --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
```

인증시 해당 proxy를 넣어준후

```{.bash}
sudo mkdir /usr/share/ca-certificates/name_you_want
sudo cp samsung.crt /usr/share/ca-certificates/name_you_want/certifile.crt
sudo dpkg-reconfig ca-certificates
sudo dpkg-reconfigure ca-certificates
sudo update-ca-certificates
```

Proxy 환경에서 해당 설정을 진행하면 apt를 통한 패키지 정보를 가져오는 것이 가능하다.

>참고 정보 페이지 : [http://bit.ly/2fcv1wC](http://bit.ly/2fcv1wC)
>
>Docker Package를 설치한 후 기동하여 Docker Pull과 같은 동작시에 docker hub로 부터 무엇인가를 다운받기 위해서는 아래와 같은 추가 설정을 해준다.
>
>14.04 기준
>```{.bash}
>/etc/default/docker
>export HTTP_PROXY="http://proxy.your.corp/"
>export HTTPS_PROXY="http://proxy.your.corp/"
>```
>
>최근 리눅스 진영에서는 데몬을 관리하는데 있어 systemd를 사용하는 추세로 움직이고 있음. 그래서 최신 리눅스 버전에서도 이에 맞게 설정해야함.
>
>1.다음과 같이 systemd drop-in directory를 만듬.
>
>```{.bash}
>sudo mkdir /etc/systemd/system/docker.service.d
>```
>
>2.프록시 설정만 override하기 위해 /etc/systemd/system/docker.service.d/http-proxy.conf파일을 작성. ( 파일명은 달라도 .conf와 같은 확장자를 사용해야함.)
>
>```{.bash}
>[Service]
>Environment="HTTP_PROXY=http://proxy.your.corp/" "HTTPS_PROXY=https://proxy.your.corp/" "NO_PROXY=localhost,127.0.0.1,docker-registry.your.corp"
>```
>
>3.설정을 적용함.
>
>```{.bash}
>sudo systemctl daemon-reload
>```
>
>4.Docker 재시작.
>
>```{.bash}
>sudo systemctl restart docker
>```
>

### 기본적인 설치 내역

참고 : [Docker Documentation](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

```{.bash}
# check kernel version
$ uname -r

#update apt source
$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates

# if you are in prpxy environment, you need to add proxy option
$ $ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

# Add APT repository
# if 12.04
deb https://apt.dockerproject.org/repo ubuntu-precise main

# if 14.04
deb https://apt.dockerproject.org/repo ubuntu-trusty main

# if 16.04
deb https://apt.dockerproject.org/repo ubuntu-xenial main

# use below commend
$ echo "<REPO>" | sudo tee /etc/apt/sources.list.d/docker.list

# APT update
$ sudo apt-get update

# verify APT package index
$ sudo apt-cache policy docker-engine

# 14.04 or 16.04 need to install `linux-image-extra-*` packages:
$ sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual

# Install docker 
$ sudo apt-get install docker-engine

# Start Docker service
$ sudo service docker start
```
