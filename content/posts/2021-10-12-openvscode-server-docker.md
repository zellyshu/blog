---

title: "OpenVSCode Server를 Docker에 설치해보기"
date: 2021-10-12 15:32:51
tags: ["Docker", "OpenVSCode"]
categories: ["Docker"]
author: "Me"
draft: false

---

## Intro

OpenVSCode Server를 띄워두고 태블릿 PC등으로 외부접속하면 테스트 목적으로 간단하게 사용할 수 있지 않을까? 라는 생각이 갑자기 들어 Docker 환경에서 우선 설치해보기로 하였다.


## OpenVSCode Server란?

[OpenVSCode Server](https://github.com/gitpod-io/openvscode-server)란 원격 컴퓨터에서 서버를 실행하고, 웹 브라우저를 통해 접근할 수 있는 VSCode 버전을 제공하는 것으로, Micorsoft사에서 오픈소스 프로젝트로 개발하고 있다.


## Installation

설치는 Docker나 Linux 환경에서 설치 가능하며, Docker 환경에서 설치를 진행하였다. 사실 명령어 하나만 입력하면 설치가 진행되므로, 오류가 안나기만을 간절히 기도하였다...

```bash
 docker run -it --init -p 3000:3000 -v "$(pwd):/home/workspace:cached" gitpod/openvscode-server
```

![2021-10-12-openvscode-server-docker02.png](/img/2021-10-12-openvscode-server-docker02.png)

설치 및 설정이 완료되면 3000번 포트를 통해 접속할 수 있다.

![2021-10-12-openvscode-server-docker04.png](/img/2021-10-12-openvscode-server-docker04.png)

접속하면, 테마 등을 커스터마이징 할 수 있다. Dracula 테마를 검색하여 적용하였더니 다음과 같이 잘 나오는 것을 확인할 수 있었다. 

![2021-10-12-openvscode-server-docker05.png](/img/2021-10-12-openvscode-server-docker05.png)


## 후기

기존 컴퓨터에 설치되어 있는 VSCode의 익스텐션들을 이것저것 OpenVSCode Server 환경에서도 설치해보았다. VSCode를 사용하는 것처럼 위화감과 같은 불편함은 느껴지지 않았다. 다만, Python의 경우 설치 과정 중 접근 권한이 꼬이는지 설치가 자꾸 에러가 났다. 차후 Linux 환경에서 한 번 더 차근차근 설정해보아야 할 것 같다...



## Reference

- https://github.com/gitpod-io/openvscode-server
- https://www.gitpod.io/blog/openvscode-server-launch
