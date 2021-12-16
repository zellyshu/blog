---

title: "FastAPI 설치 및 테스트"
date: 2021-04-08 00:00:00
tags: ["FastAPI", "Python"]
categories: ["Python", "Backend"]
author: "Me"
draft: false

---

## Intro

대학원 시절 기계 학습한 모델을 활용하여 데모 서비스를 만들어 시연해야 할 일이 종종 있었다.

당시, 개발한 형태소 분석기 모델을 가지고 다른 연구실처럼 웹에 형태소 분석기를 띄워 데모 형태로 서비스를 하자는 의견이 나왔고, 서비스 구현 파트를 맡게 되었다. (이 때는, input으로 입력값을 받도록 하고 개발 툴에서 run 하면 이용자도 들어와서 사용할 수 있을 거라는 발칙한 생각을 할 정도로 구조도 뭣도 아무것도 모르는 상황이었다.) 

Python도 아등바등 해나갔었는데 하물며 웹 프레임워크 관련 지식은 더더욱 기초지식조차 없었던 터라 일단 Python만 가지고 서비스를 만들어야겠다 생각했다.

검색을 통하여 웹 프레임워크라는 것을 알게 되었고, Django 책을 사서 따라해봤으나 도저히 무슨 말인지 이해가 안되는 총체적 난국 그 자체였다. 

그래서 최대한 쉽고 빠르게 만들 수 있다는 Flask라는 웹 프레임워크를 통하여 서비스를 만들어보기 시작했다.

하지만, 서비스가 돌아가는 구조 자체가 머릿 속에 그려지지 않았던 터라 이게 왜 안 나오고, 이게 왜 나오는지 이해하는데만 시간이 엄청 소요되었던 기억이 난다.

(처음엔 HTML과 CSS, jquery만 가지고 만들다 나중엔 화려하게 만들어보겠다는 일념하에 D3.js 및 javascript를 열심히 활용하여 만들었다는 이야기는 언젠가 작성하는 걸로...)

졸업 이후에도 간단한 토이 프로젝트 서비스를 만들 때 Flask를 사용하곤 했는데 언젠가부터 검색할 때마다 FastAPI라는 웹 프레임워크가 같이 출현하는 결과가 많아지길래 한 번 설치하고 테스트해보자 마음먹게 되었다.



## FastAPI

**FastAPI**는 현대적이고, 빠르며(고성능), 파이썬 표준 타입 힌트에 기초한 Python3.6+의 API를 빌드하기 위한 웹 프레임워크라고 공식문서에서 소개하고 있다.

주요 장점은 다음과 같다.

### 1. 빠른 속도
   - 공식 문서에서는 NodeJS 및 Go와 대등할 정도로 매우 높은 성능을 자랑한다고 이야기 하고 있다.
### 2. 쉽고, 짧으며, 직관적
   - 실제 예제 코드를 작성해보면서 Flask와 비슷하다는 느낌을 받았을 정도로 매우 경량화되어 있다.
### 3. 비동기 (Starlette 프레임워크 기반)
   - WSGI가 아닌 ASGI를 지원
### 4. API 문서 자동화 및 작성을 위한 Swagger UI, Redoc 제공
   - 개인적으로, 기존 Flask로 구현하였던 서비스를 FastAPI로 바꿔보아야겠다는 생각이 들게 한 큰 부분이었다.

추가적인 사항으로는 **Python 3.6 이상**의 버전을 지원하고 있고, 라이센스는 **MIT License**를 따르고 있다.

설치 테스트는 WSL2에 설치한 Ubuntu 20.04 LTS 환경에서 진행하였고, Conda 가상환경(Python 3.7.6)에서 설치하였다.



## 설치 및 테스트

- 설치 및 테스트는 [공식 문서](https://fastapi.tiangolo.com/ko/)를 참고했다.

  

### 1. conda 가상환경 생성 

   - python 3.7 버전에서 테스트

   

   ```bash
conda create -n fastapitest python=3.7
   ```


### 2. conda 가상환경 활성화


   ```bash
   conda activate fastapitest
   ```

   

   

### 3. pip로 FastAPI 및 [Uvicorn](https://www.uvicorn.org/) 설치

   

   ```bash
   pip install fastapi
   pip install uvicorn[standard]
   ```

   

   ![2021-04-08-fastapi-install-test01.PNG](/img/2021-04-08-fastapi-install-test01.PNG)

   <center><I>FastAPI로 인하여 설치된 라이브러리 리스트</I></center><br>

### 4. main.py 생성

   

   ```python
   from typing import Optional
   from fastapi import FastAPI
   
   app = FastAPI()
   
   @app.get("/")
   def read_root():
       return {"Hello": "World"}
   
   @app.get("/items/{item_id}")
   def read_item(item_id: int, q: Optional[str] = None):
       return {"item_id": item_id, "q": q}
   ```

   

### 5. 서버 실행

   

   ```bash
   uvicorn main:app --reload
   ```

   ![2021-04-08-fastapi-install-test02.PNG](/img/2021-04-08-fastapi-install-test02.PNG)

   

   

### 6. GET 확인

   ```
   http://127.0.0.1:8000/items/5?q=somequery
   ```

   ![2021-04-08-fastapi-install-test03.PNG](/img/2021-04-08-fastapi-install-test03.PNG)

   ![2021-04-08-fastapi-install-test04.PNG](/img/2021-04-08-fastapi-install-test04.PNG)

   <center><I>Postman으로 테스트 시에도 결과가 잘 나오는 것 확인 가능</I></center><br>

### 7. main.py 내용 추가

   

   ```python
   from typing import Optional
   
   from fastapi import FastAPI
   // 추가 
   from pydantic import BaseModel
   
   app = FastAPI()
   
   // 추가 
   class Item(BaseModel):
       name: str
       price: float
       is_offer: Optional[bool] = None
   
   
   @app.get("/")
   def read_root():
       return {"Hello": "World"}
   
   
   @app.get("/items/{item_id}")
   def read_item(item_id: int, q: Optional[str] = None):
       return {"item_id": item_id, "q": q}
   
   
   // 추가
   @app.put("/items/{item_id}")
   def update_item(item_id: int, item: Item):
       return {"item_name": item.name, "item_id": item_id}
   ```

   

   

### 8. 대화형 API 문서 확인

   - —reload 옵션으로 인하여, 자동으로 업데이트 됨

   

   ```
   http://127.0.0.1:8000/docs
   ```

   

   ![2021-04-08-fastapi-install-test07.PNG](/img/2021-04-08-fastapi-install-test07.PNG)

   - PUT 메소드 내 매개변수들을 업데이트 함으로써 내용이 수정되는 것을 확인할 수 있음 (Try it out - Execute)

     ![2021-04-08-fastapi-install-test08.PNG](/img/2021-04-08-fastapi-install-test08.PNG)

   <center><I>Try It Out 선택 시 매개변수 입력 폼 활성화</I></center><br>

   ![2021-04-08-fastapi-install-test10.PNG](/img/2021-04-08-fastapi-install-test10.PNG)

   <center><I>item_id 에 값 입력</I></center><br>

   ![2021-04-08-fastapi-install-test11.PNG](/img/2021-04-08-fastapi-install-test11.PNG)

   <center><I>Execute 클릭 시 하단에 결과 출력</I></center><br>

### 9. Alternative Docs (대안 문서) 확인

   - —reload 옵션으로 인하여, 자동으로 업데이트 됨

   ```
   http://127.0.0.1:8000/redoc
   ```

   

   ![2021-04-08-fastapi-install-test12.PNG](/img/2021-04-08-fastapi-install-test12.PNG)

   <center><I>ReDoc UI로도 확인 가능</I></center><br>



## etc

- 공식 문서가 상당히 잘 기술되어 있는 것이 특징
- FastAPI 의 소개와 설치 및 테스트, 헤더 매개변수 등의 경우 한글화가 되어 있으나 다른 부분은 한글화가 이루어지지 않은 상태. (2021년 04월 08일 기준)





## Reference

- https://fastapi.tiangolo.com/ko/
- https://github.com/tiangolo/fastapi



