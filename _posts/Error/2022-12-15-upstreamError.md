---
title: "[Error] upstream prematurely closed connection while reading response header from upstream"
excerpt: "upstream prematurely closed connection"

categories:
    - Error
tags:
    - [Error]

permalink: /error/3

toc: true
toc_sticky: true

date: 2022-12-15
last_modified_at: 2022-12-15
---

#### **문제**

배포된 사이트에서 클라이언트로부터 서버로 통신을 받을 때 502 Bad Gateway가 떠서 nginx의 에러로그를 살펴보니 다음과 같이 로그가 떴다.

![Alt text](../../assets/images/posts_img/Error/2022-12-15-upstream.png)

여기서 upstream은 Django를 말하니 풀이해보면 장고에서 응답 헤더를 읽는동안 연결이 끊겼다는 것이다. timeout 에러인것을 확인하고 검색을 통해 nginx에서 proxy timeout 설정을 해주었다. 참고로 nginx의 timeout default 값은 60이다. 

```
location / {

	...

	proxy_read_timeout 300;
    	proxy_connect_timeout 300;
    	proxy_send_timeout 300;

    	...
    
}
```

설정 후 테스트를 해주고 재시작을 해주었지만 같은 에러가 계속되었다.

내 서버는 nginx - gunicorn - django로 통신이 가기 때문에 혹시나 하고 gunicorn의 timeout default 값을 검색했다.

#### **해결**

생각했던대로 gunicorn의 timeout default 값은 30초여서 여기서 끊기는것이었다. 그래서 gunicorn.service 파일을 열어 다음과 같이 설정했다. 설정은 ExecStart에 해주면 된다.

```
[Unit]
...

[Service]
...
ExecStart=
	...
	--timeout=120
	...

[Install]
...
```