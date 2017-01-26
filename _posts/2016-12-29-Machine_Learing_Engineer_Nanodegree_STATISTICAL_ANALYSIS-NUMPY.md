---
published: true
title: Machine Learning - STATISTICAL ANALYSIS 02
layout: post
author: jungwook
category: Machine Learning
tag:
- machine learning
- statistical ANALYSIS
- numpy
---
[How to Install Python 2.7.12 on Ubuntu](http://tecadmin.net/install-python-2-7-on-ubuntu-and-linuxmint/#)
[pyenv-설치하기](http://yujuwon.tistory.com/entry/pyenv-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)

## How to setup python?

기본적으로 python의 버젼은 2.7.12와 3.x대 버젼 두가지가 있다. 두 버젼에 따른 문법의 차이가 있기 때문에 2.7.12 버젼으로 작성된 코드를 3.x대 버젼에서 실행이 안될수도 있다. 따라서 멀티 환경을 구축하는게 좋은다. 해당 버젼을 오가기 위해서 다음과 같이 진행하면 된다.

### 1. python 2.7 부터 설치

+ 필요 패키지 설치

```{.bash}
$ sudo apt-get install build-essential checkinstall
$ sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
```

+ python 2.7.12 설치
		
```{.bash}
$ cd /usr/src
$ wget https://www.python.org/ftp/python/2.7.12/Python-2.7.12.tgz
```

해당 폴더에 대한 권한이 필요하므로 sudo 를 줄 것!

+ Compile Python Source

```{.bash}
$ cd Python-2.7.12
$ sudo ./configure
$ sudo make altinstall
```

altinstall은 기본 파이썬 바이너리 파일 / usr / bin / python을 바꾸는 것을 막기 위해 사용됨.




### 2. 가상환경 설정을 위한 pyenv 설치

pyenv는 apt-get이나 pip로 받을 수는 없고, git을 통해 받아야함.

```{.bash}
$> sudo apt-get install curl git-core
```

실행은 반드시 루트 권한으로 실행되어야 함.

```{.bash}
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```	

설치 중에 아래와 같은 경고 문구가 나올 수 있음.

>Checking connectivity... done.
>
>WARNING: seems you still have not added 'pyenv' to the load path.
>
> \# Load pyenv automatically by adding
>
> \# the following to ~/.bash_profile:

로드 path를 잡아달라는 말이므로,

```{.bash}
export PYENV_ROOT="${HOME}/.pyenv"

if [ -d "${PYENV_ROOT}" ]; then
   	export PATH="${PYENV_ROOT}/bin:${PATH}"
   	eval "$(pyenv init -)"
fi
```

위의 내용을 넣어주자. 그리고

```{.bash}
source .bashrc
```

위의 명령어로 .bashrc를 재로드한다.

그리고 pyenv가 최신버젼인지 확인하는 차원에서 업데이트 한다.

```{.bash}
$> pyenv update
```

그리고 지원하는 버젼을 확인하기 위해서

```{.bash}
$> pyenv install --list 
```

현재 최신 버젼이 2.x대는 2.7.12 그리고 3.x대는 3.5.2 이므로 아래와 같은 커멘드로 두개의 버젼을 받아본다.

```{.bash}
$> pyenv install 2.7.12
$> pyenv install 3.5.2	
```

설치가 완료되면 다음과 같은 형식으로 python버젼을 전환할 수 있다

```{.bash}
$> pyenv shell 2.7.12
$> pyenv shell 3.5.2	
```

이외에도 가상환경을 설정할 수가 있는데,

```{.bash}
$> pyenv virtualenv 2.7.12 [가상환경이름] 
```	

위와 같이 설정하여 셋업이 되면,

```{.bash}
$> source /home/ubuntu/.pyenv/versions/mypython3/bin/activate
```

기존 virtualenv와 같이 activation을 한 후 사용하고

deactivate를 하면 설정이 해제된다.
