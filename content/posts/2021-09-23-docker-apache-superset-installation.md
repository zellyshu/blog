---

title: "WSL2 (Win10) Ubuntu20.04에서 Docker Desktop 설치하고 Apache Superset 테스트해보기"
date: 2021-09-23 11:18:21
tags: ["Data-Visualization", "Docker", "Apache-Superset", "OpenSource", "WSL2", "Ubuntu"]
categories: ["Data-Visualization", "Docker", "Apache", "OpenSource"]
author: "Me"
draft: false

---

## Intro

WSL2를 통해 Ubuntu를 설치한 뒤 Windows 환경이라는 제약하에 못해 본 것들을 하나 둘 해보던 와중 [Apache Superset](https://superset.apache.org/)이라는 오픈소스 시각화 BI툴을 설치하지 못했던 기억이 났다. Mac이나 Linux에서밖에 설치할 수 없어 Windows 환경에서는 VM 등의 가상머신을 이용할 수밖에 없었던 것으로 기억하고 있으나, WSL2의 지원 이후 Docker를 통해 설치가 가능하다는 것을 알게 되었다. 어차피 Docker도 설치해볼 겸 겸사겸사 Docker Desktop의 설치와 Apache Superset의 설치를 진행하게 되었다.


## Docker

![22021-09-23-docker-installation00.png](/img/2021-09-23-docker-installation00.png)

유료화 관련 정책으로 요근래 다시 시끌시끌해진 Docker를 오랜만에 다시 설치해보았다. Windows에서도 이젠 설치가 무사히(?) 된다는 것에 또 한 번 놀랐다. [Docker 홈페이지](https://www.docker.com/products/docker-desktop)에서 다운로드 한 뒤 .exe 파일을 통하여 설치하면 끝이다. Docker Image만 있으면 손쉽게 같은 환경을 구성할 수 있다는 건 다시 봐도 의존성 오류 때문에 몇 날 며칠을 머리 싸매고 고뇌하던 내게 너무나도 큰 혁신이었던 기억이 난다.

![22021-09-23-docker-installation01.png](/img/2021-09-23-docker-installation01.PNG)

절차에 따라 설치를 마무리하면 튜토리얼 비스무리한게 나온다.
이제 Apache Superset을 설치할 시간이다.


## Apache Superset

![2021-09-23-apache-superset-installation00.png](/img/2021-09-23-apache-superset-installation00.png)

Apache Superset 홈페이지에서 Apache Superset에 대하여 "Apache Superset is a modern data exploration and visualization platform" 이라 설명하고 있다. 회사 업무로 인하여, Tableau를 자주 사용하고 있는 나에게 있어 Apache Superset은 낯설지 않은 느낌으로 다가오는 BI 툴이었다. 실제로 여러 곳에서 Tableau의 대체재로 Apache Superset을 많이 꼽는 편이다. 연결할 수 있는 DB의 종류도 대표적인 DB라 할 수 있는 Oracle, MySQL, MSSQL 뿐만 아니라 Amazon REDSHIFT, Google BigQuery, IBM DB2 등도 지원하고 있어 오픈소스임에도 상용 DB까지 폭 넓게 지원하고 있다는 장점이 있다.


### Apache Superset Installation

WSL2의 Docker 환경에서 설치하였다. 설치 방법은 다음과 같다.

1. git clone
	```bash
	$ git clone https://github.com/apache/superset.git
	```
2. Docker를 통한 apache superset 컨테이터 실행

	```bash
	$ cd superset
	$ docker-compose -f docker-compose-non-dev.yml up
	```


하지만 설치 도중 (역시나...?) 에러가 발생하였다. 

![2021-09-23-apache-superset-installationerror01.png](/img/2021-09-23-apache-superset-installationerror01.png)


찾아보니 Windows OS의 Docker 환경에서 설치할 때 발생하는 에러로 보였고, 다음과 같이 해결할 수 있었다.

### superset_worker exited with code 127 Error 해결
```bash
docker pull preset/superset

docker run -d -p 8080:8080 --name superset preset/superset

docker exec -it superset superset fab create-admin  
--username admin  
--firstname Superset  
--lastname Admin  
--email [admin@superset.com](mailto:admin@superset.com)  
--password admin

docker exec -it superset superset db upgrade

docker exec -it superset superset load_examples

docker exec -it superset superset init
```

Error Reference
- https://github.com/apache/superset/issues/8632#issuecomment-630079535


### Test 

![2021-09-23-apache-superset-installation02.png](/img/2021-09-23-apache-superset-installation02.png)

설치가 완료되면, Docker에서 관련 리소스를 확인할 수 있다.
한 눈에 사용량 등을 파악할 수 있어 관리 및 운영 시 용이하다.

그리고, 8080번 포트(default)에 접속하면, Apache Superset에 접근할 수 있다.

> http://127.0.0.1:8080

해당 url로 접속하여 위에서 설정한 ID와 PW를 입력하면, Dashboard 메뉴로 이동한다.

![2021-09-23-apache-superset-installation03.png](/img/2021-09-23-apache-superset-installation03.png)

위의 이미지와 같이 시각화 예제 리스트가 있으며, 클릭해보면 출력되는 내용들을 확인할 수 있다.

![2021-09-23-apache-superset-installation04.png](/img/2021-09-23-apache-superset-installation04.png)

![2021-09-23-apache-superset-installation05.png](/img/2021-09-23-apache-superset-installation05.png)

![2021-09-23-apache-superset-installation06.png](/img/2021-09-23-apache-superset-installation06.png)

예제 화면을 보면, Tableau에서 Dashboard 화면을 구성하듯 Apache Superset에서도 유사하게 Dashboard를 구성할 수 있을 것으로 보인다. Tableau에서보다 간단하게 구성할 수 있을 것 같기도 하다.
어떤 Tool이 익숙하냐의 차이긴 하겠다만...


## 후기

Tableau를 통하여 업무를 진행하다보면 Power BI와 같이 자주 비교되는 대상이 바로 Apache Superset인데, 매번 문서를 통한 비교내역만 보다가 실제로 설치하여 구성해보니 비교점을 확실히 눈으로 파악할 수 있어 좋았다. 개인적으로, DB및 개발쪽 관련 지식이 있으신 분들이 Apache Superset가 Tableau보다 친숙한 느낌을 받을 것 같다는 생각이 들었다. 
또한, 오픈소스는 언제나 관심을 가지고 있는 영역이기 때문에 시각화 BI 오픈소스 툴이 어떤 모습인지 파악해보는 것도 좋은 공부가 되었다. 이왕 설치하여 환경을 구성해놓았으니, Apache Superset을 활용할 방법에 대하여 계속 모색해 보아야겠다.



## Reference

-  https://www.lainyzine.com/ko/article/a-complete-guide-to-how-to-install-docker-desktop-on-windows-10/
-  https://github.com/apache/superset
-  https://superset.apache.org/docs/installation/installing-superset-using-docker-compose



