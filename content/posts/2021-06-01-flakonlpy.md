---

title: "CentOS 7 + Flask + gunicorn + Nginx 조합으로 Oracle Cloud에서 돌아가는 서비스 만들어보기"
date: 2021-06-01 00:00:00
tags: ["CentOS7", "Flask", "gunicorn", "Nginx", "Python", "OracleCloud", "Konlpy"]
categories: ["Python", "Cloud", "Service"]
author: "Me"
draft: false

---

## Intro

본 포스팅은 Python의 한국어 형태소 분석 라이브러리인 [konlpy](https://konlpy.org/en/latest/)를 활용하여 CentOS 7 + Flask + gunicorn + Nginx 조합으로 웹 애플리케이션 서비스를 만들어본 토이 프로젝트 개발기다. 개발된 서비스는 [링크(클릭)](http://flakonlpy.kro.kr/)를 통하여 이동하면 사용할 수 있으나, 이용자에게 제공되는 완전하고 안정된 형태의 서비스라기보다 Oracle Cloud의 무료 티어를 활용해보기 위한 테스트 목적이 강하므로 이 점을 인지하여 준다면 감사할 것 같다. (갑자기 다운되거나 오류 발생 확률 매우매우매우 높음ㅠㅠ)





## 왜 만들었나?

취직하여 일을 시작한 지 얼마 되지 않았을 무렵, 사내 팀에서 AWS를 사용하고 있어 자연스레 클라우드 환경에 관심이 많아진 상태였다. 물론, 대학원생 시절 AWS와 GCP를 사용해본 적은 있으나 (딥러닝 모델 학습하다가 한달에 몇백만원 찍히고 놀래서 서버 다 내린 건 언젠가 풀 또 하나의 이야기...) 완전히 친숙하다고 이야기하기엔 아주 애매한 사이였고, 클라우드 환경 하에서 웹 애플리케이션 서비스를 하나 만들어 배포해보면 지금보다는 더 익숙해지지 않을까 라는 결론에 자연스레 이르렀다. 

AWS와 GCP 중 어떤 걸 선택하여 테스트해볼까 고민하던 중, Oracle Cloud의 무료 티어가 눈에 들어오게 되었고 생각보다 괜찮은 사양이었기 때문에 이왕 해보는 거 기존에 사용해본 적 없었던 것을 사용해보자라는 마음으로 Oracle Cloud를 선택하게 되었다. 

처음 목적은 클라우드 환경과 친숙해지기 + flask와 gunicorn 조합을 활용해보기 였기 때문에 석사과정 때 몇 날 며칠을 밤새 만들었던 '[한국어 형태소 분석기](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE07207339)' 의 데모 서비스를 그대로 올려 사용하려 했다. 하지만 무료 티어의 한계로 인해(ㅠㅠ) 선학습된 tensorflow 모델을 올릴 때 memory out이 발생하는 등의 자원부족으로 인한 오류가 발생하였고, 대체할 만한 가벼운 서비스를 빠르게 만들어보아야 겠다고 생각하게 되었다.

![2021-06-01-flakonlpy01.PNG](/img/2021-06-01-flakonlpy01.PNG)

<center><I>무지하게 많이 봤던 에러 화면...</I></center><br>

무언가를 입력하면 어떠한 결과가 출력되는 형태의 서비스를 생각하다보니, konlpy에서 지원하는 형태소 분석기마다의 모든 결과를 뿌릴 수 있도록 하는 웹페이지를 만들면 어떨까 하는 생각이 들었다. 화면도 조금만 수정하면 될 것 같아서 빠르게 실행에 옮겼다.



## CentOS 7 + Flask + gunicorn + Nginx

OS는 CentOS 7로 결정하고, Flask와 gunicorn 조합을 활용해보기로 마음 먹었다. 아래는 간략한 설명(을 가장한 사족)이다.

 

### CentOS 7

CentOS 8로 해볼까 하다가 지원 관련 이슈가 괜히 신경쓰여서 7로 선택했다. 선택해 놓고 보니 CentOS 지원 종료 이슈가 계속 마음에 걸려 언젠가는 [Rocky Linux](https://rockylinux.org/ko/)로 변경해볼까 하는 생각이 계속 든다...



### [Flask](https://flask.palletsprojects.com/en/2.0.x/)

Flask는 Django와 달리 매우매우 가볍다. 또한, 붙이고 싶은 대로 이것저것 붙이는 것이 자유로운 웹 프레임워크다. Tensorflow로 선학습된 모델을 올려 가볍게 테스트용 서비스를 만들 때 자주 사용했었다. 하지만, 너무 자유로워서 이게 왜 돌아가는 거지 싶을 때가 종종 있었다...



### [gunicorn](https://gunicorn.org/)

기존에는 uwsgi를 자주 사용했었고, 이번에도 사용하려 하였다. 하지만, uwsgi가 자원을 많이 소모한다는 정보를 어디선가 들었고, 테스트의 목적이라면 전혀 사용해본 적이 없는 것을 사용해보아야 진정한 테스트가 아니겠는가라는 이유로 gunicorn을 활용하게 되었다. 실제로 적용해보고 나니 굉장히 쉽고 빨라서 다른 개발에도 사용하면 괜찮겠다란 생각이 들었다.



### [Nginx](https://www.nginx.com/)

Apache는 사내에서도 사용해 볼 일이 있었기 때문에 스터디 목적으로라도 Nginx를 적용해보아야 겠다는 생각을 했고, 실제로 적용해보니 굉장히 가볍고 빨랐다. 최대한 빠르게 만들어서 테스트해야지라는 목적에도 잘 부합하는 선택이었다고 생각한다.



## 이외의 설정들

### FrontEnd

이전에 bootstrap 기반의 테마를 다운받아 적당히 커스텀했던 것을 가져와서 사용하였다. 당시 Frontend에 대한 경험이 너무 적었고, (물론 지금이라고 더 낫다고 할 순 없지만...) 물어볼 사람도 너무나 적었기 때문에 하나하나 뜯어가며 하드코딩했던 기억이 난다. 당시에도 반응형으로 만들고 싶었지만, 모바일을 비롯 해상도에 따라 깨져 보이는 것은 본인도 알고 있으며(ㅠㅠㅠ), 관련 분야 실력이 부족한 것을 한탄할 뿐이다.



### URL

접근은 '[내도메인한국](https://xn--220b31d95hq8o.xn--3e0b707e/)'에서 별도의 URL을 생성해서 redirect 해주는 형태로 쉽게 접근할 수 있도록 하였다. 실제 서비스가 아니기 때문에 SSL 같은 요소는 따로 적용하지 않았다.



## 설치

python 및 nginx 를 설치한다.

```bash
sudo yum install python-pip python-devel gcc nginx
```

Konlpy 라이브러리를 사용하기 위한 선행 설치 요건들을 설치해준다.

```bash
sudo yum install gcc-c++ java-1.8.0-openjdk-devel
sudo yum install curl git
bash <(curl -s https://raw.githubusercontent.com/konlpy/konlpy/master/scripts/mecab.sh)
```



설치가 완료되면, pip를 통하여 flask와 gunicorn을 설치해준다.

관리가 용이하도록 virtualenv 모듈을 통하여 가상환경을 만들어 설치해주었다.

```bash
virtualenv venv
source venv/bin/activate

pip install gunicorn flask konlpy
```

 

설치 완료 후, konlpy 라이브러리가 잘 설치되었는지 확인해보았다.

```python
import konlpy
mecab = konlpy.tab.Mecab()
print(mecab.pos("아버지가 방에 들어가신다."))
```



확인 결과 다음과 같이 출력이 잘 되었다.

![2021-06-01-flakonlpy06.JPG](/img/2021-06-01-flakonlpy06.JPG)

<center><I>분석 결과 출력이 잘 된 것을 확인할 수 있다.</I></center><br>

gunicorn을 실행하기 위하여 wsgi.py 를 만든 뒤, 다음의 명령어를 입력한다.

- wsgi.py

<script src="https://gist.github.com/zellyshu/0c9e8e33e815e6bc7209583fd4f1fec9.js"></script>

```bash
gunicorn --bind 0.0.0.0:5000 wsgi:app
```



![2021-06-01-flakonlpy07.JPG](/img/2021-06-01-flakonlpy07.JPG)

<center><I>무사히 실행되는 것을 확인할 수 있다.</I></center><br>

이제 시스템에 서비스를 등록하고,  관련 설정을 해준다.

```bash
sudo vi /etc/systemd/system/flask_konlpy_webapp.service
```



```bash
[Unit]
Description=Gunicorn instance to serve flask_konlpy_webapp
After=network.target

[Service]
User=opc
Group=nginx
WorkingDirectory=/home/opc/flask_konlpy_webapp
Environment="PATH=/home/opc/flask_konlpy_webapp/venv/bin"
ExecStart=/home/opc/flask_konlpy_webapp/venv/bin/gunicorn --workers 3 -t 120 --bind unix:flask_konlpy_webapp.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
```



서비스를 시작하고 서비스의 상태를 확인해보자.

```bash
sudo systemctl start flask_konlpy_webapp.service
sudo systemctl enable flask_konlpy_webapp.service
sudo systemctl status flask_konlpy_webapp.service
```



Active: <span style="color:green">active (running)</span> 으로 잘 실행되고 있음을 확인할 수 있다.

![2021-06-01-flakonlpy08.JPG](/img/2021-06-01-flakonlpy08.JPG)

<center><I>서비스가 잘 실행되는 것을 확인할 수 있다.</I></center><br>

Nginx 관련 설정은 다음과 같이 진행한다.

```bash
sudo vi /etc/nginx/nginx.conf
```



Server 부분을 찾아서 다음과 같이 추가 및 수정해준다.

server_name 부분의 server_domain_or_IP는 본인의 환경에 맞게 설정해준다.

```bash
    server {
        listen 80;
        server_name server_domain_or_IP;

        include /etc/nginx/default.d/*.conf;

        location / {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_http_version 1.1;
            proxy_set_header Connection "";
            proxy_pass http://unix:/home/opc/flask_konlpy_webapp/flask_konlpy_webapp.sock;
        }
        error_page 404 /404.html;
            location = /40x.html {
        }
        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```



Nginx 관련 접근 권한 및 설정을 변경해준다.

```bash
sudo usermod -a -G opc nginx
chmod 710 /home/opc
sudo nginx -t
```



Nginx 설정이 완료되었다면, Nginx 서비스를 시작해주도록 하자.

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```



Active: <span style="color:green">active (running)</span> 으로 잘 실행되고 있음을 확인할 수 있다.

![2021-06-01-flakonlpy09.JPG](/img/2021-06-01-flakonlpy09.JPG)

<center><I>Nginx가 잘 실행되는 것을 확인할 수 있다.</I></center><br>



## 서비스

서비스는 [링크](http://flakonlpy.kro.kr/)를 선택하면 사용할 수 있고, 링크를 선택하면 다음과 같은 화면이 나온다.

아무 소개가 없으면 너무 허전할 것 같아 간단한 소개와 형태소 분석 입출력 창으로 이동하는 버튼을 만들어 놓았다. (그래도 휑해보이는 건 디자인 감각이 부족한 탓으로 돌려본다...ㅠㅠ)



 ![2021-06-01-flakonlpy02.PNG](/img/2021-06-01-flakonlpy02.PNG)

<center><I>어울리지 않는 아x폰 껍데기는 웃으며 넘어가도록 하자... </I></center><br>

**Click Me !** 라는 (~~다소 어이없는~~) 문구의 버튼을 클릭하면 형태소 분석 입출력 창으로 이동하게 되며, 원하는 형태소 분석기를 선택 후 **Analysis** 버튼을 누르면 해당되는 형태소 분석기를 통해 분석된 결과가 <u>분석 결과</u> 창에 출력되도록 했다.



![2021-06-01-flakonlpy03.PNG](/img/2021-06-01-flakonlpy03.PNG)

<center><I>형태소 분석기 화면 </I></center><br>

형태소 분석기의 경우 다음의 5가지 중 하나를 선택할 수 있고, 각 형태소 분석기마다의 결과는 다음과 같다. 입력 문장은 '아버지가방에들어가셨다.'로 하였으며, 띄어쓰기를 하지 않은 이유는 형태소 분석기들이 '아버지가/방에'로 분석할지 '아버지/가방에'로 분석할지 궁금해서였다. 분석 결과는 어절 단위와 음절 단위 중 이용하는 형태소 분석기에 따라 입력 값의 전처리 방향에 대해 조금이나마 도움을 줄 수 있지 않을까 싶었다.



- [꼬꼬마](http://kkma.snu.ac.kr/)

  - '아버지'와 '가방'으로 분석되었다.

  - ![2021-06-01-flakonlpykkma.PNG](/img/2021-06-01-flakonlpykkma.PNG)

    <center><I>꼬꼬마 형태소 분석기의 분석 결과</I></center><br>

- [코모란](https://github.com/shin285/KOMORAN)

  - '아버지'와 '가방'으로 분석되었다.

  - ![2021-06-01-flakonlpykomoran.PNG](/img/2021-06-01-flakonlpykomoran.PNG)

    <center><I>komoran 형태소 분석기의 분석 결과</I></center><br>

- [한나눔](http://semanticweb.kaist.ac.kr/home/index.php/HanNanum)

  - '아버지가방에들어가'로 분석된 것으로 보아 음절 단위의 분석은 다소 어려움이 있는 것으로 보인다.

  - ![2021-06-01-flakonlpyhannanum.PNG](/img/2021-06-01-flakonlpyhannanum.PNG)

    <center><I>한나눔 형태소 분석기의 분석 결과</I></center><br>

- [Mecab](http://eunjeon.blogspot.com/)

  - '아버지' 와 '방'으로 분석되었다. 

  - ![2021-06-01-flakonlpymecab.PNG](/img/2021-06-01-flakonlpymecab.PNG)

    <center><I>Mecab 형태소 분석기의 분석 결과</I></center><br>

- [Okt](https://github.com/open-korean-text/open-korean-text)

  - '아버지' 와 '가방'으로 분석되었다.

  - ![2021-06-01-flakonlpyokt.PNG](/img/2021-06-01-flakonlpyokt.PNG)

    <center><I>Okt 형태소 분석기의 분석 결과</I></center><br>

## 결과

물론, 예시 문장 하나만으로 어떠한 형태소 분석기가 음절 단위의 형태소 분석을 모두 잘 해낸다와 같은 결과는 과도한 해석일 것이다. 다만, konlpy에서 지원하는 한국어 형태소 분석기는 어절 단위의 분석이 음절 단위의 분석보다 분석 정확도가 높으므로 형태소 분석기를 사용할 때 입력 값을 적절히 전처리 해주면 보다 좋은 분석 결과를 도출할 수 있다라는 (~~당연한 결과는~~) 것은 알 수 있었다. 

물론 해당 서비스의 개발 목적이 형태소 분석기 자체가 아니라 Flask + gunicorn + Nginx 조합으로 웹 애플리케이션 서비스를 배포해보자 라는 목적이었기 때문에 일련의 성과는 얻을 수 있었다고 생각한다. 간단한 형태의 서비스도 이렇게나 머리 아픈데 완전한 형태의 서비스를 만들고, 지속적으로 유지보수 및 업데이트 해나가는 개발자 분들에게 존경을 표하는 바이다...



## 후기

- KKma와 okt가 다른 형태소 분석기에 비해 유독 느리다. 일정 시간이 넘어버리면 timeout이 되어 아예 오류 페이지로 이동해버린다. (ㅠㅠ)
- .sock 서비스 접근 권한 오류가 발생했었다. 처음엔 에러 원인도 몰랐다가 error_log를 찍어서 보니 접근 권한 오류가 발생한 것을 확인할 수 있었고, 수정했다.
- Oracle Cloud의 무료 티어가 생각보다 쓸만하다는 생각이 계속 들었다. 간단한 서비스라면 무료 티어를 활용하는 방안도 하나의 선택지가 될 것으로 보인다.
- 설계 자체에 대한 아쉬움이 계속 남는다. 석사과정 당시엔 관련 서비스 설계에 대한 지식이 부족했고, 돌아만 가면 되지라는 생각으로 만들었던 서비스이기 때문에 완성도가 굉장히 조악하다. 만약 지금이라면 구조 자체를 형태소 분석기 모델이 돌아가는 RESTAPI 서버 하나와 React or Vue로 구성된 FrontEnd 서버 하나로 이원화시켜 운영할 것 같다. Backend는 값에 대해서만 처리하고, Frontend에서는 입출력단만 처리하여 뿌릴 수 있도록 하는 형태로 하면 어땠을까 생각해본다.
- 개발된 코드는 [Github](https://github.com/zellyshu/flakonlpy)에 올려두었다. 굉장히 조악하고, 미흡한 코드지만 조금이나마 도움이 되었으면 한다...



## Reference

- https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-centos-7



