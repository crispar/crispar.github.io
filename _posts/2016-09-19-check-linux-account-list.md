---
published: true
title: How to check linux account lists
layout: post
author: jungwook
category: Bash
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

>```bash
>cat /etc/passwd
>cat -f1 -d: /etc/passwd
>```

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

>```bash
>grep /bin/bash /etc/passwd
>grep /bin/bash /etc/passwd | cut -f1 -d:
>```

**예시**

>```bash
>jekyll@ip-192-168-0-7:~$ grep /bin/bash /etc/passwd
>root:x:0:0:root:/root:/bin/bash
>ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
>python:x:1001:1001::/home/python:/bin/bash
>jekyll:x:1002:1002::/home/jekyll:/bin/bash
>jekyll@ip-192-168-0-7:~$ 
>```
>
>```bash
>jekyll@ip-192-168-0-7:~$ grep /bin/bash /etc/passwd | cut -f1 -d:
>root
>ubuntu
>python
>jekyll
>jekyll@ip-192-168-0-7:~$
>```
>

## <a name='genericuser'>일반 사용자 목록</a>

useradd 명령어로 생성되는 일반 사용자 계정은 UID가 500 이상이다.[^1]

## <a name='overuid500'>UID 500 이상</a>

**명령어**

>```bash
>awk -F':' '{if($3>500)print $1}' /etc/passwd
>```

**실행예시**

>```bash
>jekyll@ip-192-168-0-7:~$ awk -F':' '{if($3>=500)print $1}' >/etc/passwd
>nobody
>ubuntu
>python
>jekyll
>jekyll@ip-192-168-0-7:~$
>```

## <a name='overuidmin'>UID_MIN 이상</a>

**명령어**

>```bash
>u1=$(grep "^UID_MIN" /etc/login.defs | awk '{print $2}')
>u2=$(grep "^UID_MAX" /etc/login.defs | awk '{print $2}')
>awk -F':' -v "u1=$u1" -v "u2=$u2" '{ if ( $3>=u1 && $3<=u2 ) print $0}' /etc/passwd
>awk -F':' -v "u1=$u1" -v "u2=$u2" '{ if ( $3>=u1 && $3<=u2 ) print $1}' /etc/passwd
>```

**실행예시**

>```bash
>jekyll@ip-192-168-0-7:~$ awk -F':' -v "u1=$u1" -v "u2=$u2" '{ if ( $3>=u1 && $3<=u2 ) print $1}' /etc/passwd
>ubuntu
>python
>jekyll
>```

## <a name='bashwithgenericuser'>bash 및 일반 사용자 계정 목록</a>

**명령어**

>```bash
>a=$(grep ^UID_MIN /etc/login.defs | awk '{print $2}')
>b=$(grep ^UID_MAX /etc/login.defs | awk '{print $2}')
>c=$(grep /bin/bash /etc/passwd | awk -F':' '{print $1}')
>d=$(awk -F':' -v "a=$a" -v "b=$b" '{ if ( $3>=a && $3<=b ) print $1}' /etc/passwd)
>echo -e "$c\n$d" | sort | uniq
>```

**실행예시**

>```bash
>jekyll@ip-192-168-0-7:~$ a=$(grep ^UID_MIN /etc/login.defs | awk '{print $2}')
>jekyll@ip-192-168-0-7:~$ b=$(grep ^UID_MAX /etc/login.defs | awk '{print $2}')
>jekyll@ip-192-168-0-7:~$ c=$(grep /bin/bash /etc/passwd | awk -F':' '{print $1}')
>jekyll@ip-192-168-0-7:~$ d=$(awk -F':' -v "a=$a" -v "b=$b" '{ if ( $3>=a && $3<=b ) print $1}' /etc/passwd)
>jekyll@ip-192-168-0-7:~$ echo -e "$c\n$d" | sort | uniq
>jekyll
>python
>root
>ubuntu
>jekyll@ip-192-168-0-7:~$
>```

[^1]: /etc/login.defs의 UID_MIN 이 500임(기본값). 수세 리눅스에서는 1000

