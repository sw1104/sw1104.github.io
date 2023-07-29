---
title: "[Error] Could not resolve org.springframework.boot:spring-boot-gradle-plugin:3.0.2."
excerpt: "자바8과 스프링부트3 에러"

categories:
    - error
tags:
    - [error]

permalink: /error/java-spring-boot-version-error

toc: true
toc_sticky: true

date: 2023-03-10
last_modified_at: 2023-03-10
---

## Java8 버전과 Spring Boot3 버전 에러

### 문제

자바를 이용하여 프로젝트를 진행하기 위해 초기세팅을 하는 과정에서 아래 에러를 마주하였다.

![Alt text](../../assets/images/posts_img/Error/2023-03-10-java8Springboot3Error.png)

### 해결

해당 에러는 자바 8버전에서 스프링부트3를 지원하지 않기 때문에 발생한다. 따라서 스프링부트2를 사용하면 해결된다.