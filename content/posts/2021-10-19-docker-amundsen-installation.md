---

title: "Amundsen을 Docker에 설치해보기"
date: 2021-10-19 16:38:57
tags: ["Amundsen", "Data", "Docker"]
categories: ["Amundsen", "Data Discovery Tool", "Docker"]
author: "Me"
draft: false

---

## Intro

RSS 구독을 통하여 다양한 기술 블로그 및 회사들의 글을 읽으며 아침 일과를 시작하는 편이다. 그러던 중, 뱅크샐러드 기술 블로그에서 ['# 뱅크샐러드 Data Discovery Platform의 시작'](https://medium.com/bagelcode/data-discovery-platform-at-bagelcode-b58a622d17fd) 이라는 글을 읽게 되었는데 갑자기 흥미가 생겨서 데이터 디스커버리 툴에 대하여 찾아보게 되었다. 


## Data Discovery Tool?

Data Discovery Tool이란, 말 그대로 데이터의 탐색을 돕는 툴로서 여러 소스에서 데이터를 수집 및 결합하고 데이터의 패턴과 추세를 식별하는 데 도움이 되는 도구를 의미한다. BI 툴에서 많이 찾아볼 수 있다. 특히, 데이터를 다루는 많은 회사들이 Data Discovery Tool들을 사용하고 있으며, Linkedin의 [Data Hub](https://datahubproject.io/), Netflix의 [Metacat](https://github.com/Netflix/metacat), Lyft의 [Amundsen](https://www.amundsen.io/) 등 오픈소스로 제공하는 회사들도 많다. 


## Amundsen

처음에는 Linkedin의 Data Hub를 간단하게 설치해보려 했으나, 베이글코드라는 회사에서 사용하는 사내 디스커버리 툴인 Amundsen을 어떻게 도입하게 되었는지를 다루는 [# Data Discovery Platform at Bagelcode](https://medium.com/bagelcode/data-discovery-platform-at-bagelcode-b58a622d17fd) 라는 글을 읽게 되면서 Python 기반으로 이루어진 Amundsen을 설치해보자 라는 생각이 더 강해지게 되었다. (구글 검색을 했을 때, DataHub에 관한 자료탐색 시 DataHub라는 이름이 어떻게 보면 너무 흔한 이름이다 보니 Linkedin의 DataHub라는 툴에 대한 내용이 찾기 어려웠던 점은 덤)


### Installation

설치 선행 조건은 다음과 같다.

> - Python = 3.6 or 3.7
> - Node = v10 or v12 (v14 may have compatibility issues)
> - npm >= 6

설치 방법은 Docker로 설치하면 되는데, Backend를 Neo4j로 하느냐, Atlas로 하느냐에 따라 2가지로 나뉘게 된다.


1. 적어도 3GB의 Docker 여유 용량이 있는지 확인할 것
2. Git Clone
	```bash
		$ git clone --recursive https://github.com/amundsen-io/amundsen.git
	```
3. Backend 선택
	```bash
		# For Neo4j Backend
		$ docker-compose -f docker-amundsen.yml up
	
		# For Atlas
		$ docker-compose -f docker-amundsen-atlas.yml up
	```
4. Sample data 설치
	```bash
		 $ python3 -m venv venv
		 $ source venv/bin/activate
		 $ pip3 install --upgrade pip
		 $ pip3 install -r requirements.txt
		 $ python3 setup.py install
		 $ python3 example/scripts/sample_data_loader.py
	```
5. http://localhost:5000 접속
	![2021-10-19-docker-amundsen-installation05](/img/2021-10-19-docker-amundsen-installation05.png)
6.  **test** 검색 후 결과 확인

검색해보면, 하단의 이미지와 같이 나오게 된다.
![2021-10-19-docker-amundsen-installation06](/img/2021-10-19-docker-amundsen-installation06.png)
![2021-10-19-docker-amundsen-installation07](/img/2021-10-19-docker-amundsen-installation07.png)
![2021-10-19-docker-amundsen-installation08](/img/2021-10-19-docker-amundsen-installation08.png)


### 설치 중 에러 발생했던 내용 정리

 #### ERROR: Service 'amundsenfrontend' failed to build : Build failed ~
![2021-10-19-docker-amundsen-installation-error01](/img/2021-10-19-docker-amundsen-installation-error01.png)
-  Atlas를 통한 설치 시에 발생한 에러. 권한 관련 이슈로 보이는데 결국 잡지 못하고 neo4j를 통한 설치 방법으로 우회


#### error in amundsen-databuilder setup command: 'extras_require' must be a dictionary whose values are strings or lists of strings containing valid project/version requirement specifiers.
- github 내 Issue에 관련 사항이 있었음. amundsen\databuilder 폴더 내의 requirements-dev.txt 의 내용을 amunsen 폴더의 requirements-dev.txt의 내용으로 변경하면 된다. 확인해보니 경로 관련 이슈가 있었던 것으로 보인다.
- 참고 : https://github.com/amundsen-io/amundsen/issues/1447



## 후기

설치 과정은 중간에 에러만 발생하지 않았다면, 상당히 쉬운 편이었다. 
앞으로는 Amundsen을 활용하여 여기저기 내 컴퓨터 및 클라우드에 흩어져 있는 데이터를 효율적으로, 체계적으로 관리해보려 노력해야겠다.



## Reference

- https://blog.banksalad.com/tech/the-starting-of-datadiscoveryplatform-era-in-banksalad/
- https://medium.com/bagelcode/data-discovery-platform-at-bagelcode-b58a622d17fd
- https://github.com/amundsen-io/amundsen/
- https://www.softwareadvice.com/bi/data-discovery-tools-comparison/
