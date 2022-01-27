---
title: "Rancher Desktop 1.0으로 Docker Desktop 대체해보기"
date: 2022-01-27 16:28:49
creation date: 2022-01-27 13:46
modification date: 2022-01-27 16:28
tags: ["Rancher Desktop", "Docker Desktop", "Kubernetes", "Container Management", "OpenSource"]
categories: ["Rancher Desktop", "Docker Desktop", "OpenSource"]
author: "Me"
draft: false

---

## Intro

Docker Desktop의 유료화 소식을 지난해 9월에 듣고 대체재를 찾아보다 결국 포기한 적이 있었다. 하지만 메일로 22년 1월 31일까지 유료 구독에 등록해야 Docker Desktop을 사용할 수 있다는 이야길 보고 정말 이젠 대체재를 찾아야겠단 생각이 들었다. 그러던 중 [Rancher Desktop](https://rancherdesktop.io/) 이란 오픈소스 프로젝트([Apache-2.0 License](https://github.com/rancher-sandbox/rancher-desktop/blob/main/LICENSE))를 알게 되었고, 1.0.0 ver이 22년 1월 27일 release 되자마자 바로 다운로드 받아보았다.

## 

## Rancher Desktop

![img-shadow](https://rancherdesktop.io/images/how-it-works-rancher-desktop.svg)

<center>Rancher Desktop 작동 원리</center>

[Electron](github.com/electron/electron)으로 구현되어서 Windows, MacOS, Linux 에서 사용 가능한 것이 특징이며, 특히 Mac의 경우 Intel 과 Silicon Mac 모두 지원하고 있다. 컨테이너 관리뿐만 아니라 간편하게 쿠버네티스 환경을 만들 수 있다는 것을 장점으로 내세우고 있으며, [Docs](https://docs.rancherdesktop.io/)에서 자세한 내용을 확인할 수 있다.



특히 간단하게 Docker 이미지 관리 정도만 하려 하는 나에게 있어 더할 나위 없이 툴이라고 생각했기 때문에 바로 설치를 진행했다.



##

# Installation

홈페이지에서 운영체제에 맞는 설치파일을 다운로드 받으면 된다. 설치 요구조건은 다음과 같다.

### System Requirements

| **OS**  | **Memory** | **CPU** |
| ------- | ---------- | ------- |
| macOS   | 8GB        | 4CPU    |
| Windows | 8GB        | 4CPU    |
| Linux   | 8GB        | 4CPU    |

| **OS**   | **Version**               |
| -------- | ------------------------- |
| macOS    | Catalina 10.15 or higher  |
| Windows  | Home build 1909 or higher |
| Ubuntu   | Ubuntu 20.04 or higher    |
| openSUSE | Leap 15.3 or higher       |
| Fedora   | Fedora 33 or higher       |



Windows 10에서 설치를 진행하였기 때문에, .exe 파일을 다운로드 받았다. 하지만, 설치 파일을 다운로드 받아 실행했더니 아무런 반응이 없었다. (...)

당황하지 않고 관리자 권한으로 파일을 실행했더니 설치가 진행되었다. 설치 완료 후 프로그램을 실행하면 다음과 같은 화면을 마주할 수 있고, 다운로드까지 끝나면 설치 완료다.

![2022-01-27-rancher-desktop-installation01.png](/img/2022-01-27-rancher-desktop-installation01.png)



##

# Docker Image Pull Test

Image 메뉴를 클릭하면 Docker Image를 Pull 하거나 Build 할 수 있다.

Add Image를 클릭하면 Image를 Pull/Build 할 수 있고, 원하는 이미지와 특정 버전과 같은 Tag를 입력만 하면 된다. 기본 Docker Image 저장소는 [Docker hub](https://hub.docker.com/) 이며, 사설 저장소와 같이 특정 저장소에 저장된 Docker Image도 받아올 수 있다. 뒤에 특정 버전과 같은 Tag를 붙이지 않으면 자동으로 latest 버전을 받아오는데, 해당 Docker Image가 latest Tag를 지원하지 않는다면 오류가 발생한다.

테스트로 [hello-world](https://hub.docker.com/_/hello-world) 이미지를 Pull 해 보았다.

![2022-01-27-rancher-desktop-installation02.png](/img/2022-01-27-rancher-desktop-installation02.png)

다운로드가 다 되면 Image List에서 추가된 Docker Image를 확인할 수 있다.

![2022-01-27-rancher-desktop-installation03.png](/img/2022-01-27-rancher-desktop-installation03.png)

Windows Terminal을 실행해보면 Rancher Desktop 탭이 생성된 것을 확인할 수 있다.

![2022-01-27-rancher-desktop-installation05.png](/img/2022-01-27-rancher-desktop-installation05.png)

Rancher Desktop 탭에서 docker 명령어를 입력하면 된다.

```bash
docker run hello-world
```

![2022-01-27-rancher-desktop-installation07.png](/img/2022-01-27-rancher-desktop-installation07.png)

hello-world Image가 잘 설치된 것을 확인할 수 있다.



##

## 후기

개인이 Docker Desktop을 사용하면 무료이기는 하지만, 회사에서 도입을 생각하는 경우가 생긴다거나 등의 상황들이 생길 수도 있지 않을까 싶어 Docker Desktop의 대체재인 Rancher Desktop을 설치하고 간단하게 Docker hub에서 Image를 불러오는 테스트를 진행해보았다.

쿠버네티스 환경은 다음에 기회가 된다면 테스트 하는 것으로 하려 한다...




## 

## Reference

- https://rancherdesktop.io/
- [도커 데스크톱, 대기업 사용자에게는 유료화된다 - CIO Korea](https://www.ciokorea.com/news/206529)
- [윈도우 10에서 Docker Desktop 없이 Docker 사용하기](https://www.bearpooh.com/92)
- [Docker Desktop 유료화와 대응방법 - velog](https://velog.io/@loganjeon/Docker-Desktop-유료화와-대응방법)
- [Docker Desktop 대탈출, multipass로 갑니다](https://jybaek.tistory.com/934)
