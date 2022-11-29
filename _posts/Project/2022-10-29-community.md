---
title: "커뮤니티 서비스 팀 프로젝트"
excerpt: "원티드 프리온보딩 팀 프로젝트"

categories:
    - Project
tags:
    - [Project, wanted]

permalink: /Project/wanted-1/

toc: true
toc_sticky: true

date: 2022-10-29
last_modified_at: 2022-10-29
---

# community_service

## 프로젝트 개요

커뮤니티 서비스 형태의 API 프로젝트이다.\
자유게시판, 공지사항, 운영게시판이 있으며 회원 등급의 권한 별로 CRUD가 가능하다.\
또한 유저 성별, 시간대별 유저 접속, 게시판 작성 비율 등 여러 통계 기능이 있다.

[프로젝트 github](https://github.com/PreOnboarding-Team-F/community_service)

### 개발 인원
- Backend 4명

**담당자**
| 박주현  | 이상우 | 정효진 | 이수현 |
|---|---|---|---|
| 통계기능 | 댓글기능 | 게시판기능 |  유저기능 |

### 개발 기간
2022-10-26~2022-10-28 (3일)

### 기술 스택
- Framework: express
- ORM : prisma
- DB : mysql

## ERD
![image](https://user-images.githubusercontent.com/55984573/198518910-64d8373e-6a68-4a93-a499-003ce7ab5bff.png)

## 회고록

내가 구현한 기능은 댓글과 대댓글 관련 API이다.
댓글과 대댓글 관련해서 CRUD는 API를 나누지 않고 각각 하나의 API에서 댓글과 대댓글로 나누어지도록 구현을 하였다. 

지금까지 프로젝트를 하면서 구현했던거랑 비슷한 방향으로 흘러서 크게 어려운 점은 없었다. 다만 orm을 `typeorm`만 사용하다가 `prizma`라는 orm을 이용하다보니 처음에 적응하는데 시간이 좀 필요했다.

그리고 지금까지는 동기들과 프로젝트를 진행하다보니 개개인의 실력이 다르더라도 크게 차이가 나지 않았지만, 원티드 프리온보딩에는 현업에서 일을 하다 오신 분도 계시고 1년 가까이 공부하신 분도 계시다보니 말씀하시는 것과 코드를 통해서 배울점을 많이 느낄 수 있었다.

