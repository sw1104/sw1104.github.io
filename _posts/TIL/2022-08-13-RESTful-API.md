---
title: "RESTful API"
excerpt: "RESTful에 대해.."

categories:
  - TIL
tags:
  - [network]

permalink: /TIL/RESTful API/

toc: true
toc_sticky: true

date: 2022-08-13
last_modified_at: 2022-08-13
---

# REST API
REST API란 REST하게 API를 서술하는 방법을 말한다. 
REST(Representation State Transfer)의 약자로 상태를 전달하는 것을 나타내는 방법을 의미한다. 자연을 이름으로 구분하여 자원의 상태를 주고받는다.

# 장점
가장 명확한 장점은 self-descriptiveness 이다. 그 자체만으로도 API의 목적이 쉽게 이해되기 때문이다.

# RESTful API vs SOAP

### SOAP
SOAP(Simple Object Access Protocol)은 XML기반의 메세지 전송 프로토콜이다. REST와는 보안이나 메세지 전송 방식등이 다르다. 보편적인 웹 서비스보다는 기업 또는 특정 조직 내부에서 사용하기에 적합하다.

### SOAP 특징
- 원격 프로시저 호출패턴
- 플랫폼에 종속되지 않아 이 기종 간 통신 가능
- 헤더와 바디를 가지며 헤더에는 메타 정보, 바디에는 실제 정보가 들어감
- 규약, 에러 처리, 분산 처리에 대한 옵션이 내장되어 있음

### REST와 SOAP의 차이

SOAP은 프로토콜이고, REST는 API 설계스타일이다.

- REST는 데이터 중심으로 묘사, SOAP은 행위, 기능 중심으로 서술
- REST는 여러 형태의 데이터(html, json. text)를 전송하지만 SOAP은 XML 데이터만 전송
- REST는 요청과 응답에 대한 캐시를 사용할 수 있음.
- REST는 ACID 특성과 관련된 내용이 없지만 SOAP은 자체 기준이 있다.

#### REST
![](https://velog.velcdn.com/images/sangwoo/post/e5c59660-fc3d-4d73-b161-de7ef38e68f2/image.png)

#### SOAP
![](https://velog.velcdn.com/images/sangwoo/post/92dc2b1e-9776-4d74-8518-eb201f45db4c/image.png)

# RESTful API 설계
REST는 설계 규칙이므로 일반적으로 널리 통용되는 규칙이 있다. 프로그램의 원활한 유지보수와, 개발자들 사이에서의 커뮤니케이션을 돕도록 만들어졌다.

## Uniform Interface

REST는 API를 설계할 때 자원을 중심으로 만드는 것이 원칙이므로 URI 자원으로 한정된 일관적인 인터페이스를 구현한다.

일바적으로 HTTP를 구성하는 URL, HTTP method, Status Code를 통해 구현한다.
- URL은 동사가 아닌 명사로 구성한다.
- Resource에 대한 행위를 HTTP method(GET, POST 등)로 표현한다.
- Resource 사이에 연관 관계 및 계층 관계가 있는경우 `/` 를 이용한다.
- URL마지막 문자로 `/`를 포함하지 않는다.
- URL이 길어지는 경우 `-`을 사용하여 가독성을 높인다.
- 파일 확장자는 URL에 포함시키지 않는다.

>예시
[GET]	/users/1
[POST]	/users/2
[DELETE]	/users/1
[GET] /users/portfolios
[GET] /users/1/ordered-items
[GET] /users/1/profile-photo

