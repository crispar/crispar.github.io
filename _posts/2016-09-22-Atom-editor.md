---
published: true
title: use atom editor
layout: post
author: jungwook
category: editor
tags:
- Editor
- Atom
---

블로그 질좀 할려고 Markdown Editor가 필요했는데, 윈도우즈 에디터 중에 내가 찾아본 것 중에는 정상적으로 동작하는 케이스가 별로 없었던거 같다. Markdown Pad는 preview기능이 동작을 안하고 ( 다른 버젼에선 모르겠고 Windows 10 에서는 안됨.), 막상 단순 에디터임에도 불구하고 단독에디터를 쓴다는게 약간 비효율적인것 같아서 구글링을 하다보니까 Github에서 만든 에디터가 있고 패키지 설치를 통한 기능 확장이 가능하다고 한다. 그래서 [Atom Editor](http://bit.ly/2cpNhRy)를 써보기로 했다.

머 에디터 어느게 좋은지 모르겠지만, [특정 블로그](http://bit.ly/2df8GDb)에 보면 장점이 아래와 같다고 한다.

+ 한글 입력에 문제가 없음. ( ㅡ.ㅡ sublime text는 문제가 있다고 하긴했는데 안겪어봐서 모르겠다.)
+ Github와의 연동이 잘되어 있다. ( 오호...리얼? )
+ 충분한 각종 패키지 툴이 있다. ( 현재 2,800여개 정도 된다고 한다.)
+ Node.js를 이용한 웹개발에는 당연히 Node.js에서 만든 프로그램을 사용하는것이 좋다.(Github만든데에서 Node.js 만들었다고?)

암튼 머 그렇다고 한다. ( 이런건 약간 관심외라. 일단 써본다.)

## 1. Atom 설치
별거 없다. [Atom site](http://bit.ly/2cpNhRy)에 가서 그냥 패키지 받고 설치하면 된다. 복잡한 단계가 있는건 아니므로 그냥 깔고 실행하면 된다. 다만, 프록시 환경에서는 추가적인 설정[^1]이 좀 있어야한다. 왜냐하면 Atom Editor의 장점이 pakage를 활용한 기능확장인데 proxy환경에서 proxy세팅을 하지 않으면, install메뉴에서 package들이 정상적으로 로드되지 않아 추가 패치키 설치가 안된다.

## 2. Atom Markdown Enhanced 설치
일단, 주 목적이 Markdown 편집기로 쓸려고 설치한 거니까 추가 패키지를 설치해 봤다. File->Settings에서 Install항목으로 이동한 다음에 serch box에서 그냥 찾아주면 된다. 매우 easy하다.

...

[^1] c:/Users/윈도우계정/.atom/ 경로상에 `.apmrc`라는 파일을 하나 만들고, 아래 내용을 추가함.


>```bash
>http-proxy=http://xx.xx.xx.xx:xxxx/
>http-proxy=http://xx.xx.xx.xx:xxxx/
>```


