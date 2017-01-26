---
published: true
title: How to install jekyll blog on github
layout: post
author: jungwook
category: Tutorial
tags:
- jekyll
---
GITHUB에 Jekyll 블로그 사이트를 개설 했다. 먼가 하고 싶은 것들이 있는데, 하고 난 후에 기억에 잘 남지도 않고 먼가 했다는 기록을 좀 남기는게 좋을 것 다는 생각이 들어서 블로그를 시작해 보려고 한다. 

오늘은 Jekyll 블로그 개설에 대한 내용을 추가할려고 한다.

## **설치해야하는 프로그램**

기본적으로 [Jekyll 한글 사이트](https://jekyllrb-ko.github.io)에 가면 아래와 같은 내용들이 나온다.
 - [Ruby](http://www.ruby-lang.org/en/downloads/) ( Jekyll 2 사용시 v1.9.3 이상,  Jekyll3 사용시 v2이상의 패키지 포함)
 - [RubyGems](http://rubygems.org/pages/download)
 - 리눅스, 유닉스 혹은 맥 OSX
 - [NodeJS](http://nodejs.org/) 또는 다른 JavaScript 실행 환경 ( Jekyll2와 그 이전 버젼에서, CoffeeScript지원에 필요.)
 - [Python 2.7] ( Jekyll 2 나  그 이전 버전일 경우)

> ## **참고**
> 설치하다가 잘 안되서 문서를 이것 저것 찾아보았다. 중요한건 jekyll 최신 버젼을 설치할려고 하면 ( 내 기준은 3.2.1 인듯 ) Ruby 버젼을 v2.0.0 이상으로 하라고 하는데, 설치 했음에도 버젼정보가 이상하게 보이는 일들이 발생했다. 결과적으로 인터넷의 [Jordan Biserkov의 블로그](http://biserkov.com/blog/2016/06/04/Steps-to-install-Jekyll-on-Ubuntu-on-Windows/)를 보고 해결했다. 내용은 다음과 같다.
>
>**Step 0: Add the brightbox repository**
>
```bash
>sudo apt-add-repository ppa:brightbox/ruby-ng
>sudo apt-get update
```
>**Step 1: Install ruby 2.3 and -dev package**
>
```bash
>sudo apt-get install ruby2.3 ruby2.3-dev
```
>Verify the install by running ruby -v
>You should get something similar to
>
```bash
>ruby 2.3.1p112 (2016-04-26 revision 54768)
```
>**Step 2: update ruby gems**
>
```bash
>sudo gem update --system
```
>**Step 3: install build-essential**
>
```bash
>sudo apt-get install build-essential --no-install-recommends
```
>**Step 4: install jekyll itself**
>
```bash
>sudo gem install jekyll
```
>Verify the install by running jekyll -v
>You should get something similar to
>
```bash
>jekyll 3.1.6
```
>**Bonus steps**
>If you’re using pagination:
>
```bash
>sudo gem install jekyll-paginate
```
>To save yourself some typing add the following to your .bashrc
>
```bash
alias jek='jekyll serve --force_polling --incremental'
```
> 해당 내용을 실행하고 나니 정상적으로 루비 버젼이 설치가되고 Gem command의 실행이 가능했다. 

모든 프로그램이 설치가 된 후 jekyll blog의 기본 페이지 생성을 위해 아래 명령을 실행한다.

```bash
jekyll new 폴더명 --force
```
그리고 해당 페이지 서비스의 내부 확인을 위한 구동을 위해 하기 명령을 수행한다.

```bash
jekyll serve
```
> ## **참고**
>내가 `jekyll serve`를 입력했을 때 다음과 같은 에러가 났다. 
>
>`jekyll 3.2.1 | Error:  Invalid argument - Failed to watch "/home/ubuntu/my_git_jekyll/.git/branches": the given event mask contains no legal events; or fd is not an inotify file descriptor.` 
>
>구글에서 해당 에러로 찾아보니, 다음 페이지에
[https://github.com/jekyll/jekyll/issues/5233](https://github.com/jekyll/jekyll/issues/5233) 명령를 실행시에 아래와 같이 실행하면 된다고한다.
>
>`jekyll serve --force_polling`
>
>[jekyll HomePage](http://jekyllrb-ko.github.io/docs/configuration/)에 보면 해당 옵션이 단순히 감시옵션을 강제로 키는 거라고 하는데 정확히 해당 동작에 어떤 영향을 미치는 지는 모르겠다.

그러면 다음과 같이 구동 화면을 볼수 있는 내부 링크를 알려준다.

`http://localhost:4000`

해당 동작이 마무리 되면, github에 해당 코드를 올려야한다. 설정된 파일들이 생성된 폴더에서

```bash
git init
git remote add <repository name> <repository url>
git config --global user.name <username>
git config --global user.email <user email>
git add .
git commit -m "blog init"
git push -u <repository name> master
```
위와 같이 진행한 후에 github의 url인 (userid).github.com를 넣게 되면 해당 페이지가 나온다.

>### 참고 : VM에서 jekyll 구동시 webpage를 찾을 수 없다고 나오는 경우
>
> vagrant로 구동 중인 상태에서 jekyll페이지를 열려고 시도하는 경우 page view가 정상적이지 않은 상황이 발생했다.
>
> 해당 케이스의 경우 명령어 실행을
>```
>$ jekyll serve --host 0.0.0.0
>```
>위와 같이 `--host 0.0.0.0`을 주니까 정상동작했다.
>
