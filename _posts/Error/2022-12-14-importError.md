---
title: "[Error] ImportError: libGL.so.1: cannot open shared object file: No such file or directory"
excerpt: "ImportError: libGL.so.1: cannot open shared object file: No such file or directory"

categories:
    - error
tags:
    - [error]

permalink: /error/2

toc: true
toc_sticky: true

date: 2022-12-14
last_modified_at: 2022-12-14
---

#### **문제**

python django를 이용한 프로젝트를 로컬에서 작업을 마치고 배포를 위해 AWS EC2(OS: ubuntu) 옮겨 패키지를 설치하니 밑에 에러가 날 반겨주었다.

```
ImportError: libGL.so.1: cannot open shared object file: No such file or directory
```

#### **해결**

검색을 통해 스택오버플로우에서 다음과 같은 해결법을 찾아내어 해결할 수 있었다.

```
sudo apt-get update && apt-get install libgl1
```

외에도 다른 해결 방법이 있으니 특히 도커를 이용한다면 밑에 스택오버플로우를 참고하여 자신에게 필요한 해결법을 찾는 것을 추천한다.



[stackoverflow](https://stackoverflow.com/questions/55313610/importerror-libgl-so-1-cannot-open-shared-object-file-no-such-file-or-directo)