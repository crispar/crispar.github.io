---
published: true
title: How to load verious distribution linux on Docker Host
layout: post
author: jungwook
category: Container
tags:
- Docker
-
---
Reference Page : [wiki](https://en.wikipedia.org/wiki/Linux_kernel), [ㅍㅍㅋㄷ](http://bluese05.tistory.com/10)

### **Linux의 구성**

Linux는 크게 **kernel space**와 **user sapce**로 나뉨.

![Linux 구성]("https://crispar.github.io/images/Screenshot_20161001-064103.jpg","Linux structure") {.aligncenter}

**user space**
+ 흔히 userland라고 불리는 공간으로, application이 실행되는 공간.
+ Application이 실행되는데 필요한 **library**나 **system daemon**들도 user space영역임.

**kernel space**
+ 직접적으로 Hardware 리소스를 제어하고 관리하는 영역. ( 사용자가 임의 접근 할 수 없음. )
+ 하지만 application이 동작하기 위해서는 이것이 실행되기 위한 hardware 리소스를 할당받아야함. 이때 리소스 할당 요청을 kernel로 하게 되어 있음. 해당 interface를 우리는 system call라고 부름.

실제로 Linux에서 중요한 부분은 Kernel space임. 그것은 OS가 하는 일이 단순히 말하자면, CPU, 메모리 그리고 파일 같은 자원을 효율적으로 관리하는 시스템이기 때문임.

### **Linux distribution**

+ Linux의 배포판 종류는 매우 다양함.
+ 결과적으로 해당 배포판 간의 인과성은 동일한 Kernel을 사용한다는 점으로 명령 체계, 패키지 관리 방식 및 UI가 다르지만 Linux라고 묶을 수 있는 이유가 바로 이것 때문임.

각 리눅스 배포판은 해당하는 Foundation에서 관리하지만, kernel은 [Linux Foundation](https://www.kernel.org)에서 관리하고 있다.

### **Docker host가 다양한 Container를 실행할 수 있는 이유 - namespace


Docker에 Host OS의 배포판과 무관하게 다양한 Container를 올릴수 있는 이유는 namespace때문이다.

![Docker blog](이미지주소 "이미지제목") {.aligncenter}

Docker의 Container구현에는 다양한 Linux 기반 기술들이 들어 있는데, 이 중 namespace가 핵심 기술 중 하나임.

+ Linux Name Space는 하나의 시스템을 마치 독립된 시스템 공간으로 구성되도록 격리 시키는 가상화 기술

+ Linux 영역중 user space를 격리시키는 기술

+ 완전히 분리된 환경처럼 보이지만, 실제로는 user space영역을 가상화 시켜 분리한 기술이므로 동일한 kernel space를 사용하게 됨.

+ 기존의 Hypervisor 기술과는 완전히 다름.

각각의 리눅스 배포판은 대부분이 동일한 Linux kernel 위에 다양한 package management system이 user space에 구성된 것임. namespace는 kernel 영역은 공유하고, user space를 격리하는 것이므로 이를 이용해 충분히 다양한 배포판을 제공할 수 있다.

+ Docker는 각 container들이 kernel을 공유.

+ Docker containersms Docker host의 kernel에 완전히 의존적임.

+ Docker host에 어떠한 kernel 레벨의 작업을 하게 되면, 모든 Container가 영향을 받음.

+ Docker Host는 비교적 최신 기술이 들어감. 따라서 최근 kernel버전 만을 지원함. ( 3.8 이상 ), Container의 경우에도 최근 kernel버젼을 사용하는 배포판만을 지원. *(=> Why? kernel은 하위 호환성을 가지고 있지 않은가? )*

## Linux Name space

<a href="#LNS">1. Linux namespace</a>
<a href="#LNS-UTS">2. Linux namespace-UTS</a>
<a href="#LNS-IPC">3. Linux namespace-IPC</a>
<a href="#LNS-PID">4. Linux namespace-PID</a>
<a href="#LNS-NS">5. Linux namespace-NS</a>
<a href="#LNS-NET">6. Linux namespace-NET</a>

### <a name='LNS'>Linux namespace</a>

+ Namespace는 lightweight 가상화 기술로 하나의 system에서 수행되지만, 각각이 별개의 독립된 공간인 것 처럼 격리된 환경을 제공

+ Docker와 LXC가 모두 namespace기반의 가상화 기술

+ Hardware Resource 자체를 가상화하는 Hypervisor와는 구조적으로 다름.

Linux Name Space는 크게 6가지로 분류됨.

1. UTS namespace : hostname을 변경하고 분할
2. IPC namespace : IPC 프로세스간 통신 격리
3. PID namespace : PID를 분리 관리
4. NS namespace : File System의 mount 지점을 분할하여 격리
5. NET namespace : Network Interface, iptable등의 networ 리소스와 관련된 정보를 분할.
6. USER namespace : user와 group id를 격리

Linux namespace를 구현할 때, clone()나 unshare() 혹은 setns()라는 systemcall을 이요하여 만듬. systemcall을 사용할 때는 위의 6가지에 해당되는 flag를 옵션으로 추가하여 생성함.

```{.bash}
ubuntu@ip-192-168-0-7:~$ cat /usr/include/linux/sched.h | grep CLONE_NEW
#define CLONE_NEWNS	0x00020000	/* New namespace group? */
#define CLONE_NEWUTS		0x04000000	/* New utsname group? */
#define CLONE_NEWIPC		0x08000000	/* New ipcs */
#define CLONE_NEWUSER		0x10000000	/* New user namespace */
#define CLONE_NEWPID		0x20000000	/* New pid namespace */
#define CLONE_NEWNET		0x40000000	/* New network namespace */
ubuntu@ip-192-168-0-7:~$ 
```

해당 flag는 `/usr/include/linux/sched.h`에 정의되어 있음.

Linux namespace를 생성하는 clone(), unshare(), setns()에 대한 사용법은 man 페이지에서 자세히 확인이 가능함.

\- clone() : <http://man7.org/linux/man-pages/man2/clone.2.html>

```{.bash}
#include <sched.h>

int clone(int (*fn) (void *), void *child_stack, 
                int flags, void *arg, ...
               /* pid_t *ptid, struct user_desc *tls, pid_t *ctid */ );
```

\- unshare() : <http://man7.org/linux/man-pages/man2/unshare.2.html >

```{.bash}
#include <sched.h>

int unshare (int flags);
```

\- setns() : <http://man7.org/linux/man-pages/man2/setns.2.html >

```{.bash}
#include <sched.h>

int setns(int fd, int nstype);
```

