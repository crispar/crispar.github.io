---
published: true
title: Docker on Windows Server 2016
athuor: jungwook
layout: post
category: Article
tag:
- Docker
---

해당 글은 다음 링크의 번역 글임.

[https://lostechies.com/gabrielschenker/2015/08/19/docker-on-windows-server-2016/](https://lostechies.com/gabrielschenker/2015/08/19/docker-on-windows-server-2016/)

기사 일시 : 2015.08.19

오늘 Docker Team은 Windows Server 2016을 위한 Docker-engine의 기술적인 맛보기가 이용 가능함을 발표했습니다. 이것은 우리와 Windows 플래폼 상에서 주로 일하는 우리와 모든 소프트웨어 팀에게 매우 큰 의미가 있습니다.

Windows를 위한 Docker Engine은 Linux counterpart와 마찬가지로 open source입니다. 이것은 어떻한 가상화를 사용하지 않아서, 리눅스를 사용하는 것이 아니라 윈도우 상에서 자연스럽게 돌고 이러한 운영 체제를 컨테이너로 추상화합니다. 이것을 가능하도록 하기위해 윈도우즈 코어팀은 리눅스에서 namespaces와 cgroups로 알려진 것과 동일한 기능을 구현했습니다.

이것이 무엇을 의미하는 걸까요? 우리는 현존하거나 미래에 윈도우즈를 기반으로 돌아가는 어플리케이션 모두를 컨테이너화 할 수 있게 되었다는 것을 의미합니다.

저는 Docker team과 이것을 가능하도록 한 마이크로 소프트에 있는 사람들에게 찬사를 보냅니다.
