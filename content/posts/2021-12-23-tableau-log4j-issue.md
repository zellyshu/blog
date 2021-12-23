---
title: "Tableau Log4j 보안 이슈 대응 및 해결방안 정리"
date: 2021-12-23 14:39:01
creation date: 2021-12-23 14:37
modification date: 2021-12-23 14:39
tags: ["Tableau", "Log4j", "Tableau Desktop", "Tableau Server"]
categories: ["Tableau"]
author: "Me"
draft: false

---

## Intro

최근 발생한 [Log4j 보안 취약점 사태](https://www.krcert.or.kr/data/secNoticeView.do?bulletin_writing_sequence=36389)로 인하여 시끌시끌한 가운데, Tableau에서도 Log4j 보안 취약점에 대한 대응방안을 내놓았다. 따라서 이와 관련된 내용을 정리하고, 실제 업데이트를 해본 사항을 기록해둔다.

## 

## Tableau에서의 Log4j

Log4j Apache 라이브러리를 사용하는 Tableau 제품은 보안 취약점을 가지고 있으며, 이에 영향을 받는 제품들은 Log4j 보안 취약점 이슈가 해결된 패치 버전으로 업그레이드하거나, jndilookup.class를 삭제하여야 한다.

> https://kb.tableau.com/articles/issue/Apache-Log4j2-vulnerability-Log4shell

> 이슈 대응방안
> 
> 1. Log4j 보안 취약점 이슈가 해결된 패치 버전으로 업그레이드
> 2. jndilookup.class 삭제

### 

### Log4j 보안 취약점 이슈에 영향을 받는 제품 환경

하단의 제품 버전이거나 그보다 낮은 버전일 경우 Log4j 보안 취약점 이슈에 영향을 받는 제품이다.

| Product                       | Version                                                                                                                                                                                                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Tableau Server                | 2021.4, 2021.3.4, 2021.2.5, 2021.1.8, 2020.4.11, 2020.3.14, 2020.2.19, 2020.1.22, 2019.4.25, 2019.3.26, 2019.2.29, 2019.1.29, 2018.3.29                                                                                                                            |
| Tableau Desktop               | 2021.4, 2021.3.4, 2021.2.5, 2021.1.8, 2020.4.11, 2020.3.14, 2020.2.19, 2020.1.22, 2019.4.25, 2019.3.26, 2019.2.29, 2019.1.29, 2018.3.29                                                                                                                            |
| Tableau Prep Builder          | 2021.4.1, 2021.3.2, 2021.2.2, 2021.1.4, 2020.4.1, 2020.3.3, 2020.2.3, 2020.1.5, 2019.4.2, 2019.3.2, 2019.2.3, 2019.1.4, 2018.3.3                                                                                                                                   |
| Tableau Public Desktop Client | 2021.4                                                                                                                                                                                                                                                             |
| Tableau Reader                | 2021.4                                                                                                                                                                                                                                                             |
| Tableau Bridge                | 20214.21.1109.1748, 20213.21.1112.1434, 20212.21.0818.1843, 20211.21.0617.1133, 20204.21.0217.1203, 20203.20.0913.2112, 20202.20.0721.1350, 20201.20.0614.2321, 20194.20.0614.2307, 20193.20.0614.2306, 20192.19.0917.1648, 20191.19.0402.1911, 20183.19.0115.1143 |

필자의 경우, Log4j 보안 취약점 이슈가 해결된 패치 버전으로 업그레이드 하여 해당 문제를 조치하였으며 관련 내용은 다음과 같다.

### 

### 방법1. 패치 버전 업그레이드

#### 

#### 패치 버전 정리

2021.12.19 일자 패치 버전으로 업그레이드 하면, 현재 알려져 있는 [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228) & [CVE-2021-45046](https://nvd.nist.gov/vuln/detail/CVE-2021-45046) 문제를 해결할 수 있다.

| Product                       | Version                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tableau Server                | [2021.4.2](https://www.tableau.com/support/releases/server/2021.4.2), [2021.3.6](https://www.tableau.com/support/releases/server/2021.3.6), [2021.2.7](https://www.tableau.com/support/releases/server/2021.2.7), [2021.1.10](https://www.tableau.com/support/releases/server/2021.1.10), [2020.4.13](https://www.tableau.com/support/releases/server/2020.4.13)      |
| Tableau Desktop               | [2021.4.2](https://www.tableau.com/support/releases/desktop/2021.4.2), [2021.3.6](https://www.tableau.com/support/releases/desktop/2021.3.6), [2021.2.7](https://www.tableau.com/support/releases/desktop/2021.2.7), [2021.1.10](https://www.tableau.com/support/releases/desktop/2021.1.10), [2020.4.13](https://www.tableau.com/support/releases/desktop/2020.4.13) |
| Tableau Prep Builder          | [2021.4.3](https://www.tableau.com/support/releases/prep/2021.4.3)                                                                                                                                                                                                                                                                                                    |
| Tableau Public Desktop Client | [2021.4.2](https://public.tableau.com/en-us/s/download)                                                                                                                                                                                                                                                                                                               |
| Tableau Reader                | [2021.4.2](https://www.tableau.com/products/reader/download)                                                                                                                                                                                                                                                                                                          |
| Tableau Bridge                | [20214.21.1109.1748](https://www.tableau.com/support/releases/bridge/20214.21.1217.2252)                                                                                                                                                                                                                                                                              |



#### Tableau Server Upgrade

> https://help.tableau.com/current/server/ko-kr/upgrade.htm
> 
> - Windows
>   - 2018.2 이상에서 업그레이드(Windows)
>     - https://help.tableau.com/current/server/ko-kr/sug_plan.htm
>   - 2018.1 이하에서 업그레이드 수행(Windows)
>     - https://help.tableau.com/current/server/ko-kr/sug_upgrade_samehrdwr_pretsm.htm
> - Linux
>   - https://help.tableau.com/current/server-linux/ko-kr/upgrade.htm



필자는 Windows Server 2012 R2 환경에 설치되어있는 Tableau Server 2020.4.3 버전을 Tableau 2020.4.13 버전으로 업그레이드 하였다. 순서는 다음과 같다.



##### 설치 순서

- Tableau Server 설치된 위치 확인

- 현재 Tableau Server 백업
  
  - [서버 업그레이드 - Tableau Server 백업 - Tableau](https://help.tableau.com/current/server/ko-kr/server-upgrade-prepare-backup.htm)
    - `tsm maintenance backup -f ts_backup -d`

- 서버 업그레이드 설치 파일 다운로드
  - [2020.4.13](https://www.tableau.com/support/releases/server/2020.4.13)
- 단일 서버 업그레이드 설치 실행 ([단일 서버 업그레이드 -- 설치 실행 - Tableau](https://help.tableau.com/current/server/ko-kr/server-upgrade-baseline-singlenode-setup.htm))
  - 원격 서버 로그온
  - Tableau Server 설치 프로그램 복사한 폴더로 이동하여 설치 프로그램 실행
  - 설치 프로그램에서 기존 버전의 위치가 표시되면 새 버전을 같은 위치에 설치
  - 마지막 페이지에서 업그레이드가 아직 완료되는 메시지가 표시됨
    - 업그레이드 스크립트 upgrade-tsm 실행하여 업그레이드 완료
  - Tableau Server 시작
    - TSM or 명령 프롬포트
     ![2021-12-23-tableau-log4j-issue01.png](/img/2021-12-23-tableau-log4j-issue01.png)
  - Tableau Server 업그레이드 여부 확인
     ![2021-12-23-tableau-log4j-issue02.png](/img/2021-12-23-tableau-log4j-issue02.png)

###

### 방법2. jndilookup.class 삭제

> - [Tableau Server Mitigation Steps](https://kb.tableau.com/articles/issue/apache-log4j2-vulnerability-log4shell-tableau-server-mitigation-steps)
> - [Tableau Desktop Mitigation Steps](https://kb.tableau.com/articles/issue/apache-log4j2-vulnerability-log4shell-tableau-desktop-mitigation-steps)
> - [Tableau Prep Builder Mitigation Steps](https://kb.tableau.com/articles/issue/apache-log4j2-vulnerability-log4shell-tableau-prep-builder-mitigation-steps)
> - [Tableau Bridge Mitigation Steps](https://kb.tableau.com/articles/issue/apache-log4j2-vulnerability-log4shell-tableau-bridge-mitigation-steps)



Windows 또는 Linux 및 설치 환경에 따라 해당 링크에서 해결 방법을 확인할 수 있다. 해당 방법은 진행하지 않았기 때문에 관련 링크만 정리해두었다.

## 

## 후기

Tableau Server 뿐 아니라 Desktop 및 Prep 등도 급하게 업데이트를 진행하였으나, 공식 홈페이지에 관련 내용이 잘 나와있으므로 별도로 정리하지는 않았다. 앞으로 이러한 치명적인 보안 이슈가 발생하지 않았으면 한다...


## 

## Reference

- https://kb.tableau.com/articles/issue/Apache-Log4j2-vulnerability-Log4shell
- https://help.tableau.com/current/server/ko-kr/upgrade.htm
- https://help.tableau.com/current/server/ko-kr/sug_plan.htm
- https://help.tableau.com/current/server/ko-kr/sug_upgrade_samehrdwr_pretsm.htm
- https://help.tableau.com/current/server-linux/ko-kr/upgrade.htm
