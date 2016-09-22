shed: true
title: How to check linux account lists
layout: post
author: jungwook
category: bash
tags:
- Linux
- Bash
---

[참고 사이트 : <http://bit.ly/2cwZKDV>](http://bit.ly/2cwZKDV)

## 목차

1. <a href="#AllList">전체 목록</a>
1. <a href="#bashuser">bash 사용자 목록</a>
1. <a href="#genericuser">일반 사용자 목록</a>
   + 3.1 <a href="#overuid500">UID 500 이상</a>
   + 3.2 <a href="#overuidmin">UID_MIN 이상</a>
1. <a href="#bashwithgenericuser">bash 및 일반 사용자 계정 목록</a>
1. <a href="#comment">주석</a>

## <a name='AllList'>전체 목록</a>
**명령어**
>cat /etc/passwd
>cat -f1 -d: /etc/passwd

**예시**

>```bash
>jekyll@ip-192-168-0-7:~$ cat -n /etc/passwd | tail -n 10
>    32	kernoops:x:112:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
>    33	pulse:x:113:121:PulseAudio daemon,,,:/var/run/pulse:/bin/false
>    34	rtkit:x:114:123:RealtimeKit,,,:/proc:/bin/false
>    35	saned:x:115:124::/home/saned:/bin/false
>    36	whoopsie:x:116:125::/nonexistent:/bin/f>alse
>    37	speech-dispatcher:x:117:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/sh
>    38	hplip:x:118:7:HPLIP system user,,,:/var/run/hplip:/bin/false
>    39	python:x:1001:1001::/home/python:/bin/bash
>    40	xrdp:x:119:126::/var/run/xrdp:/bin/false
>    41	jekyll:x:1002:1002::/home/jekyll:/bin/bash
>jekyll@ip-192-168-0-7:~$ 
>
>```
>```bash
>jekyll@ip-192-168-0-7:~$ cut -f1 >-d: /etc/passwd | tail -n 10
>kernoops
>pulse
>rtkit
>saned
>whoopsie
>speech-dispatcher
>hplip
>python
>xrdp
>jekyll
>jekyll@ip-192-168-0-7:~$ 
>```
## <a name='bashuser'>bash 사용자 목록</a>

useradd로 계정을 만들면 기본적으로 /bin/bash 환경이 적용된다. bash 사용자 목록이 의미가 있는 경우가 많다.

**명령어**
>grep /bin/bash /etc/passwd
>grep /bin/bash /etc/passwd | cut -f1 -d:

**예시**
>```bash
>jekyll@ip-192-168-0-7:~$ grep /bin/bash /etc/passwd
>root:x:0:0:root:/root:/bin/bash
>ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
>python:x:1001:1001::/home/python:/bin/bash
>jekyll:x:1002:1002::/home/jekyll:/bin/bash
>jekyll@ip-192-168-0-7:~$ 
>```


