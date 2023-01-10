---
title: "[Error] CORS error"
excerpt: "has been blocked by CORS policy: Response to preflight request doesn't pass access control check:
No 'Access-Control-Allow-Origin' header is present on the requested resource."

categories:
    - Error
tags:
    - [Error]

permalink: /error/4

toc: true
toc_sticky: true

date: 2022-12-15
last_modified_at: 2022-12-15
---

#### **문제**

클라이언트와 통신을 하면서 CORS 에러를 마주하였다. 로컬에서 작업을 할 때 django settings.py에서  cors 세팅을 해줘서 한번도 뜨지 않았는데 갑자기 뜨니 적잔히 당황을 했다.

> has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.

#### **해결**

로컬과의 차이라면 nginx의 유무였기 때문에 nginx에서 다시 해주어야 하는건가 하는 생각에 nginx에 다음 코드를 추가해서 해결을 했다.

```
proxy_hide_header Access-Control-Allow-Origin;
add_header 'Access-Control-Allow-Origin' '허용하고자 하는 도메인';
```


#### **참고** 


[enable-cors.org](https://enable-cors.org/server_nginx.html)