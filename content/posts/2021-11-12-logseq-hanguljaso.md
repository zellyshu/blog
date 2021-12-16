---

title: "macOS에서 작성한 Logseq 내용이 Windows 환경에서 보이지 않을 때 (한글자소분리 이슈)"
date: 2021-11-12 15:44:05
creation date: 2021-10-27 11:22
modification date: 2021-11-12 15:44
tags: ["Logseq", "한글자소분리", "macOS", "Windows"]
categories: ["Logseq", "한글자소분리"]
author: "Me"
draft: false

---

## Intro

최근 Logseq와 Obsidian을 같이 활용하여 생각을 정리해보고자 하고 있다. 회사에서는 Windows를 사용하지만, 평상시에는 mac OS 및 Apple 생태계의 기기들을 주로 사용하는 나에게 있어 크로스 플랫폼은 필수적인 요소인데 이를 만족하는 것이 Obsidian이었다. 최근 모바일 앱도 나와서 쏠쏠하게 사용 중에 있는데, 이를 활용하여 [Logseq + Obsidian 함께 활용하기](https://luran.me/425)를 보고 Logseq를 통해서도 정리해보고자 마음을 먹게 된 것이다. 



## 그런데 글이 안보인다...?

icloud를 통하여 동기화시켜서 mac 환경과 windows 환경에서 모두 사용할 수 있게끔 세팅을 마쳤다. 그렇게 사용하고 있다가 mac에서 정리해둔 글을 Windows에서 확인하려 했는데 글이 보이지 않았다. 정확히는 [[]] 로 링크를 걸어둔 부분들이 보이지 않았다. 그래서 클라우드 업로드 내역을 확인해보니 해당 파일이 있다는 것은 확인할 수 있었다. 하지만 파일명이 전부 깨져있었다... 



## 한글 자소분리 이슈

mac에서 email로 file 같은 걸 첨부해서 보낼 때마다 심심치 않게 파일명이 다 깨져서 오는 일이 있다. 한글자소분리 이슈때문인데 이와 마찬가지로 mac에서 작성한 글이 windows 환경에서 logseq에서 글이 보이지 않았던 이유는 mac에서 작성한 마크다운 파일이 클라우드에 자동 업로드 되면서 한글로 된 파일명이 자소분리가 되어 업로드되기 때문이었다. 

이를 해결해주는 프로그램을 감사히도 만들어주신 분이 있어 [[Windows] 한글 자소 교정기 ver.2](https://namocom.tistory.com/630)을 다운받아서 바로 파일명을 일괄 수정해주었다. 수정 이후 logseq에 들어가보니 잘 나오는 것을 확인할 수 있었다.


## 후기

너무나 간단한 이유였지만 혹시나 누군가에게 도움이 될까 싶어 이슈사항을 정리해둔다.



## Reference

- https://namocom.tistory.com/630
- https://luran.me/425