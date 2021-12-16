---

title: "Oracle Cloud 가입부터 간단한 설정까지"
date: 2021-03-31 00:00:00
tags: ["Oracle", "Cloud"]
categories: ["Cloud", "Server"]
author: "Me"
draft: false

---

## Intro

본 포스팅은 Oracle Cloud 의 가입과 Free Tier에서 클라우드 초보자의 입장에서 개발 환경을 설정해 본 경험을 (= 미래의 내가 잊어먹지 않도록 정리해둔 것을) 다룬 것이다. 나와 같은 분들과 Oracle Cloud를 처음 사용하시는 분들께 조금이나마 참고 자료로서 도움이 되었으면 하는 마음에 글을 작성하였다.





## Oracle Cloud란?

Oracle Cloud는 어디선가 한 번쯤은 들어봤을 Oracle사에서 운영하는 Cloud Flatform으로, GCP, AWS와 비교하여 Free Tier를 2개나 지원하고 있다. 특히 Free Tier에서도 DB 2개, 컴퓨팅 VMs, 100GB 블록의 볼륨, 10GB 객체 스토리지를 제공하고 있다는 것이 특장점이다. 이는 이미 시장을 점유하고 있는 AWS, GCP, Azure와 비교해도 전혀 사용에 있어 지장 없는 스펙이라 할 수 있다.



![oraclecloud_01_2.png](/img/2021-03-31-oracle-cloud_oraclecloud_01_2.png)

<center><I>Oracle Cloud 공식 홈페이지에서는 AWS와 같은 타 서비스와 비교하여, Oracle Cloud의 장점을 어필하고 있다.</I></center><br>



## 가입

가입은 과정을 따라 진행하면 되는데 Cloud Tenant 라고 이름을 설정하는 부분이 있다. 요것도 기억을 해주어야 나중에 로그인할 때 헤매지 않는다...



![oraclecloud_01.png](/img/2021-03-31-oracle-cloud_oraclecloud_01.png)

<center><I>Oracle Cloud 로그인 화면</I></center><br>



## 설정

로그인하게 되면 다른 클라우드 서비스에서 으레 볼 수 있는 대시보드 화면을 마주하게 된다.



![oraclecloud_02.PNG](/img/2021-03-31-oracle-cloud_oraclecloud_02.jpg)

<center><I>Oracle Cloud Dashboard</I></center><br>



위의 사진을 보면 서울 지역으로 생성한 것을 확인할 수 있고, (서울 TO가 부족하여 생성이 안되는 경우가 종종 있다고 하니 이 점 참고) 본래 목적인 VM 인스턴스를 생성하여 SSH로 어디서든 접근하여 사용할 수 있도록 설정하였다. (토이 프로젝트용 1개, 개인 웹서버용 1개를 생성하여 현재 무리 없이 사용 중에 있다.)

물론, 웹에서도 터미널을 띄워 개발할 수 있지만 상당히 불편하기 때문에 개발을 용이하게 하기 위하여 SSH 접속 환경을 만들어주었다. (Window 환경에서 진행)



![oraclecloud_03.PNG](/img/2021-03-31-oracle-cloud_oraclecloud_03.jpg)

<center><I>Puttygen으로 Key 생성</I></center><br>

![oraclecloud_05.PNG](/img/2021-03-31-oracle-cloud_oraclecloud_05.jpg)

<center><I>생성한 Key로 SSH 접속</I></center><br>



SSH 접속을 위하여 Puttygen으로 SSH key를 생성하고, 이를 생성한 인스턴스에 설정한 뒤 Putty를 이용하여 접근하니 하단 이미지와 같이 접속이 잘 되는 것을 확인할 수 있었다. 참고로, username의 default값은 centos의 경우 opc이며, ubuntu의 경우에는 ubuntu로 설정되어 있다.



![oraclecloud_06.PNG](/img/2021-03-31-oracle-cloud_oraclecloud_06.jpg)

<center><I>SSH 연결 성공</I></center><br>



그리고, 다른 클라우드 플랫폼과 마찬가지로 방화벽 포트를 추가적으로 열어주어야 특정 서비스의 접근 및 외부 환경에서의 접근이 가능하다. 방화벽 포트는 '수신 규칙' 및 '송신 규칙'에서 변경할 수 있으며, 필요한 포트만 열어 보안 문제를 신경 쓰는 것이 좋을 것으로 보인다.



