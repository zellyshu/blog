---

title: "PostgreSQL 설치 테스트 (Ubuntu 20.04, WSL2)"
date: 2021-04-02 00:00:00
tags: ["PostgreSQL", "Database"]
categories: ["Database"]
author: "Me"
draft: false

---

## Intro

MySQL, Oracle, MSSQL 등의 DB는 접해보았으나 PostgreSQL은 이야기만 들어보고 실제 사용해 본 경험은 없어서 설치하고, 테스트 목적의 테이블을 생성하고, DB를 삽입하여 조회해보는 작업을 해보았다. 

**PostgreSQL**은 오픈 소스 객체-관계형 데이터베이스 관리 시스템(ORDBMS) 이다. 평소 관심만 가지고 있다가 추후 토이 프로젝트 시 DB를 선택할 때 PostgreSQL을 도입해보고 싶다는 막연한 생각에 맨땅에 헤딩하듯이 설치 테스르를 진행한 것이다.

DB에 조예가 깊지 않아 설치하고, 잘 돌아가는지 정도의 레벨로 테스트를 진행하였고 관련 내용을 간략하게 정리한 것임을 미리 알린다.

WSL2에 설치한 Ubuntu 20.04 LTS 환경에서 테스트를 진행하였고, Linux 환경에 PostgreSQL 12.6을 설치하였다.



## 설치

- Ubuntu 20.04 (WSL2 에서 진행함)  



### 1. apt-get update 진행

   

   ```bash
   sudo apt-get update
   ```

   

   

   ![2021-04-02-postgresql-install-test01.JPG](/img/2021-04-02-postgresql-install-test01.JPG)

   

### 2. apt-get install 명령어를 통하여 postgresql, postgresql-contrib을 설치

   

   

   ```bash
   sudo apt-get install postgresql postgresql-contrib
   ```

   

   

   ![2021-04-02-postgresql-install-test02-1.JPG](/img/2021-04-02-postgresql-install-test02-1.JPG)

   ![2021-04-02-postgresql-install-test02-2.JPG](/img/2021-04-02-postgresql-install-test02-2.JPG)

   

   

### 3. postgres 계정 로그인

   

   

   ```bash
   sudo -i -u postgres
   ```

   

   

   ![2021-04-02-postgresql-install-test03.JPG](/img/2021-04-02-postgresql-install-test03.JPG)

   

   

### 4. psql 명령어로 DB 접근

   - CREATE, ALTER 등의 명령어 실행 테스트

   

   

   ```bash
   psql
   exit
   ```

   

   

   ![2021-04-02-postgresql-install-test04.JPG](/img/2021-04-02-postgresql-install-test04.JPG)

   

   

### 5. psqladmin 계정과 psql_db 생성

   

   

   ```bash
   psql -h localhost -U psqladmin -d psql_db
   ```

   

   

   ![2021-04-02-postgresql-install-test05.JPG](/img/2021-04-02-postgresql-install-test05.JPG)

   

   

### 6. 스키마, 시퀀스, 테이블 생성

   - https://racoonlotty.tistory.com/entry/Ubuntu%EC%97%90-PostgreSQL-%EC%84%A4%EC%B9%98-1?category=868396 포스팅 참고 (6~12)

     

     

   ![2021-04-02-postgresql-install-test06.JPG](/img/2021-04-02-postgresql-install-test06.JPG)

   

   

### 7. 시퀀스 변경

   

   

   ```sql
   ALTER SEQUENCE menu_id_seq OWNED BY menu.id;
   ```

   

   

   ![2021-04-02-postgresql-install-test07.JPG](/img/2021-04-02-postgresql-install-test07.JPG)

   

   

### 8. 인덱스 생성

   

   

   ```sql
   CREATE UNIQUE INDEX "UNIQUE" ON users(email);
   ```

   

   

   ![2021-04-02-postgresql-install-test08.JPG](/img/2021-04-02-postgresql-install-test08.JPG)

   

   

### 9. 데이터 삽입 1

   

   

   ![2021-04-02-postgresql-install-test09.JPG](/img/2021-04-02-postgresql-install-test09.JPG)

   <center><I><a>https://racoonlotty.tistory.com/entry/Ubuntu%EC%97%90-PostgreSQL-%EC%84%A4%EC%B9%98-1?category=868396</a> 링크 참고</I></center><br>

### 10. 데이터 삽입 2

![2021-04-02-postgresql-install-test10.JPG](/img/2021-04-02-postgresql-install-test10.JPG)


<center><I><a>https://racoonlotty.tistory.com/entry/Ubuntu%EC%97%90-PostgreSQL-%EC%84%A4%EC%B9%98-1?category=868396</a> 링크 참고</I></center><br>

### 11. 'menu' 테이블 select

 

~~~sql
select * from menu;
~~~


​    

​    

![2021-04-02-postgresql-install-test11.JPG](/img/2021-04-02-postgresql-install-test11.JPG)


​    

​    

### 12. 릴레이션 리스트 확인

​    

​    

~~~sql
\d
~~~


​    



![2021-04-02-postgresql-install-test12.JPG](/img/2021-04-02-postgresql-install-test12.JPG)





## 설치 시 발생했던 에러정리

### 1. socket 관련 에러 (psql 명령어로 DB접근 시 socket 에러 발생)

   

   

   ![2021-04-02-postgresql-install-testerror_pid.JPG](/img/2021-04-02-postgresql-install-testerror_pid.JPG)

   

   

   #### 1. https://stackoverflow.com/questions/31645550/postgresql-why-psql-cant-connect-to-server의 3번째 답변으로 해결 (postgres service를 재시작)


​      
​      

```bash
sudo /etc/init.d/postgresql restart
```


​      
​      
​      
​      ![2021-04-02-postgresql-install-testerror_pid_solution.JPG](/img/2021-04-02-postgresql-install-testerror_pid_solution.JPG)





## 추가로 익혀야 할 사항들

- 기본키가 자동으로 증가되도록 하려면?
  - nextval (regclass), serial 등 사용
  - 참고자료 URL
    - https://mine-it-record.tistory.com/341





## Reference

- https://racoonlotty.tistory.com/entry/Ubuntu%EC%97%90-PostgreSQL-%EC%84%A4%EC%B9%98-1?category=868396
- https://dejavuqa.tistory.com/16
- https://postgresql.kr/



