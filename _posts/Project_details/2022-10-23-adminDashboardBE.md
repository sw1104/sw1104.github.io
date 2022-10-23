---
title: "기업 인턴십 프로젝트 - 관리자 대시보드 BACKEND CODE"
excerpt: "관리자 대시보드 파헤치기"

categories:
    - Project
tags:
    - [Project, wecode]

permalink: /Project_details/cooperation/adminDashboard/backend

toc: true
toc_sticky: true

date: 2022-10-23
last_modified_at: 2022-10-23
---

# 기획

기업 인턴십 프로젝트에서 내가 맡은 부분 중 하나인 관리자 대시보드에는 `전체 토큰 수량`이 나오고 유저에게 `발행된 토큰 수량`과 발행되지 않고 `남아있는 토큰 수량` 그리고 `전체 유저수`와 `유저가 가지고 있는 토큰 수량` 그리고 마지막으로 `최근 토큰을 발행해간 유저의 이메일과 수량`을 구현하기로 기획을 하고 시작을 했다.

# 구현 사항

## JWT TOKEN에서 유저의 정보를 받아오는 방법 수정

API 개발을 진행하면서 동료가 개발한 로그인시 토큰을 발행하는 로직에서 PAYLOAD에 담겨있는 유저\_id와 등급\_id를 매번 꺼내서 비교해주어야 하다보니 비효율적이라 생각이 들었다. 그래서 프론트로부터 JWT TOKEN을 받아 검증하는 과정에서 바로 body에 담아주도록 수정했다.

```javascript
// 기존 코드
const validation = async (req, res, next) => {
    if (req.header("accessToken")) {
        const token = req.header("accessToken");
        const result = verify(token);
        info = jwt.decode(token);
        if (result.ok && info.userGrade == 1) {
            next();
        } else {
            res.status(401).json({
                ok: false,
                message: result.message
            });
        }
    }
};

// 수정된 코드
const validation = async (req, res, next) => {
    if (req.header("accessToken")) {
        const token = req.header("accessToken");
        const result = verify(token);
        info = jwt.decode(token);
        if (result.ok && info.userGrade == 1) {
            // 유저의 아이디와 등급을 변수에 할당해준 뒤
            let userId = info.userId;
            let userGrade = info.userGrade;
            // 문자로 할당된 유저의 아이디와 등급을 숫자형으로 바꿔주고 body에 담아주었다.
            req.body.userId = Number(userId);
            req.body.userGrade = Number(userGrade);
            next();
        } else {
            res.status(401).json({
                ok: false,
                message: result.message
            });
        }
    }
};
```

---

## 전체 토큰 수량

```javascript
// 관리자만 볼 수 있는 페이지이기 때문에 관리자 등급인 1을 service에서 검증해준다.
// 이 부분은 모든 로직에 들어가 있어 앞으로는 생략한다.
const getFullToken = async (userGrade) => {
    if (userGrade === 1) {
        return await adminDashboardDao.getFullToken();
    }
    throw new Error("GRADE ERROR", 400);
};
// query
const getFullToken = async () => {
    return await AppDataSource.query(
        `
        SELECT 
            full_token AS fullToken
        FROM master_wallets
        `
    );
};
```

---

## 발행된 토큰 수량

wallets table에 유저가 가지고 있는 토큰에 대한 정보가 있고 all_token은 해당 유저가 가지고 있는 토큰 수량이기 때문에 all_token 컬럼을 모두 더해줘서 발행된 토큰으로 구현했다.

```javascript
const getIssuedToken = async () => {
    return await AppDataSource.query(
        `
        SELECT 
            sum(all_token) AS allToken
        FROM wallets
        `
    );
};
```

---

## 남은 토큰 수량

전체 토큰 수량에서 발행된 토큰 수량을 빼주는 방식으로 구현했다.

```javascript
const getRemainToken = async () => {
    return await AppDataSource.query(
        `
        SELECT 
            mw.full_token - sum(w.all_token) AS remainToken
        FROM master_wallets mw 
        INNER JOIN wallets w 
        GROUP BY mw.full_token
        `
    );
};
```

---

## 전체 유저 수

users table의 로우 개수 - 1(관리자)를 해서 구현했다.

```javascript
const getMembers = async () => {
    return await AppDataSource.query(
        `
        SELECT
            count(*)-1 AS member
        FROM users
        `
    );
};
```

---

## 유저가 가지고있는 토큰 수량

users와 wallets table을 join해서 id와 email, all_token을 응답한다.

```javascript
const getPersonalToken = async () => {
    return await AppDataSource.query(
        `
        SELECT 
            w.user_id,
            u.email,
            w.all_token
        FROM wallets w 
        INNER JOIN users u ON u.id = w.user_id
        `
    );
};
```

## 최근 발행된 토큰 수량

최신화를 위해 updated_at을 이용하여 내림차순으로 구현했다.

```javascript
const getNewIssuedToken = async () => {
    return await AppDataSource.query(
        `
        SELECT 
            u.email,
            h.add_token
        FROM histories h
        INNER JOIN users u ON u.id = h.user_id
        WHERE h.add_token > 0 AND h.state_id = 2
        ORDER BY h.updated_at DESC LIMIT 24
        `
    );
};
```

---

## 토큰 회수

wallets 테이블의 회수 컬럼에는 유저가 가진 all_token을 담아서 회수 이력을 만들어주고 유저가 가진 토큰과 포인트를 0으로 만들어주는 방식으로 구현했다.

```javascript
const tokenCollect = async (userId) => {
    return await AppDataSource.query(
        `
        UPDATE wallets w
        INNER JOIN users u
        ON u.id = w.user_id
        SET
            w.collect_token = w.all_token,
            w.all_token = 0,
            u.point = 0
        WHERE w.user_id = ${userId}
        `
    );
};
```
