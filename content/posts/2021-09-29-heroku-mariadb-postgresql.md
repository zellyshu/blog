---

title: "Heroku를 통한 무료 MariaDB & PostgreSQL 저장소 만들기"
date: 2021-09-29 15:41:07
tags: ["MariaDB", "PostgreSQL", "Database"]
categories: ["Database"]
author: "Me"
draft: false

---

## Intro

토이 프로젝트를 진행할 때 테스트 용도로 DB를 불러올 일이 종종 생겼다. 토이 프로젝트이기 때문에 정말 기본만 간단하게 구성된 DB와 연결해 몇가지 테스트만 해보면 될 상황에도 그 때 그 때 DB를 만들고 하는 건 여간 귀찮은 일이 아니었다. 그래서 최대한 무료 범위 내에서 해결하고자 알아보았고, Heroku로도 테스트 목적에는 손색이 없을 것이라 생각하여 바로 실행에 옮겼다.


## DB 생성

신용카드를 등록해두면, 월 1000시간 / 500 MB 제한으로 무료 사용할 수 있다. 또한, MariaDB만 하나 구축해두려다가 PostgreSQL도 하나 더 구축해두었다. 사실 '구축' 이라고 하기에도 너무 민망한 것이 버튼 몇 번으로 설정이 끝나기 때문이다. 기억을 기록해두기 위해 적는 것이라는 점 알아주셨으면 좋겠다...


### Heroku 를 통한 무료 MariaDB (JawsDB) 생성
- 'Create New app' 으로 새로운 app을 생성한 뒤,
- 'Resources' 메뉴에서 JawsDB Maria 라는 Add-ons 를 추가하면 끝
	- Billing에서 신용카드를 등록해야 함
	- Kitefin Shared Plan의 경우만 Free이며, 사양은 다음과 같다.


		| <center>속성</center>              | <center>내용</center>   |
		| ----------------- | ------: |
		| Direct SQL Access | ✅     |
		| Connections       | 10     |
		| RAM               | Shared |
		| Storage Capacity  | 5 MB   |
		| Daily Backups     | ✅     |
		| Backup Retention  | 1 Day  |
		| Databites         | ✅     |

- Settings에서 DB 관련 정보를 확인할 수 있다.
- Common Runtime과 Private Spaces 비교
	- Common Runtime
		-  ![2021-09-29-heroku-mariadb-postgresql01.png](/img/2021-09-29-heroku-mariadb-postgresql01.png)
	- Private Spaces
		- ![2021-09-29-heroku-mariadb-postgresql02.PNG](/img/2021-09-29-heroku-mariadb-postgresql02.PNG)


### Heroku 를 통한 무료 PostgreSQL 생성
- 'Create New app' 으로 새로운 app을 생성한 뒤,
- 'Resources' 메뉴에서 Heroku Postgres 라는 Add-ons 를 추가하면 끝
	- Billing에서 신용카드를 등록해야 함
	- Hobby Dev Plan의 경우만 Free이며, 사양은 다음과 같다.


		| <center>속성</center>              | <center>내용</center>   |
		| ------------------- | -------: |
		| Postgres Extensions | ✅      |
		| RAM                 | 0 Bytes |
		| Direct SQL access   | ✅      |
		| Row Limit           | 10,000  |
		| Storage Capacity    | 1 GB    |
		| Dataclips                    |   ✅      |
		| Connection Limit                    |   20      |
		| Rollback                    | 0 Seconds        |
		| PostGIS                   |  ✅       |
		| PGBackups                    | ✅        |
		| PGBackups Retained                    | 2 Backups       |

- Settings에서 DB 관련 정보를 확인할 수 있다.
- Common Runtime과 Private Spaces 비교
	- Common Runtime
		-  ![2021-09-29-heroku-mariadb-postgresql03.PNG](/img/2021-09-29-heroku-mariadb-postgresql03.PNG)
	- Private Spaces
		- ![2021-09-29-heroku-mariadb-postgresql04.PNG](/img/2021-09-29-heroku-mariadb-postgresql04.PNG)


## DBeaver 연결

![2021-09-29-heroku-mariadb-postgresql06.PNG](/img/2021-09-29-heroku-mariadb-postgresql06.PNG)

평소 DB Tool로 DBeaver를 사용하고 있으므로, DBeaver에 연결하여 불러봤더니 잘 나오고 있으므로 외부 연결도 무리 없음을 확인하였다. 


## 후기

![2021-09-29-heroku-mariadb-postgresql05.PNG](/img/2021-09-29-heroku-mariadb-postgresql05.PNG)

DBeaver에서도 접근이 가능하기 때문에 테스트 용도로서의 DB를 만들어두자 라는 목적은 충실하게 이루었다고 볼 수 있을 것 같다. 또한, 몇 번의 버튼 클릭만으로 설정이 완료되기 때문에 매우 간편하게 언제든 (물론 Heroku의 무료 정책상 제약은 있지만) 접근 가능한 DB를 만들 수 있다는 점은 강력한 메리트라 생각한다.



## Reference

- https://www.heroku.com/
- https://zeddios.tistory.com/1233



