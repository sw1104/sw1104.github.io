---
title: "[Python] requirements.txt로 패키지 관리"
excerpt: "requirements.txt로 패키지 관리"

categories:
    - Python
tags:
    - [project]

permalink: /python/requirements

toc: true
toc_sticky: true

date: 2022-12-21
last_modified_at: 2022-12-21
---

파이썬을 이용하여 로컬에서 프로젝트를 진행한 뒤 EC2로 옮기면서 패키지를 옮기기 위해 찾아보았다.

requirements.txt는 파이썬에서 사용한 패키지의 목록이 나열되어있는 텍스트 파일이다. 

이름은 꼭 requirements로 할 필요는 없지만 대부분 이 이름으로 관리하니 이렇게 관리하면 된다.

프로젝트 디렉토리로 가서 다음 명령어를 작성하면 자동으로 이 텍스트 파일을 만들어준다.

```
pip freeze > requirements.txt
```

이제 다른 환경에서 해당 텍스트파일을 가지고 패키지를 설치하려면 다음 명령어를 작성한다.

```
pip install -r requirements.txt
```